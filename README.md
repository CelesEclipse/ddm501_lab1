# DDM501 Lab01

## Project Structure

Below is the directory layout of the project:

\`\`\`text
first-ml-product/
├── app/
│   ├── __init__.py
│   └── main.py              # FastAPI app
├── tests/
│   └── test_api.py          # Automated tests with pytest
├── evidence/                # Project execution screenshots (for submission)
│   ├── part2.png
│   ├── part3.png
│   └── part6.png
├── train.py                 # Model training script
├── model.joblib             # Output model
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── pytest.ini              # Optional config
└── README.md
\`\`\`

## Prerequisites

Make sure the following software are installed:
- **Python**: Version 3.10+
- **Docker Desktop**: Latest version (make sure the Docker daemon is running)
- **Git**: Latest version

## Local Installation & Setup

### 1. Clone the Repository
\`\`\`bash
git clone <github-repo>
cd first-ml-product
\`\`\`

### 2. Create and Activate a Virtual Environment
- **Windows (PowerShell):**
  \`\`\`powershell
  python -m venv venv
  .\venv\Scripts\Activate.ps1
  \`\`\`
- **macOS / Linux:**
  \`\`\`bash
  python -m venv venv
  source venv/bin/activate
  \`\`\`

### 3. Install Dependencies
\`\`\`bash
pip install -r requirements.txt
\`\`\`

### 4. Train the ML Model
Run the training script to evaluate the Iris dataset and generate the serialized model artifact (`model.joblib`):
\`\`\`bash
python train.py
\`\`\`
*Expected output in log:* `Test accuracy: 1.000` and `Saved model to model.joblib`

### 5. Run the FastAPI Server Locally
Start the local Uvicorn development web server:
\`\`\`bash
uvicorn app.main:app --reload --port 8000
\`\`\`
Access the local service home at: [http://localhost:8000](http://localhost:8000)

## Containerization with Docker

To isolate the runtime environment and guarantee the production-ready application runs consistently anywhere, use Docker Compose.

### Start the Application
Run the service in the background (detached mode) and force a rebuild:
\`\`\`bash
docker compose up --build -d
\`\`\`

### Verify Service Health Status
Check if the application container passes the custom HTTP healthcheck endpoint (`/health`):
\`\`\`bash
docker compose ps
\`\`\`
*The `STATUS` column should display `(healthy)` after a few seconds.*

### View Container Logs
\`\`\`bash
docker compose logs -f
\`\`\`

### Stop the Application
\`\`\`bash
docker compose down
\`\`\`

## API Endpoints & Usage

When the application is running (locally or via Docker on port `8000`), the following endpoints are available:

- **GET `/`**: Returns basic service status information.
- **GET `/health`**: Returns a JSON payload showing container health condition (`{"status": "healthy"}`).
- **POST `/predict`**: Requires 4 numerical flower features to return the classification results and prediction confidence.

### Interactive API Documentation (Swagger UI)
FastAPI automatically generates live, interactive documentation. Open your browser and navigate to:
👉 **[http://localhost:8000/docs](http://localhost:8000/docs)**

## Running Automated Tests

Automated testing is handled via `pytest`. We utilize a custom `pytest.ini` configuration file to seamlessly inject the root directory (`.`) into the `PYTHONPATH`, resolving module importing pathways across all operating systems (`Module not found 'app'` error when working on Windows)

To run the automated test suite locally within your activated virtual environment:
\`\`\`bash
pytest -v
\`\`\`
*Expected result:* 4 test cases passed successfully (`test_root`, `test_health`, `test_predict_valid`, `test_predict_invalid_type`).