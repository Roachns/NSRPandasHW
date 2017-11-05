import pandas as pd
import numpy as np

purchase_data1 = "purchase_data.json"
purchase_data2 = "purchase_data2.json"

data1 = pd.read_json(purchase_data1)
data1.head()

data2= pd.read_json(purchase_data2)
data2.head()

merged_data = pd.merge(data1, data2, how="outer")
merged_data.dtypes

merged_data['Price'] = merged_data.Price.astype(int)
merged_data.dtypes

player_count = set(merged_data['SN'])
player_count = len(player_count)
frame_df = pd.DataFrame({"Player Count":[player_count]})
frame_df

items = set(merged_data['Item ID'])
items = len(items)

avg_price = round(merged_data["Price"].mean(),2)
tot_purchases =  len(merged_data["Price"])
tot_revenue = round(merged_data["Price"].sum(),2)

frame2_df= pd.DataFrame({"Number of Unique Items":[items], "Average Price": '$' + str(avg_price), "Number of Purchases": [tot_purchases], "Total Revenue": '$' + str(tot_revenue)})
frame2_df

total_gender = merged_data["Gender"].count()
male = merged_data["Gender"].value_counts()['Male']
female = merged_data["Gender"].value_counts()['Female']
non_gender_specific = total_gender - male - female

male_percent = (male/total_gender) * 100
female_percent = (female/total_gender) * 100
non_gender_specific_percent = (non_gender_specific/total_gender) * 100


male_percent = str(round(male_percent, 2)) + '%'
female_percent = str(round(female_percent, 2)) + '%'
non_gender_specific_percent = str(round(non_gender_specific_percent, 2)) + '%'

frame3_df = pd.DataFrame({
    "Gender":["Male", "Female", "Other / Non-Disclosed"], 
    "Total Count": [male, female, non_gender_specific], 
    "Percent of Players": [male_percent, female_percent, non_gender_specific_percent]
    })
frame3_df
gender_numbers = frame3_df.set_index('Gender')
gender_numbers

df = merged_data.groupby('Gender').Price.agg(['count', 'mean', 'sum'])
df.columns = ['Total Purchases','Average Price', "Total Purchases"]
df['Average Price'] = '$' + (round(df['Average Price'], 2).astype(str))
df


hp_bins = [ 0, 9, 14, 19, 24, 29, 34, 39, 44, 49]
hp_labels = ["0-9", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-50"]

bin_df = pd.cut(merged_data["Age"], hp_bins, labels=hp_labels)

merged_data["Age Group"] = pd.cut(merged_data["Age"], hp_bins, labels=hp_labels)
merged_data.head()

df = merged_data.groupby('Age Group').Price.agg(['count', 'mean', 'sum'])
df.columns = ['Total Purchases','Average Price', "Total Purchases"]
df['Average Price'] = '$' + (round(df['Average Price'], 2).astype(str))
df

df = merged_data.groupby('SN').Price.agg(['count', 'mean', 'sum'])
df.columns = ['Total Purchases','Average Price', "Total Purchase Value"]
df = df.sort_values('Total Purchase Value', ascending = False)
df['Average Price'] = '$' + (round(df['Average Price'], 2).astype(str))
df = df.iloc[:5]
df

df = merged_data.groupby(['Item ID', 'Item Name']).Price.agg(['count', 'mean', 'sum'])
df.columns = ['Total Purchases','Average Price', "Total Purchase Value"]
df = df.sort_values('Total Purchase Value', ascending = False)
df['Average Price'] = '$' + (round(df['Average Price'], 2).astype(str))
df = df.iloc[:5]
df



