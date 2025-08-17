from flask import Flask, render_template, request
import pandas as pd
import matplotlib.pyplot as plt
import os
app = Flask(__name__)
# Load data
def load_data():
    # Replace 'path_to_your_data.csv' with the actual path to your CSV file
    data = pd.read_csv('path_to_your_data.csv')
    data['date'] = pd.to_datetime(data['date'])  # Ensure 'date' column is in datetime format
    return data
@app.route('/', methods=['GET', 'POST'])
def index():
    data = load_data()
    filtered_data = data
    # Get unique manufacturers for the dropdown
    manufacturers = data['manufacturer'].unique()
    if request.method == 'POST':
        start_date = request.form['start_date']
        end_date = request.form['end_date']
        vehicle_type = request.form['vehicle_type']
        manufacturer = request.form['manufacturer']
        # Filter data based on selections
        filtered_data = data[(data['date'] >= start_date) & (data['date'] <= end_date) & 
