# LLM + OpenMetadata Integration for Auto-Tagging and Lineage Tracking

"""
This script demonstrates how to use OpenAI's GPT-4 to:
1. Auto-tag table columns with labels like 'PII', 'Sensitive', etc.
2. Infer lineage from SQL statements.
3. Push tags into OpenMetadata via API.
"""

import openai
import requests
import json

# --- CONFIG --- #
OPENAI_API_KEY = "your-openai-key"
OPENMETADATA_SERVER = "http://localhost:8585/api"  # default OM server
OPENMETADATA_TOKEN = "your-openmetadata-token"  # if auth enabled
HEADERS = {"Authorization": f"Bearer {OPENMETADATA_TOKEN}"}

openai.api_key = OPENAI_API_KEY

# --- 1. Auto-tag columns based on sample values --- #

def generate_tags_with_llm(column_samples):
    prompt = f"""
    You are a data governance assistant. Based on the sample values, classify each column:

    {json.dumps(column_samples, indent=2)}

    Return JSON like:
    {{ "column1": "PII", "column2": "Sensitive", "column3": "None" }}
    """
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return json.loads(response['choices'][0]['message']['content'])

# --- 2. Push tags to OpenMetadata --- #

def tag_column(table_fqn, column_name, tag_name):
    url = f"{OPENMETADATA_SERVER}/v1/tags/usage/{tag_name}"
    column_fqn = f"{table_fqn}.{column_name}"
    payload = {
        "entityFQN": column_fqn,
        "labelType": "Manual",
        "state": "Confirmed"
    }
    response = requests.put(url, headers=HEADERS, json=payload)
    if response.status_code == 200:
        print(f"Tagged {column_name} with {tag_name}")
    else:
        print(f"Error tagging {column_name}: {response.text}")

# --- 3. Infer lineage from SQL using LLM --- #

def extract_lineage_from_sql(sql):
    prompt = f"""
    Given the following SQL statement, return a JSON object of source and destination tables:

    SQL:
    {sql}

    Output format:
    {{ "source_tables": [...], "destination_table": "..." }}
    """
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return json.loads(response['choices'][0]['message']['content'])

# --- Example Usage --- #

if __name__ == "__main__":
    # Sample data for tagging
    samples = {
        "email": ["test@example.com", "user@company.com"],
        "dob": ["1990-01-01", "1985-05-10"],
        "age": ["32", "45"]
    }

    # Run tagging
    predicted_tags = generate_tags_with_llm(samples)
    table_fqn = "sample_db.sample_schema.users"
    for col, tag in predicted_tags.items():
        if tag != "None":
            tag_column(table_fqn, col, tag)

    # Lineage detection
    sql_query = """
    INSERT INTO monthly_revenue 
    SELECT customer_id, SUM(amount) 
    FROM transactions 
    GROUP BY customer_id;
    """
    lineage = extract_lineage_from_sql(sql_query)
    print("Inferred Lineage:", lineage)
