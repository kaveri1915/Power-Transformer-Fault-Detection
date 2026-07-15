# Step 1 — Create venv and activate
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Linux/Mac

# Step 2 — Install all packages
pip install django mysqlclient pillow scikit-learn numpy pandas joblib matplotlib seaborn imbalanced-learn openpyxl reportlab django-widget-tweaks tensorflow

# Step 3 — Create MySQL database (run in MySQL shell)
# CREATE DATABASE transformer_db;

# Step 4 — Create management command folders
mkdir monitoring\management
mkdir monitoring\management\commands
type nul > monitoring\management\__init__.py
type nul > monitoring\management\commands\__init__.py

# Step 5 — Make and run migrations
python manage.py makemigrations accounts
python manage.py makemigrations monitoring
python manage.py makemigrations faults
python manage.py makemigrations predictions
python manage.py makemigrations alerts
python manage.py makemigrations reports
python manage.py makemigrations admin_panel
python manage.py migrate

# Step 6 — Create superuser
python manage.py createsuperuser

# Step 7 — Load 3 transformers + 1000+ real-pattern readings
python manage.py load_initial_data

# Step 8 — Collect static files
python manage.py collectstatic --noinput

# Step 9 — (After downloading DGA dataset) Train ML models
cd ml_models
python train_rf_model.py
python train_risk_model.py
cd ..

# Step 10 — Run server
python manage.py runserver
