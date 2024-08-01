import streamlit as st
import pandas as pd
import plotly.graph_objs as go
import plotly.express as px

# Sample DataFrame
data = {
    'date_value': ['202301', '202302', '202303', '202304', '202305'],
    'value': [100, 150, 200, 250, 300],
    'group_key': ['A_allowedamount', 'A_allowedamount', 'A_allowedamount', 'A_allowedamount', 'A_allowedamount'],
    'novelty': [0, 0, 0, 0, 1]
}
df = pd.DataFrame(data)

# Convert date_value to datetime
df['date_value'] = pd.to_datetime(df['date_value'], format='%Y%m')

# Streamlit app
st.title('Value Time Series Visualization')

# Selector for group_key
selected_group_key = st.selectbox('Select Group Key', df['group_key'].unique())

# Filter DataFrame based on selection
filtered_df = df[df['group_key'] == selected_group_key]

# Determine value format
if '_allowedamount' in selected_group_key:
    filtered_df.loc[:, 'formatted_value'] = filtered_df['value'].apply(lambda x: f"${x:,.2f}")
elif '_totalclaims' in selected_group_key:
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