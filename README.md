# Python-week-8-Assignment

# Importing necessary libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Task 1: Load and Explore the Dataset
try:
    # Load the COVID-19 dataset
    url = 'https://covid.ourworldindata.org/data/owid-covid-data.csv'
    df = pd.read_csv(url)

    # Display the first few rows
    print("First few rows of the dataset:")
    print(df.head())

    # Check the structure of the dataset
    print("\nDataset info:")
    print(df.info())

    # Check for missing values
    print("\nMissing values:")
    print(df.isnull().sum())

    # Clean the dataset by dropping rows with missing values in key columns
    df_clean = df.dropna(subset=['total_cases', 'new_cases', 'total_deaths', 'continent'])

except Exception as e:
    print("An error occurred while loading the dataset:", e)

# Task 2: Basic Data Analysis
print("\nDescriptive statistics for numerical columns:")
print(df_clean.describe())

# Grouping by continent and computing mean total_cases
continent_group = df_clean.groupby('continent')['total_cases'].mean()
print("\nAverage total cases per continent:")
print(continent_group)

# Task 3: Data Visualization
sns.set(style="whitegrid")
plt.figure(figsize=(12, 8))

# Line chart: trend of new cases over time for one country (e.g., India)
india_data = df_clean[df_clean['location'] == 'India']
plt.plot(india_data['date'], india_data['new_cases'], label='India - New Cases')
plt.xticks(rotation=45)
plt.title('Trend of New COVID-19 Cases Over Time in India')
plt.xlabel('Date')
plt.ylabel('New Cases')
plt.legend()
plt.tight_layout()
plt.show()

# Bar chart: average total cases per continent
plt.figure(figsize=(10, 6))
continent_group.plot(kind='bar', color='skyblue')
plt.title('Average Total COVID-19 Cases by Continent')
plt.xlabel('Continent')
plt.ylabel('Average Total Cases')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Histogram: distribution of new cases
plt.figure(figsize=(10, 6))
sns.histplot(df_clean['new_cases'], bins=50, kde=True, color='orange')
plt.title('Distribution of New COVID-19 Cases')
plt.xlabel('New Cases')
plt.ylabel('Frequency')
plt.tight_layout()
plt.show()

# Scatter plot: total cases vs total deaths
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df_clean, x='total_cases', y='total_deaths', hue='continent', alpha=0.6)
plt.title('Total Cases vs Total Deaths by Continent')
plt.xlabel('Total Cases')
plt.ylabel('Total Deaths')
plt.tight_layout()
plt.show()
