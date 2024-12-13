# MediAssist: AI-Driven Patient Health History Chatbot with Advanced Clinical Capabilities

Welcome to **MediAssist**, an AI-powered chatbot designed to enhance clinical workflows by summarizing patient health histories, automating medical coding, stratifying patient risks, and providing retrieval-augmented generation (RAG) capabilities for complex decision-making tasks. 

This repository contains all resources, documentation, and tools required to deploy and understand the MediAssist system.

---

## Features

- **Patient History Summarization**: Generate concise patient summaries from clinical records.
- **Medical Coding Automation**: Assign standardized ICD codes for diagnoses and procedures.
- **Patient Risk Stratification**: Categorize patients into risk levels based on clinical data.
- **Retrieval-Augmented Generation (RAG)**: Retrieve and generate contextual information to aid clinical decisions.

---

## Dataset Overview: MIMIC-IV

MediAssist is built using the **MIMIC-IV dataset**, a publicly available database containing comprehensive de-identified clinical data from real hospital encounters. It includes:

- **Patient Demographics**: Age, gender, ethnicity, and admission types.
- **Diagnoses**: Standardized ICD codes for medical conditions and procedures.
- **Medications**: Details of prescriptions, dosages, and administration schedules.
- **Clinical Notes**: Free-text notes summarizing patient outcomes, treatments, and discharge instructions.
- **Vital Signs and Lab Results**: Captures real-time measurements and lab test outcomes.

**Key Highlights**:
- Hosts over **40 million observations** from critical care units.
- Structured and unstructured data enable diverse use cases, such as summarization, risk analysis, and coding.
- Requires institutional approval for access via [PhysioNet](https://physionet.org/content/mimiciv/3.1/).

---

## Architecture Overview

MediAssist integrates multiple layers for data processing, transformation, and application:

1. **Data Sources**:
   - MIMIC-IV Dataset: Demographics, diagnoses, medications, and discharge summaries.
2. **Data Ingestion & Transformation**:
   - Tools: Alteryx, Snowflake (Snowpark), dbt.
   - Steps: Data cleaning, standardization, schema inference, and transformations.
3. **Data Storage**:
   - Dimensional and fact tables for structured analysis.
4. **Application Layer**:
   - Models: Llama-3.2-11B-Vision-Instruct and fine-tuned GPT-4o.

Refer to the documentation for a detailed architecture diagram and component breakdown.

---

## Repository Contents

- **Source Code**: Complete implementation for all MediAssist functionalities.
- **Documentation**: Comprehensive guides for setup, architecture, and usage.
- **Setup Instructions**: Steps to configure your environment and deploy the system.
- **Testing Scripts**: Automated scripts to verify system functionality.
- **Example Conversations**: Sample dialogues demonstrating MediAssist capabilities.
- **Fine-Tuning Datasets**: Includes preprocessed MIMIC-IV datasets and a sample JSONL file for model fine-tuning.
- **RAG Knowledge Base**: Indexed documents for efficient retrieval in RAG modules.

---

## Setup Instructions

1. Clone the repository:
   ```bash
   git clone https://github.com/Draconian10/MediAssist.git
   cd MediAssist
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Configure database connections:
   - Update `config.yaml` with Snowflake credentials.

4. Fine-tune models (if required):
   - Use the `fine_tuning` folder for dataset preparation and training scripts.

5. Launch the application:
   ```bash
   python deploy_snowpark_apps.py
   ```

---

## Example Usage

### **1. Patient History Summarization**
- **Input**: "Summarize the health history for patient ID 12345."
- **Output**:
  ```
  Patient admitted on 2023-10-01 for hypertension management.
  Medications: Lisinopril (10 mg daily).
  Discharge Summary: Stable vitals, discharged on 2023-11-01 with recommendations for dietary changes and regular follow-ups.
  ```

### **2. Medical Coding Automation**
- **Input**: "Assign ICD codes to the following discharge note: 'Patient diagnosed with essential hypertension and Type 2 diabetes.'"
- **Output**:
  ```
  ICD Codes:
  - I10: Essential Hypertension
  - E11.9: Type 2 Diabetes Mellitus Without Complications
  ```

### **3. Patient Risk Stratification**
- **Input**: "What is the risk level for patient ID 7890?"
- **Output**:
  ```
  Risk Level: High
  Contributing Factors:
  - Multiple admissions for congestive heart failure.
  - Comorbidities include chronic kidney disease (stage 3).
  - Recent lab results indicate elevated creatinine levels.
  ```

### **4. RAG Query for Clinical Data Retrieval**
- **Input**: "Retrieve recent lab results and vital signs for patient ID 56789."
- **Output**:
  ```
  Latest Lab Results:
  - Hemoglobin A1c: 7.2%
  - White Blood Cell Count: 8.1 K/µL
  Recent Vital Signs:
  - Blood Pressure: 140/90 mmHg
  - Heart Rate: 78 bpm
  ```

---

## Challenges and Solutions

### Data-Related Challenges

1. **Missing Data**:
   - **Challenge**: Significant missingness in critical fields like `deathtime` and `discharge_location` posed difficulties for analysis and model training.
   - **Solution**: Null values were imputed using dbt transformations. For example, missing dates were replaced with placeholders like `1900-01-01 00:00:00`, and categorical fields were standardized with `N/A`. Incomplete records were flagged to mitigate potential biases during downstream processing.

2. **High Variability in Clinical Text**:
   - **Challenge**: Discharge summaries and clinical notes exhibited unstructured formats with inconsistent terminology.
   - **Solution**: Advanced NLP techniques were employed for preprocessing, including domain-specific tokenizers tailored for medical abbreviations and terminology, ensuring better standardization and model compatibility.

3. **Imbalanced Data for Risk Stratification**:
   - **Challenge**: High-risk patient cases were underrepresented in the dataset, leading to skewed model predictions.
   - **Solution**: Data augmentation techniques, such as SMOTE (Synthetic Minority Oversampling), were applied to balance the dataset. Additionally, weighted training processes were introduced to ensure the model focused on underrepresented high-risk cases.

---

### Technical Challenges

1. **Text Parsing Errors**:
   - **Challenge**: Multiline text fields in the Discharge table caused parsing failures during ETL processes.
   - **Solution**: Parsing settings were adjusted to handle multiline text efficiently. Validation checks were also added to ensure no critical data was lost during ingestion.

2. **Memory Constraints in Large Tables**:
   - **Challenge**: Large data volumes, especially from the Pharmacy table, caused memory errors during ingestion.
   - **Solution**: Data ingestion was optimized by splitting large tables into smaller chunks. Efficient indexing and data partitioning were implemented to reduce memory usage.

3. **Schema Inference Issues**:
   - **Challenge**: Inconsistent schema inference during Snowpark ingestion led to data type mismatches (e.g., string fields recognized instead of dates).
   - **Solution**: Schema corrections were manually performed, and dbt transformations ensured consistent data types across tables to eliminate errors.
