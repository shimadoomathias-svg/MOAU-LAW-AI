# MOAU LAW AI

A mobile-friendly legal document summarizer and summary-fidelity evaluator, built with FastAPI and OpenRouter.

## Features
- Summarize a legal document at Brief, Standard, or Detailed length.
- Evaluate a proposed summary against the original for omissions, distortions, unsupported claims, and potential material inaccuracies.
- Upload PDF, DOCX, TXT, MD, or CSV files, or paste text.
- Jurisdiction field (defaults to Nigeria).
- API key is read from a server-side environment variable, not browser code.

## Important limitations
- This is an early working version, not a validated legal research system. Always verify findings and citations against the original.
- Scanned/image-only PDFs may need OCR before upload.
- Very long documents are capped to a portion of extracted text in this first version; the interface reports processed character counts.
- Do not upload confidential, privileged, personal, or sensitive client documents until you have reviewed the AI provider's current privacy/data-retention terms and have authorization to use the service.
- A free model may be rate-limited, unavailable, or changed by the provider. Free-tier availability is not guaranteed.

## Deploy to Render
1. Create a **new** GitHub repository for MOAU LAW AI. Keep any existing project/repository unchanged.
2. Upload all files and folders in this package to the repository root (including the `static` folder).
3. In Render, create a new **Web Service** and select the new repository.
4. Runtime should be Python. Render can use `render.yaml`, or set:
   - Build command: `pip install -r requirements.txt`
   - Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. In the service's Environment settings, add:
   - `OPENROUTER_API_KEY` = your private OpenRouter API key
   - `OPENROUTER_MODEL` = `nvidia/nemotron-3-super-120b-a12b:free`
   - `APP_PUBLIC_URL` = your actual deployed website URL
6. Deploy. Open `/api/health` on the live website; it should return `"status":"ok"` and `"api_key_configured":true`.
7. Test summarization using a short, public, non-confidential legal document first.

## Local test (optional)
Python 3.10+ recommended.
```bash
pip install -r requirements.txt
# Set OPENROUTER_API_KEY as an environment variable, then:
uvicorn main:app --reload
```
Open `http://127.0.0.1:8000`.

## API endpoints
- `GET /api/health`
- `POST /api/summarize`
- `POST /api/evaluate`
