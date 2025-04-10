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

3. What are the industries with the highest contribution to carbon emissions?
```sql
SELECT industry_groups.industry_group,
ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_PCF
FROM industry_groups
JOIN product_emissions
ON industry_groups.id = product_emissions.industry_group_id
GROUP BY industry_groups.industry_group
ORDER BY ROUND(SUM(product_emissions.carbon_footprint_pcf),2) DESC
LIMIT 10;
```
The Result:
|industry_group|total_PCF|
|--------------|---------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

4. What are the companies with the highest contribution to carbon emissions
```sql
SELECT companies.company_name,
	ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_PCF
FROM companies
JOIN product_emissions
ON companies.id = product_emissions.industry_group_id
GROUP BY companies.company_name
ORDER BY ROUND(SUM(product_emissions.carbon_footprint_pcf),2) DESC
LIMIT 10;
```
The Result:
|company_name|total_PCF|
|------------|---------|
|"Interface, Inc."|9801558.00|
|"Daikin Industries, Ltd."|2582264.00|
|"Osaka Gas Co., Ltd."|577595.00|
|"Toppan Printing Co., Ltd."|363776.00|
|"Elitegroup computer systems co., Ltd."|258712.00|
|"Casio Computer Co., Ltd."|111131.00|
|"Coca-Cola Enterprises, Inc."|72486.00|
|"Fuji Xerox Co., Ltd."|62369.00|
|"Staples, Inc."|46544.00|
|"PepsiCo, Inc."|23017.00|

5. What are the countries with the highest contribution to carbon emissions
```sql
SELECT countries.country_name,
	ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_PCF
FROM countries
JOIN product_emissions
ON countries.id = product_emissions.industry_group_id
GROUP BY countries.country_name
ORDER BY ROUND(SUM(product_emissions.carbon_footprint_pcf),2) DESC
LIMIT 10;
```
The Result:
|country_name|total_PCF|
|------------|---------|
|Indonesia|9801558.00|
|Colombia|2582264.00|
|Malaysia|577595.00|
|Switzerland|363776.00|
|Finland|258712.00|
|Belgium|111131.00|
|Chile|72486.00|
|France|62369.00|
|Sweden|46544.00|
|Netherlands|23017.00|

6. What is the trend of carbon footprints (PCFs) over the years
```sql
SELECT 
    industry_groups.industry_group AS industry_group,
    ROUND(SUM(
    	CASE
	    	WHEN product_emissions.year = 2013
    		THEN product_emissions.upstream_percent_total_pcf ELSE 0
    	END), 2) AS emissions_2013,
    ROUND(SUM(
    	CASE 
	    	WHEN product_emissions.year = 2014
	    	THEN product_emissions.upstream_percent_total_pcf ELSE 0 
	    END), 2) AS emissions_2014,
    ROUND(SUM(
    	CASE 
	    	WHEN product_emissions.year = 2015 
	    	THEN product_emissions.upstream_percent_total_pcf ELSE 0 
	    END), 2) AS emissions_2015,
    ROUND(SUM(
    	CASE 
	    	WHEN product_emissions.year = 2016 
	    	THEN product_emissions.upstream_percent_total_pcf ELSE 0 
	    END), 2) AS emissions_2016,
    ROUND(SUM(
    	CASE 
	    	WHEN product_emissions.year = 2017 
	    	THEN product_emissions.upstream_percent_total_pcf ELSE 0 
	    END), 2) AS emissions_2017
FROM 
    product_emissions
LEFT JOIN 
    industry_groups ON industry_groups.id = product_emissions.industry_group_id
GROUP BY 
    industry_groups.industry_group
ORDER BY 
    emissions_2013, emissions_2014, emissions_2015, emissions_2016, emissions_2017;
```
The Result:
|industry_group|emissions_2013|emissions_2014|emissions_2015|emissions_2016|emissions_2017|
|--------------|--------------|--------------|--------------|--------------|--------------|
|Household & Personal Products|0.0|0.0|0.0|0.0|0.0|
|Trading Companies & Distributors and Commercial Services & Supplies|0.0|0.0|0.0|0.0|0.0|
|Semiconductors & Semiconductors Equipment|0.0|0.0|0.0|0.0|0.0|
|Commercial & Professional Services|0.0|0.0|0.0|0.0|139.09|
|Energy|0.0|0.0|0.0|24.56|0.0|
|"Consumer Durables, Household and Personal Products"|0.0|0.0|7.03|0.0|0.0|
|Tires|0.0|0.0|13.77|0.0|0.0|
|Gas Utilities|0.0|0.0|25.02|0.0|0.0|
|Electrical Equipment and Machinery|0.0|0.0|49.41|0.0|0.0|
|Tobacco|0.0|0.0|57.14|0.0|0.0|
|"Mining - Iron, Aluminum, Other Metals"|0.0|0.0|70.07|0.0|0.0|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0.0|0.0|223.98|0.0|0.0|
|"Textiles, Apparel, Footwear and Luxury Goods"|0.0|0.0|310.71|0.0|0.0|
|Chemicals|0.0|0.0|528.02|0.0|0.0|
|Food & Beverage Processing|0.0|0.0|546.34|0.0|0.0|
|Containers & Packaging|0.0|0.0|596.4|0.0|0.0|
|Semiconductors & Semiconductor Equipment|0.0|95.64|0.0|144.78|0.0|
|Retailing|0.0|228.75|150.3|0.0|0.0|
|Food & Staples Retailing|0.0|419.53|436.82|84.49|0.0|
|"Pharmaceuticals, Biotechnology & Life Sciences"|7.02|1.41|0.0|0.0|0.0|
|Utilities|25.02|0.0|0.0|24.68|0.0|
|Telecommunication Services|44.18|49.35|49.2|0.0|0.0|
|Automobiles & Components|74.37|11.26|1.61|61.99|0.0|
|Software & Services|151.08|277.1|525.31|214.57|0.0|
|Media|187.1|187.1|158.54|154.35|0.0|
|Consumer Durables & Apparel|347.17|210.06|0.0|421.32|0.0|
|Capital Goods|530.88|440.35|3.94|161.36|5.64|
|Technology Hardware & Equipment|1539.82|1368.13|1891.58|1361.01|245.14|
|Materials|1617.21|1438.12|0.0|653.0|771.48|
|"Food, Beverage & Tobacco"|1677.71|420.64|0.0|439.67|263.53|
