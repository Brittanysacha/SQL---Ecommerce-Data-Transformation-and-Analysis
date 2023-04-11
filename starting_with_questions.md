Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT all_sessions.city, 
	SUM(product_sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN product_sales_report ON all_sessions.product_SKU = product_sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.city IS NOT NULL
		   AND all_sessions.city != 'not available in demo dataset' 
GROUP BY all_sessions.city
ORDER BY revenue DESC;

SELECT all_sessions.country, 
	SUM(product_sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN product_sales_report ON all_sessions.product_SKU = product_sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.country IS NOT NULL
		   AND all_sessions.country != 'not available in demo dataset' 
GROUP BY all_sessions.country
ORDER BY revenue DESC;

OR

SELECT all_sessions.city, 
	SUM((all_sessions.product_price/1000000) * product_sales_report.total_ordered) AS revenue
FROM all_sessions 
	JOIN product_sales_report ON all_sessions.product_SKU = product_sales_report.product_SKU
WHERE all_sessions.city IS NOT NULL
	AND all_sessions.city != 'not available in demo dataset' 
GROUP BY all_sessions.city
ORDER BY revenue DESC;

SELECT all_sessions.country, 
	SUM((all_sessions.product_price/1000000) * product_sales_report.total_ordered) AS revenue
FROM all_sessions 
	JOIN product_sales_report ON all_sessions.product_SKU = product_sales_report.product_SKU
WHERE all_sessions.country IS NOT NULL
	AND all_sessions.country != 'not available in demo dataset' 
GROUP BY all_sessions.country
ORDER BY revenue DESC;

Answer: 
United States - $6,032,642.25
Mountain View - $1,039,252.88


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

<!-- Query for average products ordered from visitors in each city -->

SELECT all_sessions.city, 
	AVG(sales_report.total_ordered) AS products_ordered
FROM all_sessions 
	JOIN sales_report ON all_sessions.product_SKU = sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
GROUP BY all_sessions.city
ORDER BY products_ordered DESC;

<!-- Query for average products ordered from visitors in each city -->
SELECT all_sessions.country, 
	SUM(sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN sales_report ON all_sessions.product_SKU = sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.country IS NOT NULL
		   AND all_sessions.country != 'not available in demo dataset' 
GROUP BY all_sessions.country
ORDER BY products_ordered DESC;

Answer:

Below is a list of the average number of products ordered from visitors in each city:

Sacramento, 189.00
Rio de Janeiro, 189.00
Shinjuku, 178.70
Toronto, 102.06
Barcelona, 73.10
Chicago, 65.03
Los Angeles, 63.02
Santa Fe, 60.00
Rome, 53.00
Bogota, 45.21
Cupertino, 42.00
Dublin, 39.52
Calgary, 39.00
Palo Alto, 38.77
Vancouver, 37.00
Zhongli District, 34.50
Munich, 30.87
Hyderabad, 29.31
Melbourne, 26.71
San Francisco, 23.29
Berlin, 23.11
Detroit, 23.00
San Bruno, 22.50
(not set), 22.08
Osaka, 21.43
Mountain View, 21.39
not available in demo dataset, 20.22
Chennai, 20.15
Houston, 20.09
Ann Arbor, 20.02
Austin, 17.83
Tel Aviv-Yafo, 17.70
Pittsburgh, 16.85
Buenos Aires, 16.00
Sao Paulo, 15.00
Sunnyvale, 14.56
New York, 14.36
Irvine, 13.93
San Jose, 13.15
Kuala Lumpur, 13.00
Atlanta, 12.76
Jakarta, 12.21
London, 12.16
Kirkland, 12.00
Ipoh, 11.00
South San Francisco, 11.00
Stockholm, 9.20
Brisbane, 9.00
Colombo, 9.00
Redmond, 9.00
Seattle, 7.70
Paris, 7.66
Washington, 7.65
Madrid, 7.50
San Diego, 7.00
Jaipur, 7.00
East Lansing, 7.00
Santa Clara, 6.62
Pune, 6.50
Warsaw, 6.19
Mexico City, 6.12
Singapore, 6.10
Helsinki, 6.00
Montreal, 6.00
Bangkok, 5.14
Jacksonville, 5.00
Hong Kong, 4.98
Cambridge, 4.83
Minato, 4.40
Seoul, 4.05
Dallas, 4.00
Tempe, 3.81
San Antonio, 2.00
Orlando, 2.00
Amsterdam, 2.00
Kolkata, 2.00
Hamburg, 2.00
Bengaluru, 1.71
Philadelphia, 1.26
Milpitas, 1.00
Rosario, 1.00
Sydney, 0.61
Mumbai, 0.15
Bratislava, 0.00
Oakland, 0.00
Zurich, 0.00
Shibuya, 0.00
Istanbul, 0.00
Fortaleza, 0.00
La Victoria, 0.00
Lucknow, 0.00
Prague, 0.00

Below is a list of the average number of products ordered from visitors in each country:

United States, 6032642.25
Japan, 290850.67
United Kingdom, 253421.88
Indonesia, 230888.72
Denmark, 224380.51
India, 186943.14
Netherlands, 175028.34
Philippines, 152563.77
Canada, 150918.38
Sweden, 140532.1
Italy, 101611.51
Taiwan, 93260.06
Spain, 88482.07
Germany, 82189.02
Mexico, 75159.33
France, 71940.84
Australia, 61532.34
Morocco, 55286.6
Israel, 50604.64
Brazil, 38870.53
Ireland, 32102.46
Malaysia, 30414.81
Singapore, 30326.84
Czechia, 26380.65
Guatemala, 25497.99
Poland, 25145.27
(not set), 21405.8
Argentina, 20804.25
Colombia, 20086.92
Serbia, 17771.74
Turkey, 14428.8
Laos, 12060.65
Belgium, 11479.23
Switzerland, 9778.86
Panama, 9693.75
Nepal, 6995.5
Hungary, 6995.5
Austria, 6778.87
Pakistan, 6617.48
Russia, 5181.04
Reunion, 5096.4
Ukraine, 5096.4
Vietnam, 5064.3
Romania, 4407
Venezuela, 4150.94
Thailand, 3926.28
Hong Kong, 3775.95
Portugal, 3336.3
United Arab Emirates, 2798.2
China, 2511.21
Sri Lanka, 2412.98
Norway, 2283.42
Egypt, 1868.2
Tanzania, 1277.01
South Korea, 1270.88
Chile, 1187.18
New Zealand, 994.52
Mauritius, 881.4
Estonia, 732.32
Finland, 677.64
Maldives, 669.92
Georgia, 501.93
Bahamas, 377.72
South Africa, 339.76
Croatia, 305.78
Bangladesh, 293.8
C'ote d‚ÄôIvoire, 279.82
Dominican Republic, 212.88
Iraq, 131.94
Greece, 0
Peru, 0
Slovakia, 0
Lithuania, 0
Latvia, 0
Qatar, 0
Tunisia, 0




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


Queries
<!-- Query for top types of product/country -->
SELECT
  CASE 
    WHEN country IN ('Canada', 'Mexico', 'United States') THEN 'North America'
    WHEN country IN ('Brazil', 'Argentina', 'Chile', 
					 'Columbia', 'Venezuela', 'Peru') THEN 'South America'
    WHEN country IN ('Belize', 'Costa Rica', 'El Salvador', 
					 'Guatemala', 'Honduras', 'Nicaragua',
					 'Panama', 'Bahamas', 'Barbados',
					 'Cuba', 'Dominican Republic', 'Grenada', 
					 'Haiti', 'Jamaica', 'Sint Maarten',
					 'Trinidad & Tobago') THEN 'Central America' 
    WHEN country IN ('France', 'Germany', 'United Kingdom',
					 'Belgium', 'Austria', 'Spain'
					'Italy', 'Ireland', 'Switzerland',
					 'Hungary', 'Russia', 'Ukraine', 
					 'Romania', 'Lithuania', 'Latvia',
					 'Greece', 'Gibraltar', 'Serbia',
					 'Netherlands', 'Denmark', 'Portugal',
					 'Finland', 'Estonia', 'Croatia', 
					 'Slovakia', 'Sweden', 'Georgia'
					 'San Marino') THEN 'Europe'
    WHEN country IN ('Japan', 'China', 'India', 
					 'Indonesia', 'Taiwan', 'Israel', 
					 'Malyasia', 'Singapore', 'Pakistan'
					 'Vietnam', 'Phillipines', 'Thailand', 
					 'Laos', 'Hong Kong', 'Nepal', 
					 'South Korea', 'Sri Lanka', 'Iraq',
					 'Bahrain', 'Reunion') THEN 'Asia-Pacific'
    WHEN country IN ('Morocco', 'Tanzania', 'Mauritius',
					 'South Africa', 'Cote dIvoire', 'Tunisia') THEN 'Africa'
    WHEN country IN ('Australia', 'New Zealand', 'Fiji') THEN 'Oceania'
    ELSE 'Other' 
  END AS region,
  products_sales_report.product_name,
  SUM(products_sales_report.total_ordered) AS "total_products",
  COUNT(*) AS total_orders
FROM all_sessions 
  JOIN products_sales_report ON all_sessions.product_SKU = products_sales_report.product_SKU
  JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
      AND all_sessions.visit_id = analytics.visit_id
WHERE all_sessions.country IS NOT NULL
      AND all_sessions.country != 'not available in demo dataset' 
GROUP BY region, products_sales_report.product_name
ORDER BY region, total_products DESC, products_sales_report.product_name;

<!-- Query for top types of product in each city. --> 
SELECT
  CASE 
    WHEN city IN ('Sacramento', 'Toronto', 'Chicago',
				  'Los Angeles', 'Santa Fe', 'Cupertino',
				  'Boston', 'Palo Alto', 'Vancouver'
				  'San Francisco', 'Detroit', 'San Bruno',
				  'Ann Arbor', 'Houston', 'Pittsburgh',
				  'Sunnyvale', 'New York', 'Irvine',
				  'San Jose', 'Atlanta', 'Kirkland',
				  'South San Francisco', 'Jacksonville',
				  'Dallas', 'San Antonio', 'Orlando',
				  'Philadelphia', 'Milpitas', 'Oakland',
                  'Salem', 'Santa Clara', 'Mountain View'
                  'Waterloo', 'Fort Worth','University Park',
                  'Austin', 'LaFayette', 'Washington',
                  'Bellingham', 'Cupertino', 'San Antonio',
                  'Greer', 'Dallas', 'Jacksonville',
                  'Burnaby', 'Ashburn', 'Rexburg',
                  'Bellflower', 'Council Bluffs', 'Pleasanton',
                  'Chicago', 'Houston', 'Portland',
                  'Menlo Park', 'Piscataway Township', 'Westville',
                  'South El Monte', 'Indianapolis', 'Johnson City',
                  'Kitchener', 'Norfolk', 'Santa Monica', 
                  'Mississauga', 'Calgary', 'San Antonio', 
                  'Jersey City', 'Berkeley', 'Sherbrooke', 
                  'Tampa', 'Tempe', 'The Dalles',
                  'Denver', 'Druid Hills', 'Columbus', 
                  'Charlottetown', 'Edmonton', 'Forest Park', 
                  'Fremont Goose', 'Creek', 'Kansas City', 
                  'Las Vegas', 'Mexico City', 'Minneapolis',
                  'Monterrey', 'Montreal', 'Nashville',
                  'Ottawa', 'Phoenix', 'Quebec City',
                  'Raleigh', 'Redmond', 'Redwood City', 
                  'Richardson', 'San Diego', 'St. Johns' 
                  'St. Louis','Stanford', 'Wellesley', 
                  'Westlake Village', 'Parsippany-Troy Hills', 'Columbia'
                  'Culican', 'Eau Claire', 'El Paso',
                  'Hayward', 'Kalamazoo', 'Lake Oswego',
                  'Madison', 'Akron', 'Appleton',
                  'Avon', 'Boulder', 'Charlotte') THEN 'North America'
    WHEN city IN ('Rio de Janeiro', 'Bogota', 'Buenos Aires',
				  'Sao Paulo', 'Rosario', 'La Victoria'
                  'Maracaibo', 'Buenos Aires','Belo Horizonte', 
                  'Odessa', 'Sao Paulo','Ama',
                  'Fortaleza', 'Montevideo', 'Asuncion',
                  'Belo Horizonte') THEN 'South America'
    WHEN city IN ('Panama City', 'Santiago', 'San Salvador') THEN 'Central America'
    WHEN city IN ('Barcelona', 'Rome', 'Dublin',
				  'Munich', 'Berlin', 'Watford',
				  'Stockholm', 'Paris', 'London',
				  'Warsaw', 'Amsterdam', 'Hamburg',
				  'Bratislava', 'Prague', 'Zurich',
				  'Istanbul', 'Moscow', 'Saint Petersburg'
                  'Courbevoie', 'Stuttgart', 'Belgrade',
                  'Quimper', 'Wroclaw', 'Brussels',
                  'East Lansing', 'Esbjerg', 
                  'Kiev', 'Zagreb', 'Budapest', 
                  'Villeneuve-d Ascq', 'Stockholm', 
                  'Moscow', 'Wrexham', 'Cluj-Napoca', 
                  'Nice', 'Coventry', 'Cork', 
                  'Egham', 'Copenhagen', 'Oslo', 
                  'Rotterdam', 'Cambridge', 'Iasi', 
                  'Oviedo', 'Vienna', 'Rome', 
                  'Salford', 'Lisbon', 'Marlboro', 
                  'Ghent', 'Chico', 'Milan', 
                  'Poznan', 'Thessaloniki', 'Pozuelo de Alarcon', 
                  'Glasgow', 'Montreuil', 'Helsinki',
                  'Athens', 'Madrid', 'Kovrov', 
                  'Timisoara', 'Frankfurt', 'Krakow',
                  'Skopje', 'Oxford', 'Groningen',
                  'Bucharest', 'Bordeaux', 'Gothenburg',
                  'Manchester', 'Kharkiv', 'Antwerp',
                  'Dublin', 'Marseille', 'Antalya',
                  'Bilbao', 'Brno') THEN 'Europe'                
    WHEN city IN ('Shinjuku', 'Zhongli District', 'Hyderabad',
				  'Chennai', 'Kuala Lumpur', 'Ipoh',
				  'Jaipur', 'Pune', 'Bangkok',
				  'Minato', 'Seoul', 'Tel Aviv-Yafo' 
                  'Taguig', 'Kharagpur', 'Izmir', 
                  'Riyadh', 'Petaling Jaya', 'Singapore', 
                  'Hanoi', 'Manila', 'Quezon City', 
                  'Gurgaon', 'Bandung', 'Vladivostok',
                  'Chiyoda', 'Longtan District', 'Salem',
                  'Jaipur', 'Ipoh', 'Karachi', 
                  'Kolkata', 'La Victoria', 'Medellin',
                  'Osaka', 'Lucknow', 'Chuo', 
                  'Vilnius', 'Doha', 'Hong Kong',
                  'Lahore', 'Sapporo', 'Indore'
                  'Bhubaneswar', 'Yokohama', 'Mumbai',
                  'Nagoya', 'Nanded', 'Neipu Township',
                  'New Delhi', 'Patna', 'Sakai',
                  'Shibuya', 'Colombo', 'Dubai',
                  'Makati', 'Jakarta', 'Ho Chi Minh City',
                  'Chandigarh', 'Bengaluru', 'Bejing',
                  'Ahmedabad') THEN 'Asia-Pacific'
    WHEN city IN ('Cape Town', 'Nairobi', 'Sandton',
                  'Amã', 'Alexandria') THEN 'Africa'
    WHEN city IN ('Melbourne', 'Brisbane', 'Sydney',
                  'Adelaide', 'Auckland', 'Perth',
                  'Brisbane') THEN 'Oceania'
    ELSE 'Other' 
  END AS region,
  products_sales_report.product_name,
  SUM(products_sales_report.total_ordered) AS "total_products",
  COUNT(*) AS total_orders
FROM all_sessions 
  JOIN products_sales_report ON all_sessions.product_SKU = products_sales_report.product_SKU
  JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
      AND all_sessions.visit_id = analytics.visit_id
WHERE all_sessions.city IS NOT NULL
      AND all_sessions.city != 'not available in demo dataset' 
GROUP BY region, products_sales_report.product_name
ORDER BY region, total_products DESC, products_sales_report.product_name;


Answer:
- The Learning Thermostat 3rd Gen-USA - Stainless Steel was the most popular product among countries in the African countries, with 282 units sold.

- The blackout cap was the most popular product among countries in the Central American region, with 1701 units sold.

The  17oz Stainless Steel Sport Bottle was the most popular producy among the Asia-Pacific region with 7348 units sold.

- The Foam Can and Bottle Cooler was the most popular product among countries in the European region, with 10879 units sold.

- The 17oz Stainless Steel Sport Bottle was the most popular product among countries in the North American region, with 36740 units sold.

- The Protect smoke + CO White Wired Alarm USA was the most popular product among countries in the oceanic region, with 882 units sold.

- The SPF-15 Slim & Slender Lip Balm was the most popular product among countries in the South American region, with the actual number of units sold not mentioned.

The Asia-Pacific countries top purchases were stationary and other school supplies, Central American countries bought clothing and clothing accessories, African countries had varied top purchases, European countries had mixed purchases although did not purchase a lot of clothes or accessories instead favouring outdoor goods, North American countries purchased large quantities of a wide variety of products, Oceanic countries purchased bought a lot of electronic accessories, clothing and clothing accessories, and South American countries purchased a lot of hygeine/personal care products and clothing. By looking at both cities and countries, we are able to see that a significant amount of purchases did not have a listed country or city associated with their information. This is a gap in the dataset that will in the future need to be addressed. 


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

<!-- Top selling product by country query -->
SELECT 
    sub.product_name,
    sub.country,
    sub.total_quantity
FROM (
    SELECT 
        products_sales_report.product_name,  
        all_sessions.country,
        SUM(sales_by_sku.total_ordered) AS total_quantity,
        ROW_NUMBER() OVER (PARTITION BY all_sessions.country ORDER BY SUM(sales_by_sku.total_ordered) DESC) AS rn
    FROM all_sessions
        JOIN products_sales_report ON all_sessions.product_sku = products_sales_report.product_sku
        JOIN sales_by_sku ON sales_by_sku.product_sku = products_sales_report.product_sku
    WHERE product_name IS NOT NULL
        AND country IS NOT NULL
        AND products_sales_report.product_sku IS NOT NULL
        AND sales_by_sku.total_ordered IS NOT NULL
        AND product_name != 'not available in demo dataset' AND product_name != '(not set)'
        AND country != 'not available in demo dataset' AND country != '(not set)'
        AND products_sales_report.product_sku != 'not available in demo dataset' AND products_sales_report.product_sku != '(not set)'
    GROUP BY products_sales_report.product_name, all_sessions.country
) sub
WHERE sub.rn = 1
ORDER BY sub.country;

<!-- Top selling product by city query -->
SELECT 
    sub.product_name,
    sub.city,
    sub.total_quantity
FROM (
    SELECT 
        products_sales_report.product_name,  
        all_sessions.city,
        SUM(sales_by_sku.total_ordered) AS total_quantity,
        ROW_NUMBER() OVER (PARTITION BY all_sessions.city ORDER BY SUM(sales_by_sku.total_ordered) DESC) AS rn
    FROM all_sessions
        JOIN products_sales_report ON all_sessions.product_sku = products_sales_report.product_sku
        JOIN sales_by_sku ON sales_by_sku.product_sku = products_sales_report.product_sku
    WHERE product_name IS NOT NULL
        AND country IS NOT NULL
        AND products_sales_report.product_sku IS NOT NULL
        AND sales_by_sku.total_ordered IS NOT NULL
        AND product_name != 'not available in demo dataset' AND product_name != '(not set)'
        AND country != 'not available in demo dataset' AND country != '(not set)'
        AND products_sales_report.product_sku != 'not available in demo dataset' AND products_sales_report.product_sku != '(not set)'
    GROUP BY products_sales_report.product_name, all_sessions.city
) sub
WHERE sub.rn = 1
ORDER BY sub.city;


Answer:

Below is a list of the top-selling product from each city:

Men's Watershed Full Zip Hoodie Grey, Adelaide, 3
Canvas Tote Natural/Navy, Ahmedabad, 62
Men's 100% Cotton Short Sleeve Hero Tee Navy, Akron, 13
Hard Cover Journal, Amsterdam, 85
Foam Can and Bottle Cooler, Ann Arbor, 253
Colored Pencil Set, Antwerp, 3
Compact Selfie Stick, Ashburn, 5
Men's 100% Cotton Short Sleeve Hero Tee Navy, Asuncion, 13
Hard Cover Journal, Athens, 85
Protect Smoke + CO White Wired Alarm-USA, Atlanta, 126
G Noise-reducing Bluetooth Headphones, Auckland, 0
Leatherette Journal, Austin, 319
Blackout Cap, Avon, 189
Men's Short Sleeve Hero Tee Charcoal, Bandung, 6
Foam Can and Bottle Cooler, Bangkok, 253
26 oz Double Wall Insulated Bottle, Barcelona, 97
Men's Airflow 1/4 Zip Pullover Lapis, Beijing, 0
Custom Decals, Bellflower, 60
Learning Thermostat 3rd Gen-USA - Stainless Steel, Bellingham, 94
Collapsible Shopping Bag, Belo Horizonte, 43
Hard Cover Journal, Bengaluru, 85
Switch Tone Color Crayon Pen, Berlin, 34
Learning Thermostat 3rd Gen-USA - Stainless Steel, Bogota, 94
Ballpoint LED Light Pen, Boston, 456
Bottle Opener Clip, Boulder, 11
Learning Thermostat 3rd Gen-USA - Copper, Bratislava, 0
Twill Cap, Brisbane, 9
Leatherette Journal, Brno, 319
Men's 100% Cotton Short Sleeve Hero Tee Black, Brussels, 4
Canvas Tote Natural/Navy, Bucharest, 62
Recycled Mouse Pad, Budapest, 39
Sport Bag, Buenos Aires, 70
 Men's 100% Cotton Short Sleeve Hero Tee White, Burnaby, 15
Recycled Mouse Pad, Calgary, 39
 Cam Outdoor Security Camera - USA, Cambridge, 112
 Men's 100% Cotton Short Sleeve Hero Tee Navy, Chandigarh, 13
7" Dog Frisbee, Charlotte, 208
 Hard Cover Journal, Chennai, 275
 17oz Stainless Steel Sport Bottle, Chicago, 668
Android Men's Long Sleeve Badge Crew Tee Heather, Chico, 2
 Women's Scoop Neck Tee Black, Chuo, 0
 Men's 100% Cotton Short Sleeve Hero Tee Black, Cluj-Napoca, 4
 Men's 100% Cotton Short Sleeve Hero Tee White, Colombo, 15
 Device Stand, Columbia, 5
Rocket Flashlight, Copenhagen, 16
 Custom Decals, Cork, 30
 Kick Ball, Council Bluffs, 3
 Men's 100% Cotton Short Sleeve Hero Tee White, Courbevoie, 15
 Youth Short Sleeve Tee Red, Coventry, 0
 Men's Short Sleeve Hero Tee Black, Culiacan, 8
 Cam Indoor Security Camera - USA, Cupertino, 102
 Sunglasses, Dallas, 146
Crunch Noise Dog Toy, Denver, 14
 22 oz Water Bottle, Detroit, 23
 Men's 100% Cotton Short Sleeve Hero Tee Black, Doha, 4
Colored Pencil Set, Druid Hills, 3
Leatherette Journal, Dubai, 319
 17oz Stainless Steel Sport Bottle, Dublin, 334
25L Classic Rucksack, East Lansing, 7
 Men's Short Sleeve Hero Tee Charcoal, Eau Claire, 12
 Stylus Pen w/ LED Light, Edmonton, 2
 Men's Microfiber 1/4 Zip Pullover Blue/Indigo, Forest Park, 0
Android Luggage Tag, Fortaleza, 0
 Bib White, Frankfurt, 7
 Protect Smoke + CO White Battery Alarm-USA, Fremont, 24
Windup Android, Ghent, 16
Android Wool Heather Cap Heather/Black, Goose Creek, 0
 Men's Quilted Insulated Vest Black, Greer, 1
 22 oz Water Bottle, Gurgaon, 40
 Blackout Cap, Hamburg, 189
26 oz Double Wall Insulated Bottle, Hanoi, 97
 Men's Vintage Badge Tee White, Hayward, 0
Android Onesie Gold, Helsinki, 6
Keyboard DOT Sticker, Ho Chi Minh City, 53
Leatherette Journal, Hong Kong, 319
 Blackout Cap, Houston, 189
 Hard Cover Journal, Hyderabad, 255
 Men's Vintage Henley, Iasi, 2
Ballpoint Stylus Pen, Indianapolis, 0
 Hard Cover Journal, Indore, 85
Bottle Opener Clip, Ipoh, 11
Android 17oz Stainless Steel Sport Bottle, Irvine, 167
 Hard Cover Journal, Istanbul, 85
 Women's Short Sleeve Hero Tee White, Izmir, 4
 Device Stand, Jacksonville, 5
 Men's 100% Cotton Short Sleeve Hero Tee White, Jaipur, 15
22 oz  Bottle Infuser, Jakarta, 100
 Youth Short Sleeve Tee White, Jersey City, 7
Satin Black Ballpoint Pen, Kalamazoo, 105
 Women's Short Sleeve Hero Tee Red Heather, Kansas City, 1
 Men's Long Sleeve Raglan Ocean Blue, Karachi, 0
 Tri-blend Hoodie Grey, Kharagpur, 11
 Women's V-Neck Tee Charcoal, Kharkiv, 4
Keyboard DOT Sticker, Kiev, 53
 Learning Thermostat 3rd Gen-USA - Stainless Steel, Kirkland, 94
Keyboard DOT Sticker, Kitchener, 53
 Leather Journal, Kolkata, 82
26 oz Double Wall Insulated Bottle, Kuala Lumpur, 97
22 oz  Bottle Infuser, La Victoria, 50
 Men's 100% Cotton Short Sleeve Hero Tee Navy, LaFayette, 13
 Tube Power Bank, Lahore, 1
Recycled Mouse Pad, Lake Oswego, 39
 Men's 100% Cotton Short Sleeve Hero Tee Black, Las Vegas, 4
 Blackout Cap, Lisbon, 189
 17oz Stainless Steel Sport Bottle, London, 668
26 oz Double Wall Insulated Bottle, Longtan District, 97
 17oz Stainless Steel Sport Bottle, Los Angeles, 1336
 Men's Vintage Tank, Lucknow, 0
 Tote Bag, Madison, 3
SPF-15 Slim & Slender Lip Balm, Madrid, 120
 G Noise-reducing Bluetooth Headphones, Makati, 0
 G Noise-reducing Bluetooth Headphones, Manchester, 0
8 pc Android Sticker Sheet, Manila, 19
Android Men's Vintage Henley, Maracaibo, 2
 Men's Microfiber 1/4 Zip Pullover Blue/Indigo, Marlboro, 0
 Men's 100% Cotton Short Sleeve Hero Tee Black, Marseille, 4
Bottle Opener Clip, Medellin, 11
 Hard Cover Journal, Melbourne, 170
 Cam Indoor Security Camera - USA, Menlo Park, 102
 Learning Thermostat 3rd Gen-USA - Stainless Steel, Mexico City, 94
22 oz  Bottle Infuser, Milan, 100
Crunch Noise Dog Toy, Milpitas, 14
 Sunglasses, Minato, 146
 Men's Vintage Badge Tee White, Minneapolis, 0
Collapsible Shopping Bag, Mississauga, 43
 Men's 100% Cotton Short Sleeve Hero Tee White, Montevideo, 15
Android 17oz Stainless Steel Sport Bottle, Montreal, 167
Collapsible Shopping Bag, Montreuil, 43
SPF-15 Slim & Slender Lip Balm, Moscow, 60
 Cam Indoor Security Camera - USA, Mountain View, 2448
Leatherette Journal, Mumbai, 319
Foam Can and Bottle Cooler, Munich, 253
 Women's Convertible Vest-Jacket Sea Foam Green, Nagoya, 0
 Men's Long & Lean Tee Grey, Nairobi, 0
 Men's Short Sleeve Hero Tee Charcoal, Nanded, 6
 Learning Thermostat 3rd Gen-USA - Stainless Steel, Nashville, 94
 Hard Cover Journal, New Delhi, 85
 17oz Stainless Steel Sport Bottle, New York, 1002
Suitcase Organizer Cubes, Norfolk, 5
Keyboard DOT Sticker, Oakland, 53
 Men's Short Sleeve Hero Tee Black, Orlando, 8
 Protect Smoke + CO White Wired Alarm-USA, Osaka, 42
 Men's 100% Cotton Short Sleeve Hero Tee Red, Oslo, 1
 5-Panel Snapback Cap, Oviedo, 7
 17oz Stainless Steel Sport Bottle, Palo Alto, 1002
 Men's Convertible Vest-Jacket Pewter, Panama City, 0
Android 17oz Stainless Steel Sport Bottle, Paris, 167
22 oz Android Bottle, Patna, 2
 Custom Decals, Perth, 30
 Women's Short Sleeve Hero Tee Black, Petaling Jaya, 3
Leatherette Journal, Philadelphia, 319
 Cam Indoor Security Camera - USA, Phoenix, 102
 Men's 100% Cotton Short Sleeve Hero Tee Red, Piscataway Township, 1
 Cam Outdoor Security Camera - USA, Pittsburgh, 112
Electronics Accessory Pouch, Pleasanton, 0
 Men's 100% Cotton Short Sleeve Hero Tee Navy, Portland, 13
 Men's 100% Cotton Short Sleeve Hero Tee Black, Poznan, 4
 Hard Cover Journal, Pozuelo de Alarcon, 85
 Men's 100% Cotton Short Sleeve Hero Tee White, Prague, 30
Ballpoint LED Light Pen, Pune, 456
 Custom Decals, Quebec City, 30
 Men's 100% Cotton Short Sleeve Hero Tee Navy, Quezon City, 13
22 oz  Bottle Infuser, Redmond, 50
 Cam Indoor Security Camera - USA, Redwood City, 102
 17oz Stainless Steel Sport Bottle, Rexburg, 334
Micro Wireless Earbud, Richardson, 4
 Blackout Cap, Rio de Janeiro, 189
Leatherette Journal, Riyadh, 319
Leatherette Journal, Rome, 319
 Men's Vintage Badge Tee Black, Rosario, 1
 Blackout Cap, Sacramento, 189
Leatherette Journal, Saint Petersburg, 319
 17oz Stainless Steel Sport Bottle, Salem, 668
 Car Clip Phone Holder, Salford, 2
Android 17oz Stainless Steel Sport Bottle, San Antonio, 334
Foam Can and Bottle Cooler, San Bruno, 253
Recycled Mouse Pad, San Diego, 39
Android 17oz Stainless Steel Sport Bottle, San Francisco, 1002
Ballpoint LED Light Pen, San Jose, 456
 Tri-blend Hoodie Grey, San Mateo, 11
 Device Holder Sticky Pad, San Salvador, 0
Ballpoint LED Light Pen, Santa Clara, 456
SPF-15 Slim & Slender Lip Balm, Santa Fe, 60
 Slim Utility Travel Bag, Santa Monica, 13
Sport Bag, Santiago, 70
7" Dog Frisbee, Sao Paulo, 104
Ballpoint LED Light Pen, Seattle, 456
Ballpoint LED Light Pen, Seoul, 456
 Blackout Cap, Sherbrooke, 189
 Men's Vintage Badge Tee Sage, Shibuya, 0
 17oz Stainless Steel Sport Bottle, Shinjuku, 334
 Blackout Cap, Singapore, 189
 Men's Vintage Badge Tee White, South El Monte, 0
 Rucksack, South San Francisco, 11
 2200mAh Micro Charger, St. John's, 0
 Bib Red, St. Louis, 7
 Men's  Zip Hoodie, Stanford, 3
Metal Texture Roller Pen, Stockholm, 48
 Cam Outdoor Security Camera - USA, Sunnyvale, 672
 Blackout Cap, Sydney, 189
Metal Texture Roller Pen, Taguig, 48
Android Wool Heather Cap Heather/Black, Tampa, 0
Android 17oz Stainless Steel Sport Bottle, Tel Aviv-Yafo, 334
Windup Android, Tempe, 16
 Laptop and Cell Phone Stickers, The Dalles, 13
 Bib Red, Thessaloniki, 7
 Screen Cleaning Cloth, Timisoara, 12
 17oz Stainless Steel Sport Bottle, Toronto, 334
Waterproof Backpack, University Park, 5
 Hard Cover Journal, Vancouver, 85
 Men's 100% Cotton Short Sleeve Hero Tee White, Vienna, 15
 Snapback Hat Black, Villeneuve-d'Ascq, 1
 Men's 100% Cotton Short Sleeve Hero Tee Red, Vilnius, 1
 Canvas Tote Natural/Navy, Vladivostok, 62
Keyboard DOT Sticker, Warsaw, 106
7" Dog Frisbee, Washington, 104
 Men's Long Sleeve Raglan Ocean Blue, Waterloo, 0
Colored Pencil Set, Wellesley, 3
 Men's Long Sleeve Raglan Ocean Blue, Westlake Village, 0
Sport Bag, Westville, 70
 Laptop and Cell Phone Stickers, Wrexham, 13
 Slim Utility Travel Bag, Yokohama, 13
 Men's Watershed Full Zip Hoodie Grey, Zagreb, 6
 Hard Cover Journal, Zhongli District, 85
Sport Bag, Zurich, 140


Below is a list for the topselling-product from each country

22 oz  Bottle Infuser, Albania, 50
 Twill Cap, Algeria, 9
 Hard Cover Journal, Argentina, 170
Windup Android, Armenia, 16
22 oz  Bottle Infuser, Australia, 250
 Custom Decals, Austria, 30
 Twill Cap, Bahamas, 3
 Men's 100% Cotton Short Sleeve Hero Tee White, Bahrain, 15
Keyboard DOT Sticker, Bangladesh, 53
 Stylus Pen w/ LED Light, Belarus, 2
 17oz Stainless Steel Sport Bottle, Belgium, 334
 Men's Short Sleeve Hero Tee Black, Bolivia, 8
 Toddler Short Sleeve Tee Red, Bosnia & Herzegovina, 2
Waterproof Backpack, Botswana, 5
Foam Can and Bottle Cooler, Brazil, 506
 Toddler Short Sleeve Tee Red, Brunei, 2
Spiral Notebook and Pen Set, Bulgaria, 69
 Twill Cap, Cambodia, 9
 17oz Stainless Steel Sport Bottle, Canada, 1002
Sport Bag, Chile, 70
Android Hard Cover Journal, China, 15
22 oz  Bottle Infuser, Colombia, 200
 Power Bank, Costa Rica, 56
Ballpoint LED Light Pen, Croatia, 456
Suitcase Organizer Cubes, Cyprus, 5
 Hard Cover Journal, Czechia, 510
 Custom Decals, Cote d‚ÄôIvoire, 30
 Hard Cover Journal, Denmark, 425
22 oz  Bottle Infuser, Dominican Republic, 50
 Men's  Zip Hoodie, Ecuador, 3
22 oz  Bottle Infuser, Egypt, 50
 Men's 100% Cotton Short Sleeve Hero Tee Navy, El Salvador, 13
 Men's Short Sleeve Hero Tee Black, Estonia, 8
 Hard Cover Journal, Ethiopia, 85
Keyboard DOT Sticker, Finland, 53
Foam Can and Bottle Cooler, France, 253
 Custom Decals, Georgia, 90
Ballpoint LED Light Pen, Germany, 1368
 Men's 100% Cotton Short Sleeve Hero Tee White, Ghana, 15
 Men's Vintage Henley, Gibraltar, 2
Leatherette Journal, Greece, 319
 Blackout Cap, Guatemala, 189
 Cam Outdoor Security Camera - USA, Honduras, 112
Leatherette Journal, Hong Kong, 319
22 oz  Bottle Infuser, Hungary, 50
 Insulated Stainless Steel Bottle, Iceland, 0
 17oz Stainless Steel Sport Bottle, India, 668
 Spiral Journal with Pen, Indonesia, 290
 Men's Vintage Badge Tee Black, Iraq, 1
 17oz Stainless Steel Sport Bottle, Ireland, 334
Android 17oz Stainless Steel Sport Bottle, Israel, 334
Leatherette Journal, Italy, 638
 Men's  Zip Hoodie, Jamaica, 3
Ballpoint LED Light Pen, Japan, 912
 Tri-blend Hoodie Grey, Jersey, 11
Suitcase Organizer Cubes, Jordan, 5
Collapsible Shopping Bag, Kazakhstan, 43
 Women's Short Sleeve Hero Tee Grey, Kenya, 2
 Men's Vintage Badge Tee Black, Kosovo, 1
 17oz Stainless Steel Sport Bottle, Kuwait, 334
Rocket Flashlight, Kyrgyzstan, 16
 Hard Cover Journal, Laos, 170
 Twill Cap, Latvia, 9
22 oz  Bottle Infuser, Lithuania, 50
 Twill Cap, Macau, 3
22 oz  Bottle Infuser, Macedonia (FYROM), 50
Ballpoint LED Light Pen, Malaysia, 456
 Men's Performance Full Zip Jacket Black, Maldives, 1
 Custom Decals, Mali, 30
 Women's Fleece Hoodie Black, Malta, 0
 Men's Fleece Hoodie Black, Martinique, 2
 Power Bank, Mauritius, 56
Ballpoint LED Light Pen, Mexico, 456
 Custom Decals, Moldova, 30
 Custom Decals, Montenegro, 30
 Learning Thermostat 3rd Gen-USA - Stainless Steel, Morocco, 94
22 oz  Bottle Infuser, Myanmar (Burma), 50
22 oz  Bottle Infuser, Nepal, 50
Ballpoint LED Light Pen, Netherlands, 456
22 oz  Bottle Infuser, New Zealand, 150
Compact Selfie Stick, Nicaragua, 5
Keyboard DOT Sticker, Nigeria, 53
22 oz  Bottle Infuser, Norway, 50
 Hard Cover Journal, Oman, 85
22 oz  Bottle Infuser, Pakistan, 200
 Laptop Backpack, Palestine, 3
22 oz  Bottle Infuser, Panama, 50
 Hard Cover Journal, Papua New Guinea, 85
 Men's 100% Cotton Short Sleeve Hero Tee Navy, Paraguay, 13
 Hard Cover Journal, Peru, 85
Android 17oz Stainless Steel Sport Bottle, Philippines, 167
Keyboard DOT Sticker, Poland, 106
 Blackout Cap, Portugal, 189
 Custom Decals, Puerto Rico, 30
 Men's 100% Cotton Short Sleeve Hero Tee Black, Qatar, 4
Foam Can and Bottle Cooler, Romania, 253
Leatherette Journal, Russia, 319
 Doodle Decal, Reunion, 34
22 oz  Bottle Infuser, San Marino, 50
 17oz Stainless Steel Sport Bottle, Saudi Arabia, 334
Switch Tone Color Crayon Pen, Serbia, 34
 17oz Stainless Steel Sport Bottle, Singapore, 334
 Stylus Pen w/ LED Light, Sint Maarten, 2
 Hard Cover Journal, Slovakia, 170
 Hard Cover Journal, Slovenia, 85
Sport Bag, South Africa, 70
Ballpoint LED Light Pen, South Korea, 456
Ballpoint LED Light Pen, Spain, 456
 Custom Decals, Sri Lanka, 30
 Cam Outdoor Security Camera - USA, Sweden, 112
Ballpoint LED Light Pen, Switzerland, 456
Ballpoint LED Light Pen, Taiwan, 456
 Twill Cap, Tanzania, 9
Foam Can and Bottle Cooler, Thailand, 253
 Hard Cover Journal, Trinidad & Tobago, 85
22 oz  Bottle Infuser, Tunisia, 50
 Hard Cover Journal, Turkey, 255
 Laptop and Cell Phone Stickers, Uganda, 13
 Hard Cover Journal, Ukraine, 255
Leatherette Journal, United Arab Emirates, 319
 Hard Cover Journal, United Kingdom, 1785
 17oz Stainless Steel Sport Bottle, United States, 6346
22 oz  Bottle Infuser, Uruguay, 50
 Hard Cover Journal, Venezuela, 170
26 oz Double Wall Insulated Bottle, Vietnam, 97
 Screen Cleaning Cloth, Zimbabwe, 12



Notable trends among data :
The data reveals some interesting trends that can help in understanding the consumer behavior across different regions. The top-selling product across 22 cities were cotton t-shirts, but only among 5 countries. Interestingly, the top selling countries were all non-western nations, possibly due to the hot and humid climate in those regions. The hard cover and leatherette journals, on the other hand, emerged as popular products within European cities. The US market, both overall and within individual cities, showed significant consumer activity and a high demand for top-selling products. However, it is important to note that there were some cities and countries where no products were purchased, resulting in no top-selling product. 

The insights gained from analyzing these trends could prove to be a valuable resource for businesses looking to create targeted marketing strategies and product offerings specific to certain city-regions. While there is still a need for more comprehensive data collection to draw conclusive insights about product preferences, one noticeable trend that emerges is that some of the more popular items, such as sport bottles and security cameras, are sold at a very low profit point. It is important to note that these products are commonly used as loss-leaders to attract customers to purchase supplementary products. However, further consideration may be required to evaluate whether these products are effectively driving overall sales and if a different strategy may be more effective.



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

SELECT all_sessions.city, 
	SUM(product_sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN product_sales_report ON all_sessions.product_SKU = product_sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.city IS NOT NULL
		   AND all_sessions.city != 'not available in demo dataset' 
GROUP BY all_sessions.city
ORDER BY revenue DESC;

SELECT all_sessions.country, 
	SUM(product_sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN product_sales_report ON all_sessions.product_SKU = product_sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.country IS NOT NULL
		   AND all_sessions.country != 'not available in demo dataset' 
GROUP BY all_sessions.country
ORDER BY revenue DESC;


Answer: 

While examining the database, I found that the available data was not consistent enough to generate a comprehensive summary of revenue generated from each city and country. Unfortunately, a significant number of results were missing city or country information (returning as null or not available in demo dataset), which prevented me from drawing any meaningful conclusions. Therefore, it is imperative that additional steps are taken in the data collection process to ensure that customer location is accurately and consistently recorded. With more complete and reliable data, we can gain a deeper understanding of the ecommerce store's performance and make informed decisions to improve its revenue generation. 
