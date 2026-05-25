# Data Management

The system stores each created workflow in a relational database (PostgreSQL), including unique identifiers for each case and the corresponding labels. Following annotation, labels are uploaded back to the PACS server. Detailed metadata, such as the Series Instance UID and other relevant attributes, are stored in the database.

The platform prepares the dataset for the self-configuring model by transferring data from the PACS server to the segmentation server, organizing the data, and setting up the computational environment and resource allocations.



    
## PACS Overview

<details>
<summary><strong>📦 Definition: What is a DICOM PACS Server?</strong></summary>    
    DICOM stands for Digital Imaging and Communications in Medicine, a global standard for transmitting, storing, and sharing medical images and associated data. PACS, or Picture Archiving and Communication System, is a medical imaging technology that provides economical storage, easy access, and comprehensive management of images from multiple modalities, such as X-ray, MRI, and CT scans.
    
    A DICOM PACS server is a specialized server that stores these images in a standard DICOM format and facilitates their retrieval and viewing by healthcare professionals through networked devices.
    
    **Key Components of a DICOM PACS Server**
    
    1. **Image Acquisition Devices**: These include imaging modalities like MRI, CT, ultrasound, and X-ray machines that generate digital medical images in the DICOM format.
    2. **PACS Server**: The heart of the system, this server stores the images and provides the necessary infrastructure to retrieve, archive, and manage the images.
    3. **Workstations**: These are the computers or terminals used by radiologists and other healthcare providers to view, interpret, and analyze the medical images.
    4. **RIS (Radiology Information System)**: Often integrated with PACS, the RIS handles patient data, scheduling, and management of imaging records, complementing the PACS's image storage and retrieval functions.
    5. **DICOM Viewer**: A software application installed on the workstations that allows medical professionals to view and manipulate the DICOM images.
    
    **How a DICOM PACS Server Works: Step-by-Step Workflow**
    
    1. **Image Acquisition**
    
    The process begins with the acquisition of medical images using various imaging modalities. These devices, such as MRI and CT scanners, generate images in the DICOM format, which includes not only the visual data but also metadata containing patient information and imaging parameters.
    
    1. **DICOM Image Transfer**
    
    Once the images are captured, they are automatically sent to the PACS server over the hospital network using the DICOM protocol. This ensures that the images, along with their associated metadata, are securely transmitted and stored in a standardized format.
    
    1. **Image Storage**
    
    Upon receiving the images, the PACS server stores them in a centralized database. The server is designed to handle large volumes of data, ensuring that images are archived securely and can be retrieved quickly when needed. The server may also create backups to prevent data loss.
    
    1. **Worklist Management**
    
    The PACS server is often integrated with the Radiology Information System (RIS), which manages patient data and scheduling. The RIS generates a worklist for radiologists, listing the studies that need to be reviewed. This integration allows the PACS server to prioritize and manage imaging studies efficiently.
    
    1. **Image Retrieval and Viewing**
    
    When a healthcare provider needs to review an image, they can access the PACS server through a DICOM viewer installed on their workstation. The PACS server retrieves the requested images and displays them on the viewer, allowing the provider to analyze and interpret the images. The viewer often includes tools for enhancing and manipulating the images to aid in diagnosis.
    
    1. **Reporting and Distribution**
    
    After the radiologist or healthcare provider reviews the images, they can generate a report based on their findings. This report is often stored alongside the images on the PACS server and can be shared with other healthcare providers, ensuring that the patient's care team has access to the same information.

</details>

<details>
<summary><strong>🧠 Orthanc (Our PACS Server)</strong></summary>
    
    we are using orthanc as pacs
    
    # Orthanc: A Lightweight Open-Source PACS Server
    
    ## 📌 What is Orthanc?
    
    **Orthanc** is a lightweight, open-source DICOM server that acts as a **PACS (Picture Archiving and Communication System)**. It’s designed to make it easy to manage, store, retrieve, and share medical imaging data in **DICOM format**.
    
    - 🧠 Created by Sébastien Jodogne (University Hospital of Liège)
    - 🔓 Licensed under GPL — free and open-source
    - 🪶 Lightweight — suitable for laptops, hospitals, and cloud environments
    
    ---
    
    ## What Can Orthanc Do?
    
    | Function | Description |
    | --- | --- |
    | 🗃️ Store DICOM Files | Stores imaging data from CT, MRI, X-ray, etc. |
    | 🔍 Search | Query patients, studies, series, or images using DICOM or REST API |
    | 📤 Retrieve | Serve DICOM images to viewers like OHIF |
    | 🔄 Convert | Convert DICOM to PNG/JPEG and vice versa |
    | 🌐 API Access | Exposes REST API and DICOMweb API for automation |
    | 🔌 Extend | Supports plugins (PostgreSQL, S3, Web viewer, etc.) |
    
    ---
    
    ## Orthanc Architecture
    
    Orthanc has a modular architecture with two main storage layers:
    
    | Component | Description |
    | --- | --- |
    | 🧾 **Metadata Storage** | Stores patient info, study details, etc. using **SQLite** or **PostgreSQL** |
    | 🖼️ **Image Storage** | Stores actual DICOM files in the local filesystem or external storage (e.g., S3) |
    
    Everything else — querying, web access, DICOM protocol handling — is managed by internal services or plugins.
    
    ---
    
    ## Key Features
    
    ### ✅ Out-of-the-Box
    
    - Full DICOM server capability
    - Web UI at `http://localhost:8042`
    - RESTful API and DICOMweb support
    - Lua and Python scripting
    - Secure login system
    
    ### Popular Plugins
    
    - **PostgreSQL Plugin** – robust metadata storage
    - **S3 Plugin** – cloud-based image storage
    - **DICOMweb Plugin** – web-based access for viewers like OHIF
    - **WebViewer Plugin** – basic in-browser viewer
    - **Worklist Plugin** – HL7/RIS integration
    
    ---
    
    ## How You Interact With Orthanc
    
    | Client / Tool | Communication Method |
    | --- | --- |
    | Radiology Devices (CT/MRI) | DICOM protocol (C-STORE, C-FIND, etc.) |
    | OHIF Viewer | DICOMweb (QIDO-RS, WADO-RS) |
    | Developers | REST API (HTTP/JSON) |
    | Hospital Systems | HL7 / Worklists (with plugins) |
    
    ---
    
    ## Common Use Cases
    
    | Use Case | Description |
    | --- | --- |
    | 🏥 Local PACS | Central PACS server in clinics or hospitals |
    | 🧪 Research | Backend for AI pipelines, analysis, and datasets |
    | 🧰 Development | Sandbox for developers testing DICOM viewers or tools |
    | ☁️ Cloud Imaging | Remote PACS with S3 or cloud databases |
    | 🔗 Viewer Integration | Frontend for OHIF and other web viewers |
    
    ---
    
    ## Security Features
    
    - Basic authentication
    - HTTPS support (via reverse proxy)
    - Audit logs
    - Role-based access via plugins
    
    ---
    
    # 🖥️ How Orthanc Is Used With OHIF Viewer
    
    ## Quick Recap
    
    - **Orthanc** acts as the **backend PACS server**, storing and managing DICOM data.
    - **OHIF** is a **web-based DICOM viewer** used to display medical images via the browser.
    - They communicate using the **DICOMweb standard**, which includes QIDO-RS, WADO-RS, and STOW-RS APIs.
    
    ---
    
    ## How the Integration Works
    
    1. **DICOM Upload**
        
        A DICOM file (e.g., MRI scan) is uploaded to Orthanc.
        
    2. **Metadata Stored**
        
        Orthanc extracts metadata (patient name, study ID, date) and saves it in the database.
        
    3. **File Storage**
        
        The image itself is stored in Orthanc’s file system or cloud storage (S3, etc.).
        
    4. **Study Retrieval by OHIF**
        - OHIF sends a **QIDO-RS** request to list available studies.
        - Orthanc returns a list of studies and their metadata.
    5. **Image Viewing**
        - When a study is selected, OHIF sends **WADO-RS** requests for individual image slices.
        - Orthanc streams the images back to be rendered in the browser.
    6. **Result**
        
        The user sees high-resolution medical images with scroll, zoom, measure, and annotate tools.
        
    
    ---
    
    ## What You Need for Integration
    
    | Component | Purpose |
    | --- | --- |
    | **Orthanc DICOMweb Plugin** | Enables DICOMweb support (QIDO/WADO/STOW) |
    | **OHIF Viewer Config** | Points OHIF to Orthanc's DICOMweb endpoint (e.g., `http://localhost:8042/dicom-web`) |
    | **DICOM Files** | Must be uploaded to Orthanc to view in OHIF |
    
    ---
    
    - **Orthanc is the DICOMweb-compatible PACS**, and **OHIF is the viewer**.
    - Together, they provide a full, modern, web-based medical imaging system.
    - OHIF interacts with Orthanc using **standard web APIs** — no installation needed on the client.
    - Ideal for hospitals, research labs, cloud imaging services, and developers.

</details>

<details>
<summary><strong>🗄️ PostgreSQL + pgAdmin</strong></summary>    
    we are using postgresql and pgadmin
    
    ---
    
    ## 📌 **Definition of pgAdmin**
    
    **pgAdmin** is the **official graphical user interface (GUI) tool** for managing **PostgreSQL databases**.
    
    - It allows users to interact with PostgreSQL using an intuitive visual interface instead of raw SQL commands.
    - pgAdmin makes it easy to:
        - Create and manage databases and tables
        - Run and debug SQL queries
        - Monitor performance and server status
        - Manage user roles and permissions
        - Export and import data
    
    Think of it as **phpMyAdmin for PostgreSQL**.
    
    ---
    
    ## 🔗 **How pgAdmin Integrates with Orthanc and OHIF**
    
    ### 🧠 Quick Recap of the System:
    
    | Component | Role |
    | --- | --- |
    | **Orthanc** | DICOM server (PACS) that stores medical images and metadata |
    | **OHIF** | Web-based DICOM viewer that fetches data from Orthanc |
    | **pgAdmin** | GUI tool used to view and manage the **PostgreSQL database** behind Orthanc |
    
    ---
    
    ### 🧩 The Integration Chain:
    
    ```
    [Medical Devices] → [Orthanc (PACS)] ←→ [PostgreSQL Database] ←→ [pgAdmin GUI]
                                               ↑
                                           [OHIF Viewer]
    
    ```
    
    ---
    
    ### 🔍 Breakdown of Each Link:
    
    ### ✅ 1. **Orthanc ↔ PostgreSQL**
    
    - Orthanc uses a PostgreSQL database (if configured) to store all DICOM **metadata**, such as:
        - Patient name, study date, modality type
        - Study, series, and instance UIDs
    - The actual image files are stored on disk or cloud storage.
    
    ### ✅ 2. **pgAdmin ↔ PostgreSQL**
    
    - pgAdmin connects to this same PostgreSQL database and lets you:
        - Browse the tables used by Orthanc
        - View metadata for studies and images
        - Run queries to filter or analyze imaging records
        - Inspect how Orthanc structures its internal data (patients, studies, series, etc.)
    
    ### ✅ 3. **OHIF ↔ Orthanc**
    
    - OHIF uses **DICOMweb APIs** (QIDO-RS and WADO-RS) to:
        - Request metadata from Orthanc → Orthanc queries the PostgreSQL DB
        - Fetch images → Orthanc serves them from its file system
    
    ---
    
    ### 📌 Summary of Roles:
    
    | Component | Stores | Accessed by |
    | --- | --- | --- |
    | **Orthanc** | DICOM files + metadata | OHIF, pgAdmin (indirectly) |
    | **PostgreSQL** | Metadata (via Orthanc) | pgAdmin |
    | **pgAdmin** | GUI tool for the DB | Admin, developers |
    | **OHIF** | None (viewer only) | Orthanc (as data source) |

</details>

<details>
<summary><strong>🔗 Connection Summary</strong></summary>

    ## Your Setup Components
    
    | Component | Role |
    | --- | --- |
    | **OHIF Viewer** | Frontend web app for viewing medical images |
    | **Orthanc** | PACS server (backend) that stores DICOM files and metadata |
    | **pgAdmin** | GUI to manage the PostgreSQL database used by Orthanc |
    
    ---
    
    ## How It All Reflects on Orthanc's Internal Structure
    
    ### 🟩 1. **Orthanc’s Internal Services Handle Requests from OHIF**
    
    - When you open **OHIF**, it talks to Orthanc using the **DICOMweb plugin**:
        - **QIDO-RS**: OHIF asks “What studies are available?”
        - **WADO-RS**: OHIF says “Show me this image.”
    - These requests go through Orthanc’s **internal DICOMweb services** (enabled via a plugin).
    - Orthanc:
        - Queries its **PostgreSQL database** for metadata (e.g., study date, patient ID).
        - Reads DICOM image files from disk or object storage.
        - Sends the response back to OHIF.
    
    ✅ **So the querying, web access, and DICOM handling are happening inside Orthanc** (some via plugins).
    
    ---
    
    ### 🟦 2. **Orthanc Uses PostgreSQL to Store Metadata**
    
    - Orthanc stores DICOM metadata (not images!) in **PostgreSQL**:
        - Patients
        - Studies
        - Series
        - Instances
    - This is what OHIF searches and filters against.
    
    ---
    
    ### 🟨 3. **pgAdmin Lets You View and Explore That Metadata**
    
    - pgAdmin connects directly to the **PostgreSQL database** used by Orthanc.
    - Through pgAdmin, you can:
        - See the same study metadata OHIF uses.
        - Write custom SQL queries (e.g., find all CT scans from 2024).
        - Debug or audit what's stored in the PACS.
    
    ✅ **pgAdmin doesn't connect to OHIF — it connects to the backend DB shared by Orthanc.**
    
    ---
    
    ## 🔄 How It All Works Together — End to End:
    
    ```
    [Doctor uploads scan] → [Orthanc]
                             ↓
                    (Stores DICOM files on disk)
                    (Stores metadata in PostgreSQL)
                             ↓
                   [pgAdmin can view/edit metadata]
                             ↓
                   [OHIF requests metadata/images]
                             ↑
                 (Orthanc handles this via DICOMweb)
    
    ```
    
    ---
 </details>
   
<details>
<summary><strong>🌐 DICOMweb API</strong></summary>    
    ---
    
    # 🌐 DICOMweb API: The Web-Based Bridge Between PACS and Viewers
    
    ## 📌 What Is DICOMweb API?
    
    **DICOMweb** is a modern, web-friendly version of the DICOM standard. It defines **RESTful APIs** for accessing medical imaging data (like CT, MRI, and X-rays) using **HTTP and JSON/XML** — just like a typical web service.
    
    > In simple terms:
    > 
    > 
    > 🧠 **DICOMweb = DICOM over HTTP**, designed for web apps and cloud environments.
    > 
    
    ---
    
    ## 🧠 Why DICOMweb Exists
    
    Traditional DICOM relies on older networking protocols (like C-FIND, C-STORE) that are:
    
    - Hard to use with web or mobile apps
    - Complex to integrate in cloud systems
    
    DICOMweb solves this by:
    
    - Using familiar web technologies (URLs, JSON, REST)
    - Enabling easy access for tools like **OHIF Viewer**, **AI pipelines**, and **mobile apps**
    
    ---
    
    ## Where DICOMweb Fits in Your System
    
    Here’s how it connects OHIF and Orthanc:
    
    ```
    [OHIF Viewer (Web)] ← REST API → [Orthanc PACS Server] ←→ [PostgreSQL + DICOM files]
                             ↑
                      Uses DICOMweb API
    
    ```
    
    - **OHIF** uses DICOMweb to:
        - Search for studies (QIDO-RS)
        - Download images (WADO-RS)
        - Upload scans (STOW-RS)
    - **Orthanc** uses built-in services or plugins to support these endpoints.
    - **PostgreSQL** (visible via pgAdmin) stores the metadata for these images.
    
    ---
    
    ## Main DICOMweb API Types & Endpoints
    
    ### 1. 🔍 **QIDO-RS (Query based on ID for DICOM Objects)**
    
    **Purpose**: Search for metadata (not the images themselves)
    
    | Endpoint | Description |
    | --- | --- |
    | `GET /dicom-web/studies` | List all studies |
    | `GET /dicom-web/studies?PatientName=John*` | Filter by patient name |
    | `GET /dicom-web/studies/{StudyUID}/series` | List all series in a study |
    | `GET /dicom-web/studies/{StudyUID}/series/{SeriesUID}/instances` | List image instances in a series |
    
    Used by OHIF to show available patients or studies before loading the images.
    
    ---
    
    ### 2. 📥 **WADO-RS (Web Access to DICOM Objects)**
    
    **Purpose**: Retrieve actual DICOM image data or metadata.
    
    | Endpoint | Description |
    | --- | --- |
    | `GET /dicom-web/studies/{StudyUID}` | Study metadata |
    | `GET /dicom-web/studies/{StudyUID}/series/{SeriesUID}` | Series metadata |
    | `GET /dicom-web/studies/{StudyUID}/series/{SeriesUID}/instances/{InstanceUID}` | Single image instance |
    | `GET /dicom-web/studies/{StudyUID}/series/{SeriesUID}/instances/{InstanceUID}/frames/1` | A specific image frame (used in CT/MRI stacks) |
    
    🖼️ This is how OHIF actually loads and displays the medical images in your browser.
    
    ---
    
    ### 3. 📤 **STOW-RS (Store over the Web)**
    
    **Purpose**: Upload DICOM images using HTTP.
    
    | Endpoint | Description |
    | --- | --- |
    | `POST /dicom-web/studies` | Upload new DICOM files |
    
    🧪 Useful in cloud PACS, AI tools, or browser-based DICOM uploaders.
    
    ---
    
    ## 🧩 Integration Recap
    
    | Component | Role |
    | --- | --- |
    | **OHIF Viewer** | Web client using DICOMweb APIs |
    | **Orthanc** | DICOMweb server responding to QIDO/WADO/STOW |
    | **pgAdmin** | GUI to view Orthanc’s PostgreSQL database (metadata only) |
    
    ---
    
</details>

<details>
<summary><strong>🧑‍💻 Code & Endpoints (TBD)</strong></summary>

Coming soon...

</details>