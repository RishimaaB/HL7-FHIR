# Unstructured Clinical Data to a FHIR Compliant dataset

This project aims to convert unstructured clinical data into a structured format and map it to corresponding FHIR resources.

## 1. Project Structure

The repository contains four main folders, each with specific scripts, input files, and output files. Follow the steps below to execute the scripts in the correct order.

### Folder 1: clinical_notes_to_structured_fields

This folder contains the necessary files to convert unstructured clinical data into a structured format.

- **Input File**: `unstructured_data_input_file.csv`
- **Output File**: `structured_format_output_file.csv`
- **Script**: `unstructured_to_structured_form.ipynb`

#### Execution:

1. **Run the Script**: Start by executing the `unstructured_to_structured_form.ipynb` notebook.
2. **Dependencies**: 
   - pandas

### Folder 2: data_transformation_medication_field

This folder handles the extraction and transformation of medication names.
- **Input Files**:
  - `structured_format_output_file.csv`
  - `rxnorm_medications_without_TTY_filter.csv`
- **Output File**: `medication_order_output_file.csv`
- **Script**: `medication_names_extracted.ipynb`

#### Execution:

1. **Run the Script**: Execute the `medication_names_extracted.ipynb` notebook.
2. **Dependencies**: 
    - pandas
    - aiohttp
    - nest_asyncio
    - tqdm

### Folder 3: data_transformation_dosage_instructions

This folder is focused on extracting and structuring dosage instructions for the medications.

- **Input Files**:
  - `Discharge_medications.csv`
  - `cleaned_medications.xlsx`
  - `orderable_drug_form.xlsx`
  - `route.xlsx`
- **Intermediate Output File**:
  - `structured_discharge_dosage_data.xlsx`
- **Final Output File**:
  - `dosage_dischargemedication_instructions_with_details.xlsx`
- **Script**: `extracted_dosage_instructions.ipynb`

#### Execution:

1. **Run the Script**: Execute the `extracted_dosage_instructions.ipynb` notebook.
2. **Dependencies**:
   - pandas

### Folder 4: mapping

This folder contains two independent subfolders for mapping `MedicationRequest` and `Patient` resources to FHIR formats.

#### Subfolder 4.1: medication_request_resources

- **Input File**: `medications_on_discharge_4696_main.csv`
- **Output Files**: JSON files in the `json_files` subfolder
- **Script**: `mapping_to_medication_resources_discharged_refined_main.ipynb`

#### Execution:

1. **Run the Script**: Execute the `mapping_to_medication_resources_discharged_refined_main.ipynb` notebook.
2. **Dependencies**:
   - pandas
   - fhir.resources (which includes Bundle, BundleEntry, BundleEntryRequest, MedicationRequest, Reference, CodeableReference, CodeableConcept, Coding, Dosage)
   - pydantic

#### Subfolder 4.2: patient_resources

- **Input File**: `Patient_resources.csv`
- **Output File**: `fhir_patient_bundle_transaction_main_file.json`
- **Script**: `mapping_to_patient_fhir_resources_main_file.ipynb`

#### Execution:

1. **Run the Script**: Execute the `mapping_to_patient_fhir_resources_main_file.ipynb` notebook.
2. **Dependencies**:
   - pandas
   - fhir.resources (which includes Bundle, Patient, Narrative)


## 2. Deploying the Local HAPI FHIR Test Server Using Docker

To test the generated FHIR resources, you can deploy a local HAPI FHIR Test Server using Docker.

### Prerequisites

1. **Docker**: Ensure Docker is installed on your system. 
2. **Application Configuration**: Place your `application.yaml` file in a local folder. This file is essential for configuring the HAPI FHIR server.

### Steps to Deploy

1. **Pull the Latest HAPI FHIR Docker Image**:

   Open your terminal and run the following command to pull the latest HAPI FHIR image:

 ```bash
sudo docker pull hapiproject/hapi:latest
```

2. **Run the Docker Container**:

After pulling the image, run the following command to start the HAPI FHIR server, making sure to replace yourLocalFolder with the path to the folder containing your application.yaml file:

   ```bash
sudo docker run -p 8080:8080 -v $(pwd)/yourLocalFolder:/configs -e "SPRING_CONFIG_LOCATION=file:///configs/application.yaml" -e "hapi.fhir.default_encoding=xml" hapiproject/hapi:latest
```

Once the container is running, you can access the HAPI FHIR Test Server by navigating to http://localhost:8080/ in your web browser.

## 3. Usage
Follow the step-by-step instructions in the "Project Structure" section above to process the data and generate the corresponding FHIR resources.
