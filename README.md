[README.md](https://github.com/user-attachments/files/33380472/README.md)
# E-Commerce Customer Segmentation

An end-to-end machine learning project that groups e-commerce customers according to purchasing behavior, spending patterns, loyalty, and digital engagement. The goal is to help businesses move beyond one-size-fits-all marketing and develop more relevant customer campaigns.

## Project Overview

The project uses **unsupervised machine learning** to identify customer segments and translate them into actionable business personas. It includes data ingestion, exploratory data analysis (EDA), preprocessing, clustering analysis, a Streamlit interface, and Docker configuration.

### Business Objectives

- Identify natural groups of customers based on purchase and engagement behavior.
- Prepare customer data by handling missing values and scaling numerical features.
- Explore K-Means clustering using the Elbow Method and Silhouette Score.
- Compare K-Means with Agglomerative Hierarchical Clustering and DBSCAN.
- Profile clusters and connect them to potential marketing and retention actions.
- Provide a Streamlit app where users can upload customer CSV data, view segment summaries, and download the results.

## Customer Segments

The application maps cluster labels to business personas:

| Segment / Persona | Potential Business Action |
|---|---|
| Premium Loyal Customers | Premium membership, cross-selling, and personalized offers |
| Discount-Driven Customers | Targeted coupons, bundles, and promotion-led campaigns |
| Window Shoppers | Conversion campaigns and personalized recommendations |
| Occasional Buyers | Reactivation and seasonal campaigns |
| At-Risk High-Value Customers | Retention and win-back campaigns |
| Low-Engagement Customers | Low-cost lifecycle engagement |
| Moderate-Value / Growth Customers | Nurture engagement and encourage repeat purchases |

These personas are business interpretations. Their accuracy depends on the trained model, cluster profiles, and the data supplied.

## Dataset

The main dataset, `ecommerce_customer_segmentation_dataset.csv`, contains **10,000 customer records and 25 columns**, including demographic, transactional, product-category, loyalty, and digital-engagement information.

Examples of clustering features include:

- `Annual_Income`
- `Customer_Tenure_Months`
- `Total_Orders`
- `Total_Spending`
- `Average_Order_Value`
- `Website_Visits_Per_Month`
- `Mobile_App_Usage_Rate`
- `Discount_Usage_Rate`
- `Cart_Abandonment_Rate`
- `Return_Rate`
- `Average_Days_Between_Orders`
- `Product_Categories_Purchased`
- `Customer_Support_Calls`
- `Loyalty_Points`
- `Last_Purchase_Days_Ago`
- `Marketing_Response_Rate`

`Customer_ID` is an identifier and should not be used as a numerical clustering feature. Categorical and profiling columns should be considered carefully rather than automatically included in distance-based clustering.

### Data files

- `ecommerce_customer_segmentation_dataset.csv` — main customer dataset.
- `ecommerce_customer_data_dictionary.csv` — column descriptions and recommended use.
- `test_customer_data.csv` — sample customer records for testing the app.

## Tech Stack

- **Language:** Python
- **Data processing:** Pandas, NumPy
- **Machine learning:** scikit-learn
- **Model persistence:** Joblib
- **Visualizations:** Matplotlib, Seaborn
- **Web application:** Streamlit
- **Containerization:** Docker

## Project Structure

The pipeline and app use the following artifact paths. Keep the corresponding project modules and generated artifacts in place when running the project.

```text
.
├── artifacts/
│   ├── data/
│   │   ├── raw/
│   │   │   └── ecommerce_customer_segmentation_dataset.csv
│   │   └── processed/
│   ├── models/
│   └── visuals/
├── logs/
├── src/
│   └── customer_segmentation/
│       ├── logger.py
│       ├── data_ingestion.py
│       ├── data_eda.py
│       └── model_trainer.py
├── model_pipeline.py
├── streamlit_app.py
├── requirements.txt
├── Dockerfile
├── ecommerce_customer_data_dictionary.csv
└── test_customer_data.csv
```

The `src/customer_segmentation/` modules are imported by `model_pipeline.py`; ensure they are included in your repository. The pipeline expects the main dataset at `artifacts/data/raw/ecommerce_customer_segmentation_dataset.csv`.

## Setup and Installation

### 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER>
```

Replace the placeholders with your repository URL and folder name.

### 2. Create and activate a virtual environment (recommended)

**Windows PowerShell:**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, you can use Command Prompt instead:

```bat
.venv\Scripts\activate.bat
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Run the Machine Learning Pipeline

1. Place the main CSV at:

   `artifacts/data/raw/ecommerce_customer_segmentation_dataset.csv`

2. Ensure the `src/customer_segmentation/` modules are present.
3. Run:

```bash
python model_pipeline.py
```

The pipeline performs data ingestion and EDA, prepares clustering features, runs Elbow and Silhouette analyses, explores hierarchical clustering and DBSCAN, trains the final K-Means model, saves preprocessing objects, and creates segmented customer data and cluster profiles.

The current pipeline script sets `optimal_k = 7`. Review the Elbow and Silhouette results before changing the selected number of clusters.

The app expects these trained files under `artifacts/models/`:

```text
kmeans_model.joblib
scaler.joblib
imputer.joblib
```

Run the pipeline successfully before launching the app if these model artifacts have not already been generated.

## Run the Streamlit App

```bash
streamlit run streamlit_app.py
```

Then open the local URL printed by Streamlit (usually `http://localhost:8501`).

### Using the app

1. Upload a CSV containing all required clustering feature columns.
2. Review the dataset preview and data summary.
3. Click **Run Customer Segmentation**.
4. Review the customer distribution, cluster profiles, and assigned customer segments.
5. Download the results as `segmented_customer_data.csv`.

The app applies the saved imputer and scaler before assigning clusters with the saved K-Means model. The uploaded CSV must contain the required feature columns; extra columns may be retained in the output.

## Run with Docker

Make sure Docker Desktop or Docker Engine is running and the trained model files are available at the expected paths.

### Build the image

```bash
docker build -t ecommerce-customer-segmentation .
```

### Start the container

```bash
docker run --rm -p 8501:8501 ecommerce-customer-segmentation
```

Open `http://localhost:8501` in your browser.

If your app requires model artifacts at runtime, make sure `artifacts/models/` is included in the Docker build context and is not excluded by `.dockerignore`.

## Outputs

Depending on the pipeline modules and successful execution, outputs include:

- EDA summaries and visualizations under `artifacts/visuals/`
- Saved model and preprocessing artifacts under `artifacts/models/`
- Processed customer data under `artifacts/data/processed/`
- Segmented customer data at `artifacts/data/processed/segmented_customer_data.csv`
- Logs from the configured logger

## Evaluation and Interpretation

- **Elbow Method:** helps inspect how within-cluster variation changes as the number of clusters increases.
- **Silhouette Score:** helps assess how well-separated clusters are.
- **Hierarchical clustering:** provides a dendrogram-based view of relationships between customers.
- **DBSCAN:** identifies dense groups and can mark some records as noise/outliers.

No single metric fully determines the best segmentation. Review numerical results alongside cluster profiles and business usefulness.

## Important Notes

- Clustering is unsupervised: cluster IDs are labels, not pre-existing ground-truth customer categories.
- Persona names assigned to cluster IDs should be validated against the actual cluster profiles. Cluster numbering can change when a model is retrained.
- Use the same feature order and preprocessing objects at training and inference time.
- Do not commit sensitive customer data, credentials, or unnecessary generated artifacts to a public repository.
- Confirm that the model files and the `src/customer_segmentation/` package are included before deploying the Streamlit app.

## Future Improvements

- Add automated tests for data validation and prediction.
- Make the selected cluster count configurable and document the model-selection decision.
- Add richer cluster visualizations and exportable profiling reports.
- Add deployment instructions for a cloud hosting platform.
- Track model and preprocessing versions for reproducibility.

## Author

**Soham pokharkar**


