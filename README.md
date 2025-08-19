# Potato Disease Classification

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcToCfn5ViIyEuEjjHRZwtaB4eeJoACAX3aA0Q&s)

This project aims to detect and classify diseases in potato plants using deep learning techniques. It includes the full pipeline from model training to deployment across web, mobile, and cloud platforms. The app utilizes a convolutional neural network (CNN) trained on a custom image dataset of potato leaves, and offers a complete end-to-end solution including model API, ReactJS frontend, React Native mobile app, and deployment on Google Cloud Platform.

---

## 📁 Project Structure
```bash
.
├── api                  # FastAPI backend for model serving
├── frontend             # ReactJS frontend web app
├── mobile-app           # React Native mobile app
├── training             # Jupyter notebooks and training scripts
├── models               # Trained TensorFlow/TF Lite models
├── gcp                  # Google Cloud deployment scripts
└── README.md
```

---

## 🛠️ Setup Instructions

### ✅ Python Environment Setup
```bash
# Install Python dependencies for training
pip3 install -r training/requirements.txt

# Install Python dependencies for API
pip3 install -r api/requirements.txt
```

### ✅ TensorFlow Serving Setup
- [Install TensorFlow Serving](https://www.tensorflow.org/tfx/serving/setup)

---

## ⚙️ Frontend Setup (ReactJS)
```bash
# Install NodeJS & NPM first (from official website)
cd frontend
npm install --from-lock-json
npm audit fix

# Copy environment variables
cp .env.example .env
# Update API URL in `.env`
```

---

## 📱 React Native App Setup
- Follow the **React Native CLI Quickstart** from [React Native Docs](https://reactnative.dev/docs/environment-setup)

```bash
cd mobile-app
yarn install

# For macOS
cd ios && pod install && cd ../

# Copy environment variables
cp .env.example .env
# Update API URL in `.env`
```

---

## 📈 Model Training
```bash
# Open Jupyter notebook
jupyter notebook

# Navigate to training directory and open:
training/potato-disease-training.ipynb

# In cell #2, update the dataset path
# Run all cells sequentially to train the model
# Save the final model to `models/` with a version number
```

---

## 🚀 Running the API Server
### Option 1: FastAPI Standalone
```bash
cd api
uvicorn main:app --reload --host 0.0.0.0
# Accessible at http://0.0.0.0:8000
```

### Option 2: FastAPI with TensorFlow Serving
```bash
# Prepare config
cp models.config.example models.config

# Run TF Serving (update config path)
docker run -t --rm -p 8501:8501 \
  -v C:/Code/potato-disease-classification:/potato-disease-classification \
  tensorflow/serving \
  --rest_api_port=8501 \
  --model_config_file=/potato-disease-classification/models.config

# Run FastAPI
uvicorn main-tf-serving:app --reload --host 0.0.0.0
```

---

## 🌐 Running the Web Frontend
```bash
cd frontend
cp .env.example .env
# Update `REACT_APP_API_URL` in .env
npm run start
```

---

## 📉 TF Lite Model Conversion
```bash
jupyter notebook
# Open: training/tf-lite-converter.ipynb
# Update dataset path in cell #2
# Run all cells
# Model will be saved in tf-lite-models/
```

---

## ☁️ Deploying to Google Cloud Platform

### Option A: TF Lite Model Deployment
```bash
# Upload tf-lite model to bucket at path models/potatoes.h5
cd gcp
gcloud auth login
gcloud functions deploy predict_lite \
  --runtime python38 \
  --trigger-http \
  --memory 512 \
  --project <project_id>
```
Test via Postman using the generated URL.

### Option B: Full TF Model Deployment
```bash
# Upload model.h5 to bucket at path models/potato-model.h5
cd gcp
gcloud auth login
gcloud functions deploy predict \
  --runtime python38 \
  --trigger-http \
  --memory 512 \
  --project <project_id>
```
Test via Postman using the generated URL.

---

## 🔗 Reference
- [TensorFlow Serving with GCF](https://cloud.google.com/blog/products/ai-machine-learning/how-to-serve-deep-learning-models-using-tensorflow-2-0-with-cloud-functions)
- [React Native CLI Setup](https://reactnative.dev/docs/environment-setup)
- [TensorFlow Model Deployment](https://www.tensorflow.org/tfx/serving)

---

## 📧 Contact
For issues, please raise a GitHub issue or contact the maintainer via email.
