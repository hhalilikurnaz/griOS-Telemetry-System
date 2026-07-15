<div align="center">

# 🍋 Garden Digital Twin (Bahçe İkizi)

**A Comprehensive Digital Operation Platform for Precision Lemon Farming**

[![Phase 1](https://img.shields.io/badge/Phase_1-Platform_%26_AI-blue)](#)
[![Phase 2](https://img.shields.io/badge/Phase_2-Drone_Mapping-purple)](#)
[![Phase 3](https://img.shields.io/badge/Phase_3-IoT_%26_Smart_Irrigation-green)](#)
[![Stack](https://img.shields.io/badge/Stack-Monorepo_|_React_|_Python-black)](#)

</div>

---

## 🚀 Project Vision & Target Area

The Garden Digital Twin is a comprehensive digital operation platform designed specifically for lemon orchards in the Mersin Erdemli Tömük region. The system is developed, tested, and integrated sequentially across three distinct phases, culminating in a unified map that blends above-ground data (satellite and drone) with subterranean IoT sensor telemetry. 

*   **Target Pilot Area:** Mersin, Erdemli, Tömük Village.
*   **Parcel Details:** 2.66 hectares, containing 847 Yediveren lemon trees with a 12-year maturity, planted in a 6x6m layout.
*   **Environmental Factors:** Sandy-loamy soil (pH 7.2-8.0), facing climate risks like summer droughts and winter frosts.

---

## 🏗️ System Architecture (Phase Integration)

```mermaid
graph TD
    subgraph Phase 1: Platform & AI
        S[Sentinel-1 & 2 Satellites] -->|8-Layer Health Score| T[Tarlam Web/Mobile]
        AI[AI Lemon Doctor] -->|RAG Chatbot| T
        M[Market/HKS Scraping] -->|Price Analytics| T
    end

    subgraph Phase 2: Drone & Yield Intelligence
        D[Multispectral & RGB Drones] -->|Orthomosaic Mapping| CV[Computer Vision / YOLOv8]
        CV -->|Fruit Count & Disease Detection| T
    end

    subgraph Phase 3: IoT & Automation
        IoT[Soil & Weather Sensors] -->|LoRaWAN| G[Gateway]
        G -->|Real-time Data| T
        T -->|Autonomous Triggers| V[Smart Solenoid Valves]
        GES[Solar Panels] -->|Power Supply| V
    end
🗺️ Phased Development Roadmap
Phase 1: Platform (Web & Mobile Application)
Tarlam (Digital Operations Center): Features parcel boundary mapping, tree inventory management, and unique garden profiles.  
PDF

8-Layer Digital Health Score: Utilizes Sentinel-2 and Sentinel-1 satellite data to calculate NDVI, NDWI, EVI, LAI, LST, SAR, Phenology, and Time Series development.  
PDF

Finance & Production: Includes automated profit/loss calculations, barcode scanning for pesticide logging, and AB-standard export reporting.  
PDF

Market & News Intelligence: Integrates with the Hal Registration System (HKS) and scrapes retail chains (Migros, Carrefour, etc.) to visualize profit margins.  
PDF

AI Lemon Doctor (Sola): A 24/7 chatbot utilizing a RAG pipeline to diagnose diseases like Thrips and Citrus Scab via photo uploads.  
PDF
+ 1

Mobile Offline Field Book: Built with an offline-first architecture using SQLite, allowing photo and audio note capture without internet, syncing automatically when reconnected.  
PDF
+ 1

Phase 2: Advanced Drone Mapping & Yield Intelligence
Yield Prediction: Employs YOLOv8 or Faster R-CNN on drone imagery (50-80m altitude) for tree-level fruit counting and income projection based on quality.  
PDF

Disease Detection: Uses a custom CNN with a ResNet-50 backbone and transfer learning to pinpoint specific diseases per tree and map regional outbreak risks.  
PDF

Smart Spraying Plans: Calculates precise pesticide dosages only for infected areas, saving up to 85% in chemicals and 60% in water.  
PDF

Carbon Emission (MRV): Calculates CO2 capture utilizing tree volume data (DSM) to generate international standard MRV reports (Verra VCS, Gold Standard).  
PDF

Phase 3: IoT, Sensors & Smart Irrigation
Sensor Network: Installs multi-depth soil sensors (15cm, 30cm, 60cm) to measure moisture, pH, and electrical conductivity (EC), transmitting via LoRaWAN protocol.  
PDF

Autonomous Irrigation: Integrates smart solenoid valves into existing drip lines to water specific zones only when moisture drops below AI-defined thresholds.  
PDF

Energy Independence: Powers the entire sensor and pump control ecosystem using solar energy panels (GES) with battery backups[cite: 1].

💻 Technical Stack & Ecosystem
The project is structured as a monorepo containing interconnected environments for web, mobile, API, and satellite processing.  
PDF

Backend & Data Processing
Frameworks: Node.js and Python (FastAPI)[cite: 1].

Database: PostgreSQL combined with PostGIS for geographical data, caching with Redis, and Supabase for authentication and storage[cite: 1].

Earth Observation: Utilizes Google Earth Engine and Sentinel Hub APIs processed via Python libraries like GDAL, Rasterio, and NumPy.  
PDF
+ 1

Frontend & Mobile
Web: React combined with Mapbox/Leaflet for dynamic layer toggling[cite: 1].

Mobile: React Native and Expo, featuring background synchronization and conflict resolution for offline use.  
PDF
+ 1

AI & Machine Learning
Chatbot: Powered by OpenAI GPT-4o using a Retrieval-Augmented Generation (RAG) pipeline, pgvector for similarity search, and prompt templates.  
PDF
+ 1

Computer Vision: PyTorch, Custom CNNs, and YOLOv8 for object detection and disease identification[cite: 1].

📂 Vibe Coding Contexts (Skill Architecture)
Development is guided by specialized .md context files ensuring high-velocity iteration:  
PDF

01-project-structure.md: Defines the monorepo setup and route guards.  
PDF

02-auth-onboarding.md: Manages JWT auth flows and the 4-step onboarding wizard.  
PDF

03-satellite-analysis.md: Contains business rules and color ramps for the 8-layer satellite indices.  
PDF

06-ai-chatbot.md: Outlines the RAG pipeline, intent classification, and vision API integration.  
PDF

09-mobile-offline.md: Details the SQLite schema and exponential backoff sync queues for offline resilience.  
PDF
