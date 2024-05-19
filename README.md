# Flask ML/DL Application

This project demonstrates a Flask application that serves a Machine Learning/Deep Learning model.

## Table of Contents

1. [Setup Development Environment]
2. [ML/DL Model Details]
3. [Running the Application Locally]
4. [Deploying the Application]
5. [API Documentation]

## Setup Development Environment

### Prerequisites

- Python 3.x
- Virtualenv

### Installation Steps

1. **Clone the repository**:
   https://github.com/aman26195/Image-Processing-API

2. **Create and activate a virtual environment**:
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

3. **Install dependencies**:
    ```bash
    pip install -r requirements.txt
    ```

## ML/DL Model Details

- **Model**: [VGG pre-trained model]
- **Framework**: TensorFlow/PyTorch/Other
  

## Running the Application Locally

1. **Set environment variables:
    ```bash
    export FLASK_APP=app.py
    export FLASK_ENV=development
    ```

2. **Run the Flask application**:
    ```bash
    flask run
    ```

    The application will be accessible at `http://127.0.0.1:5000`.

### Deploying on a Local Computer

#### 1. Install Required Software

1. **Update package list and install Python, pip, and virtualenv**:
    ```bash
    sudo apt-get update
    sudo apt-get install python3 python3-pip python3-venv
    ```

2. **Install Gunicorn**:
    ```bash
    pip3 install gunicorn
    ```

3. **Install Nginx**:
    ```bash
    sudo apt-get install nginx
    ```

#### 2. Set Up Your Flask Application

1. **Create a virtual environment and install Flask**:
    ```bash
    cd /path/to/your/app
    python3 -m venv venv
    source venv/bin/activate
    pip install flask
    ```

2. **Ensure your `requirements.txt` is updated**:
    ```bash
    pip freeze > requirements.txt
    ```

#### 3. Run Gunicorn

1. **Test Gunicorn with your Flask app**:
    ```bash
    gunicorn --bind 0.0.0.0:8000 app:app
    ```

    Your Flask app should now be accessible at `http://localhost:8000`.

#### 4. Configure Nginx

1. **Create an Nginx configuration file**:
    ```bash
    sudo nano /etc/nginx/sites-available/flaskapp
    ```

2. **Add the following configuration to the file**:
    ```nginx
    server {
        listen 80;
        server_name your_domain_or_IP;

        location / {
            proxy_pass http://127.0.0.1:8000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
    ```

3. **Enable the configuration by creating a symbolic link**:
    ```bash
    sudo ln -s /etc/nginx/sites-available/flaskapp /etc/nginx/sites-enabled
    ```

4. **Test the Nginx configuration**:
    ```bash
    sudo nginx -t
    ```

5. **Restart Nginx**:
    ```bash
    sudo systemctl restart nginx
    ```

#### 5. Set Up a Systemd Service for Gunicorn

1. **Create a systemd service file for Gunicorn**:
    ```bash
    sudo nano /etc/systemd/system/flaskapp.service
    ```

2. **Add the following configuration to the file**:
    ```ini
    [Unit]
    Description=Gunicorn instance to serve Flask application
    After=network.target

    [Service]
    User=yourusername
    Group=www-data
    WorkingDirectory=/path/to/your/app
    Environment="PATH=/path/to/your/app/venv/bin"
    ExecStart=/path/to/your/app/venv/bin/gunicorn --workers 3 --bind unix:/path/to/your/app/flaskapp.sock -m 007 app:app

    [Install]
    WantedBy=multi-user.target
    ```

3. **Reload systemd to read the new service file**:
    ```bash
    sudo systemctl daemon-reload
    ```

4. **Enable and start the service**:
    ```bash
    sudo systemctl enable flaskapp
    sudo systemctl start flaskapp
    ```

### Verify Deployment
**Check the status of the Gunicorn service**:
    ```bash
    sudo systemctl status flaskapp
    ```

## API Documentation

### Available Endpoints

1. **Predict**
    - **URL**: `/predict`
    - **Method**: `POST`
    - **Description**: Takes input data and returns model predictions.

### Input Parameters

- **Content-Type**: `application/json`
- **Request Body**:
    ```json
    {
        "input_data": "your_input_data_here"
    }
    ```

### Response

- **Success**: `200 OK`
    ```json
    {
        "status": "success",
        "prediction": "model_prediction_here"
    }
    ```

eg:
{
"fruit"="Orange",
"probability":1.0

}
