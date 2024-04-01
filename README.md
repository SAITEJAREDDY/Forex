Step 1: Start importing relevant libraries like Polygon to get data from Polygon Library RestClient to get real-time data. Import database libraries SQLite and PyMongo to manipulate the data and insert it into the Database. 

Step 2: Initiate SQLite and Mongo DB database connection and creation

Step 3: Through RestClient functionality initialize the Object/Instance
client_polygon = RESTClient('**KEY**')

Step 4: Create a loop with a range of 2 hours(7200 sec ) and call function “get_real_time_currency_conversion” on the object of RestClient to use appropriate currency symbols to get data.
EURUSD = client_polygon.get_real_time_currency_conversion("EUR", "USD")

Step 5: Get attribute data of Forex, timestamp of the transaction. 

FXrate_EURUSD = EURUSD.converted
FX_timestamp_EURUSD = datetime.utcfromtimestamp(EURUSD.last.timestamp / 1000)

Step 6: Convert the timestamp data from UTC to date time string.

TS24_EURUSD = FX_timestamp_EURUSD.strftime('%Y-%m-%d %H:%M:%S')

Step 7: Repeat steps 4-6 for different Forex inputs replacing “EUR” with from value and “USD” with Value.

Step 8: Capture the current time and convert the format to string.

 writing_time = datetime.utcnow()
 DB_time = writing_time.strftime('%Y-%m-%d %H:%M:%S')

Step 9: Insert data into SQLite database, repeat for all currencies.
cursor_sqlite.execute('''INSERT INTO conversion_data 
                                  (source_currency, target_currency, converted_value, timestamp, writing_time) 
                                  VALUES (?, ?, ?, ?, ?)''', 
                                  ('EUR', 'USD', FXrate_EURUSD, TS24_EURUSD,            DB_time))

Step 10: Define a dictionary  data_EURUSD with appropriate columns, Insert data into MongoDB.
collection_mongodb.insert_one(data_EURUSD)

Step 11: Catch any errors and set the delay of 1 sec at the close of the loop. Close the DB connections. 
Step 12: Initialize MongoDB connection and define the dictionary to store sum and count variables.
Step 13: Capture the count of FX rate values and the sum of all FX values. Calculate Avg by dividing the sum by the count. 
