# Method Pseudocode

> Non-executable research pseudocode. This document exposes the conceptual pipeline only. It intentionally omits concrete classes, tensor dimensions, stage indices, layer counts, expert implementations, preprocessing constants, and checkpoint-loading details.

## Algorithm 1: Reliability-Calibrated Frequency-Aware Multimodal SOD

```text
INPUT:
    primary image X_rgb
    auxiliary image X_aux                 # depth, thermal, or NIR
    frozen pretrained segmentation encoder E

OUTPUT:
    salient-object probability map Y
    auxiliary boundary prediction Y_edge

1:  R <- PATCH_EMBED(X_rgb)
2:  A <- PATCH_EMBED(X_aux)
3:  R <- ADD_POSITION_ENCODING(R)

    # MoFE: modality-aware frequency extraction and early fusion
4:  (R_low, R_high) <- FREQUENCY_DECOMPOSE(R)
5:  (A_low, A_high) <- FREQUENCY_DECOMPOSE(A)
6:  candidates <- {
        MIX(R_low,  A_low),
        MIX(R_low,  A_high),
        MIX(R_high, A_low),
        MIX(R_high, A_high)
    }
7:  routing_weights <- FREQUENCY_ROUTER(candidates)
8:  F_fused <- WEIGHTED_EXPERT_FUSION(candidates, routing_weights)
9:  F_freq  <- SELECT_DETAIL_CARRYING_COMPONENT(candidates, routing_weights)
10: R <- ENCODER_BLOCK(R + F_fused)

    # RCFA: recurrent propagation of calibrated frequency residuals
11: FOR each subsequent encoder block b DO
12:     IF b is a selected cross-stage adaptation location THEN
13:         F_freq <- ALIGN_TO_CURRENT_STAGE(F_freq, R)
14:         R_base <- LIGHTWEIGHT_BASE_ADAPTER(R)
15:         R_freq <- LIGHTWEIGHT_FREQUENCY_ADAPTER(F_freq)

            # Context gate: decide how much frequency information is useful
16:         G_context <- CONTEXT_GATE(R, F_freq)

            # Reliability gate: test cross-modal high-frequency agreement
17:         H_rgb  <- HIGH_PASS(R)
18:         H_freq <- HIGH_PASS(F_freq)
19:         cues <- {
                DIRECTIONAL_CONSISTENCY(H_rgb, H_freq),
                ENERGY_BALANCE(H_rgb, H_freq),
                NORMALIZED_DIFFERENCE(H_rgb, H_freq)
            }
20:         G_reliable <- BOUNDED_RELIABILITY_GATE(cues)

            # Residual calibration is initialized as a conservative update
21:         R_calibrated <- R_base
                              + LEARNABLE_SCALE
                              * G_context
                              * G_reliable
                              * R_freq
22:         R <- ENCODER_BLOCK_b(R + R_calibrated)
23:         F_freq <- R_calibrated
24:     ELSE
25:         R <- ENCODER_BLOCK_b(R)
26:     END IF
27:     IF b ends an encoder stage THEN
28:         APPEND(stage_features, R)
29:     END IF
30: END FOR

    # Cross-scale refinement and structure-sensitive decoding
31: pyramid <- CROSS_SCALE_EXPERT_REFINEMENT(stage_features)
32: fpn_features <- PRETRAINED_NECK(pyramid)
33: F_structure <- STATE_SPACE_DETAIL_DECODER(fpn_features)
34: (Y_logits, Y_edge_logits) <- MASK_DECODER(
        semantic_features = fpn_features,
        structural_features = F_structure
    )
35: Y      <- RESIZE_AND_ACTIVATE(Y_logits)
36: Y_edge <- RESIZE_AND_ACTIVATE(Y_edge_logits)
37: RETURN Y, Y_edge
```

## Algorithm 2: Training Objective

```text
INPUT:
    predicted mask Y
    predicted boundary Y_edge
    binary saliency mask G

1:  G_edge <- MORPHOLOGICAL_BOUNDARY(G)
2:  L_region <- DICE_LOSS(Y, G) + IOU_LOSS(Y, G)
3:  L_edge   <- DICE_LOSS(Y_edge, G_edge)
4:  L_total  <- L_region + EDGE_WEIGHT * L_edge
5:  UPDATE only the task-specific modules and the designated tunable
    parameters of the pretrained model
```

## Deliberately omitted from this preview

- Executable network definitions and package imports.
- Exact channel widths, stage locations, expert architectures, and routing implementation.
- Dataset loaders, augmentation, normalization, and resize policy.
- Parameter-freezing rules and optimizer parameter groups.
- Training schedules, initialization details, and checkpoint conversion.
- Complete decoder implementation and inference/postprocessing code.

These details are necessary to reproduce the model and will be provided with the full implementation release.

