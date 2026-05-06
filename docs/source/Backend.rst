Backend
=======

Overview
--------

The PricePump Backend (often referred to as the **PricePump Engine**) is a high-performance Python service built with **FastAPI**. It acts as the central intelligence of the application, handling complex fuel consumption calculations, real-time market pricing integration, and vehicle data retrieval via the DVLA API.

Key Responsibilities:
- **Financial Calculations**: Estimating trip costs based on engine size, vehicle weight, and environmental factors.
- **External API Integration**: Fetching vehicle specs from the DVLA and real-time weather data.
- **Data Persistence**: Logging journey history and managing user profiles via SQLAlchemy and SQLite.
- **Real-time Updates**: Automatically syncing the latest UK fuel price data.

Getting Started
---------------

The backend service is designed to be lightweight and scalable. To start the engine locally:

1. **Install Dependencies**:
   Ensure you have Python 3.8+ installed, then run:

   .. code-block:: bash

      pip install -r requirements.txt

2. **Run the API**:
   Launch the service using Uvicorn:

   .. code-block:: bash

      python main.py

The API will be available at ``http://0.0.0.0:8000`` with interactive documentation at ``/docs``.