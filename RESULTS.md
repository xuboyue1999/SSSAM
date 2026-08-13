# Published Prediction Results

All values below were computed from the prediction maps in `results/` using the project evaluation protocol. Up arrows indicate that larger values are better; MAE uses the opposite direction.

## RGB-D

| Dataset | Images | meanF | maxF | wF | MAE | Em | Sm |
|---|---:|---:|---:|---:|---:|---:|---:|
| DES | 135 | 0.92829 | 0.94384 | 0.90955 | 0.01721 | 0.95576 | 0.92303 |
| LFSD | 100 | 0.87868 | 0.90149 | 0.86408 | 0.05000 | 0.90617 | 0.88571 |
| NJU2K | 500 | 0.94651 | 0.95611 | 0.93862 | 0.01879 | 0.93910 | 0.93928 |
| NLPR | 300 | 0.94220 | 0.95389 | 0.93446 | 0.01416 | 0.97312 | 0.94365 |
| SIP | 929 | 0.92026 | 0.93924 | 0.89469 | 0.03668 | 0.93090 | 0.90153 |
| SSD | 80 | 0.90675 | 0.93454 | 0.89196 | 0.02357 | 0.93554 | 0.91815 |
| STERE | 1,000 | 0.92584 | 0.93801 | 0.91549 | 0.02424 | 0.93765 | 0.92950 |

## RGB-T

| Dataset | Images | meanF | maxF | wF | MAE | Em | Sm |
|---|---:|---:|---:|---:|---:|---:|---:|
| VT821 | 821 | 0.88284 | 0.90314 | 0.87388 | 0.02522 | 0.93142 | 0.90953 |
| VT1000 | 1,000 | 0.93521 | 0.94442 | 0.93365 | 0.01293 | 0.95504 | 0.94256 |
| VT5000 test | 2,500 | 0.90960 | 0.92529 | 0.89789 | 0.02020 | 0.95035 | 0.92074 |

## RGB-NIR zero-shot transfer

The RGB-T checkpoint was applied directly without RGB-NIR fine-tuning.

| Dataset | Images | meanF | maxF | wF | MAE | Em | Sm |
|---|---:|---:|---:|---:|---:|---:|---:|
| RGB-NIR | 780 | 0.92801 | 0.94116 | 0.92094 | 0.01954 | 0.96382 | 0.92984 |

Machine-readable RGB-D and RGB-T tables are available in `metrics/`. The SHA-256 manifest fixes the exact bytes of every published map and metric file.

