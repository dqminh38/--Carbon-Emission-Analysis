# --Carbon-Emission-Analysis
This is where i do a educational project to analysis question about "Carbon Emission" using publicly available data from nature.com
**1.Which products contribute the most to carbon emissions**?
**Querry:**
SELECT
  id,
  product_name,
  carbon_footprint_pcf
FROM
  product_emissions
ORDER BY 
  carbon_footprint_pcf DESC
LIMIT 1;
**Result**
| id           | product_name                 | carbon_footprint_pcf | 
| -----------: | ---------------------------: | -------------------: | 
| 22917-4-2015 | Wind Turbine G128 5 Megawats | 3718044              | 
**Querry:**
SELECT
p.product_name,
i.industry_group
FROM
product_emissions p
JOIN
industry_groups i
ON p.industry_group_id = i.id
WHERE p.product_name = "Wind Turbine G128 5 Megawats";
**Result**
| product_name                 | industry_group                     | 
| ---------------------------: | ---------------------------------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 
**Conclusion**
So i executed this 2 querries, it's show that Wind Turbine G128 5 Megawats come from Electrical Equipment and Machinery is the most to carbon emission. It can be easily understand that this product came from heavy industry which always top of the carbon emission.
**2.What are the industry groups of these products?**
**Querry:**
SELECT
  p.industry_group_id,
  i.industry_group
FROM
  product_emissions p
JOIN
  industry_groups i
ON p.industry_group_id = i.id
GROUP BY 
  p.industry_group_id;
**Result**
| industry_group_id | industry_group                                                         | 
| ----------------: | ---------------------------------------------------------------------: | 
| 1                 | "Consumer Durables, Household and Personal Products"                   | 
| 2                 | "Food, Beverage & Tobacco"                                             | 
| 3                 | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4                 | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5                 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 
| 6                 | "Textiles, Apparel, Footwear and Luxury Goods"                         | 
| 7                 | Automobiles & Components                                               | 
| 8                 | Capital Goods                                                          | 
| 9                 | Chemicals                                                              | 
| 10                | Commercial & Professional Services                                     | 
| 11                | Consumer Durables & Apparel                                            | 
| 12                | Containers & Packaging                                                 | 
| 13                | Electrical Equipment and Machinery                                     | 
| 14                | Energy                                                                 | 
| 15                | Food & Beverage Processing                                             | 
| 16                | Food & Staples Retailing                                               | 
| 17                | Gas Utilities                                                          | 
| 18                | Household & Personal Products                                          | 
| 19                | Materials                                                              | 
| 20                | Media                                                                  | 
| 21                | Retailing                                                              | 
| 22                | Semiconductors & Semiconductor Equipment                               | 
| 23                | Semiconductors & Semiconductors Equipment                              | 
| 24                | Software & Services                                                    | 
| 25                | Technology Hardware & Equipment                                        | 
| 26                | Telecommunication Services                                             | 
| 27                | Tires                                                                  | 
| 28                | Tobacco                                                                | 
| 29                | Trading Companies & Distributors and Commercial Services & Supplies    | 
| 30                | Utilities                                                              | 
**Conclusion**
After executed this querry, i found that there are about 30 different industry groups in this dataset. Let see what we can find from this.
**3.What are the industries with the highest contribution to carbon emissions?**
**Querry:**
SELECT
  i.industry_group,
  SUM(p.carbon_footprint_pcf) AS total_carbon_footprint
FROM
  product_emissions p
JOIN
  industry_groups i
ON p.industry_group_id = i.id
GROUP BY i.industry_group
ORDER BY total_carbon_footprint DESC
LIMIT 10;
**Result**
| industry_group                                   | total_carbon_footprint | 
| -----------------------------------------------: | ---------------------: | 
| Electrical Equipment and Machinery               | 9801558                | 
| Automobiles & Components                         | 2582264                | 
| Materials                                        | 577595                 | 
| Technology Hardware & Equipment                  | 363776                 | 
| Capital Goods                                    | 258712                 | 
| "Food, Beverage & Tobacco"                       | 111131                 | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486                  | 
| Chemicals                                        | 62369                  | 
| Software & Services                              | 46544                  | 
| Media                                            | 23017                  | 
**Querry:**
SELECT
  industry_group_id,
  product_name,
  carbon_footprint_pcf
FROM
  product_emissions
GROUP BY 
  product_name
HAVING industry_group_id = 13
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
**Result**
| industry_group_id | product_name                                                                  | carbon_footprint_pcf | 
| ----------------: | ----------------------------------------------------------------------------: | -------------------: | 
| 13                | Wind Turbine G128 5 Megawats                                                  | 3718044              | 
| 13                | Wind Turbine G132 5 Megawats                                                  | 3276187              | 
| 13                | Wind Turbine G114 2 Megawats                                                  | 1532608              | 
| 13                | Wind Turbine G90 2 Megawats                                                   | 1251625              | 
| 13                | T300                                                                          | 655                  | 
| 13                | DC988 Type 11 Cordless Power Drill equipment with rechargeable NiCd batteries | 88                   | 
| 13                | D21710 Type 5 Corded Power Drill                                              | 56                   | 
**Conclusion**
It's make sense for this industry group for being the top of contribution to carbon emissions. The products from this industry are also contribute the most to carbon emissions.
**4.What are the companies with the highest contribution to carbon emissions?**
**Querry:**
SELECT
  c.company_name,
  SUM(p.carbon_footprint_pcf) AS total_carbon_footprint
FROM
  product_emissions p
JOIN
  companies c
ON p.industry_group_id = c.id
GROUP BY c.company_name
ORDER BY total_carbon_footprint DESC
LIMIT 10;
**Result**
| company_name                            | total_carbon_footprint | 
| --------------------------------------: | ---------------------: | 
| "Interface, Inc."                       | 9801558                | 
| "Daikin Industries, Ltd."               | 2582264                | 
| "Osaka Gas Co., Ltd."                   | 577595                 | 
| "Toppan Printing Co., Ltd."             | 363776                 | 
| "Elitegroup computer systems co., Ltd." | 258712                 | 
| "Casio Computer Co., Ltd."              | 111131                 | 
| "Coca-Cola Enterprises, Inc."           | 72486                  | 
| "Fuji Xerox Co., Ltd."                  | 62369                  | 
| "Staples, Inc."                         | 46544                  | 
| "PepsiCo, Inc."                         | 23017                  | 
**Querry:**
SELECT
  company_id,
  product_name,
  SUM(carbon_footprint_pcf) AS total_carbon_footprint
FROM
  product_emissions
GROUP BY product_name
HAVING company_id = 13
ORDER BY total_carbon_footprint DESC
LIMIT 10;
**Result**
| company_id | product_name                                                                                                                                     | total_carbon_footprint | 
| ---------: | -----------------------------------------------------------------------------------------------------------------------------------------------: | ---------------------: | 
| 13         | Interface China Carpet                                                                                                                           | 30                     | 
| 13         | Interface Australia Carpet                                                                                                                       | 29                     | 
| 13         | Interface Asia carpet                                                                                                                            | 28                     | 
| 13         | Interface Americas Carpet                                                                                                                        | 24                     | 
| 13         | Interface Europe carpet                                                                                                                          | 20                     | 
| 13         | Interface Global LVT                                                                                                                             | 17                     | 
| 13         | Interface Australia carpet tile (The average square meter of carpet tile sold as Cool Carpet by our Interface business in Australia)             | 17                     | 
| 13         | Interface Thailand carpet tile (The average square meter of carpet tile sold as Cool Carpet by our Interface business in Southeast Asia)         | 15                     | 
| 13         | Interface United States carpet tile (The average square meter of carpet tile sold as Cool Carpet by our Interface business in the United States) | 13                     | 
| 13         | Interface US carpet                                                                                                                              | 13                     | 
**Querry:**
SELECT
  COUNT(product_name) AS Total_products_Interface_In,
  COUNT(DISTINCT product_name)
FROM
  product_emissions
WHERE 
  company_id = 13;
**Result**
| Total_products_Interface_In | COUNT(DISTINCT product_name) | 
| --------------------------: | ---------------------------: | 
| 20                          | 15                           | 
**Conclusion**
So this company is contribute the most to carbon emissions but their products do not have that high of carbon footprint. So i executed one more querry to find out if they have big quantity of products or not.
**5.What are the countries with the highest contribution to carbon emissions?**
**Querry:**
SELECT
  c.country_name,
  SUM(p.carbon_footprint_pcf) AS total_carbon_footprint
FROM
  product_emissions p
JOIN
  countries c
ON p.industry_group_id = c.id
GROUP BY c.country_name
ORDER BY total_carbon_footprint DESC
LIMIT 10;
**Result**
| country_name | total_carbon_footprint | 
| -----------: | ---------------------: | 
| Indonesia    | 9801558                | 
| Colombia     | 2582264                | 
| Malaysia     | 577595                 | 
| Switzerland  | 363776                 | 
| Finland      | 258712                 | 
| Belgium      | 111131                 | 
| Chile        | 72486                  | 
| France       | 62369                  | 
| Sweden       | 46544                  | 
| Netherlands  | 23017                  | 
**Querry:**
SELECT
  country_id,
  product_name,
  SUM(carbon_footprint_pcf) AS total_carbon_footprint
FROM
  product_emissions
GROUP BY product_name
HAVING country_id = 13
ORDER BY total_carbon_footprint DESC
LIMIT 10;
**Result**
| country_id | product_name | total_carbon_footprint | 
| ---------: | -----------: | ---------------------: | 
| 13         | Fajar Board  | 721                    | 
**Conclusion**
Indonesia is the highest contribution to carbon emissions that only produce one product.
