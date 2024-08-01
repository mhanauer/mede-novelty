import streamlit as st
import pandas as pd
import plotly.graph_objs as go
import plotly.express as px

# Sample DataFrame with chronic_condition and value_type
data = {
    'date_value': ['202301', '202302', '202303', '202304', '202305', '202306', '202307', '202308', '202309', '202310', 
                   '202301', '202302', '202303', '202304', '202305', '202306', '202307', '202308', '202309', '202310'],
    'value': [100, 150, 200, 250, 800, 300, 350, 400, 450, 1200, 200, 250, 300, 350, 900, 250, 300, 350, 400, 1000],
    'chronic_condition': ['Diabetes', 'Diabetes', 'Diabetes', 'Diabetes', 'Diabetes',
                          'Diabetes', 'Diabetes', 'Diabetes', 'Diabetes', 'Diabetes',
                          'Hypertension', 'Hypertension', 'Hypertension', 'Hypertension', 'Hypertension',
                          'Hypertension', 'Hypertension', 'Hypertension', 'Hypertension', 'Hypertension'],
    'value_type': ['Allowed Amount', 'Allowed Amount', 'Allowed Amount', 'Allowed Amount', 'Allowed Amount',
                   'Total Claims', 'Total Claims', 'Total Claims', 'Total Claims', 'Total Claims',
                   'Allowed Amount', 'Allowed Amount', 'Allowed Amount', 'Allowed Amount', 'Allowed Amount',
                   'Total Claims', 'Total Claims', 'Total Claims', 'Total Claims', 'Total Claims'],
    'novelty': [0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1]
}
df = pd.DataFrame(data)

# Convert date_value to datetime
df['date_value'] = pd.to_datetime(df['date_value'], format='%Y%m')

# Streamlit app
st.title('Value Time Series Visualization')

# Selectors for chronic_condition and value_type
selected_chronic_condition = st.selectbox('Select Chronic Condition', df['chronic_condition'].unique())
selected_value_type = st.selectbox('Select Value Type', df['value_type'].unique())

# Filter DataFrame based on selections
filtered_df = df[(df['chronic_condition'] == selected_chronic_condition) & (df['value_type'] == selected_value_type)]

# Determine value format
if selected_value_type == 'Allowed Amount':
    filtered_df.loc[:, 'formatted_value'] = filtered_df['value'].apply(lambda x: f"${x:,.2f}")
elif selected_value_type == 'Total Claims':
    filtered_df.loc[:, 'formatted_value'] = filtered_df['value'].apply(lambda x: f"{x:,}")

# Plot the data with Plotly
fig = go.Figure()

# Add the line for values
fig.add_trace(go.Scatter(
    x=filtered_df['date_value'], 
    y=filtered_df['value'], 
    mode='lines+markers', 
    name='Value',
    text=filtered_df['formatted_value'],  # Use text for formatted values
    hovertemplate='%{text}'  # Use the text field for hovertemplate
))

# Highlight novelty points
novelty_points = filtered_df[filtered_df['novelty'] == 1]
fig.add_trace(go.Scatter(
    x=novelty_points['date_value'], 
    y=novelty_points['value'], 
    mode='markers', 
    marker=dict(color='red', size=10), 
    name='Novelty',
    text=novelty_points['formatted_value'],  # Use text for formatted values
    hovertemplate='%{text}'  # Use the text field for hovertemplate
))

# Add labels and title
fig.update_layout(
    title='Value Over Time',
    xaxis_title='Date',
    yaxis_title='Value',
    hovermode='x unified'
)

# Display the plot in Streamlit
st.plotly_chart(fig)
