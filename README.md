# **🔍 LLM + OpenMetadata Integration**

Use large language models like GPT-4 to automate your data governance workflows—auto-tag sensitive fields, detect lineage from SQL, and push metadata directly into [OpenMetadata](https://open-metadata.org/).

---

## 🚀 Features

✅ Auto-tag columns (e.g., **PII**, **Sensitive**, etc.)  
✅ Infer lineage from SQL queries using LLMs  
✅ Push tags and lineage into OpenMetadata via API  
✅ Drop-in Python script with clean interfaces  
✅ Easy to integrate into CI/CD for data validation

---

## 📦 Folder Structure

```
llm-openmetadata-integration/
├── scripts/
│   └── llm_openmetadata.py   # Main script
├── README.md                 # This file
```

---

## 🔧 Requirements

- Python 3.8+
- OpenMetadata (running locally or remotely)
- OpenAI API key (GPT-4 or compatible model)
- `requests`, `openai` libraries

Install dependencies:

```bash
pip install openai requests
```

---

## ⚙️ Configuration

Open `scripts/llm_openmetadata.py` and set the following:

```python
OPENAI_API_KEY = "your-openai-key"
OPENMETADATA_SERVER = "http://localhost:8585/api"
OPENMETADATA_TOKEN = "your-openmetadata-token"
```

---

## 🧪 Example Usage

Run the script directly:

```bash
python scripts/llm_openmetadata.py
```

This will:
1. Classify sample column data like `email`, `dob`, `age`
2. Use GPT-4 to tag them as `PII`, `Sensitive`, etc.
3. Detect lineage from a SQL query (e.g., source/destination tables)
4. Push results to OpenMetadata using its REST API

---

## 📈 Sample Output

```json
{
  "email": "PII",
  "dob": "PII",
  "age": "None"
}
```

```bash
Tagged email with PII
Tagged dob with PII
Inferred Lineage: {
  "source_tables": ["transactions"],
  "destination_table": "monthly_revenue"
}
```

---

## 📌 Next Steps

- Add CI/CD validation (e.g., GitHub Actions)
- Trigger auto-tagging on new table ingestion
- Train a domain-specific LLM for higher accuracy
- Merge with dbt or Airflow metadata flows

---

## 🧠 Credits

Built using:
- [OpenAI GPT-4](https://openai.com/)
- [OpenMetadata](https://open-metadata.org/)
- ❤️ by data engineers who hate manual tagging
