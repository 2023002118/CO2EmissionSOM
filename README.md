# README.md

## Global CO2 Emissions Clustering using Self-Organizing Maps (SOM)

This project applies a **Self-Organizing Map (SOM)** to cluster countries based on their annual CO2 emissions profiles from 1950 to 2024. By training an unsupervised neural network, we group countries with similar historical trajectories of CO2 emissions, revealing distinct environmental and industrial patterns.

### —— Project Workflow ——

1. **Data Ingestion & Cleaning**
   - Loads the global CO2 emissions dataset.
   - Filters for completeness: retains countries with at least 70 valid years of records.
   - Structures a country-by-year matrix ($166$ countries $\times 75$ year features from 1950 to 2024) and fills internal gaps using linear interpolation.

2. **Feature Engineering & Preprocessing**
   - Applies a $\log(1+x)$ transformation to reduce the skewness caused by extreme scale differences in carbon output.
   - Standardizes the transformed features per year across countries using a standard z-score scaling.

3. **Self-Organizing Map Implementation & Training**
   - Initializes a $7 \times 7$ grid of neurons ($49$ total).
   - Implements competitive batch learning with an exponentially decaying neighborhood radius ($\sigma$ from $3.0$ to $0.5$).
   - Tracks model convergence using the **Quantization Error (QE)** metric.

4. **Post-SOM Clustering (K-Means on SOM Grid)**
   - Uses the trained SOM weights of occupied neurons as input features for a final K-Means algorithm to partition the map into optimal global clusters.
   - Compares results with a direct **K-Means baseline** using Silhouette scores and Adjusted Rand Index (ARI).

5. **Evaluation & Cross-Validation**
   - Splits the countries into training ($80\%$) and testing ($20\%$) sets.
   - Evaluates generalization capabilities on held-out countries by verifying training and testing quantization errors.

6. **Export & Results Packaging**
   - Exports structured results including cluster profiles, country assignments, and evaluation metrics into a CSV directory.
   - Zips the results directory into a downloadable project archive.

### —— Directory of Generated Artifacts ——

All generated outputs are saved under the `CO2_SOM_Results/` directory:

* **`country_cluster_assignments.csv`**: Map assignments and final cluster designations for each of the 166 countries.
* **`annual_cluster_emission_profiles.csv`**: Average annual trajectories for the resulting country clusters.
* **`fuel_type_cluster_profiles.csv`**: Breaks down average emissions by industrial source (coal, oil, gas, and cement) across clusters.
* **`som_validation_metrics.csv`**: Comprehensive training diagnostic dashboard.
* **`baseline_comparison.csv`**: Direct comparison metrics with standard direct-to-trajectory K-Means.
* **`held_out_evaluation.csv`**: Training vs. testing generalization stats (quantization errors and silhouette scores).
* **`training_history.csv`**: Step-by-step training convergence history.