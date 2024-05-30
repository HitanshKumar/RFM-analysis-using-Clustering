
# RFM analysis using Clustering🧐

RFM stands for:  
➡️Recency  
➡️Frequency  
➡️Monetary    

This data of an organisation can be used to determine the customer segmentation and do determine the most valuable customers for the frim


## Deployment🔨

Following are the imp code snipits⬇️

1️⃣ This code is imp for making the RFM data⤵️
```bash
day = '2012-01-01'
day = pd.to_datetime(day)
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])

recency = df.groupby(['CustomerID']).agg({"InvoiceDate": lambda x:((day - x.max()).days)})
```
2️⃣ Usage of standard scalar library⤵️
```bash
  from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaled = scaler.fit_transform(RFM)
# here we standardized the mean also known as z-score normalization, of all the 3 features
# here we ensure the mean = 0 and stdev = 1, so that each features contribute equally to the dataset/analysis
```
3️⃣ Trying to find the inertia/errors of different clusters to figure best n for cluster⤵️
```bash
  from sklearn.cluster import KMeans

inertia = []

for i in np.arange(1,11):
    kmeans = KMeans(n_clusters = i)
    kmeans.fit(scaled)
    inertia.append(kmeans.inertia_) 
```
4️⃣ Plotting the elbow curve to find the elbow point to fidn best number of clusters⤵️
```bash
plt.plot(inertia,marker='o',color = 'r')
plt.xlabel('clusters')
plt.ylabel('intera_values')
plt.title('Elbow method')
plt.grid(True)
plt.show()
# analyzing the below plot one could somewhat say that a reasonable elbow point or best number
# of clusters is 2 or 3
```
5️⃣ Assigning different groups or clusters⤵️
```bash
def func(row):
    if row['clusters'] == 1:
        return 'Alpha'
    elif row['clusters'] == 3:
        return 'Beta'
    else :
        return 'Gamma'
# assigning different groups    

```
6️⃣ Grouping RFM data into alpha beta and gamma or assigning each customerID a different group and plotting the clusters⤵️
```bash
import matplotlib.pyplot as plt

# Define colors and markers for each cluster
cluster_colors = {'Alpha': 'gold', 'Beta': 'skyblue', 'Gamma': 'lightgreen'}
cluster_markers = {'Alpha': '*', 'Beta': 'o', 'Gamma': 's' }

# Plot the clusters
plt.figure(figsize=(10, 6))

for cluster, color in cluster_colors.items():
    plt.scatter(RFM.loc[RFM['group'] == cluster, 'Recency'], 
                RFM.loc[RFM['group'] == cluster, 'Frequency'],
                label=cluster,
                color=color,
                marker=cluster_markers[cluster],
                s=200)  # Adjust size for markers

plt.xlabel('Recency')
plt.ylabel('Frequency')
plt.title('RFM Clusters')
plt.legend()
plt.grid(True)
plt.show()
```
## Color reference for clusters

| Clusters         | Colors                                                   |
| ----------------- | ------------------------------------------------------------------ |
| Alpha | 🟡 |
| Beta | 🔵 |
| Gamma | 🟢 |



## Conclusion🎯

- The RFM data analysis main purpose is to identify or perhaps sort out the customers of a business. For the marketing team to better understand their customer type and what customer brings out the most monetary value for the business.

- Here in our data we can see that no specific conclusion can be drawn from the Recency of a customer but the frequency does seem to have a significant relationship with the monetary value of the customer that is there is a direct realtion between the two.

