import pandas as pd

def combine_excel_sheets(excel_path):
    """
    Combines all sheets from an Excel file into a single DataFrame with a new column
    indicating the sheet source.

    Args:
        excel_path (str): Path to the Excel file.

    Returns:
        pandas.DataFrame: The combined DataFrame.
    """

    df_list = []
    for sheet_name in pd.ExcelFile(excel_path).sheet_names:
        df = pd.read_excel(excel_path, sheet_name=sheet_name)
        df['Sheet_Source'] = sheet_name  # Add a new column with sheet name
        df_list.append(df)

    combined_df = pd.concat(df_list, ignore_index=True)  # Combine DataFrames
    return combined_df

# Example usage
excel_path = "your_excel_file.xlsx"
combined_df = combine_excel_sheets(excel_path)

print(combined_df)
