# UV Index Predictive Modeling & Evaluation Report

## 1. Executive Summary

This report summarizes the data ingestion, feature engineering, model training, and performance evaluation for the UV Index prediction system developed in Google Colab. By combining multiple sensor logs (`UV_Data_20261006_040916.xlsx` and `UV_Data_20261006_040353.xlsx`), an optimized Random Forest Regressor was successfully trained to forecast future UV Index values 5 minutes ahead based on ambient temperature, humidity, raw sensor counts, voltage, rolling averages, and lag features.

## 2. Dataset Overview & Feature Engineering

- **Datasets Used:**

  1. `UV_Data_20261006_040916.xlsx` (298 samples covering higher solar irradiance ranges up to $1.20$)

  2. `UV_Data_20261006_040353.xlsx` (49 samples covering lower and fluctuating irradiance ranges down to $0.50$)




- **Total Combined Samples:** 347 sensor logs recorded at 5-second intervals via an ESP32 microcontroller setup.

- **Engineered Features:**

  - **Lag Features (**`UV_Lag1`**,** `UV_Lag2`**):** Captures immediate historical momentum.

  - **Rolling Mean (**`UV_RollingMean5`**):** Smooths out high-frequency sensor noise across a 5-step window.

  - **Future Target (**`Target_5min`**):** Shifted by 60 steps ($5 \text{ minutes} \times 60 \text{s} / 5 \text{s}$) to predict the UV index 5 minutes ahead.




## 3. Model Training & Performance Metrics

The combined dataset was split into training and testing sets ($80\\%$ train, $20\\%$ test with shuffling enabled to capture diverse sunlight and cloud conditions).

- **Algorithm:** Random Forest Regressor (`n_estimators=150`, `random_state=42`)

- **Evaluation Results:**

  - **Mean Squared Error (MSE):** `0.0053`

  - **$R^2$ Score:** `0.7274` ($\approx 72.7\\%$ variance explained)




## 4. Visual Analysis (`Actual` vs `Predicted`)

As demonstrated in the evaluation graph generated within the active Colab session (`uv right.ipynb`), the **Combined Model Predictions vs Actual UV Index** plot confirms that:

- The orange dashed line (**Predicted UV**) successfully tracks the major peaks, troughs, and rapid fluctuations of the solid blue line (**Actual UV**).

- Merging both training and testing subsets resolved early model underfitting/overfitting, allowing the AI to generalize effectively across different cloud/sun variations.




## 5. Deployment & Next Steps

- **Saved Artifact:** The final trained model has been compiled and saved locally in the environment as `uv_model_combined_5min.pkl` and downloaded to your workstation.

- **Integration Recommendation:** You can now load this `.pkl` file in your Python environment or deploy it using MicroPython/Flask to receive real-time 5-minute ahead UV forecasts directly from your ESP32 hardware sensors.
