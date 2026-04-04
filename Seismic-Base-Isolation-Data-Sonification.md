# Seismic Base Isolation Data Sonification
## Early Data Howto

*Source: Google AI research session, April 4, 2026*
*Reference: https://share.google/aimode/PJ4d2cobQORlz84At*

---

## 1. What Data Can Be Extracted

The Geffen Galleries sit on a seismic base isolation system. The isolators — **Triple Friction Pendulum (TFP) bearings** — generate continuous structural data during any seismic event or ambient vibration. Data types available for sonification:

| Data Metric | Description |
|---|---|
| **Vibration & Acceleration** | High-frequency accelerations; audified by shifting into audible range |
| **Horizontal Displacement** | Lateral sway/sliding distance of each isolator |
| **Natural Frequency (Period)** | Isolation lengthens building period; represents as downward pitch slide |
| **Damping & Energy Dissipation** | Kinetic energy → heat (hysteresis); maps to timbre or "grainy" texture |
| **Structural Health Indicators** | Long-term strain, material degradation, corrosion anomalies |

**Analysis focus areas:**
- **Period Lengthening** — structural period shifts away from high-energy earthquake ranges
- **Acceleration Reduction** — up to 80%+ reduction in horizontal X/Y directions
- **Isolator Displacement** — large horizontal displacements at isolation level

---

## 2. Mapping Data to Sound

| Data Metric | Sound Attribute | Interpretation |
|---|---|---|
| Peak Acceleration | Loudness (Velocity) | Intensity of seismic force |
| Spectral Frequency | Pitch (Note Number) | Higher freq (stiffer response) = higher pitch |
| Temporal Displacement | Rhythm / Panning | Stereo pan represents spatial movement of building base |
| Damping Ratio | Decay / Timbre | Faster decay = higher damping, more energy absorption |

---

## 3. Why Granular Synthesis

Granular synthesis is the correct sonification method for base isolation data — not simple pitch-shifting. It captures the **mechanical grinding or sliding** nature of a Friction Pendulum System.

**The mapping logic:**
- **Seismic Displacement → Grain Position** — as the isolator slides further from center, the grain pointer moves deeper into the source sample
- **Acceleration (G-force) → Grain Density** — higher shaking intensity triggers more grains per second, creating a denser, more chaotic texture
- **Damping Ratio → Grain Envelope (Duration/Decay)** — high damping creates short, plucky grains; low damping creates long, overlapping, blurred grains

When a TFP isolator is sliding, grains sound like a physical object moving across a surface — an intuitive acoustic representation of structural safety state.

---

## 4. Python Implementation (Core Logic)

Uses `numpy` to window seismic data into grains. No ObsPy. No scipy beyond wavfile I/O.

```python
import numpy as np
from scipy.io import wavfile

def generate_seismic_grain(source_audio, seismic_data):
    # Parameters
    grain_size_ms = 50
    sr = 44100
    grain_samples = int(sr * (grain_size_ms / 1000))

    output = []

    for val in seismic_data:
        # 1. Map seismic value to a position in the source audio
        #    Normalize val (0 to 1) and multiply by audio length
        pos = int(val * (len(source_audio) - grain_samples))

        # 2. Extract the grain
        grain = source_audio[pos : pos + grain_samples]

        # 3. Apply a Hanning window (to prevent clicks)
        window = np.hanning(len(grain))
        weighted_grain = grain * window

        output.extend(weighted_grain)

    return np.array(output, dtype=np.int16)
```

**Key parameters to map from TFP data:**
- `grain_size_ms` ← damping ratio
- `pos` ← horizontal displacement (normalized)
- grain density (calls per second) ← acceleration (G-force)

---

## 5. AWS Pipeline (Reference Architecture)

For live or near-live data ingestion from installed sensors:

1. **Trigger** — IoT sensor on a TFP bearing sends JSON payload to AWS IoT Core
2. **Process** — IoT Core triggers a Terraform-managed Lambda function
3. **Synthesize** — Lambda pulls a base texture from S3, applies granular logic against sensor values, renders a new audio clip
4. **Output** — Sonified result saved back to S3 or streamed to a live endpoint

### Terraform Configuration (Skeleton)

```hcl
# IAM Role for Lambda
resource "aws_iam_role" "seismic_lambda_role" {
  name = "seismic_base_isolation_role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action    = "sts:AssumeRole"
      Effect    = "Allow"
      Principal = { Service = "lambda.amazonaws.com" }
    }]
  })
}

# Lambda Function
resource "aws_lambda_function" "seismic_analysis" {
  filename      = "seismic_analysis.zip"
  function_name = "calculate_base_isolation_performance"
  role          = aws_iam_role.seismic_lambda_role.arn
  handler       = "index.handler"
  runtime       = "python3.9"

  environment {
    variables = {
      DB_NAME = "seismic_data"
    }
  }
}
```

**Key AWS components:**
- **Lambda** — Python/Node.js numerical modeling or real-time sensor data processing
- **IAM Role** — permissions for S3, DynamoDB, CloudWatch access
- **CloudWatch Events / EventBridge** — schedule-based or data-arrival triggers
- **S3 / DynamoDB** — historical sensor data, analysis logs, audio output storage

---

## 6. SeisClaw / SOS Integration Notes

- Use **pure MiniSEED binary parsing** — no ObsPy dependency
- Direct waveform data → granular sonification pipeline
- Grain parameters driven by isolator displacement and acceleration values
- Target: continuous live audio stream suitable for parametric speaker output (Holosonics)

---

*Next step: TBD instrument specification for TFP sensor data API / access point at LACMA Geffen.*
