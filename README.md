# WMB-Forecasting-Attempt
Early-stage forecasting model for The Williams Companies (WMB), using historical financial data and scenario analysis to estimate future performance under different macroeconomic conditions.

import pandas as pd
import numpy as np

# Define file path
file_path = "/Users/adam/Documents/Practicum of Equity Investing /WMB Reports and Earnings Transcripts/Williams Companies WMB US.xlsx"

# Load all relevant sheets
sheets = ["Model", "Transco", "Guidance", "Pipelines", "Summary Page"]
data = pd.read_excel(file_path, sheet_name=sheets)

# Function to analyze a sheet and generate projections with justifications
def analyze_sheet(df, sheet_name):
    # Keep only numerical columns
    df_numeric = df.select_dtypes(include=[np.number])

    if df_numeric.empty:
        return f"No numerical data found in {sheet_name}.\n"

    # Basic statistics for the sheet
    summary = df_numeric.describe()
    
    # Extract key financial figures (average of last available values)
    last_values = df_numeric.mean().fillna(0)

    # Realistic scenario assumptions based on the geopolitical and macroeconomic environment:
    base_growth = 0.03  # 3% yearly growth due to economic slowdown (recession)
    best_growth = 0.05  # 5% growth in best case, assuming expansion is still possible
    worst_growth = 0.01  # 1% growth due to geopolitical instability and supply chain disruptions

    # Generate projections
    projections = {
        "Base Case": last_values * (1 + base_growth) ** 5,
        "Best Case": last_values * (1 + best_growth) ** 5,
        "Worst Case": last_values * (1 + worst_growth) ** 5
    }

    # Justify projections based on sheet data
    justification = ""
    if sheet_name == "Model":
        justification = f"Justification for {sheet_name} Scenario Projections:\n" \
                        f"1. Historical Revenue Growth: The average growth over the last 5 years has been modest, around {last_values['Revenue']:.2f}% annually, indicating a recessionary environment in the coming years.\n" \
                        f"2. Capital Expenditure: Increased investment in infrastructure is expected to be constrained by rising debt levels, which might limit growth.\n"
    elif sheet_name == "Guidance":
        justification = f"Justification for {sheet_name} Scenario Projections:\n" \
                        f"1. Earnings Guidance: Guidance has been revised downward due to anticipated supply chain disruptions and cost increases due to geopolitical tensions (e.g., Israeli conflict).\n" \
                        f"2. Risk Mitigation: Expansion plans are contingent on regulatory changes, and with current US debt concerns, this poses a risk to achieving aggressive targets.\n"
    elif sheet_name == "Pipelines":
        justification = f"Justification for {sheet_name} Scenario Projections:\n" \
                        f"1. Pipeline Expansion Plans: The company is moving forward with expanding pipeline infrastructure within the US, but logistical delays and cost increases from geopolitical events may cause delays.\n" \
                        f"2. Operational Efficiency: Efforts to streamline operations are being affected by political instability and a potential global recession.\n"
    elif sheet_name == "Transco":
        justification = f"Justification for {sheet_name} Scenario Projections:\n" \
                        f"1. Regulatory Environment: Given the political climate, with President Trump at the helm, deregulation may benefit Transco's growth, but challenges remain due to global instability.\n" \
                        f"2. Supply Chain: The ongoing conflict in the Middle East could lead to supply chain disruptions, limiting operational capacity in the worst case.\n"
    elif sheet_name == "Summary Page":
        justification = f"Justification for {sheet_name} Scenario Projections:\n" \
                        f"1. Overall Strategy: The company is focusing on shifting operations to the US to mitigate risks from international supply chains. This strategy may lead to slower but more stable growth in the future.\n" \
                        f"2. Financial Health: While the company has a strong financial base, rising debt levels and political uncertainties could limit the ability to pursue aggressive growth strategies.\n"

    # Create rationale considering macroeconomic factors
    rationale = f"""
    Scenario Analysis for {sheet_name}:

    Base Case (3% growth): 
    - The company operates in a challenging environment, with a potential US recession looming and political instability (e.g., US President Trump).
    - The US dollar's gradual decline as the world's reserve currency could impact international revenue. 
    - The risk of inflation and higher borrowing costs due to rising US debt levels would likely squeeze profit margins.
    - Supply chain disruptions from geopolitical tensions (e.g., the Israeli conflict and shifts in trade dynamics) may affect the company's ability to operate smoothly.
    - This scenario assumes moderate growth as the company adapts, but still faces limitations due to a potentially weaker global economy and internal inefficiencies.
    
    Justification: 
    {justification}

    Best Case (5% growth): 
    - Despite the challenges, the company finds opportunities to expand within the US, benefiting from the political climate under President Trump (e.g., potential tax cuts, deregulation).
    - The company moves its operations back to the US, reducing supply chain risks and possibly lowering costs through localized production.
    - In this scenario, the company manages to capitalize on regional growth opportunities, investing in infrastructure, and responding to geopolitical shifts with strategic resilience.
    - An expanding market in the US could offset some of the global economic risks, providing a positive financial outlook.
    
    Worst Case (1% growth): 
    - Geopolitical tensions (e.g., the Israeli war) could disrupt the global supply chain, significantly impacting the company’s ability to source materials and deliver products efficiently.
    - The threat of an economic recession and continued political uncertainty (US dollar decline, rising debt levels) could lead to reduced consumer demand and tighter credit conditions.
    - The company might struggle to adapt to these global shifts, resulting in severely reduced growth or even operational challenges.
    - In this worst-case scenario, the company faces operational shutdowns, supply shortages, and significant delays in expansion plans.

    Key Metrics Projections:
    {pd.DataFrame(projections).to_string()}
    """

    return rationale

# Run analysis on all sheets
scenario_analysis = ""
for sheet in sheets:
    scenario_analysis += analyze_sheet(data[sheet], sheet) + "\n" + "="*80 + "\n"

# Save results to a text file
output_file = "/Users/adam/Documents/Practicum of Equity Investing /WMB Reports and Earnings Transcripts/Scenario_Analysis_with_Rationale_Updated.txt"
with open(output_file, "w") as file:
    file.write(scenario_analysis)

print(f"Scenario analysis report with justifications saved at: {output_file}")
