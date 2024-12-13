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

### Key Challenges
- Missing data and schema inconsistencies in clinical records.
- Integration complexities with Snowflake and workflow orchestration.
- High memory demands for large datasets and inference pipelines.

### Solutions
- Data imputation and domain-specific preprocessing techniques.
- Modular workflows using Apache Airflow and Dockerized deployments.
- Efficient data partitioning and Kubernetes-based scaling for model inference.
