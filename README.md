# **The Value of Time**

## **Background**

I came to this country in 2015. I started my carrer in manufacturing field as CNC programmer. I worked very hard get paid 20$/hour. I had to compete with Manufactures in China who can do my work for lesser pay. I switched my carrer to Information Technology and started getting paid 45$/hour for same amount work I used to work in manufacturing field. The Time I spend didn't change but value of Time did change. This raised a question how Time is valued differently in different countries. To answer this question we have to make two assumptions. First is common currency accepted gloablly, for this we choose Gold. Second is global hourly wage , for this we choose minimum wage in each country.

### **Data Source**

- We obtained the Price of Gold for past 20 years using the [Philadelphia Gold and Silver Index INDEXNASDAQ: XAU](https://www.investing.com/currencies/xau-usd-historical-data)
- We obtained past 20 year hourly minimum wage for 32 countries from [Organisation of Economics Co-operation and Development](https://stats.oecd.org/Index.aspx?DataSetCode=RMW#)

### **Data Cleaning and Preparation**

#### **Gold**
Drop unnecessary columns
```python
gold_price_df.drop(columns=['Open','High','Low','Change %'],inplace=True)
```
Data Conversion From String to Float
```python
gold_price_df['Price'] = gold_price_df['Price'].str.replace(',','')
gold_price_df['Price'] = gold_price_df['Price'].astype(float)
```
Get Annual Average Gold Price
```python
avg_gold_price_df = gold_price_year_df.groupby('Date').mean()
```

#### **Wage**
Drop na from dataframe
```python
minimum_wage_df.dropna(inplace=True)
```
Rename to Year and set Index
```python
minimum_wage_df.rename(columns={'Unnamed: 0':'Year'},inplace=True)
minimum_wage_df.index = minimum_wage_df.Year
```
Replace ".." for Germany & Japan with its mean
```python
minimum_wage_df['Germany'].replace('..',minimum_wage_df.loc[2015:2019,'Germany'].astype(float).mean(),inplace=True)
minimum_wage_df['Japan'].replace('..',minimum_wage_df.loc[2001:2018,'Japan'].astype(float).mean(),inplace=True)
```
Convert String to float
```python
for column in minimum_wage_df.columns:    
    minimum_wage_df[column] = minimum_wage_df[column].astype(float)
```    

### **Calculation**

#### **Buying Power**
Number of Work Hour to Earn 1oz Gold = Gold Price / Minimun Hourly Wage
```python
work_hours = pd.concat([avg_gold_price_df,minimum_wage_df], axis=1)
for Country in work_hours.columns:
    if Country != 'Price':
        work_hours[Country] = work_hours['Price'] / work_hours[Country]
```

#### **Monte Carlo Simulation**
Run 500 simulations of Gold Price in 5 years
```python
from MCForecastTools import MCSimulation

num_sims = 500
MC_GOLD = MCSimulation(
    portfolio_data = gold_df,
    num_simulation = num_sims,
    weights = [1],
    num_trading_days = 252 * 5
)

MC_GOLD.calc_cumulative_return()
line_plot = MC_GOLD.plot_simulation()
tbl = MC_GOLD.summarize_cumulative_return()
```

### **Plots**

![Gold_Price](/Resources/Images/Gold_Price.png)
![Hourly_Minimum_Wage_from_2000-2019](/Resources/Images/Hourly_Minimum_Wage_from_2000-2019.png)
![Hourly_Minimum_Wage_around_the_World_2000-2019](/Resources/Images/Hourly_Minimum_Wage_around_the_World_2000-2019.png)
![Number_of_Hours_to_Purchase_1_oz_of_Gold](/Resources/Images/Number_of_Hours_to_Purchase_1_oz_of_Gold.png)
![Number_of_Hours_to_Purchase_1_oz_o_Gold_in_2019](/Resources/Images/Number_of_Hours_to_Purchase_1_oz_o_Gold_in_2019.png)

![Top_5_countries_with_highest minimum_wage](/Resources/Images/Top_5_countries_with_highest_minimum_wage.png)

![MCSimulation_Gold](/Resources/Images/MCSimulation_Gold.png)

### **Conclusion**

- Plot **GOLD Prices from 2000 to 2019** shows that there is an up trend in Gold price. So we can conclude that the number of hours need to work to earn gold will also increase.

- Plot ***Hourly Minimum Wage from 2000 to 2019** shows that minimum wage has increased in past years. Howerver in comparaion to Gold trend plot the minimum wage growth is less. Therefore it will be diffcult to earn gold in futrue if the trend continues.

- Plot **Hourly Minimum Wage around the World for 2019** shows different hourly minimum wage in different countires. This infers that number of hours need to work to earn gold will differs vastly 
based on the country.

- Plot **Number of Hours to Purchase 1 oz of Gold** shows each country's gold buying capacity. In 2019 with Mexico's minimum wage of 1.2$ it will take an Mexican 1160 hours to earn 1 oz of Gold. In US with minimum wage of 7.3$ it will take an American 190 hours to earn 1 oz of Gold which is 10 times lesser than Mexico. 

- Plot **Number of Hours to Purchase 1 oz of Gold in 2019** shows a world map of
buying power distribtion. The bigger the circle the higher the number of hours need to work to earn gold. All cricle in neighbouring countries in Europe grows smoothly. Whereas the circle between US and Mexico has a huge jump in size which may lies as the root cause of border issues between countries.

- Plot **Top 5 countries with highest minimum wage** show Australia, Belgium, France, Luxembourg, Netherlands as Top countries with minimum wage in 2019. USA is not in the top 5. In 2001 all 5 counties started around 10.8 However Australia moved to the top in 2019 with 2019. An Australian needs to work only 110 hours to earn 1 oz of gold.

- Plot **MCSimulation_Gold** show 500 simulation of Gold price in next 5 years. 1 oz of Gold roughly cost 1500$. 1500$ invested in Gold now has 95% chance that it will turn into 2007.14$ and there is a 5% chance it will turn into 461.81$






