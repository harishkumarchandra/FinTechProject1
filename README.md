# **The Value of Time**

## **Background**

I came to this country in 2015. I started my carrer in manufacturing field as CNC programmer. I worked very hard get paid 20$/hour. I had to compete with Manufactures in China who can do my work for lesser pay. I switched my carrer to Information Technology and started getting paid 45$/hour for same amount work I used to work in manufacturing field. The Time I spend didn't change but value of Time did change. This raised a question how Time is valued differently in different countries. To answer this question we have to make two assumptions. First is common currency accepted gloablly, for this we choose Gold. Second is global hourly wage , for this we choose minimum wage in each country.

## **Data Cleaning and Preparation**

### **Data Source**

- We obtained the Price of Gold for past 20 years using the [Philadelphia Gold and Silver Index INDEXNASDAQ: XAU](https://www.investing.com/currencies/xau-usd-historical-data)
- We obtained past 20 year hourly minimum wage for 32 countries from [Organisation of Economics Co-operation and Development](https://stats.oecd.org/Index.aspx?DataSetCode=RMW#)

### **Data Cleaning**

#### **Gold**
Drop unnecessary columns
'''gold_price_df.drop(columns=['Open','High','Low','Change %'],inplace=True)'''
Data Conversion From String to Float
'''gold_price_df['Price'] = gold_price_df['Price'].str.replace(',','')
gold_price_df['Price'] = gold_price_df['Price'].astype(float)'''

#### **Wage**

The Gold Standard

Currency has played a major role in society for thousands of years. Having an established currency was the mark of humanity's first step towards "functioning modern society". Throughout history, one precious metal has stood as an inherit symbol of monetary value and that metal is none other than gold. 

Around 700 B.C., Lydians made gold into coins, which was the first time in history, or at least the first recorded instance, a society established a standardized currency system for exchange. Gold was always desired and valuable prior to this point, but this act formally solidified its power to function as a monetary unit. 

As more societies emerged around the world, more currencies came into being. Utilizing improved modes of transportation, countries began to engage in commerce with each other, and a global economic framework eventually took shape. 

Countries all established different currencies, yet all mutually recognized gold as a valuable commodity. To facilitate trading, many countries adopted the gold standard or simply settled traded disputes with gold, since all countries had a common conception of gold's value. At the end of the 1800's, a formally written and established internationally gold standard was agreed upon by the majority of developed nations.

Although the gold standard could not survive as a long term monetary system, gold still exists as a physical commodity of value that most developed nations collectively recognize and purchase regularly. 

Gold's significance in the history of commerce and the maintained international understanding makes it the perfect benchmark to compare the buying power of different currencies. Our project aims to draw insights from the buying power of different currencies using gold as the standard to determine each of their value. 

- How many hours should a person in each country work to 
        buy 1 troy oz of gold?
- Drawing living standard of each country insights


