Problem Statement

Conventional irrigation decision-making often relies on fixed threshold-based control strategies, in which irrigation is activated when soil moisture falls below a predefined value.
Although simple to implement, such approaches do not adequately represent the nonlinear relationship between soil moisture conditions, ambient temperature, and irrigation demand.
They may also produce abrupt control transitions when sensor readings fluctuate near a threshold, limiting their ability to represent gradual changes in environmental conditions.

The problem addressed in this project is the development of a fuzzy inference-based decision-support system that estimates irrigation demand from imprecise environmental input variables.
Specifically, soil moisture and ambient temperature are modeled as linguistic variables using appropriate membership functions, allowing crisp sensor readings to be mapped into fuzzy sets such as Dry, Moderate, Wet, Cool, Warm, and Hot.

A Mamdani-type fuzzy inference system (FIS) is designed to evaluate a predefined rule base using fuzzy antecedents, minimum-based rule firing, maximum-based aggregation, and centroid defuzzification.
The resulting crisp output represents a normalized irrigation-demand score on a scale of 0–100.

The objective is to investigate whether fuzzy rule-based reasoning can provide a more gradual and interpretable mapping from environmental conditions to irrigation demand than a simple moisture-threshold baseline. The system is implemented and evaluated in a simulation environment using representative input scenarios, boundary tests, and sensitivity analysis.
The study focuses on the behavior and interpretability of the fuzzy controller rather than claiming validated water savings or crop-yield improvements.
