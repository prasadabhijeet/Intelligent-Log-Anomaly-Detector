# Intelligent Log Anomaly Detector for Microservices

A production-ready machine learning tool that detects anomalies in microservice logs using TF-IDF vectorization and RandomForest classification.

## 🎯 What Is This?

This tool helps DevOps teams and developers automatically detect unusual or error-prone logs in microservices by:
- Converting unstructured text logs into machine-readable numeric features
- Training a RandomForest model on labeled log data
- Classifying new logs as **normal** or **anomalous**
- Providing confidence scores for each prediction

## 🚀 Features

✅ **TF-IDF Vectorization** - Converts logs to numeric features based on word importance  
✅ **RandomForest Classification** - Robust ML algorithm for text classification  
✅ **Feature Importance Analysis** - Understand which words indicate anomalies  
✅ **Model Evaluation** - Accuracy, precision, recall, and confusion matrix  
✅ **Real-time Anomaly Detection** - Classify new logs instantly  
✅ **Beginner-Friendly Jupyter Notebook** - Step-by-step tutorial with explanations  

## 📋 Requirements

- Python 3.8+
- pandas
- scikit-learn
- numpy
- Jupyter Notebook

### Install Dependencies

```bash
pip install pandas scikit-learn numpy jupyter
```

## 📂 Project Files

- **`log_anomaly_detection.ipynb`** - Main Jupyter notebook with complete tutorial
- **`README.md`** - This file

## 🏃 Getting Started

### 1. Launch Jupyter Notebook

```bash
jupyter notebook log_anomaly_detection.ipynb
```

### 2. Run Cells in Order

The notebook is organized in 7 steps:

1. **Install Required Packages** - Ensures all dependencies are available
2. **Import Libraries** - Load pandas, scikit-learn, etc.
3. **Prepare Sample Log Data** - Create labeled dataset of normal and anomalous logs
4. **TF-IDF Vectorization** - Convert text to numeric features
5. **Train ML Model** - Train RandomForestClassifier
6. **Evaluate Performance** - Check accuracy on test set
7. **Detect Anomalies** - Classify new logs and show results

### 3. Understand the Output

For each new log, you'll see:
```
✓ NORMAL (Anomaly confidence: 5.2%)
  Log: [INFO] User logout successful

🚨 ANOMALY (Anomaly confidence: 95.8%)
  Log: [ERROR] Cache server timeout
```

## 🧠 How It Works (Simple Explanation)

### Step 1: Prepare Data
We create a dataset of logs labeled as either:
- **0** = Normal (healthy operations)
- **1** = Anomaly (errors, warnings, failures)

### Step 2: TF-IDF Vectorization
Convert text logs into numbers:
- **"ERROR"** → might get weight 0.8 (common in anomalies)
- **"successful"** → might get weight 0.2 (common in normal logs)
- **"the"** → gets weight 0.01 (common everywhere, less useful)

### Step 3: Train Model
RandomForest learns patterns:
- "If a log contains 'ERROR' and 'failed', it's probably an anomaly"
- "If a log contains 'INFO' and 'completed', it's probably normal"

### Step 4: Detect Anomalies
For new logs, the model predicts:
- "Is this normal or anomalous?"
- "How confident am I?" (0-100%)

## 📊 Expected Output Example

```
Model Accuracy on Test Set: 85.00%

Classification Report:
              precision    recall  f1-score   support
      Normal       0.88      0.80      0.84         5
      Anomaly      0.75      0.86      0.80         3

Top 10 Most Important Features:
error                | Importance: 0.0856
failed               | Importance: 0.0742
connection           | Importance: 0.0698
timeout              | Importance: 0.0645
exception            | Importance: 0.0598
...
```

## 🔧 Customization

### Add More Training Data

Edit the `log_data` and `labels` in **Section 1**:

```python
log_data = [
    "[INFO] Your log here",
    "[ERROR] Your anomaly here",
    # ... add more logs
]
labels = [0, 1, ...]  # 0=normal, 1=anomaly
```

### Adjust Model Parameters

In **Section 3**, modify RandomForest settings:

```python
model = RandomForestClassifier(
    n_estimators=100,      # More trees = better but slower
    max_depth=5,           # Deeper trees = more complex patterns
    random_state=42
)
```

### Change TF-IDF Settings

In **Section 2**, customize vectorization:

```python
vectorizer = TfidfVectorizer(
    max_features=50,       # More features = capture more vocabulary
    lowercase=True,
    stop_words='english'   # Remove common English words
)
```

## 🚀 Next Steps (Production Deployment)

To extend this for production:

### 1. **Connect to Real Logs**
```python
# From ELK Stack, Fluentd, or Kubernetes
logs = fetch_logs_from_kubernetes()
```

### 2. **Deploy as API**
```bash
# Create Flask/FastAPI service
pip install flask
# See Flask example in extended_deployment.py
```

### 3. **Monitor in Real-time**
```python
# Stream logs and detect anomalies continuously
while True:
    new_log = get_next_log()
    prediction = model.predict(vectorizer.transform([new_log]))
    if prediction[0] == 1:
        alert_to_slack()  # Send alert
```

### 4. **Train on Production Data**
```python
# Use real logs from your cluster
df = pd.read_csv('production_logs.csv')
# Manually label a sample
# Retrain the model weekly
```

## 📚 Learning Resources

- **TF-IDF**: https://en.wikipedia.org/wiki/Tf%E2%80%93idf
- **RandomForest**: https://scikit-learn.org/stable/modules/ensemble.html
- **scikit-learn**: https://scikit-learn.org/
- **Log Analysis**: https://www.elastic.co/what-is/elk-stack

## 💡 Key Insights

✅ **TF-IDF is simple yet effective** for text-based anomaly detection  
✅ **RandomForest requires no deep learning expertise**  
✅ **Feature importance helps understand model decisions**  
✅ **Real-world logs need preprocessing** (parsing, cleaning)  
✅ **Feedback loop is crucial** (manually verify predictions and retrain)  

## 📄 License

MIT License

## 🤝 Contributing

Feel free to extend this project! Ideas:
- Add support for structured logs (JSON)
- Implement LSTM/deep learning models
- Create a web dashboard
- Add database integration for log storage
- Build Kubernetes operator for auto-labeling
