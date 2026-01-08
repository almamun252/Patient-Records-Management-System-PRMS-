Project Name: Patient Records Management System (PRMS) - AdvancedDate: January 07, 2026Status: Planning Phase (Approved)1. Project Idea & MotivationIdea: An integrated system designed to manage both patient and doctor records within a hospital or clinic setting. Beyond simple data storage, this system tracks the treatment relationships (Treatment History) between patients and doctors.Goal:Eliminate the hassle and risks associated with traditional paper-based record-keeping.Generate statistics on disease outbreaks (e.g., total infected vs. total recovered).Track which doctor treated which patient and calculate the total number of patients treated by a specific doctor.Enable advanced searching for doctors based on their specialty.2. Functional RequirementsA. User InputsPatient Data: Name, Age, Gender, Contact No, Disease (Diagnosis), Current Status (Active/Recovered).Doctor Data: Name, Age, Gender, Degree, Specialist, Contact No, Visiting Hours.Actions:Assign a doctor to a patient.Mark a patient as 'Recovered'.Deactivate a doctor's profile (Soft Delete).Request to delete patient data.B. System OutputsPatient Profile: Personal details displayed alongside a history of treating doctors.Doctor Profile: Professional details displayed alongside a list of treated patients and the total count.Disease Statistics:Total number of patients infected with a specific disease.Total number of patients recovered from that disease.Search Results: Results based on Doctor or Patient queries.Availability: List of available doctors for a specific disease.C. Key FeaturesSmart Linking: Automatically updates the doctor's profile with the patient's ID when assigned, and vice-versa.Disease Analytics: Provides real-time statistics when searching by a disease name.Doctor Management: Supports adding new doctors and deactivating those who have left (maintaining historical data without deleting the record).Advanced Search: Search by ID, Name, Disease, Specialist, or Doctor's Name.3. System Workflow / LogicStart: System initializes -> Loads patients.json and doctors.json.Main Menu:Patient ManagementDoctor ManagementAnalytics & StatsSearchLogic Flow (Assigning Doctor):User inputs Patient ID and Doctor ID.System validates if both entities are Active.If valid -> Appends Doctor ID to Patient's doctors_history.Simultaneously -> Appends Patient ID to Doctor's patients_history.Saves updates to both JSON files.Logic Flow (Disease Stats):User inputs a disease name (e.g., "Dengue").System iterates through the patient list.Count Total: Increments for every match of the disease name.Count Recovered: Increments if disease matches AND Status is "Recovered".Displays the calculated statistics.4. Technical Stack & Data StructureLanguage: PythonData Storage: Two separate JSON files (patients.json, doctors.json).Relationships:Patient Object Structure:{
  "id": "P-101",
  "name": "John Doe",
  "disease": "Fever",
  "status": "Active",
  "assigned_doctors": ["D-505", "D-508"] 
}
Doctor Object Structure:{
  "id": "D-505",
  "name": "Dr. Smith",
  "specialist": "Medicine",
  "is_active": true,
  "treated_patients": ["P-101", "P-204"]
}
5. Problem Solving StrategyRelational Data Update: Implemented a transactional update logic to ensure that when a doctor is assigned, both the patient and doctor files are updated. If one fails, the operation rolls back to prevent data inconsistency.Deactivation Logic (Soft Delete): Instead of permanently deleting a doctor's record when they leave, an is_active = False flag is used. This preserves historical treatment data while excluding them from new assignments.Case-Insensitive Searching: All string comparisons (e.g., disease names) are converted to lowercase (.lower()) to ensure "Fever" and "fever" are treated as the same entry.6. PseudocodeCLASS Person:
    Attributes: name, age, gender, contact, id

CLASS Patient(Person):
    Attributes: disease, status="Active", doctors_history=[]

CLASS Doctor(Person):
    Attributes: degree, specialist, is_active=True, patients_history=[]

CLASS HospitalManager:
    DATA_PATIENTS = {}
    DATA_DOCTORS = {}

    FUNCTION assign_doctor(p_id, d_id):
        patient = DATA_PATIENTS[p_id]
        doctor = DATA_DOCTORS[d_id]
        
        IF doctor.is_active IS False:
            PRINT "Doctor is not available"
            RETURN

        patient.doctors_history.APPEND(d_id)
        doctor.patients_history.APPEND(p_id)
        SAVE_ALL()

    FUNCTION get_disease_stats(disease_name):
        total = 0
        recovered = 0
        FOR p in DATA_PATIENTS:
            IF p.disease == disease_name:
                total += 1
                IF p.status == "Recovered":
                    recovered += 1
        PRINT total, recovered

    FUNCTION view_patient_profile(p_id):
        p = DATA_PATIENTS[p_id]
        PRINT p.details
        FOR doc_id in p.doctors_history:
            PRINT DATA_DOCTORS[doc_id].name
7. Future Scope[ ] Appointment Scheduling: Slot booking based on date and time.[ ] Prescription Generator: Generate printable PDF prescriptions directly from the system.[ ] Billing Module: Calculate doctor fees and hospital charges.[ ] Authentication: Separate login systems for Admins, Doctors, and Receptionists.
