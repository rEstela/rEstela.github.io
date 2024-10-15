---
date:
    created: 2024-10-15
categories:
  - deep learning
comments: false
---

# Tracking Your Machine Learning Projects with MLFlow

Have you ever felt lost in your machine learning projects, not knowing what code you used, what datasets were involved, or how the models performed? If so, I have a recommendation that could be a game changer for you: **MLFlow**.

<!-- more -->

MLFlow is an incredibly helpful tool designed to manage, track, and deploy machine learning models. It simplifies your workflow by allowing you to log and compare your experiments—things like hyperparameters (learning rate, number of layers, etc.), metrics (accuracy, F1-score, etc.), code versions, and even environment dependencies. This makes it so much easier to keep track of everything and compare results from different experiments.

What's even better is that MLFlow works well for both solo projects and collaborative efforts, whether you're working locally or with a team.

In this post, I’ll show you how to set up and use MLFlow **locally** on your own machine for your projects.

## Step-by-Step Guide to Setting Up MLFlow

### 1. Create a Directory for Your Experiments
The first thing you need is a directory to store your experiments. I suggest creating a folder like "mlflow-runs" where all your results will be saved.

You can create this folder via the terminal:
```bash
mkdir $HOME/mlflow-runs
cd $HOME/mlflow-runs
```

Or, if you prefer using the graphical interface, you can create the folder by right-clicking and selecting: **NEW -> FOLDER** in your file explorer.

### 2. Set Up a Virtual Environment
Next, you'll need to open a terminal (I use VSCode, but feel free to use your favorite app). Then, create and activate a new virtual environment:

```bash
python -m venv venv
source venv/bin/activate
```

### 3. Install MLFlow and Dependencies
With your virtual environment activated, install MLFlow and any other dependencies you’ll need for your project:

```bash
pip install mlflow tensorflow h5py scikit-learn matplotlib
```

### 4. Configure the MLFlow Server
Finally, configure your MLFlow server locally. This will allow you to store and visualize your experiment data through a web interface:

```bash
mlflow server \
   --host "0.0.0.0" \
   --port 5000 \
   --backend-store-uri mlruns \
   --artifacts-destination mlartifacts
```

Once you’ve run this command, MLFlow will be accessible at `http://localhost:5000`, and you'll be able to log and track your experiments.

## Logging Your Model, Parameters, and Metrics with MLFlow

Now that MLFlow is set up, let's look at how you can log your model, parameters, and results in your script. Here’s an example:

```python
import mlflow

# Initializing the MLflow tracking
mlflow.set_tracking_uri("http://localhost:5000")  # Define the URI of the MLFlow server
mlflow.set_experiment("EXPERIMENT_NAME")

with mlflow.start_run():
    # Create and train your model here
    model = create_model((X_train.shape[1], X_train.shape[2]))
    history = model.fit(
        X_train, y_train,
        epochs=10,
        batch_size=32,
        validation_data=(X_val, y_val)
    )

    # Log parameters
    mlflow.log_param("epochs", 10)
    mlflow.log_param("batch_size", 32)

    # Log metrics
    accuracy = history.history['accuracy'][-1]
    val_accuracy = history.history['val_accuracy'][-1]
    mlflow.log_metric("train_accuracy", accuracy)
    mlflow.log_metric("val_accuracy", val_accuracy)

    # Save the model as an artifact
    model.save("af_model.h5")
    mlflow.log_artifact("af_model.h5")
```

You can modify this script to fit your project. After running it, all your experiment data will be available in the MLFlow interface, where you can see detailed information about every experiment you’ve logged.

Access in your browser http://localhost:5000 and you will be able to see the MLFlow ui.

---

And that’s it! By following these steps, you’ll have a fully functional setup to track your machine learning experiments locally using MLFlow. It’s a small change to your workflow, but it can make a **huge** difference when managing complex projects.
