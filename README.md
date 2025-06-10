[V0_FILE]markdown:file="README.md" isMerged="true"
# AeroPulse.AI - AQI Monitoring Dashboard

A professional Air Quality Index (AQI) monitoring and analytics platform built with Streamlit, featuring advanced data visualization, machine learning predictions, and comprehensive environmental intelligence for Sohna, Haryana.

## Features

- **Real-time AQI Monitoring**: Live air quality data visualization and analysis
- **Professional Dashboard**: Clean, modern UI with comprehensive metrics
- **Advanced Analytics**: Statistical analysis, trend detection, and pattern recognition
- **Machine Learning**: AI-powered predictions using multiple ML algorithms
- **Geospatial Analysis**: Interactive maps with location intelligence
- **Health Risk Assessment**: Comprehensive health impact analysis and recommendations
- **Custom Branding**: Professional AeroPulse.AI logo integration

## Installation

1. Clone this repository
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the application:
   ```bash
   streamlit run app.py
   ```

## Data Source

The application uses AQI data from Sohna, Haryana, India, providing comprehensive air quality monitoring and analysis capabilities.

## Technical Stack

- **Frontend**: Streamlit with custom CSS styling
- **Data Processing**: Pandas, NumPy
- **Visualization**: Plotly, Folium
- **Machine Learning**: Scikit-learn
- **Geospatial**: Interactive maps with OpenStreetMap
- **Branding**: Custom logo generation with PIL

## Usage

The dashboard provides multiple views:
- **Dashboard**: Overview with current AQI status and key metrics
- **Analytics**: Deep statistical analysis and pattern recognition
- **Predictions**: AI-powered forecasting using machine learning
- **Geospatial**: Interactive maps and location intelligence
- **Reports**: Comprehensive insights and recommendations

## Professional Features

- Responsive design with professional styling
- Advanced data visualization techniques
- Machine learning integration
- Comprehensive health risk assessment
- Export capabilities for reports and data
- Real-time simulation of air quality monitoring

Perfect for environmental monitoring, public health assessment, and professional portfolio demonstration.
[V0_FILE]python:file="aeropulse_logo.py" isMerged="true"
import streamlit as st
import base64
from io import BytesIO
from PIL import Image, ImageDraw, ImageFont
import numpy as np

def create_aeropulse_logo(width=300, height=100, show_tagline=True):
    """
    Create the AeroPulse.ai logo and return it as an image
    
    Parameters:
    width (int): Width of the logo
    height (int): Height of the logo
    show_tagline (bool): Whether to show the .ai tagline
    
    Returns:
    PIL Image: The logo as a PIL Image object
    """
    # Create a transparent background
    logo = Image.new('RGBA', (width, height), (0, 0, 0, 0))
    draw = ImageDraw.Draw(logo)
    
    # Define colors
    primary_color = (139, 0, 0, 255)        # #8B0000 (Dark Red)
    secondary_color = (192, 192, 192, 255)  # #C0C0C0 (Silver)
    accent_color = (220, 20, 60, 255)       # #DC143C (Crimson)
    
    # Calculate sizes based on width
    icon_size = int(height * 0.8)
    icon_x = 10
    icon_y = (height - icon_size) // 2
    
    # Draw the circular background for the icon
    circle_x = icon_x + icon_size // 2
    circle_y = icon_y + icon_size // 2
    circle_radius = icon_size // 2
    
    # Draw outer glow
    for i in range(5, 0, -1):
        alpha = int(100 * (1 - i/5))
        glow_color = (primary_color[0], primary_color[1], primary_color[2], alpha)
        draw.ellipse(
            (circle_x - circle_radius - i, 
             circle_y - circle_radius - i,
             circle_x + circle_radius + i, 
             circle_y + circle_radius + i), 
            fill=None, 
            outline=glow_color, 
            width=1
        )
    
    # Draw main circle
    draw.ellipse(
        (circle_x - circle_radius, 
         circle_y - circle_radius,
         circle_x + circle_radius, 
         circle_y + circle_radius), 
        fill=(15, 16, 32, 230), 
        outline=primary_color, 
        width=2
    )
    
    # Draw air flow lines
    line_width = 2
    line_length = int(icon_size * 0.7)
    line_start_x = circle_x - line_length // 2
    
    # Top wave
    wave_y = circle_y - circle_radius // 3
    points = []
    for i in range(6):
        x = line_start_x + i * line_length // 5
        offset = circle_radius // 4 if i % 2 == 0 else -circle_radius // 4
        points.append((x, wave_y + offset))
    
    # Draw the wave with anti-aliasing
    for i in range(len(points) - 1):
        draw.line([points[i], points[i+1]], fill=primary_color, width=line_width)
    
    # Middle wave
    wave_y = circle_y
    points = []
    for i in range(6):
        x = line_start_x + i * line_length // 5
        offset = circle_radius // 4 if i % 2 == 0 else -circle_radius // 4
        points.append((x, wave_y + offset))
    
    for i in range(len(points) - 1):
        draw.line([points[i], points[i+1]], fill=secondary_color, width=line_width)
    
    # Bottom wave
    wave_y = circle_y + circle_radius // 3
    points = []
    for i in range(6):
        x = line_start_x + i * line_length // 5
        offset = circle_radius // 4 if i % 2 == 0 else -circle_radius // 4
        points.append((x, wave_y + offset))
    
    for i in range(len(points) - 1):
        draw.line([points[i], points[i+1]], fill=primary_color, width=line_width)
    
    # Add AI dots
    dot_radius = max(2, int(icon_size * 0.05))
    dots = [
        (circle_x - circle_radius // 3, circle_y - circle_radius // 3),
        (circle_x, circle_y - circle_radius // 4),
        (circle_x + circle_radius // 3, circle_y - circle_radius // 3),
        (circle_x - circle_radius // 3, circle_y + circle_radius // 3),
        (circle_x + circle_radius // 3, circle_y + circle_radius // 3)
    ]
    
    for dot in dots:
        draw.ellipse(
            (dot[0] - dot_radius, dot[1] - dot_radius,
             dot[0] + dot_radius, dot[1] + dot_radius),
            fill=accent_color
        )
    
    # Connect dots with lines
    connections = [
        (0, 1), (1, 2), (3, 4)
    ]
    
    for conn in connections:
        draw.line([dots[conn[0]], dots[conn[1]]], fill=accent_color, width=1)
    
    # Add text
    text_x = icon_x + icon_size + 15
    text_y = height // 2 - 15 if show_tagline else height // 2 - 10
    
    # Try to use Orbitron font if available, otherwise use default
    try:
        font_size = int(height * 0.4)
        font = ImageFont.truetype("Orbitron-Bold.ttf", font_size)
    except IOError:
        font_size = int(height * 0.4)
        try:
            font = ImageFont.truetype("Arial.ttf", font_size)
        except IOError:
            font = ImageFont.load_default()
    
    # Draw text with gradient effect
    text = "AERO·PULSE"
    text_width = draw.textlength(text, font=font)
    
    # Create gradient for text
    for i in range(len(text)):
        char = text[i]
        char_width = draw.textlength(char, font=font)
        progress = i / len(text)
        r = int(primary_color[0] * (1 - progress) + secondary_color[0] * progress)
        g = int(primary_color[1] * (1 - progress) + secondary_color[1] * progress)
        b = int(primary_color[2] * (1 - progress) + secondary_color[2] * progress)
        color = (r, g, b, 255)
        draw.text((text_x, text_y), char, fill=color, font=font)
        text_x += char_width
    
    # Add .ai tagline
    if show_tagline:
        try:
            tagline_font_size = int(height * 0.2)
            tagline_font = ImageFont.truetype("Arial.ttf", tagline_font_size)
        except IOError:
            tagline_font = ImageFont.load_default()
        
        tagline_x = icon_x + icon_size + 15 + text_width - 40
        tagline_y = text_y + font_size + 2
        draw.text((tagline_x, tagline_y), ".ai", fill=accent_color, font=tagline_font)
    
    return logo

def get_image_as_base64(img):
    """Convert PIL image to base64 string"""
    buffered = BytesIO()
    img.save(buffered, format="PNG")
    return base64.b64encode(buffered.getvalue()).decode()

def display_aeropulse_logo(width=300, height=100, show_tagline=True):
    """Display the AeroPulse.ai logo in Streamlit"""
    logo = create_aeropulse_logo(width, height, show_tagline)
    logo_base64 = get_image_as_base64(logo)
    
    st.markdown(
        f'<img src="data:image/png;base64,{logo_base64}" alt="AeroPulse.ai Logo" style="margin-bottom: 20px;">',
        unsafe_allow_html=True
    )

def display_animated_aeropulse_logo():
    """Display an animated version of the AeroPulse.ai logo using HTML/CSS"""
    st.markdown("""
    <style>
    .aeropulse-logo {
        display: flex;
        align-items: center;
        margin-bottom: 20px;
    }
    
    .logo-icon {
        width: 60px;
        height: 60px;
        background: linear-gradient(135deg, rgba(15, 16, 32, 0.9) 0%, rgba(26, 32, 44, 0.9) 100%);
        border-radius: 50%;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 0 30px rgba(0, 245, 212, 0.5);
        border: 2px solid rgba(0, 245, 212, 0.8);
        animation: pulse 3s infinite;
    }
    
    @keyframes pulse {
        0% { box-shadow: 0 0 0 0 rgba(0, 245, 212, 0.4); }
        70% { box-shadow: 0 0 0 10px rgba(0, 245, 212, 0); }
        100% { box-shadow: 0 0 0 0 rgba(0, 245, 212, 0); }
    }
    
    .wave {
        position: absolute;
        height: 2px;
        background: linear-gradient(to right, #00f5d4, #2de2e6);
        width: 40px;
        left: 10px;
    }
    
    .wave-1 {
        top: 20px;
        animation: wave 2s infinite;
    }
    
    .wave-2 {
        top: 30px;
        animation: wave 2.5s infinite;
    }
    
    .wave-3 {
        top: 40px;
        animation: wave 3s infinite;
    }
    
    @keyframes wave {
        0% { transform: translateX(0) scaleX(1); }
        50% { transform: translateX(2px) scaleX(0.95); }
        100% { transform: translateX(0) scaleX(1); }
    }
    
    .dot {
        position: absolute;
        width: 4px;
        height: 4px;
        background-color: #ff3864;
        border-radius: 50%;
    }
    
    .dot-1 { top: 20px; left: 20px; animation: blink 2s infinite; }
    .dot-2 { top: 20px; right: 20px; animation: blink 2.5s infinite; }
    .dot-3 { top: 30px; left: 30px; animation: blink 3s infinite; }
    .dot-4 { bottom: 20px; left: 20px; animation: blink 2.2s infinite; }
    .dot-5 { bottom: 20px; right: 20px; animation: blink 2.7s infinite; }
    
    @keyframes blink {
        0% { opacity: 0.3; }
        50% { opacity: 1; }
        100% { opacity: 0.3; }
    }
    
    .logo-text {
        margin-left: 15px;
    }
    
    .logo-title {
        font-family: 'Orbitron', sans-serif;
        font-size: 28px;
        font-weight: 700;
        background: linear-gradient(to right, #00f5d4, #2de2e6);
        -webkit-background-clip: text;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 15px rgba(0, 245, 212, 0.3);
        letter-spacing: 1px;
        margin: 0;
    }
    
    .logo-subtitle {
        font-family: 'Rajdhani', sans-serif;
        font-size: 14px;
        color: #ff3864;
        letter-spacing: 2px;
        margin-top: -5px;
    }
    </style>
    
    <div class="aeropulse-logo">
        <div class="logo-icon">
            <div class="wave wave-1"></div>
            <div class="wave wave-2"></div>
            <div class="wave wave-3"></div>
            <div class="dot dot-1"></div>
            <div class="dot dot-2"></div>
            <div class="dot dot-3"></div>
            <div class="dot dot-4"></div>
            <div class="dot dot-5"></div>
        </div>
        <div class="logo-text">
            <div class="logo-title">AERO·PULSE</div>
            <div class="logo-subtitle">.ai</div>
        </div>
    </div>
    """, unsafe_allow_html=True)

def add_logo_to_sidebar():
    """Add a small version of the logo to the Streamlit sidebar"""
    logo = create_aeropulse_logo(width=200, height=70, show_tagline=True)
    logo_base64 = get_image_as_base64(logo)
    
    st.sidebar.markdown(
        f'<img src="data:image/png;base64,{logo_base64}" alt="AeroPulse.ai Logo" style="margin-bottom: 20px; width: 100%;">',
        unsafe_allow_html=True
    )

# Example usage
if __name__ == "__main__":
    st.set_page_config(page_title="AeroPulse.ai Logo Demo", layout="wide")
    
    st.title("AeroPulse.ai Logo Options")
    
    st.header("1. Standard Logo")
    display_aeropulse_logo(width=400, height=120)
    
    st.header("2. Animated Logo (CSS)")
    display_animated_aeropulse_logo()
    
    st.header("3. Logo Variations")
    col1, col2 = st.columns(2)
    
    with col1:
        st.subheader("Small Logo")
        display_aeropulse_logo(width=200, height=70)
        
        st.subheader("Icon Only")
        logo = create_aeropulse_logo(width=100, height=100, show_tagline=False)
        icon_only = logo.crop((0, 0, 100, 100))
        st.image(icon_only)
    
    with col2:
        st.subheader("Sidebar Logo")
        add_logo_to_sidebar()
        st.write("👈 Check the sidebar for the logo")
        
        st.subheader("Large Logo")
        display_aeropulse_logo(width=600, height=150)
    
    st.header("4. Integration Example")
    st.code("""
    # Add this to the top of your Streamlit app
    from aeropulse_logo import display_aeropulse_logo, add_logo_to_sidebar
    
    # Display logo in the main area
    display_aeropulse_logo()
    
    # Or add it to the sidebar
    add_logo_to_sidebar()
    """)
[V0_FILE]python:file="app.py" isMerged="true"
import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import requests
from io import StringIO
from datetime import datetime, timedelta
import time
import folium
from streamlit_folium import st_folium
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_error
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')

from aeropulse_logo import display_aeropulse_logo, add_logo_to_sidebar, display_animated_aeropulse_logo

# Set page configuration
st.set_page_config(
    page_title="AERO·PULSE - AQI Monitoring System",
    page_icon="🌬️",
    layout="wide",
    initial_sidebar_state="expanded"
)

# For animated version in header
display_animated_aeropulse_logo()

# Professional CSS styling
def load_professional_css():
    st.markdown("""
    <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap');
    
    /* Global Styles */
    .stApp {
        background: linear-gradient(135deg, #1a1d29 0%, #2d3748 50%, #1a202c 100%);
        color: #ffffff;
        font-family: 'Inter', sans-serif;
    }
    
    /* Hide Streamlit branding */
    #MainMenu {visibility: hidden;}
    footer {visibility: hidden;}
    header {visibility: hidden;}
    
    /* Custom Header */
    .main-header {
        background: linear-gradient(135deg, rgba(45, 55, 72, 0.95) 0%, rgba(26, 32, 44, 0.95) 100%);
        backdrop-filter: blur(10px);
        border-radius: 16px;
        padding: 24px 32px;
        margin-bottom: 32px;
        border: 1px solid rgba(74, 85, 104, 0.3);
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
    }
    
    .header-content {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 16px;
    }
    
    .logo-section {
        display: flex;
        align-items: center;
        gap: 16px;
    }
    
    .logo {
        font-family: 'Inter', sans-serif;
        font-size: 28px;
        font-weight: 800;
        color: #4299e1;
        letter-spacing: -0.5px;
    }
    
    .logo-subtitle {
        font-size: 14px;
        color: #a0aec0;
        font-weight: 500;
        letter-spacing: 0.5px;
        text-transform: uppercase;
    }
    
    .header-nav {
        display: flex;
        align-items: center;
        gap: 24px;
    }
    
    .nav-item {
        color: #e2e8f0;
        font-size: 14px;
        font-weight: 500;
        padding: 8px 16px;
        border-radius: 8px;
        background: rgba(74, 85, 104, 0.3);
        border: 1px solid rgba(74, 85, 104, 0.4);
    }
    
    .status-indicator {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px 16px;
        background: rgba(56, 178, 172, 0.1);
        border: 1px solid rgba(56, 178, 172, 0.3);
        border-radius: 8px;
        font-size: 14px;
        font-weight: 600;
    }
    
    .live-indicator {
        width: 8px;
        height: 8px;
        background: #38b2ac;
        border-radius: 50%;
        animation: pulse 2s infinite;
    }
    
    @keyframes pulse {
        0% { opacity: 1; }
        50% { opacity: 0.5; }
        100% { opacity: 1; }
    }
    
    .main-title {
        font-size: 48px;
        font-weight: 800;
        color: #ffffff;
        margin: 0;
        line-height: 1.1;
        letter-spacing: -1px;
    }
    
    .main-subtitle {
        font-size: 18px;
        color: #a0aec0;
        margin-top: 8px;
        line-height: 1.5;
        font-weight: 400;
    }
    
    /* Professional Cards */
    .metric-card {
        background: linear-gradient(135deg, rgba(45, 55, 72, 0.8) 0%, rgba(26, 32, 44, 0.9) 100%);
        backdrop-filter: blur(10px);
        border-radius: 16px;
        padding: 24px;
        border: 1px solid rgba(74, 85, 104, 0.3);
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        transition: all 0.3s ease;
        height: 100%;
    }
    
    .metric-card:hover {
        transform: translateY(-4px);
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
        border-color: rgba(66, 153, 225, 0.4);
    }
    
    .metric-header {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 16px;
    }
    
    .metric-icon {
        width: 40px;
        height: 40px;
        border-radius: 10px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 18px;
        background: linear-gradient(135deg, #4299e1 0%, #3182ce 100%);
    }
    
    .metric-title {
        font-size: 14px;
        font-weight: 600;
        color: #e2e8f0;
        text-transform: uppercase;
        letter-spacing: 0.5px;
    }
    
    .metric-value {
        font-family: 'JetBrains Mono', monospace;
        font-size: 32px;
        font-weight: 700;
        color: #ffffff;
        margin-bottom: 8px;
        line-height: 1;
    }
    
    .metric-unit {
        font-size: 14px;
        color: #a0aec0;
        font-weight: 500;
        margin-left: 4px;
    }
    
    .metric-description {
        font-size: 13px;
        color: #a0aec0;
        line-height: 1.4;
    }
    
    .metric-trend {
        display: flex;
        align-items: center;
        gap: 4px;
        margin-top: 8px;
        font-size: 12px;
        font-weight: 600;
    }
    
    .trend-up { color: #f56565; }
    .trend-down { color: #48bb78; }
    .trend-stable { color: #ed8936; }
    
    /* AQI Status Colors */
    .aqi-good { color: #48bb78; }
    .aqi-moderate { color: #ed8936; }
    .aqi-unhealthy-sensitive { color: #f56565; }
    .aqi-unhealthy { color: #e53e3e; }
    .aqi-very-unhealthy { color: #c53030; }
    .aqi-hazardous { color: #9b2c2c; }
    
    /* Current AQI Display */
    .current-aqi-card {
        background: linear-gradient(135deg, rgba(66, 153, 225, 0.1) 0%, rgba(49, 130, 206, 0.1) 100%);
        border: 2px solid rgba(66, 153, 225, 0.3);
        border-radius: 20px;
        padding: 32px;
        text-align: center;
        margin: 24px 0;
    }
    
    .aqi-value {
        font-family: 'JetBrains Mono', monospace;
        font-size: 64px;
        font-weight: 800;
        color: #4299e1;
        line-height: 1;
        margin-bottom: 8px;
    }
    
    .aqi-label {
        font-size: 16px;
        color: #a0aec0;
        font-weight: 600;
        text-transform: uppercase;
        letter-spacing: 1px;
        margin-bottom: 16px;
    }
    
    .aqi-status {
        font-size: 24px;
        font-weight: 700;
        margin-bottom: 8px;
    }
    
    .aqi-location {
        font-size: 14px;
        color: #a0aec0;
        font-style: italic;
    }
    
    /* Info Cards */
    .info-card {
        background: linear-gradient(135deg, rgba(45, 55, 72, 0.6) 0%, rgba(26, 32, 44, 0.8) 100%);
        border-radius: 12px;
        padding: 20px;
        border-left: 4px solid #4299e1;
        margin: 16px 0;
    }
    
    .info-title {
        font-size: 16px;
        font-weight: 700;
        color: #4299e1;
        margin-bottom: 8px;
    }
    
    .info-content {
        font-size: 14px;
        color: #e2e8f0;
        line-height: 1.6;
    }
    
    /* Chart Containers */
    .chart-container {
        background: linear-gradient(135deg, rgba(45, 55, 72, 0.8) 0%, rgba(26, 32, 44, 0.9) 100%);
        border-radius: 16px;
        padding: 24px;
        border: 1px solid rgba(74, 85, 104, 0.3);
        margin: 16px 0;
    }
    
    .chart-title {
        font-size: 20px;
        font-weight: 700;
        color: #ffffff;
        margin-bottom: 8px;
    }
    
    .chart-subtitle {
        font-size: 14px;
        color: #a0aec0;
        margin-bottom: 20px;
        line-height: 1.5;
    }
    
    /* Alert Styles */
    .alert-warning {
        background: linear-gradient(135deg, rgba(237, 137, 54, 0.1) 0%, rgba(221, 107, 32, 0.1) 100%);
        border: 1px solid rgba(237, 137, 54, 0.3);
        border-radius: 8px;
        padding: 16px;
        margin: 16px 0;
    }
    
    .alert-danger {
        background: linear-gradient(135deg, rgba(245, 101, 101, 0.1) 0%, rgba(229, 62, 62, 0.1) 100%);
        border: 1px solid rgba(245, 101, 101, 0.3);
        border-radius: 8px;
        padding: 16px;
        margin: 16px 0;
    }
    
    /* Footer */
    .footer {
        text-align: center;
        padding: 32px;
        margin-top: 48px;
        border-top: 1px solid rgba(74, 85, 104, 0.3);
        color: #a0aec0;
        font-size: 14px;
    }
    </style>
    """, unsafe_allow_html=True)

# Load professional CSS
load_professional_css()

# Function to load data with caching
@st.cache_data(ttl=3600)
def load_aqi_data():
    """Load and preprocess AQI data from the provided URL"""
    url = "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/cleaned_sohna_aqi-q5VA5gd3ZOThg04jNukqM8DWDCHQZs.csv"
    try:
        response = requests.get(url)
        data = StringIO(response.text)
        df = pd.read_csv(data)
        
        # Enhanced data preprocessing
        df['Datetime'] = pd.to_datetime(df['Datetime'])
        df['Date'] = df['Datetime'].dt.date
        df['Month'] = df['Datetime'].dt.month
        df['Month_Name'] = df['Datetime'].dt.strftime('%B')
        df['Day_of_Week'] = df['Datetime'].dt.day_name()
        df['Hour_Num'] = df['Datetime'].dt.hour
        df['AQI'] = pd.to_numeric(df['AQI'], errors='coerce')
        df['Season'] = df['Month'].map({12: 'Winter', 1: 'Winter', 2: 'Winter',
                                       3: 'Spring', 4: 'Spring', 5: 'Spring',
                                       6: 'Summer', 7: 'Summer', 8: 'Summer',
                                       9: 'Autumn', 10: 'Autumn', 11: 'Autumn'})
        
        # Remove any rows with missing AQI values
        df = df.dropna(subset=['AQI'])
        
        return df
    except Exception as e:
        st.error(f"Error loading data: {str(e)}")
        return None

# Load data
with st.spinner('🔄 Loading AQI data and initializing analytics...'):
    df = load_aqi_data()

if df is None:
    st.error("Failed to load data. Please check your internet connection.")
    st.stop()

# AQI Health Risk Categories and Colors
AQI_CATEGORIES = {
    'Good': {'range': (0, 50), 'color': '#48bb78', 'description': 'Air quality is satisfactory'},
    'Moderate': {'range': (51, 100), 'color': '#ed8936', 'description': 'Acceptable for most people'},
    'Unhealthy for Sensitive Groups': {'range': (101, 150), 'color': '#f56565', 'description': 'Sensitive groups may experience health effects'},
    'Unhealthy': {'range': (151, 200), 'color': '#e53e3e', 'description': 'Everyone may experience health effects'},
    'Very Unhealthy': {'range': (201, 300), 'color': '#c53030', 'description': 'Health alert for everyone'},
    'Hazardous': {'range': (301, 500), 'color': '#9b2c2c', 'description': 'Emergency conditions'}
}

def get_aqi_category(aqi_value):
    """Determine AQI category based on value"""
    for category, info in AQI_CATEGORIES.items():
        if info['range'][0] <= aqi_value <= info['range'][1]:
            return category, info['color'], info['description']
    return 'Hazardous', '#9b2c2c', 'Emergency conditions'

def get_aqi_trend(current, previous):
    """Calculate AQI trend"""
    if current > previous * 1.05:
        return "↗️", "trend-up", "Increasing"
    elif current < previous * 0.95:
        return "↘️", "trend-down", "Decreasing"
    else:
        return "→", "trend-stable", "Stable"

# Calculate key metrics
current_time = datetime.now()
current_hour = current_time.hour
hourly_avg = df.groupby('Hour_Num')['AQI'].mean()
current_aqi = hourly_avg.get(current_hour, df['AQI'].mean())
previous_hour_aqi = hourly_avg.get((current_hour - 1) % 24, current_aqi)

# Get current AQI status
current_category, current_color, current_description = get_aqi_category(current_aqi)
trend_icon, trend_class, trend_text = get_aqi_trend(current_aqi, previous_hour_aqi)

# Calculate statistics
stats = {
    'avg_aqi': df['AQI'].mean(),
    'max_aqi': df['AQI'].max(),
    'min_aqi': df['AQI'].min(),
    'std_aqi': df['AQI'].std(),
    'total_records': len(df),
    'good_days': len(df[df['AQI'] <= 50]),
    'unhealthy_days': len(df[df['AQI'] > 150])
}

# Professional Header
st.markdown(f"""
<div class="main-header">
    <div class="header-content">
        <div class="logo-section">
            <div>
                <div class="logo">AQI</div>
                <div class="logo-subtitle">Analytics Platform</div>
            </div>
        </div>
        <div class="header-nav">
            <div class="nav-item">Sohna, Haryana</div>
            <div class="nav-item">🇮🇳 India</div>
            <div class="status-indicator">
                <div class="live-indicator"></div>
                Live Analytics
            </div>
        </div>
    </div>
    <div class="main-title">Air Quality Intelligence</div>
    <div class="main-subtitle">
        Advanced environmental monitoring and predictive analytics for Sohna region. 
        Real-time insights powered by machine learning and comprehensive data analysis.
    </div>
</div>
""", unsafe_allow_html=True)

# Main Dashboard
st.markdown("## 🏠 Dashboard Overview")

# Current Status Row
col1, col2, col3, col4 = st.columns([2, 1, 1, 1])

with col1:
    st.markdown(f"""
    <div class="current-aqi-card">
        <div class="aqi-value" style="color: {current_color};">{current_aqi:.1f}</div>
        <div class="aqi-label">Air Quality Index</div>
        <div class="aqi-status" style="color: {current_color};">{current_category}</div>
        <div class="aqi-location">Sohna, Haryana • {current_time.strftime('%B %d, %Y at %H:%M')}</div>
    </div>
    """, unsafe_allow_html=True)

with col2:
    st.markdown(f"""
    <div class="metric-card">
        <div class="metric-header">
            <div class="metric-icon">📈</div>
            <div class="metric-title">Trend</div>
        </div>
        <div class="metric-value">{trend_icon}</div>
        <div class="metric-description">{trend_text} from previous hour</div>
        <div class="metric-trend {trend_class}">
            {abs(current_aqi - previous_hour_aqi):.1f} AQI change
        </div>
    </div>
    """, unsafe_allow_html=True)

with col3:
    st.markdown(f"""
    <div class="metric-card">
        <div class="metric-header">
            <div class="metric-icon">🎯</div>
            <div class="metric-title">Peak AQI</div>
        </div>
        <div class="metric-value">{stats['max_aqi']:.1f}<span class="metric-unit">AQI</span></div>
        <div class="metric-description">Maximum recorded value</div>
        <div class="metric-trend trend-up">
            Highest in dataset
        </div>
    </div>
    """, unsafe_allow_html=True)

with col4:
    st.markdown(f"""
    <div class="metric-card">
        <div class="metric-header">
            <div class="metric-icon">✅</div>
            <div class="metric-title">Good Days</div>
        </div>
        <div class="metric-value">{(stats['good_days']/stats['total_records']*100):.1f}<span class="metric-unit">%</span></div>
        <div class="metric-description">Days with AQI ≤ 50</div>
        <div class="metric-trend trend-down">
            {stats['good_days']} out of {stats['total_records']//24} days
        </div>
    </div>
    """, unsafe_allow_html=True)

# Health Alert Section
if current_aqi > 150:
    alert_class = "alert-danger" if current_aqi > 200 else "alert-warning"
    st.markdown(f"""
    <div class="{alert_class}">
        <strong>⚠️ Health Alert:</strong> Current AQI levels indicate {current_category.lower()} air quality. 
        {current_description}. Consider limiting outdoor activities and using air purifiers indoors.
    </div>
    """, unsafe_allow_html=True)

# Main Charts
st.markdown("""
<div class="chart-container">
    <div class="chart-title">📈 AQI Temporal Analysis</div>
    <div class="chart-subtitle">
        This chart shows the Air Quality Index trends over time, helping identify pollution patterns, 
        seasonal variations, and long-term air quality changes in Sohna region.
    </div>
</div>
""", unsafe_allow_html=True)

# Enhanced trend chart
daily_avg = df.groupby('Date')['AQI'].agg(['mean', 'min', 'max']).reset_index()
daily_avg['Date'] = pd.to_datetime(daily_avg['Date'])

fig_trend = go.Figure()

# Add confidence band
fig_trend.add_trace(go.Scatter(
    x=daily_avg['Date'], y=daily_avg['max'],
    fill=None, mode='lines', line_color='rgba(0,0,0,0)', showlegend=False
))
fig_trend.add_trace(go.Scatter(
    x=daily_avg['Date'], y=daily_avg['min'],
    fill='tonexty', mode='lines', line_color='rgba(0,0,0,0)',
    name='Daily Range', fillcolor='rgba(66, 153, 225, 0.1)'
))

# Add main trend line
fig_trend.add_trace(go.Scatter(
    x=daily_avg['Date'], y=daily_avg['mean'],
    mode='lines', name='Daily Average AQI',
    line=dict(color='#4299e1', width=3)
))

# Add AQI category lines
for category, info in AQI_CATEGORIES.items():
    if info['range'][1] <= 300:  # Don't show hazardous line
        fig_trend.add_hline(
            y=info['range'][1], line_dash="dash", 
            line_color=info['color'], opacity=0.5,
            annotation_text=category
        )

fig_trend.update_layout(
    template='plotly_dark',
    paper_bgcolor='rgba(0,0,0,0)',
    plot_bgcolor='rgba(0,0,0,0)',
    font=dict(family="Inter", color='#ffffff'),
    height=500,
    showlegend=True,
    hovermode='x unified'
)

st.plotly_chart(fig_trend, use_container_width=True)

# Secondary Charts
col1, col2 = st.columns(2)

with col1:
    st.markdown("""
    <div class="chart-container">
        <div class="chart-title">⏰ Hourly Patterns</div>
        <div class="chart-subtitle">
            Average AQI levels throughout the day. Identifies peak pollution hours 
            and helps plan outdoor activities during cleaner air periods.
        </div>
    </div>
    """, unsafe_allow_html=True)
    
    hourly_data = df.groupby('Hour_Num')['AQI'].mean().reset_index()
    fig_hourly = px.bar(
        hourly_data, x='Hour_Num', y='AQI',
        template='plotly_dark',
        color='AQI',
        color_continuous_scale='RdYlBu_r'
    )
    fig_hourly.update_layout(
        paper_bgcolor='rgba(0,0,0,0)',
        plot_bgcolor='rgba(0,0,0,0)',
        font=dict(family="Inter", color='#ffffff'),
        height=400,
        showlegend=False
    )
    st.plotly_chart(fig_hourly, use_container_width=True)

with col2:
    st.markdown("""
    <div class="chart-container">
        <div class="chart-title">🎯 Health Risk Distribution</div>
        <div class="chart-subtitle">
            Breakdown of air quality categories throughout the monitoring period. 
            This helps understand the frequency of different health risk levels.
        </div>
    </div>
    """, unsafe_allow_html=True)
    
    health_counts = df['Health Risk'].value_counts()
    colors = [AQI_CATEGORIES.get(risk, {'color': '#gray'})['color'] for risk in health_counts.index]
    
    fig_pie = px.pie(
        values=health_counts.values,
        names=health_counts.index,
        color_discrete_sequence=colors,
        template='plotly_dark'
    )
    fig_pie.update_layout(
        paper_bgcolor='rgba(0,0,0,0)',
        font=dict(family="Inter", color='#ffffff'),
        height=400
    )
    st.plotly_chart(fig_pie, use_container_width=True)

# Footer
st.markdown("""
<div class="footer">
    <div style="font-size: 18px; font-weight: 700; color: #4299e1; margin-bottom: 8px;">
        AQI Analytics Platform
    </div>
    <div style="margin-bottom: 16px;">
        Professional Air Quality Intelligence System for Sohna, Haryana
    </div>
    <div style="font-size: 12px; opacity: 0.8;">
        Built with Streamlit • Powered by Advanced Analytics • Data Source: Sohna AQI 2023<br>
        © 2025 - Designed for Environmental Intelligence & Public Health
    </div>
</div>
""", unsafe_allow_html=True)
[V0_FILE]plaintext:file="requirements.txt" isMerged="true"
streamlit
pandas
numpy
plotly
requests
folium
streamlit-folium
scikit-learn
seaborn
pillow
