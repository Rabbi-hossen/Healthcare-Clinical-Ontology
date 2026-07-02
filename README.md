# Healthcare Clinical Ontology

An OWL 2.0 ontology modeling the core entities and relationships of a hospital clinical environment, developed with Protégé.

**Author:** RABBI HOSSEN
**Course / Professor:** Matteo Cristani
**Submission:** Exam Proposal — Healthcare Clinical Ontology

---

## 1. Project Overview

This repository contains a formal ontology representing the domain of **hospital clinical management**. The ontology captures the key actors, records, and processes involved in patient care, with the goal of providing a machine-readable, logically consistent knowledge model that can support reasoning tasks such as:

- Verifying that patients are correctly linked to diagnosed diseases and prescribed treatments
- Distinguishing between healthcare provider roles (doctors, nurses, specialists)
- Supporting consistency checking and classification via a DL reasoner (e.g. HermiT, Pellet)

The ontology is scoped to five central concepts: **Patients**, **Doctors** (and other Healthcare Providers), **Diseases**, **Medical Records**, and **Treatments**.

## 2. Objective

The objective of this project is to demonstrate competence in ontology engineering by:

1. Designing a coherent class hierarchy for a real-world clinical domain
2. Defining Object Properties and Data Properties that meaningfully connect and characterize the classes
3. Encoding the model in valid OWL 2.0 syntax
4. Populating the ontology with representative individuals (instances) to validate the design
5. Ensuring the ontology is reasoner-consistent and ready for further extension

## 3. Methodology

The ontology was developed following a standard knowledge engineering workflow:

| Step | Description |
|---|---|
| **Domain scoping** | Identified the hospital environment as the domain, bounded to Patients, Providers, Diseases, Records, and Treatments |
| **Class elicitation** | Derived a class hierarchy through top-down decomposition (e.g. `Person` → `Patient` / `HealthcareProvider`) |
| **Property definition** | Defined Object Properties to link classes (e.g. `hasDisease`, `treats`) and Data Properties to describe attributes (e.g. `patientID`, `dateOfBirth`) |
| **Formalization** | Encoded the model in **OWL 2.0** using the **OWL/XML** serialization |
| **Population** | Added representative **Named Individuals** and property assertions to demonstrate the ontology in use |
| **Validation** | Verified logical consistency using Protégé's built-in DL reasoner |

**Tools used:**
- [Protégé](https://protege.stanford.edu/) (desktop ontology editor)
- OWL 2.0 / OWL/XML serialization
- HermiT reasoner (bundled with Protégé) for consistency checking and classification

## 4. Class Hierarchy and Architecture

The ontology is organized around a top-level distinction between people involved in care and the clinical entities surrounding that care:

```
Thing
├── Person
│   ├── Patient
│   └── HealthcareProvider
│       ├── Doctor
│       │   ├── GeneralPractitioner
│       │   └── Specialist
│       └── Nurse
├── Disease
│   ├── ChronicDisease
│   └── InfectiousDisease
├── MedicalRecord
├── Treatment
│   ├── MedicalPrescription
│   ├── Surgery
│   └── Therapy
└── Medication
```

**Design notes:**
- `Patient` and `HealthcareProvider` are declared **disjoint**, so the reasoner will flag any individual mistakenly asserted as both.
- `treats` and `isTreatedBy` are declared as **inverse object properties**, allowing bidirectional navigation between doctors and patients.
- `hasMedicalRecord` and `patientID` are declared **functional**, reflecting the real-world constraint that a patient has exactly one active medical record / ID.

### Object Properties

| Property | Domain | Range |
|---|---|---|
| `hasDisease` | Patient | Disease |
| `treats` / `isTreatedBy` | Doctor / Patient | Patient / Doctor |
| `prescribesMedication` | Doctor | Medication |
| `hasMedicalRecord` | Patient | MedicalRecord |
| `undergoesTreatment` | Patient | Treatment |
| `recordsDisease` | MedicalRecord | Disease |
| `includesTreatment` | MedicalRecord | Treatment |

### Data Properties

| Property | Domain | Range |
|---|---|---|
| `patientID` | Patient | `xsd:string` |
| `dateOfBirth` | Person | `xsd:date` |
| `diseaseName` | Disease | `xsd:string` |
| `licenseNumber` | Doctor | `xsd:string` |
| `dosage` | Medication | `xsd:string` |
| `prescriptionDate` | MedicalPrescription | `xsd:date` |

### Sample Individuals

The ontology is populated with sample data to demonstrate correct usage:

- **Patients:** `JohnDoe`, `MariaRossi`, `LucaBianchi`
- **Doctors:** `DrSmith`, `DrVerdi`
- **Diseases:** `Diabetes`, `Hypertension`, `Influenza`
- **Treatment:** `InsulinTherapyRx`

Example assertions include `JohnDoe hasDisease Diabetes`, `DrSmith treats JohnDoe`, and `JohnDoe undergoesTreatment InsulinTherapyRx`.

## 5. How to Open the Ontology in Protégé

1. **Install Protégé** (if not already installed): download the desktop version from [https://protege.stanford.edu/products.php](https://protege.stanford.edu/products.php)
2. **Clone or download this repository**:
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```
3. **Open the file**:
   - Launch Protégé
   - Go to `File → Open...`
   - Select `HealthcareClinicalOntology.owl` from the cloned repository
4. **Explore the ontology**:
   - Use the **Classes** tab to inspect the class hierarchy
   - Use the **Object Properties** / **Data Properties** tabs to inspect relations and attributes
   - Use the **Individuals** tab to inspect the populated instances and their asserted facts
5. **Run the reasoner** (recommended to verify consistency):
   - Go to `Reasoner → HermiT` (or Pellet, if installed)
   - Click `Start reasoner`
   - Check the **inferred class hierarchy** and confirm no unsatisfiable classes are reported

## 6. Repository Contents

```
.
├── HealthcareClinicalOntology.owl   # Main ontology file (OWL 2.0 / OWL-XML)
└── README.md                        # This file
```

## 7. Notes for Reviewers

This ontology is submitted as part of an exam proposal and is intended as a proof-of-concept model. Corrections and feedback are welcome via GitHub issues or as specified by course instructions.
