# Healthcare Appointment No-Show Prediction

A machine learning project that predicts whether patients will show up for their healthcare appointments, helping hospitals reduce no-shows and optimize scheduling.

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Model Details](#model-details)
- [Performance Metrics](#performance-metrics)
- [Deployment](#deployment)
- [Contributing](#contributing)

## 🎯 Overview

Healthcare facilities face significant challenges with patient no-shows, leading to wasted resources and reduced operational efficiency. This project uses machine learning to predict appointment no-shows based on patient demographics and appointment characteristics, enabling proactive interventions.

## ✨ Features

- **Data Pipeline**: Automated data ingestion from Google Cloud Storage
- **ML Model**: LightGBM classifier with hyperparameter optimization
- **Experiment Tracking**: MLflow integration for reproducible experiments
- **Web Interface**: Flask-based application for real-time predictions
- **Containerization**: Docker support for easy deployment
- **CI/CD**: Jenkins pipeline for automated testing and deployment

## 📁 Project Structure

Healthcare_appointment/
├── src/ # Core ML modules
│ ├── data_ingestion.py # Download and split data from GCS
│ ├── data_preprocessing.py # Data cleaning and feature engineering
│ ├── model_training.py # Model training and hyperparameter tuning
│ ├── logger.py # Logging configuration
│ └── custom_exception.py # Custom exception handling
├── pipeline/
│ └── training_pipeline.py # Orchestrates the entire ML pipeline
├── config/
│ ├── config.yaml # Configuration parameters
│ ├── paths_config.py # File paths configuration
│ └── model_params.py # Model hyperparameters
├── artifacts/
│ ├── raw/ # Raw data from GCS
│ ├── processed/ # Preprocessed datasets
│ └── models/ # Trained model files
├── templates/ # HTML templates for web UI
├── static/ # CSS and static assets
├── notebook/ # Jupyter notebooks for exploration
├── application.py # Flask web application
├── requirements.txt # Python dependencies
├── setup.py # Package setup configuration
├── Dockerfile # Docker image configuration
└── Jenkinsfile # CI/CD pipeline configuration






