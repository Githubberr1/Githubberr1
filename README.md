from pptx import Presentation
import pandas as pd

def update_pptx_table(df, presentation_path, slide_idx=0, table_idx=0):
  """
  Updates data in a table within an existing PowerPoint presentation.

  Args:
      df (pandas.DataFrame): The DataFrame containing the data to be updated.
      presentation_path (str): The path to the PowerPoint presentation.
      slide_idx (int, optional): The index of the slide containing the table (0-based). Defaults to 0 (first slide).
      table_idx (int, optional): The index of the table to be updated (0-based). Defaults to 0 (first table).
  """

  prs = Presentation(presentation_path)  # Open the presentation

  # Get the specified slide
  slide = prs.slides[slide_idx]

  try:
    # Access the specified table (handle potential IndexError)
    table = slide.shapes[table_idx].table

    # Clear existing table data (optional)
    for row in range(1, table.rows.count):  # Skip header row
      for col in range(table.columns.count):
        table.cell(row, col).text = ''

    # Ensure table dimensions match DataFrame (adjust as needed)
    if table.rows.count - 1 != len(df) or table.columns.count != len(df.columns):
      raise ValueError("Table dimensions don't match DataFrame. Adjust table size in the presentation.")

    # Write column headers from DataFrame
    for col_idx, col_name in enumerate(df.columns):
      table.cell(0, col_idx + 1).text = col_name

    # Write DataFrame data into cells
    for row_idx, row in df.iterrows():
      for col_idx, value in enumerate(row):
        table.cell(row_idx + 1, col_idx + 1).text = str(value)

    # Save the presentation
    prs.save(presentation_path)
    print(f"Table on slide {slide_idx+1} (index {slide_idx}) updated successfully.")

  except IndexError:
    print(f"Error: Could not find table at index {table_idx} on slide {slide_idx+1} (index {slide_idx}).")

# Example usage (assuming your DataFrame is named 'data')
update_pptx_table(data.copy(), 'data_presentation.pptx', slide_idx=1, table_idx=0)  # Copy DataFrame to avoid modifying original
