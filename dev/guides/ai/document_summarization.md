---
pagination_next: null
pagination_prev: null
---

# Document summary

Document summary is an AI-powered assistant that helps you create a summary for one or a maximum of 100 documents in .pdf format. You can upload PDF files to Cognite Data Fusion (CDF) and use the Document summary API to create a document summary.

You can also run the instruction and Python code in this article as a [Jupyter Notebook](./document_summarization.ipynb) in your CDF project.

## Step 1. Upload PDF

You can upload a PDF file to CDF one of the following ways:

* Go to **_CDF_** > **_Industrial tools_** > **_Canvas_** and drag your PDF file to the canvas or upload existing files by selecting **_+ Add data_**.  
  If you don't have a good file to upload, try this [test file](./well_report.pdf).

* Go to **_CDF_** > **_Industrial tools_** > **_Data explorer_** > **_Files_** and select **_Upload_**.

* Use the Python code.

```python
response1 = client.files.upload(path="./well_report.pdf")
document_id = response1.id
print(document_id)
```

## Step 2. Create summary

Once the document is uploaded, we can start creating the summary.

It may take some time before the document has been fully processed and ready,
so we wrap the API call in a while-loop, so that we can retry until we get our answer.

```python
import json
import time

ask_path = f"/api/v1/projects/{client.config.project}/ai/tools/documents/summarize"

body = {
    "items": [
        {
            "id": document_id
        }
    ]
}

while True:
    try:
        response2 = client.post(ask_path, json=body).json()
        break

    except CogniteAPIError as e:
        if e.code == 422 and len(e.missing) > 0:
            print("Not ready yet, waiting 5 seconds...")
            time.sleep(5)
            continue

        # re-raise any unexpected exceptions
        raise

print(json.dumps(response2, indent=2))
```

See the result of the test file.

```json
{
  "items": [
    {
      "id": 6716860071641521,
      "summary": "This document is a well summary report for the Volve F well in Norway. It provides
information on the site position, well position, wellpath survey depths, and wellpath plan depths. The
report includes details such as latitude, longitude, position uncertainty, water depth, wellhead depth, and
survey dates. It also mentions the survey tools used for each survey. The document is dated April 11, 2018."
    }
  ]
}
```
