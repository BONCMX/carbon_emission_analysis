# carbon_emission_analysis

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
![image](https://github.com/user-attachments/assets/0bc20244-73fb-4ebf-b982-6dec376b07e1)

1.1 Data Model

1.2 Data Structure
Table 'product_emissions'


##2.Data Explore
dem bao nhiu sp, distinct sp

##3.Data Analysis
1. Which products contribute the most to carbon emissions?
```sql
SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC;
```
The Result:
|product_name|Average PCF|
|------------|-----------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|TCDE|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Mercedes-Benz S-Class (S 500)|85000.00|
|Mercedes-Benz SL (SL 350)|72000.00|

2.What are the industry groups of these products?
```sql
SELECT product_emissions.industry_group_id,
	   industry_groups.industry_group,
	   product_emissions.product_name,
	   ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS 'Average PCF'
FROM product_emissions
JOIN industry_groups
ON product_emissions.industry_group_id = industry_groups.id
GROUP BY product_name
ORDER BY AVG(carbon_footprint_pcf) DESC
LIMIT 10;
```
The Result:
|industry_group_id|industry_group|product_name|Average PCF|
|-----------------|--------------|------------|-----------|
|13|Electrical Equipment and Machinery|Wind Turbine G128 5 Megawats|3718044.00|
|13|Electrical Equipment and Machinery|Wind Turbine G132 5 Megawats|3276187.00|
|13|Electrical Equipment and Machinery|Wind Turbine G114 2 Megawats|1532608.00|
|13|Electrical Equipment and Machinery|Wind Turbine G90 2 Megawats|1251625.00|
|7|Automobiles & Components|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|19|Materials|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|19|Materials|TCDE|99075.00|
|7|Automobiles & Components|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|7|Automobiles & Components|Mercedes-Benz S-Class (S 500)|85000.00|
|7|Automobiles & Components|Mercedes-Benz SL (SL 350)|72000.00|
