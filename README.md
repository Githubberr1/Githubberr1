import pandas as pd


df = pd.DataFrame({'date': ['2023-06-01', '2023-07-02', '2024-05-30', '2024-06-10', '2024-07-15']})
# Convert the date column to datetime
df['date'] = pd.to_datetime(df['date'])
today = pd.to_datetime('today').date()

def filter_data(df, today):
  if today.isocalendar()[1] == 1:
    return df
  else:
    return df.loc[df['date'].dt.month == today.month]

result = filter_data(df.copy(), today)
print(result)
