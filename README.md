# Smart Irrigation System Using Mamdani Fuzzy Logic

A Python-based **fuzzy decision-support prototype** that estimates irrigation demand from soil moisture and ambient temperature. The project implements a Mamdani Fuzzy Inference System (FIS), compares its output with a simple moisture-threshold baseline, and provides an interactive dashboard in a Jupyter/Google Colab notebook.

> **Note:** This repository contains a simulation. Its output is a normalized demand score (0–100), not a measured water volume, pump duration, or field-validated irrigation recommendation.

## Contents
- [Problem Statement](#problem-statement)
- [Project overview](#project-overview)
- [How the fuzzy system works](#how-the-fuzzy-system-works)
- [Fuzzy rule base](#fuzzy-rule-base)
- [Run the project](#run-the-project)
- [Notebook workflow](#notebook-workflow)
- [Example](#example)
- [Validation and evaluation](#validation-and-evaluation)
- [Limitations](#limitations)
- [Possible future improvements](#possible-future-improvements)

## Problem Statement

Conventional irrigation decision-making often relies on fixed threshold-based control strategies, in which irrigation is activated when soil moisture falls below a predefined value. Although simple to implement, such approaches do not adequately represent the nonlinear relationship between soil moisture conditions, ambient temperature, and irrigation demand. They may also produce abrupt control transitions when sensor readings fluctuate near a threshold, limiting their ability to represent gradual changes in environmental conditions.

The problem addressed in this project is the development of a fuzzy inference-based decision-support system that estimates irrigation demand from imprecise environmental input variables. Specifically, soil moisture and ambient temperature are modeled as linguistic variables using appropriate membership functions, allowing crisp sensor readings to be mapped into fuzzy sets such as Dry, Moderate, Wet, Cool, Warm, and Hot.

A Mamdani-type fuzzy inference system (FIS) is designed to evaluate a predefined rule base using fuzzy antecedents, minimum-based rule firing, maximum-based aggregation, and centroid defuzzification. The resulting crisp output represents a normalized irrigation-demand score on a scale of 0–100.

The objective is to investigate whether fuzzy rule-based reasoning can provide a more gradual and interpretable mapping from environmental conditions to irrigation demand than a simple moisture-threshold baseline. The system is implemented and evaluated in a simulation environment using representative input scenarios, boundary tests, and sensitivity analysis. The study focuses on the behavior and interpretability of the fuzzy controller rather than claiming validated water savings or crop-yield improvements.

## Project overview

Conventional threshold-based irrigation policies switch between decisions when soil moisture crosses a fixed value. This project explores a more gradual and interpretable approach: fuzzy logic represents inputs with linguistic categories and combines them through human-readable IF–THEN rules.

**Inputs**

| Variable | Range | Fuzzy categories |
|---|---:|---|
| Soil moisture | 0–100% | Dry, Moderate, Wet |
| Temperature | 0–45 °C | Cool, Warm, Hot |

**Output**

| Variable | Range | Meaning |
|---|---:|---|
| Irrigation demand | 0–100 | Normalized irrigation-demand score |

## How the fuzzy system works

The notebook implements the following Mamdani inference pipeline:

1. **Fuzzification:** Convert crisp moisture and temperature readings into membership degrees for their fuzzy sets.
2. **Rule evaluation:** Evaluate the nine IF–THEN rules. The fuzzy AND operator is minimum: `firing strength = min(moisture degree, temperature degree)`.
3. **Mamdani implication:** Clip each rule's consequent membership function at its firing strength, using minimum.
4. **Aggregation:** Combine the clipped rule outputs using pointwise maximum.
5. **Defuzzification:** Apply the discrete centroid method to produce one crisp score between 0 and 100.

### Membership functions

The membership functions are implemented using triangular and shoulder-shaped functions:

- Soil moisture: Dry (left shoulder, 20–50), Moderate (triangle, 20–50–80), Wet (right shoulder, 50–80).
- Temperature: Cool (left shoulder, 10–22.5), Warm (triangle, 10–22.5–35), Hot (right shoulder, 22.5–35).
- Demand: None (left shoulder, 0–25), Low (triangle, 0–25–50), Medium (triangle, 25–50–75), High (right shoulder, 50–75).

The listed parameters describe the prototype's membership functions; they are not crop- or soil-specific calibration values.

## Fuzzy rule base

The controller contains one rule for each combination of the three moisture and three temperature categories.

| Rule | Soil moisture | Temperature | Demand |
|---|---|---|---|
| R1 | Dry | Cool | Medium |
| R2 | Dry | Warm | High |
| R3 | Dry | Hot | High |
| R4 | Moderate | Cool | Low |
| R5 | Moderate | Warm | Medium |
| R6 | Moderate | Hot | Medium |
| R7 | Wet | Cool | None |
| R8 | Wet | Warm | None |
| R9 | Wet | Hot | Low |

## Run the project

The main project file is [`Smart_Irrigation_Fuzzy_Logic.ipynb`](Smart_Irrigation_Fuzzy_Logic.ipynb).

### Option 1 — Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Select **File → Open notebook → GitHub**.
3. Enter this repository URL: `https://github.com/ANUJA-KUMARI/Fuzzy-Smart-Irrigation-`.
4. Open `Smart_Irrigation_Fuzzy_Logic.ipynb`.
5. Run the notebook cells from top to bottom.

### Option 2 — Run locally

1. Clone the repository:
   ```bash
   git clone https://github.com/ANUJA-KUMARI/Fuzzy-Smart-Irrigation-.git
   cd Fuzzy-Smart-Irrigation-
   ```
2. Install the required packages:
   ```bash
   python -m pip install numpy matplotlib ipywidgets jupyter
   ```
3. Launch Jupyter:
   ```bash
   jupyter notebook
   ```
4. Open `Smart_Irrigation_Fuzzy_Logic.ipynb` and run the cells in order.

The notebook installs NumPy and Matplotlib in its setup cell. The interactive demonstration uses `ipywidgets`; widget rendering support depends on the notebook environment.

## Notebook workflow

- **Setup and membership functions:** Define input/output universes, membership-function helpers, and fuzzy sets.
- **Input validation and fuzzification:** Accept moisture and temperature values within their allowed ranges and calculate membership degrees.
- **Rule base and rule firing:** Define and validate all nine rules, then calculate their firing strengths.
- **Mamdani inference:** Clip consequent sets and aggregate them using maximum.
- **Centroid output:** Calculate the normalized crisp demand score.
- **Baseline comparison:** Compare the fuzzy score with a simple threshold policy (irrigate when soil moisture is below 30%; otherwise do not irrigate).
- **Grid validation and visualization:** Evaluate 2,346 moisture/temperature combinations and inspect the resulting response surface.
- **Interactive demonstration:** Adjust the moisture and temperature sliders to view the score, category, memberships, active rules, and output visualization.

## Example

A saved notebook run with **34% soil moisture** and **33 °C temperature** produces these input membership degrees:

| Variable | Category | Membership |
|---|---|---:|
| Moisture | Dry | 0.533 |
| Moisture | Moderate | 0.467 |
| Moisture | Wet | 0.000 |
| Temperature | Cool | 0.000 |
| Temperature | Warm | 0.160 |
| Temperature | Hot | 0.840 |

The resulting centroid output is **66.53 / 100**. Rules R2, R3, R5, and R6 have non-zero firing strengths for this input.

This is an illustrative output from the prototype, not a calibrated water quantity or a claim about the correct irrigation amount for a particular crop.

## Validation and evaluation

The notebook includes checks for:

- Membership-function boundary values and full-domain coverage.
- Validity and uniqueness of the nine rule combinations.
- Rule firing strengths, Mamdani clipping, and maximum aggregation.
- Centroid calculation, including handling of a zero-membership aggregate.
- Invalid input rejection and endpoint cases.
- Finite output scores within the range [0, 100].
- Soil-moisture monotonicity on the sampled grid: at a fixed temperature, increasing moisture should not increase demand; the notebook reports zero violations in its 2,346-point grid check.

The grid check is a model-consistency test, not field validation. Temperature behavior is evaluated diagnostically rather than required to be monotonic; the notebook reports local decreases in some temperature sweeps.

The baseline comparison is descriptive. It uses a prototype threshold of 30% moisture and does not provide labeled ground-truth demand values. Therefore, the reported score differences must not be interpreted as prediction accuracy, MAE/RMSE against ground truth, proven water savings, or improved crop yield.

## Limitations

- Membership functions and rule consequents are prototype assumptions, not calibrated for a specific crop, soil type, season, or irrigation system.
- The system is a software simulation; it does not read from physical sensors or control a pump, valve, or microcontroller.
- The output is a normalized score, not litres of water, flow rate, or pump runtime.
- The temperature-response analysis shows local decreases for some fixed-moisture sweeps. The current rule base is not presented as a perfectly monotonic temperature controller.
- No field trials, agronomic validation, or measured water-saving evaluation are included.

## Possible future improvements

- Calibrate membership functions and rules with agronomist guidance and real soil/crop data.
- Validate decisions against measured irrigation requirements and field observations.
- Map the normalized score to water volume or pump runtime only after system-specific calibration.
- Integrate soil-moisture and temperature sensors with a microcontroller and safe pump/valve controls.
- Extend the model with humidity, rainfall, soil type, crop stage, and forecast data.
- Investigate alternative membership functions and rule tuning to improve temperature-response behavior.

## Repository

- **Notebook:** [Smart_Irrigation_Fuzzy_Logic.ipynb](Smart_Irrigation_Fuzzy_Logic.ipynb)
- **Repository:** [ANUJA-KUMARI/Fuzzy-Smart-Irrigation-](https://github.com/ANUJA-KUMARI/Fuzzy-Smart-Irrigation-)

---

Developed as an academic AI & Soft Computing project using Python and Mamdani fuzzy logic.
