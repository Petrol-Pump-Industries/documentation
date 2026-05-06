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

API Endpoints
-------------

The engine exposes several RESTful endpoints for the frontend to consume.

POST /calculate
^^^^^^^^^^^^^^^
Calculates the estimated fuel cost and journey time for a specific trip.

**Request Body:**

.. code-block:: json

   {
     "vrm": "AB12CDE",
     "distance_miles": 50.5,
     "route_type": "mixed",
     "user_lat": 51.5074,
     "user_lon": -0.1278,
     "passengers": 1,
     "cargo_kg": 20.0,
     "roof_rack": false,
     "ac_on": true
   }

**Response:**
Returns a JSON object containing the ``estimated_cost_range``, ``fuel_volume_l``, and ``dvla_analysis``.

POST /stations/nearby
^^^^^^^^^^^^^^^^^^^^^
Finds the nearest petrol stations and provides exact pricing for the vehicle's required fuel type.

**Request Body:**

.. code-block:: json

   {
     "vrm": "AB12CDE",
     "user_lat": 51.5074,
     "user_lon": -0.1278,
     "radius_miles": 5.0
   }

Core Services
-------------

The backend logic is modularized into several key services:

Calculation Service
^^^^^^^^^^^^^^^^^^^
The "Brain" of the engine. It uses a custom physics model to estimate fuel consumption, taking into account:
- **Vehicle Mass**: Base weight plus passengers and cargo.
- **Aerodynamics**: Impact of roof racks and open windows at high speeds.
- **Environmental Factors**: Seasonal adjustments (e.g., winter efficiency drop) and real-time weather.
- **Vehicle Age**: Applying efficiency degradation for older vehicles.

Fuel Service
^^^^^^^^^^^^
Manages fuel price data. It periodically fetches the latest UK market averages and provides location-based pricing if GPS data is available.

DVLA Service
^^^^^^^^^^^^
Interfaces with the DVLA API to retrieve technical specifications for a vehicle based on its registration number (VRM), such as engine capacity, fuel type, and manufacturing year.

Database Schema
---------------

PricePump uses **SQLAlchemy** with a **SQLite** backend for persistent storage.

- **Profile**: Manages user accounts mapped to unique device IDs.
- **JourneyHistory**: Records every calculation request, allowing users to track their spending and mileage over time.
- **Station Cache**: (Internal) Stores recently fetched fuel station data to reduce external API calls.