Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT all_sessions.city, 
	SUM(sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN sales_report ON all_sessions.product_SKU = sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.city IS NOT NULL
		   AND all_sessions.city != 'not available in demo dataset' 
GROUP BY all_sessions.city
ORDER BY revenue DESC;

SELECT all_sessions.country, 
	SUM(sales_report.total_ordered * analytics.unit_price) AS revenue
FROM all_sessions 
	JOIN sales_report ON all_sessions.product_SKU = sales_report.product_SKU
	JOIN analytics ON all_sessions.full_visitor_id = analytics.full_visitor_id 
		AND all_sessions.visit_id = analytics.visit_id
	WHERE all_sessions.country IS NOT NULL
		   AND all_sessions.country != 'not available in demo dataset' 
GROUP BY all_sessions.country
ORDER BY revenue DESC;



Answer: 
United States - $6,032,642.25
Mountain View - $1,039,252.88




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
SELECT 
    sub.product_name,
    sub.country,
    sub.total_quantity
FROM (
    SELECT 
        products.product_name,  
        all_sessions.country,
        SUM(sales_by_sku.total_ordered) AS total_quantity,
        ROW_NUMBER() OVER (PARTITION BY all_sessions.country ORDER BY SUM(sales_by_sku.total_ordered) DESC) AS rn
    FROM all_sessions
        JOIN products ON all_sessions.product_sku = products.sku
        JOIN sales_by_sku ON sales_by_sku.product_sku = products.sku
    WHERE product_name IS NOT NULL
        AND country IS NOT NULL
        AND sku IS NOT NULL
        AND total_ordered IS NOT NULL
        AND product_name != 'not available in demo dataset' AND product_name != '(not set)'
        AND country != 'not available in demo dataset' AND country != '(not set)'
        AND sku != 'not available in demo dataset' AND sku != '(not set)'
    GROUP BY products.product_name, all_sessions.country
) sub
WHERE sub.rn = 1
ORDER BY sub.country;

SELECT 
    sub.product_name,
    sub.city,
    sub.total_quantity
FROM (
    SELECT 
        products.product_name,  
        all_sessions.city,
        SUM(sales_by_sku.total_ordered) AS total_quantity,
        ROW_NUMBER() OVER (PARTITION BY all_sessions.city ORDER BY SUM(sales_by_sku.total_ordered) DESC) AS rn
    FROM all_sessions
        JOIN products ON all_sessions.product_sku = products.sku
        JOIN sales_by_sku ON sales_by_sku.product_sku = products.sku
    WHERE product_name IS NOT NULL
        AND city IS NOT NULL
        AND sku IS NOT NULL
        AND total_ordered IS NOT NULL
        AND product_name != 'not available in demo dataset' AND product_name != '(not set)'
        AND city != 'not available in demo dataset' AND city != '(not set)'
        AND sku != 'not available in demo dataset' AND sku != '(not set)'
    GROUP BY products.product_name, all_sessions.city
) sub
WHERE sub.rn = 1
ORDER BY sub.city;


Answer:

Below is a list of the top-selling product from each city:

Product Name, City, Total Quantity Sold
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









**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:

