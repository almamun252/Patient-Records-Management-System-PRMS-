# Patient Records Management System (PRMS) - Advanced

**Date:** 07 Jan 2026  
**Status:** Planning Phase (Updated)

---

## 1. Project Idea & Goals
**Idea:** An integrated system designed to manage records for both patients and doctors in a hospital or clinic setting. Beyond simple data storage, this system tracks the **Treatment History** and relationships between patients and doctors.

**Goals:**
* Eliminate the hassle and risks associated with traditional paper-based record-keeping.
* Generate statistics on disease outbreaks (e.g., total infected vs. total recovered).
* Track which doctor treated which patient and calculate the total number of patients treated by a specific doctor.
* Enable advanced searching for doctors based on their specialty.

---

## 2. Functional Requirements

### A. User Inputs
* **Patient Data:** Name, Age, Gender, Contact No, Diagnosis (Disease Name), Current Status (Active/Recovered).
* **Doctor Data:** Name, Age, Gender, Degree, Specialist, Contact No, Visiting Hours.
* **Actions:**
    * Assign a doctor to a patient.
    * Mark a patient as 'Recovered'.
    * Deactivate a doctor's profile (Soft Delete).
    * Request to delete patient data.

### B. System Outputs
* **Patient Profile:** Personal details displayed alongside a history of treating doctors.
* **Doctor Profile:** Professional details displayed alongside a list of treated patients and the total count.
* **Disease Statistics:**
    * Total number of patients infected with a specific disease.
    * Total number of patients recovered from that disease.
* **Search Results:** Results based on Doctor or Patient queries.
* **Availability:** List of available doctors for a specific disease.

### C. Key Features
* **Smart Linking:** Automatically updates the doctor's profile with the patient's ID when assigned, and vice-versa.
* **Disease Analytics:** Provides real-time statistics when searching by a disease name.
* **Doctor Management:** Supports adding new doctors and deactivating those who have left (maintaining historical data without deleting the record).
* **Advanced Search:** Search by ID, Name, Disease, Specialist, or Doctor's Name.

---

## 3. System Workflow / Logic

1.  **Start:** System initializes -> Loads `patients.json` and `doctors.json`.
2.  **Main Menu:**
    * Patient Management
    * Doctor Management
    * Analytics & Stats
    * Search

### Logic Flow (Assigning Doctor)
1.  User inputs **Patient ID** and **Doctor ID**.
2.  System validates if both entities are **Active**.
3.  If valid:
    * Appends Doctor ID to the Patient's `doctors_list`.
    * Simultaneously appends Patient ID to the Doctor's `patients_list`.
4.  Saves updates to both JSON files.

### Logic Flow (Disease Stats)
1.  User inputs a disease name (e.g., "Dengue").
2.  System iterates through the patient list.
3.  **Count Total:** Increments for every match of the disease name.
4.  **Count Recovered:** Increments if disease matches AND Status is "Recovered".
5.  Displays the calculated statistics.

---

## 4. Technical Stack & Data Structure
* **Language:** Python
* **Data Storage:** Two separate JSON files (`patients.json`, `doctors.json`).

### Relationships
**Patient Object Structure:**
```json
{
  "id": "P-101",
  "name": "Rahim",
  "disease": "Fever",
  "status": "Active",
  "assigned_doctors": ["D-505", "D-508"] 
}
```
**Doctor Object Structure:**
```json
{
  "id": "D-505",
  "name": "Dr. Karim",
  "specialist": "Medicine",
  "is_active": true,
  "treated_patients": ["P-101", "P-204"]
}
```
## 5. Problem Solving Strategy
* Relational Data Update: Implemented a transactional update logic to ensure that when a doctor is assigned, both the patient and doctor files are updated. If one fails, the operation rolls back to prevent data inconsistency.

* Deactivation Logic (Soft Delete): Instead of permanently deleting a doctor's record when they leave, an is_active = False flag is used. This preserves historical treatment data while excluding them from new assignments.

* Case-Insensitive Searching: All string comparisons (e.g., disease names) are converted to lowercase (.lower()) to ensure "Fever" and "fever" are treated as the same entry.
