# Bajaj-Finserv-Health

This project provides a **Bill Data Extraction API** that extracts line item details, subtotals, and final totals from multi-page bills/invoices.

---

## Features

- Extract individual line items with `item_name`, `item_amount`, `item_rate`, and `item_quantity`.
- Calculate totals without double-counting.
- Supports multiple page types: Bill Detail, Final Bill, Pharmacy, etc.
- Easy to deploy on **Render** or run locally.

---

## Installation (Local)

1. **Clone the repository**

```bash
git clone https://github.com/anandanmanisha27/Bajaj-finserv-health.git
cd Bajaj-finserv-health
```

2. **Create a virtual environment (optional)**

```bash
python -m venv venv
# Activate
# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate
```

3. **Install dependencies**

```bash
pip install -r requirements.txt
```

4. **Run the app locally**

```bash
python main.py
```

- API endpoint: `POST /extract-bill-data`
- Example request body:

```json
{
    "document": "https://hackrx.blob.core.windows.net/assets/datathon-IIT/sample_2.png"
}
```

---

## Deployment on Render

### 1. Update `main.py` to use Render's `$PORT`

**Flask example:**

```python
import os
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/extract-bill-data", methods=["POST"])
def extract_bill_data():
    # your extraction logic here
    return jsonify({"is_success": True, "data": {}})

if __name__ == "__main__":
    port = int(os.environ.get("PORT", 8000))
    app.run(host="0.0.0.0", port=port)
```

**FastAPI example:**

```python
import os
import uvicorn
from fastapi import FastAPI

app = FastAPI()

@app.post("/extract-bill-data")
async def extract_bill_data():
    return {"is_success": True, "data": {}}

if __name__ == "__main__":
    port = int(os.environ.get("PORT", 8000))
    uvicorn.run(app, host="0.0.0.0", port=port)
```

### 2. Add `render.yaml` for auto-deploy

```yaml
services:
  - type: web
    name: bajaj-finserv-health
    env: python
    branch: main
    buildCommand: pip install -r requirements.txt
    startCommand: python main.py
    autoDeploy: true
```

### 3. Push to GitHub

```bash
git add .
git commit -m "Add Render deployment config"
git push origin main
```

### 4. Connect Render

- Go to [Render Dashboard](https://render.com/dashboard).
- Click **New Web Service → Connect GitHub**.
- Select your repo `Bajaj-finserv-health`.
- Render will auto-deploy using `render.yaml`.

Your API will be live at:
```
https://bajaj-finserv-health.onrender.com/extract-bill-data
```

---

## Request/Response Example

**Request:**

```json
{
    "document": "https://hackrx.blob.core.windows.net/assets/datathon-IIT/sample_2.png"
}
```

**Response:**

```json
{
  "is_success": true,
  "token_usage": {
    "total_tokens": 123,
    "input_tokens": 100,
    "output_tokens": 23
  },
  "data": {
    "pagewise_line_items": [
      {
        "page_no": "1",
        "page_type": "Bill Detail",
        "bill_items": [
          {
            "item_name": "Product A",
            "item_amount": 250.0,
            "item_rate": 50.0,
            "item_quantity": 5
          }
        ]
      }
    ],
    "total_item_count": 1
  }
}
```

---

## Resources

- Sample Dataset: [TRAINING_SAMPLES.zip](https://hackrx.blob.core.windows.net/files/TRAINING_SAMPLES.zip?sv=2025-07-05&spr=https&st=2025-11-28T06%3A47%3A35Z&se=2025-11-29T06%3A47%3A35Z&sr=b&sp=r&sig=yB8R2zjoRL2%2FWRuv7E1lvmWSHAkm%2FoIGsepj2Io9pak%3D)
- Postman Collection: [HackRx Bill Extraction API](https://hackrx.blob.core.windows.net/assets/datathon-IIT/HackRx%20Bill%20Extraction%20API.postman_collection.json?sv=2025-07-05&spr=https&st=2025-11-28T07%3A21%3A28Z&se=2026-11-29T07%3A21%3A00Z&sr=b&sp=r&sig=GTu74m7MsMT1fXcSZ8v92ijcymmu55sRklMfkTPuobc%3D)
```

---

This README is **ready to be downloaded and added** to your repo.  
It contains instructions for **local run, Render deployment, and API usage**.  

