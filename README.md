# Hotel Booking Cancellation Prediction: Data Preprocessing Pipeline

## Project Overview

This project delivers a robust, production-ready data preprocessing pipeline designed to transform raw hotel booking data into a clean, machine-learning-ready dataset. The primary business objective is to mitigate the significant profitability impact caused by last-minute booking cancellations. By meticulously preparing the data, this pipeline lays the critical foundation for a future predictive model that will forecast cancellation likelihood, enabling proactive revenue management strategies.

**Note:** This project focuses exclusively on the data preparation phase. Model building is intentionally deferred, as the success of any subsequent machine learning model is fundamentally dependent on the quality of the data processed by this pipeline.

---

## Project Phases

The pipeline is structured into three distinct, sequential phases, each addressing a critical aspect of data readiness.

### Phase 1: Exploratory Data Analysis (EDA) & Data Quality Report

This initial phase diagnoses the health of the raw dataset to inform all subsequent cleaning decisions.

*   **Summary Statistics:** Comprehensive profiling of the dataset using `.describe()` and `.info()` to understand data types, distributions, and basic metrics.
*   **Missing Value Analysis:** Systematic identification and quantification of missing data across all columns. Visualized using tools like `missingno` matrices or heatmaps to reveal patterns and potential systemic issues.
*   **Outlier Detection:** Identification of anomalous values in key numerical features (e.g., `adr`, `lead_time`) using statistical methods (IQR) and visual tools (boxplots).
*   **Data Quality Assessment:** A documented report detailing the primary data quality issues discovered, providing the justification for the cleaning strategies implemented in Phase 2.

### Phase 2: Core Data Cleaning

This phase executes targeted strategies to resolve the data quality issues identified in Phase 1.

*   **Missing Value Imputation:**
    *   `company` & `agent`: Missing values replaced with a distinct label (e.g., `"None"` or `0`) to preserve the meaning of "no agent/company involved."
    *   `country`: Imputed using the mode (most frequent country) or categorized as `"Unknown"` to handle geographical unknowns.
    *   `children`: Small number of missing values imputed using the median or mode for statistical robustness.
*   **Duplicate Removal:** Identification and elimination of exact duplicate rows to ensure data integrity.
*   **Outlier Treatment:** Extreme values in sensitive columns (e.g., capping `adr` at 1000) are capped or winsorized to prevent model skewing, with clear methodological justification provided.
*   **Data Type Correction:** Ensuring all date/time columns are parsed and formatted correctly for accurate feature engineering.

### Phase 3: Feature Engineering & Final Preprocessing

This phase enriches the dataset with new, informative features and prepares it for machine learning, while strictly avoiding data leakage.

*   **Feature Engineering:**
    *   `total_guests`: Calculated as `adults + children + babies`.
    *   `total_nights`: Calculated as `stays_in_weekend_nights + stays_in_week_nights`.
    *   `is_family`: A new binary feature indicating if the booking includes children or babies.
*   **Categorical Encoding:**
    *   **One-Hot Encoding:** Applied to low-cardinality categorical variables (e.g., `meal`, `market_segment`) for optimal model compatibility.
    *   **Advanced Encoding for High-Cardinality:** Features like `country` are handled using techniques such as frequency encoding or by grouping infrequent categories into an `"Other"` bucket to manage dimensionality.
*   **CRITICAL: Data Leakage Prevention:** Columns `reservation_status` and `reservation_status_date` are **immediately dropped** as they contain target information or future knowledge that would be unavailable at prediction time, rendering any model trained with them useless in a real-world deployment.
*   **Dataset Splitting:** The final, cleaned dataset is split into training and testing sets (`test_size=0.2`, `random_state=42`) to enable unbiased model evaluation in the next phase.

---

## Business Impact

The output of this pipeline is a high-fidelity dataset that directly addresses the core business problem: **reducing revenue loss from cancellations.** By providing clean, relevant, and leakage-free data, this project empowers the data science team to build a highly accurate predictive model. This model will allow the revenue team to:

*   Identify high-risk bookings in advance.
*   Implement targeted interventions (e.g., personalized offers, deposit requirements).
*   Optimize pricing and inventory strategies.
*   Ultimately, maximize hotel profitability and operational efficiency.

---

## Repository Structure

*   `notebook/`: Jupyter notebook containing the complete, documented code for all three phases.
*   `data/`: Directory for the raw (`hotel_bookings.csv`) and final processed datasets.
*   `README.md`: This file.

---

## Technologies Used

*   Python
*   Pandas, NumPy
*   Matplotlib, Seaborn, Missingno (for visualization)
*   Scikit-learn (for data splitting and potentially encoding)

---

## Author

[Mahmoud Amr Amin ]