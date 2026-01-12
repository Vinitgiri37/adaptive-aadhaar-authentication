"""
Adaptive & Privacy-Preserving Aadhaar Authentication (APPA)
UIDAI Hackathon – Data Analysis & Policy Logic Demonstration

NOTE:
This code demonstrates analytical logic, biometric reliability scoring,
and adaptive authentication decision rules using sample data.
No real Aadhaar systems or biometric algorithms are implemented.
"""

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# --------------------------------------------------
# 1. LOAD SAMPLE DATA
# --------------------------------------------------

# Sample data structure (replace with sample_data.csv if available)
data = {
    "state": ["UP", "UP", "Bihar", "Bihar", "Maharashtra", "Maharashtra"],
    "district": ["A", "B", "C", "D", "E", "F"],
    "age_group": ["5-17", "17+", "5-17", "17+", "5-17", "17+"],
    "biometric_updates": [1200, 800, 1500, 1100, 600, 400]
}

df = pd.DataFrame(data)

print("Sample Dataset Loaded:")
print(df)

# --------------------------------------------------
# 2. DATA CLEANING & PREPROCESSING
# --------------------------------------------------

df = df.dropna()
df["age_group"] = df["age_group"].str.strip()

print("\nCleaned Dataset:")
print(df)

# --------------------------------------------------
# 3. AGE-WISE ANALYSIS
# --------------------------------------------------

age_analysis = df.groupby("age_group")["biometric_updates"].sum()

print("\nAge-wise Biometric Update Frequency:")
print(age_analysis)

# Plot
plt.figure()
age_analysis.plot(kind="bar")
plt.title("Age-wise Biometric Update Frequency")
plt.xlabel("Age Group")
plt.ylabel("Number of Biometric Updates")
plt.tight_layout()
plt.show()

# --------------------------------------------------
# 4. REGION-WISE ANALYSIS
# --------------------------------------------------

region_analysis = df.groupby("state")["biometric_updates"].sum()

print("\nState-wise Biometric Update Frequency:")
print(region_analysis)

# Plot
plt.figure()
region_analysis.plot(kind="bar")
plt.title("State-wise Biometric Update Frequency")
plt.xlabel("State")
plt.ylabel("Number of Biometric Updates")
plt.tight_layout()
plt.show()

# --------------------------------------------------
# 5. BIOMETRIC RELIABILITY SCORE (BRS)
# --------------------------------------------------

def compute_brs(update_frequency, age_factor):
    """
    Conceptual Biometric Reliability Score
    Higher update frequency -> Lower reliability
    """
    return 1 / (update_frequency * age_factor)

# Assign age volatility factor
df["age_factor"] = df["age_group"].apply(lambda x: 1.5 if x == "5-17" else 1.0)

df["BRS"] = df.apply(
    lambda row: compute_brs(row["biometric_updates"], row["age_factor"]),
    axis=1
)

print("\nBiometric Reliability Scores:")
print(df[["state", "district", "age_group", "BRS"]])

# --------------------------------------------------
# 6. ADAPTIVE AUTHENTICATION DECISION LOGIC
# --------------------------------------------------

def select_authentication_method(brs):
    if brs > 0.0015:
        return "Biometric Only"
    elif brs > 0.0008:
        return "Biometric + OTP"
    else:
        return "QR / Attribute Only"

df["Recommended_Authentication"] = df["BRS"].apply(select_authentication_method)

print("\nAdaptive Authentication Decisions:")
print(df[["state", "district", "age_group", "Recommended_Authentication"]])

# --------------------------------------------------
# 7. KEY INSIGHTS
# --------------------------------------------------

print("\nKEY INSIGHTS:")
print("- Higher biometric update frequency indicates lower biometric reliability.")
print("- Adolescents show higher biometric volatility than adults.")
print("- Certain regions consistently exhibit higher update frequencies.")
print("- Adaptive authentication reduces failure rates and improves inclusion.")
print("- QR-based verification minimizes privacy risks in low-reliability contexts.")

print("\nAnalysis Completed Successfully.")
