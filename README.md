# CARBON EMISSION ANALYSIS REPORT

## I. Introduction
**what is the project about?**

![Photo by Chris  LeBoutillier ](https://github.com/dulcie130435/Carbon-Emission-Analysis/raw/main/carbon_emission.jpg)

*Photo by Chris LeBoutillier*

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## II. Data Source: Where Our Data Comes From

Our dataset is compiled from publicly available data from <span style="color:red">nature.com</span> and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## III. Data Structure 
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![data_structure](https://github.com/dulcie130435/Carbon-Emission-Analysis/raw/main/data_structure.jpg)

## IV. Research & Key Business Question 

##### **_1. Which products contribute the most to carbon emissions?_**

```
SELECT 
    product_name, 
    SUM(carbon_footprint_pcf) AS total_carbon_emissions
FROM 
    product_emissions
GROUP BY 
    product_name
ORDER BY 
    total_carbon_emissions DESC
LIMIT 10;
```

The table below lists the products with the highest total carbon footprint (measured in CO₂ equivalent):

| product_name                                                                                                                       | total_carbon_emissions | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044                | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187                | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608                | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625                | 
| TCDE                                                                                                                               | 198150                 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687                 | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000                 | 
| Electric Motor                                                                                                                     | 160655                 | 
| Audi A6                                                                                                                            | 111282                 | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | 100621                 | 


### 🔍 Insight
- Các tuabin gió (**Wind Turbine**) là nhóm sản phẩm có lượng phát thải carbon **cao nhất**. 
- Ô tô (như Audi A6, xe Toyota, xe GM) cũng có mức phát thải tương đối cao - phản ánh **khí thải trong toàn bộ từ lúc sản xuất đến sử dụng**.
- Thiết bị công nghiệp (như **Retaining wall structure**, **Electric Motor**) cũng góp phần lớn, cho thấy ngành xây dựng và cơ khí là những lĩnh vực có mức phát thải đáng kể. 

**→ <span style="color:red">OVERALL:</span>** Sản phẩm càng lớn, nặng, hoặc dùng nhiều kim loại - thường có lượng thải CO<sub>2</sub> cao. 

##### **_2. What are the industry groups of these products?_**

```
SELECT 
    pe.product_name, 
    ig.industry_group,
    SUM(pe.carbon_footprint_pcf) AS total_carbon_emissions
FROM 
    product_emissions pe
JOIN 
    industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY 
    pe.product_name, ig.industry_group
ORDER BY 
    total_carbon_emissions DESC
LIMIT 10;
```

| product_name                                                                                                                       | industry_group                     | total_carbon_emissions | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044                | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187                | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608                | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625                | 
| TCDE                                                                                                                               | Materials                          | 198150                 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687                 | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000                 | 
| Electric Motor                                                                                                                     | Capital Goods                      | 140647                 | 
| Audi A6                                                                                                                            | Automobiles & Components           | 111282                 | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | Automobiles & Components           | 100621                 | 

### 🔍 Insight
- Các sản phẩm **Wind Turbine G128 / G132 / G114 / G90** (tuabin gió công suất lớn) đến từ ngành *Electrical Equipment and Machinery* là nguồn phát thải CO<sub>2</sub> cao nhất – chủ yếu do quá trình sản xuất thiết bị lớn, nặng.
- Nhóm **xe ô tô** như *Land Cruiser Prado, Audi A6, GM average vehicles* thuộc ngành *Automobiles & Components* cũng đóng góp đáng kể vào phát thải.
- Một số sản phẩm xây dựng như *Retaining wall structure* và thiết bị như *Electric Motor* thuộc ngành *Materials* và *Capital Goods* cũng có lượng phát thải khá lớn.

**→ <span style="color:red">OVERALL:</span>** **Tuabin gió, ô tô và vật liệu xây dựng** là 3 nhóm sản phẩm có carbon footprint cao nhất trong danh sách.


##### **_3. What are the industries with the highest contribution to carbon emissions?_**
```
SELECT 
    ig.industry_group, 
    SUM(pe.carbon_footprint_pcf) AS total_emissions
FROM 
    product_emissions pe
JOIN 
    industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY 
    ig.industry_group
ORDER BY 
    total_emissions DESC
LIMIT 10;
```

| industry_group                                   | total_emissions | 
| -----------------------------------------------: | --------------: | 
| Electrical Equipment and Machinery               | 9801558         | 
| Automobiles & Components                         | 2582264         | 
| Materials                                        | 577595          | 
| Technology Hardware & Equipment                  | 363776          | 
| Capital Goods                                    | 258712          | 
| "Food, Beverage & Tobacco"                       | 111131          | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486           | 
| Chemicals                                        | 62369           | 
| Software & Services                              | 46544           | 
| Media                                            | 23017           | 


### 🔍 Insight
- **Ngành phát thải CO₂ nhiều nhất** là *Electrical Equipment and Machinery*, với các sản phẩm tiêu biểu như **Wind Turbine G128 / G132**.
- **Ngành ô tô** (*Automobiles & Components*) cũng đóng góp lớn, với các sản phẩm như **Audi A6, Land Cruiser**, hay **GM average vehicles**, cho thấy vòng đời xe là một nguồn phát thải đáng kể.

##### **_4. What are the companies with the highest contribution to carbon emissions?_**
```
SELECT 
    c.company_name, 
    SUM(pe.carbon_footprint_pcf) AS total_emissions
FROM 
    product_emissions pe
JOIN 
    companies c ON pe.company_id = c.id
GROUP BY 
    c.company_name
ORDER BY 
    total_emissions DESC
LIMIT 10;
```
| company_name                            | total_emissions | 
| --------------------------------------: | --------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464         | 
| Daimler AG                              | 1594300         | 
| Volkswagen AG                           | 655960          | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016          | 
| "Hino Motors, Ltd."                     | 191687          | 
| Arcelor Mittal                          | 167007          | 
| Weg S/A                                 | 160655          | 
| General Motors Company                  | 137007          | 
| "Lexmark International, Inc."           | 132012          | 
| "Daikin Industries, Ltd."               | 105600          | 

### 🔍 Insight
- Công ty dẫn đầu về lượng phát thải thường là các **nhà sản xuất thiết bị lớn** như *tuabin gió* hoặc *ô tô* - do quy mô sản phẩm và quy trình sản xuất phức tạp.

##### **_5. What are the countries with the highest contribution to carbon emissions?_**

```
SELECT 
    co.country_name, 
    SUM(pe.carbon_footprint_pcf) AS total_emissions
FROM 
    product_emissions pe
JOIN 
    countries co ON pe.country_id = co.id
GROUP BY 
    co.country_name
ORDER BY 
    total_emissions DESC
LIMIT 10;
```

| country_name | total_emissions | 
| -----------: | --------------: | 
| Spain        | 9786130         | 
| Germany      | 2251225         | 
| Japan        | 653237          | 
| USA          | 518381          | 
| South Korea  | 186965          | 
| Brazil       | 169337          | 
| Luxembourg   | 167007          | 
| Netherlands  | 70417           | 
| Taiwan       | 62875           | 
| India        | 24574           | 

### 🔍 Insight
- Những nước có nền công nghiệp phát triển, đặc biệt có nhà máy sản xuất **tuabin, ô tô** hoặc **vật liệu xây dựng**, thường dẫn đầu về tổng lượng phát thải carbon từ sản phẩm.

**→ <span style="color:red">OVERALL:</span>** 
- **Các ngành công nghiệp nặng, doanh nghiệp sản xuất quy mô lớn**, và **các quốc gia công nghiệp hóa** là những nhóm chính cần ưu tiên trong các chiến lược giảm thải khí CO<sub>2</sub>.

##### **_6. What is the trend of carbon footprints (PCFs) over the years?_**

```
SELECT year, SUM(carbon_footprint_pcf) AS total_emission
FROM product_emissions
GROUP BY year
ORDER BY year;
```

| year | total_emission | 
| ---: | -------------: | 
| 2013 | 503857         | 
| 2014 | 624226         | 
| 2015 | 10840415       | 
| 2016 | 1640182        | 
| 2017 | 340271         | 

### 🔍 Insight
- Vào **năm 2015**, lượng phát thải **tăng mạnh** cao gấp gần **10 lần** so với năm 2014.
- Sau năm 2015, lượng phát thải **giảm dần** rõ rệt:
    - Giảm 85% từ năm 2015 → 2016.
    - Giảm tiếp gần 80% từ năm 2016 → 2017.

##### **_7. "What is the average carbon footprint (PCF) of each industry group across different countries? Which industries in which countries have the highest or lowest average carbon emissions?"_**

```
SELECT 
    ig.industry_group,
    co.country_name,
    AVG(pe.carbon_footprint_pcf) AS avg_pcf
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id = ig.id
JOIN countries co ON pe.country_id = co.id
GROUP BY 1,2
ORDER BY ig.industry_group, avg_pcf;
```

| industry_group                                                         | country_name   | avg_pcf      | 
| ---------------------------------------------------------------------: | -------------: | -----------: | 
| "Consumer Durables, Household and Personal Products"                   | USA            | 74.5714      | 
| "Consumer Durables, Household and Personal Products"                   | Sweden         | 409.0000     | 
| "Food, Beverage & Tobacco"                                             | Colombia       | 0.0000       | 
| "Food, Beverage & Tobacco"                                             | Switzerland    | 0.0000       | 
| "Food, Beverage & Tobacco"                                             | France         | 1.0000       | 
| "Food, Beverage & Tobacco"                                             | Germany        | 5.6667       | 
| "Food, Beverage & Tobacco"                                             | USA            | 57.8846      | 
| "Food, Beverage & Tobacco"                                             | Malaysia       | 58.7500      | 
| "Food, Beverage & Tobacco"                                             | Spain          | 85.2500      | 
| "Food, Beverage & Tobacco"                                             | Japan          | 143.7273     | 
| "Food, Beverage & Tobacco"                                             | United Kingdom | 624.7000     | 
| "Food, Beverage & Tobacco"                                             | South Africa   | 1649.5000    | 
| "Food, Beverage & Tobacco"                                             | South Korea    | 15802.8333   | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | Sweden         | 210.5000     | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | Chile          | 451.0000     | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | Finland        | 803.7000     | 
| "Mining - Iron, Aluminum, Other Metals"                                | India          | 2727.0000    | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | USA            | 24162.0000   | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | USA            | 14.3333      | 
| Automobiles & Components                                               | Taiwan         | 106.0000     | 
| Automobiles & Components                                               | USA            | 34251.7500   | 
| Automobiles & Components                                               | Germany        | 35175.1250   | 
| Automobiles & Components                                               | Japan          | 48485.7500   | 
| Capital Goods                                                          | Switzerland    | 2.0000       | 
| Capital Goods                                                          | China          | 3.0000       | 
| Capital Goods                                                          | France         | 33.0000      | 
| Capital Goods                                                          | United Kingdom | 44.3750      | 
| Capital Goods                                                          | USA            | 560.2222     | 
| Capital Goods                                                          | Sweden         | 1409.0000    | 
| Capital Goods                                                          | Japan          | 11115.5000   | 
| Capital Goods                                                          | Brazil         | 70323.5000   | 
| Chemicals                                                              | USA            | 0.3333       | 
| Chemicals                                                              | Belgium        | 1.0000       | 
| Chemicals                                                              | Brazil         | 1.3333       | 
| Chemicals                                                              | Switzerland    | 3.2500       | 
| Chemicals                                                              | Spain          | 180.0000     | 
| Chemicals                                                              | India          | 811.6000     | 
| Chemicals                                                              | Netherlands    | 3321.4286    | 
| Chemicals                                                              | South Korea    | 5810.0000    | 
| Commercial & Professional Services                                     | USA            | 101.1579     | 
| Commercial & Professional Services                                     | United Kingdom | 170.0000     | 
| Commercial & Professional Services                                     | Japan          | 370.5000     | 
| Consumer Durables & Apparel                                            | USA            | 26.8833      | 
| Consumer Durables & Apparel                                            | South Korea    | 46.6667      | 
| Consumer Durables & Apparel                                            | China          | 50.0000      | 
| Consumer Durables & Apparel                                            | Sweden         | 409.0000     | 
| Consumer Durables & Apparel                                            | Japan          | 2344.0000    | 
| Containers & Packaging                                                 | Sweden         | 81.4000      | 
| Containers & Packaging                                                 | Ireland        | 855.0000     | 
| Containers & Packaging                                                 | South Africa   | 871.0000     | 
| Electrical Equipment and Machinery                                     | France         | 19.0000      | 
| Electrical Equipment and Machinery                                     | USA            | 199.7500     | 
| Electrical Equipment and Machinery                                     | Japan          | 2268.0000    | 
| Electrical Equipment and Machinery                                     | Brazil         | 20008.0000   | 
| Electrical Equipment and Machinery                                     | Spain          | 2444616.0000 | 
| Energy                                                                 | Brazil         | 750.0000     | 
| Energy                                                                 | USA            | 1512.5000    | 
| Energy                                                                 | Spain          | 3499.5000    | 
| Food & Beverage Processing                                             | Switzerland    | 0.0000       | 
| Food & Beverage Processing                                             | USA            | 0.6000       | 
| Food & Beverage Processing                                             | France         | 1.2500       | 
| Food & Beverage Processing                                             | Japan          | 130.0000     | 
| Food & Staples Retailing                                               | Italy          | 0.7500       | 
| Food & Staples Retailing                                               | USA            | 171.5000     | 
| Food & Staples Retailing                                               | United Kingdom | 561.5000     | 
| Gas Utilities                                                          | Japan          | 61.0000      | 
| Household & Personal Products                                          | France         | 0.0000       | 
| Materials                                                              | Lithuania      | 0.0000       | 
| Materials                                                              | Italy          | 1.0000       | 
| Materials                                                              | Belgium        | 1.0000       | 
| Materials                                                              | Switzerland    | 15.0000      | 
| Materials                                                              | China          | 38.0000      | 
| Materials                                                              | Spain          | 48.6667      | 
| Materials                                                              | United Kingdom | 50.8000      | 
| Materials                                                              | Sweden         | 71.2667      | 
| Materials                                                              | France         | 551.5000     | 
| Materials                                                              | Chile          | 552.0000     | 
| Materials                                                              | Finland        | 642.6667     | 
| Materials                                                              | Brazil         | 720.7273     | 
| Materials                                                              | Indonesia      | 721.0000     | 
| Materials                                                              | Ireland        | 855.0000     | 
| Materials                                                              | South Africa   | 1017.7500    | 
| Materials                                                              | USA            | 1371.4118    | 
| Materials                                                              | India          | 1541.8750    | 
| Materials                                                              | Netherlands    | 1684.5357    | 
| Materials                                                              | South Korea    | 4076.1429    | 
| Materials                                                              | Japan          | 12889.7778   | 
| Materials                                                              | Luxembourg     | 83503.5000   | 
| Media                                                                  | France         | 111.0000     | 
| Media                                                                  | USA            | 1890.3333    | 
| Retailing                                                              | USA            | 6.0000       | 
| Semiconductors & Semiconductor Equipment                               | Taiwan         | 2.0000       | 
| Semiconductors & Semiconductor Equipment                               | USA            | 16.6667      | 
| Semiconductors & Semiconductors Equipment                              | South Korea    | 1.0000       | 
| Software & Services                                                    | Japan          | 415.0000     | 
| Software & Services                                                    | USA            | 1428.5625    | 
| Technology Hardware & Equipment                                        | Switzerland    | 4.0000       | 
| Technology Hardware & Equipment                                        | China          | 7.0000       | 
| Technology Hardware & Equipment                                        | Finland        | 12.0000      | 
| Technology Hardware & Equipment                                        | France         | 50.0000      | 
| Technology Hardware & Equipment                                        | Canada         | 66.0000      | 
| Technology Hardware & Equipment                                        | South Korea    | 79.0000      | 
| Technology Hardware & Equipment                                        | Taiwan         | 836.8667     | 
| Technology Hardware & Equipment                                        | Japan          | 1415.5000    | 
| Technology Hardware & Equipment                                        | USA            | 2005.0909    | 
| Telecommunication Services                                             | United Kingdom | 45.7500      | 
| Telecommunication Services                                             | Canada         | 52.0000      | 
| Tires                                                                  | Japan          | 1011.0000    | 
| Tobacco                                                                | Greece         | 1.0000       | 
| Trading Companies & Distributors and Commercial Services & Supplies    | Australia      | 39.8333      | 
| Utilities                                                              | Japan          | 61.0000      | 


### 🔍 Insight
- Về <span style="color:blue"> ngành & Quốc gia có lượng phát thải trung bình **cao nhất:**</span>
    - **Electrical Equipment and Machinery** ở các quốc gia có hạ tầng sản xuất tái tạo quy mô lớn như tuabin gió thường có **PCF** trung bình rất cao.
    - **Automobiles & Components** tại các nước lớn như **Nhật Bản**, **Đức** và **Mỹ** có lượng khí thải cao ổn định.

- Về <span style="color:blue"> ngành & Quốc gia có lượng phát thải trung bình **thấp nhất:**</span>
    - Ngành **"Textiles, Apparel, Footwear and Luxury Goods"** tại **Mỹ** có PCF thấp có thể do quy trình sản xuất và sản phẩm nhẹ nên tiêu thụ ít năng lượng hơn.
    - Ngành **"Pharmaceuticals, Biotechnology & Life Sciences"** là ngành công nghệ cao nhưng lượng khí thải chỉ khoảng từ **trung bình thấp đến vừa phải**.
  

##### **_8. "Main Emission Sources by Industry Group"_**

```
SELECT 
    ig.industry_group,
    AVG(pe.upstream_percent_total_pcf) AS avg_upstream,
    AVG(pe.operations_percent_total_pcf) AS avg_operations,
    AVG(pe.downstream_percent_total_pcf) AS avg_downstream
FROM product_emissions pe
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY avg_upstream DESC
LIMIT 10;
```

| industry_group                           | avg_upstream    | avg_operations  | avg_downstream  | 
| ---------------------------------------: | --------------: | --------------: | --------------: | 
| Retailing                                | 75.81           | 6.204           | 17.986          | 
| Containers & Packaging                   | 74.55           | 10.3375         | 2.6125          | 
| Tobacco                                  | 57.14           | 42.86           | 0               | 
| Semiconductors & Semiconductor Equipment | 48.084          | 43.588          | 8.33            | 
| Media                                    | 45.806          | 5.588           | 48.607333333333 | 
| Food & Staples Retailing                 | 39.201666666667 | 16.890416666667 | 6.4058333333333 | 
| Software & Services                      | 34.354705882353 | 11.322352941176 | 45.500588235294 | 
| Capital Goods                            | 32.633428571429 | 7.1417142857143 | 37.368285714286 | 
| Food & Beverage Processing               | 27.317          | 9.1825          | 13.5005         | 
| Materials                                | 24.887833333333 | 17.598111111111 | 2.5142777777778 | 

### 🔍 Insight
- <span style="color:blue"> Giai đoạn Upstream</span>
    - **Cao nhất:**
        - *Retailing* (75.81) và *Containers & Packaging (74.55) - cho thấy quá trình thu mua và vận chuyển nguyên liệu là phát thải ra rất nhiều.
    - **Thấp nhất:**
        - *Materials* (24.89) và *Food & Beverage Processing* (27.32) - nguyên liệu ít phát thải hoặc chuỗi cung ứng ngắn hơn.
   
- <span style="color:blue"> Giai đoạn Operations</span>
    - **Cao nhất:**
        - *Semiconductors & Equipment* (43.59) và *Tobacco* (42.86) - do quy trình sản xuất dùng nhiều năng lượng.
    - **Thấp nhất:**
        - *Media* (5.59) và *Retailing* (6.20) - vận hành ít phát thải.

- <span style="color:blue"> Giai đoạn Downstream</span>
    - **Cao nhất:**
        - *Media* (48.61), *Software & Services* (45.50), *Capital Goods* (37.37) - phát sinh lớn trong giai đoạn sử dụng.
    - **Thấp nhất:**
        - *Tobacco* (0.00) và *Materials* (2.51) - ít phát thải sau khi sản phẩm rời khỏi nhà máy.


## 🚀 Final Thoughts

This report highlights key trends in industry carbon emissions, helping stakeholders understand where action is most needed. 

Thanks for checking out the project! Feel free to explore the visualizations and code in the repository. Contributions and suggestions are welcome!


