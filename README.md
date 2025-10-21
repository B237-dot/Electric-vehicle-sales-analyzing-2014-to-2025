# Electric-vehicle-sales-analyzing-2014-to-2025
india-ev-data-story/
│
├── README.md
├── requirements.txt
├── ev_analysis.py
├── data/
│   └── EV_India.csv   # Place your dataset here
├── visuals/
│   ├── ev_growth.png
│   ├── top_states.png
│   └── ev_type_distribution.png
└── notebooks/
    └── ev_analysis.ipynb
pandas
matplotlib
seaborn
jupyter
openpyxl
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
df = pd.read_csv('data/EV_India.csv')

# Clean data
df['State'] = df['State'].str.strip().str.title()
df['Year'] = pd.to_datetime(df['Registration_Date']).dt.year
df = df.dropna(subset=['Vehicle_Type', 'State'])

# EV growth over years
ev_trend = df.groupby('Year')['Vehicle_ID'].count().reset_index()
ev_trend.rename(columns={'Vehicle_ID': 'Total_Registrations'}, inplace=True)

# Plot EV growth
plt.figure(figsize=(8,5))
sns.lineplot(data=ev_trend, x='Year', y='Total_Registrations', marker='o')
plt.title('EV Registrations Growth in India (2019–2024)')
plt.xlabel('Year')
plt.ylabel('Number of EVs')
plt.tight_layout()
plt.savefig('visuals/ev_growth.png')
plt.show()

# Top 5 states by EV adoption
top_states = df.groupby('State')['Vehicle_ID'].count().reset_index()
top_states = top_states.sort_values(by='Vehicle_ID', ascending=False).head(5)

plt.figure(figsize=(8,5))
sns.barplot(data=top_states, x='Vehicle_ID', y='State')
plt.title('Top 5 States for EV Adoption')
plt.xlabel('Total Registrations')
plt.ylabel('State')
plt.tight_layout()
plt.savefig('visuals/top_states.png')
plt.show()

# EV type distribution
type_share = df['Vehicle_Type'].value_counts()
plt.figure(figsize=(6,6))
type_share.plot(kind='pie', autopct='%1.1f%%', title='EV Type Distribution')
plt.ylabel('')
plt.tight_layout()
plt.savefig('visuals/ev_type_distribution.png')
plt.show()


git init
git add .
git commit -m "Initial commit - EV data storytelling project"
git branch -M main
git remote add origin https://github.com/Sushmitha/india-ev-data-story.git
git push -u origin main
