# 🚀 CV Auto Processor — by Iván Herrero Galván

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![UI](https://img.shields.io/badge/UI-Tkinter_/_ttk-3776AB?style=flat-square&logo=python)
![LLM API](https://img.shields.io/badge/API-Google_GenAI-4285F4?style=flat-square&logo=google)
![Default Model](https://img.shields.io/badge/Default_Model-Gemini_3.5_Flash-4285F4?style=flat-square&logo=google)
![Word Rendering](https://img.shields.io/badge/Word_Templates-docxtpl-2B579A?style=flat-square&logo=microsoft-word)

## Project Overview

**CV Auto Processor** is a professional, desktop-based automation tool designed to transform raw PDF resumes into perfectly structured, beautifully styled Word documents (`.docx`). By leveraging state-of-the-art Generative AI models via the **Google Gemini API**, the application extracts professional profiles, experience records, formal and informal education, languages, and technical skills. 

Rather than executing opaque, black-box conversions, **CV Auto Processor** introduces an interactive, privacy-first workflow. Users can inspect, clean, and redact sensitive personal information from the raw text in an embedded editor before sending it to the AI. The extracted data is validated against a strict JSON Schema and rendered directly into high-quality corporate Word templates, saving hours of manual formatting.

---

## Architecture & Core Workflow

### Phase 1: PDF Ingestion & Normalization
* **In-Memory Reading:** Utilizes `pypdf` to extract raw textual content from uploaded PDF resumes.
* **OCR Text Normalization:** Employs a custom normalization engine (`ocr_cleanup.py`) that uses regular expressions to eliminate double spaces, clean up non-breaking spaces (`\xa0`), and normalize fragmented letter spaces caused by PDF extraction anomalies.

### Phase 2: Privacy-First Text Sanitization
* **Interactive Editor:** The application loads the cleaned plain text into a centered, fully editable Tkinter text widget.
* **Sensitive Data Redaction:** Users can inspect the raw text and manually remove sensitive or private information (such as personal addresses, ID numbers, or contact details) before sending the data to Gemini.

### Phase 3: AI-Powered Structuring & Validation
* **Google Gemini Integration:** Communicates with the Google Gemini API using the modern `google-genai` SDK. Supports advanced models including **Gemini 3.5 Flash**, **Gemini 2.5 Flash**, **Gemini 3 Flash Preview**, and **Gemini 3.1 Flash Lite**.
* **Strict JSON Schema Enforcement:** Prompts the model to return structured data strictly adhering to a defined JSON Schema (`json_schema_V1.1.json`).
* **Validation Layer:** Employs `jsonschema` to validate the returned JSON structure in real-time, raising errors if fields, data types, or structures do not match the strict schema specification.

### Phase 4: Word Document Rendering
* **Jinja2 Word Templates:** Utilizes `docxtpl` to dynamically render data into a pre-configured Word template (`Modelo_CV_word.docx`) decorated with Jinja2-style tags.
* **Context Normalization:** Cleans values, converting any potential `None` or missing values into blank strings (`""`) and formatting arrays to prevent template crashes or ugly output formatting.
* **Automatic Output:** Saves the beautifully formatted `.docx` file in the same directory as the original PDF.

---

## Version History & Changelog

### Version 1.1 — May 21, 2026
* **Word Document Presentation Enhancements:**
  - Integrated the profile summary information directly inside the **"Knowledge and Competences"** section.
  - Moved **Language** details into "Knowledge and Competences" for better visual flow.
  - Added a dedicated **Availability** section (`disponibility`), defaulting to `"15 días"` unless specified otherwise in the CV.
  - Implemented bullet point markers (`points`) for all items in "Knowledge and Competences".
  - Created new **"Design"** and **"Simulation"** subsections within the "Technical Competence" section.
* **Schema Upgrade:** Updated to JSON Schema `V1.1` to support all the new formatting and subsection variables.

### Version 1.0.1 — May 20, 2026
* **LLM Engine Upgraded:** Added and enabled the new **Gemini 3.5 Flash** model as the default option, ensuring faster and more accurate processing.
* **Cleaned Workspace:** Eliminated residual debugging files (`.md` and `.json` files) generated during the extraction phase, ensuring that only the final, polished `.docx` file is saved to disk.

### Version 1.0 — May 14, 2026
* **Initial Release:** Fully functional program.
  - PDF parser, Tkinter GUI, Gemini integration, Schema validation, and docxtpl Word generation.

### Version 1.1.1 (ToDo) — ![Status](https://img.shields.io/badge/Status-Planned_ToDo-yellow?style=flat-square)
* **API Key Persistence:** Implementing an API Key saving mechanism to securely store the key locally, eliminating the need to re-enter it on every application launch.

---

## Technical Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Programming Language** | Python 3.10+ | Core application logic and scripts. |
| **Graphical Interface** | Tkinter / ttk | Clean and responsive native desktop GUI. |
| **AI SDK & Models** | Google GenAI SDK (`google-genai`) | Integration with Gemini models (3.5 Flash, 2.5 Flash, etc.). |
| **PDF Extraction** | `pypdf` | Fast, local PDF reading and text extraction. |
| **Document Generation** | `docxtpl` (docx-template) | Dynamic Word rendering using Jinja2 templates. |
| **Data Validation** | `jsonschema` | Strict enforcement and verification of JSON outputs. |
| **Config Management** | `python-dotenv` | Loads environment variables (like API Keys) from `.env`. |

---

## Project Structure

```text
CV_Auto_gemini/
├── format_converter/
│   ├── word_model/
│   │   ├── Modelo_CV_Etiquetas.docx  # Word template with Jinja2 tags
│   │   └── Modelo_CV_word.docx       # Default output layout template
│   ├── ia_extractor.py               # Gemini API client & prompt assembly
│   ├── json_to_word.py               # Generates Word documents from schema data
│   ├── pdf_to_md.py                  # Extracts text from PDF documents
│   ├── ocr_cleanup.py                # Cleans and normalizes OCR text
│   └── generar_plantilla_base.py     # Helper script to prepare base layouts
├── interfaz/
│   └── ui.py                         # Tkinter-based user interface
├── json_schema/
│   ├── json_schema.json              # Version 1.0 JSON validation schema
│   └── json_schema_V1.1.json         # Version 1.1 updated schema
├── .env                              # Environment variables (contains API Key, ignored in Git)
├── ABOUT.txt                         # Version metadata
├── CV_Auto_V1.0.1.py                 # Main entrypoint script
├── requirements.txt                  # Python dependency list
├── updates                           # Changelog log in Spanish
└── README.md                         # This file (User Documentation)
```

---

## Installation & Configuration

### Prerequisites
1. **Python 3.10+** installed on your system.
2. A **Google Gemini API Key**. You can obtain one for free at [Google AI Studio](https://aistudio.google.com/).

### Setup Steps
1. **Clone or Download the Repository:**
   ```bash
   git clone https://github.com/your-username/CV_Auto_gemini.git
   cd CV_Auto_gemini
   ```

2. **Create and Activate a Virtual Environment (Recommended):**
   ```bash
   python -m venv .venv
   
   # On Windows:
   .venv\Scripts\activate
   
   # On Linux/Mac:
   source .venv/bin/activate
   ```

3. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure Environment Variables:**
   Create a `.env` file in the root directory and add your Google Gemini API Key:
   ```env
   GEMINI_API_KEY=your_gemini_api_key_here
   ```

---

## Usage Workflow

1. **Launch the Application:**
   Run the main script from your terminal:
   ```bash
   python CV_Auto_V1.0.1.py
   ```

2. **Import your Resume:**
   Click on the **"📂 1. Seleccionar PDF"** button to load a resume in PDF format. The raw text will be extracted and displayed in the central text area.

3. **Sanitize Data (Optional):**
   Edit the extracted text directly in the central text window to delete personal phone numbers, emails, addresses, or any other sensitive content.

4. **Select the AI Model:**
   Select your preferred Gemini model from the dropdown menu (e.g., `gemini-3.5-flash` for high performance).

5. **Generate Word Document:**
   Click on the **"✨ 2. Extraer Word con IA"** button. The application will connect to the Gemini API, validate the response against the schema, and automatically output a fully formatted `.docx` file in the same directory as the input PDF.

---

## Author & Licensing

* **Author:** Iván Herrero Galván
* **Date:** May 2026
* **Version:** 1.1 (Current Stable)
* **License:** Private/Educational use. Contact the author for distribution queries.
