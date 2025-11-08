The data pipeline for the financial dashboard

   An entire financial data pipeline that gets data from three financial APIs (Alpha Vantage, NewsAPI, and FRED), stores it in a PostgreSQL staging database, puts it into a clear star schema for analysis, and shows the results on a Flask online dashboard that you can interact with.

   Things to Do

   - Data Extraction: Gets stock prices, news stories, and economic indicators from APIs that are outside of the system  - Staging Database: Keeps raw data with simply text columns for API integration  - Data Transformation: Uses SQL to set up a correct star schema  - Interactive Dashboard: Shows data on the web using Plotly charts and updates in real time.  - Automated Pipeline: Takes care of the whole data flow from start to finish.

   ## 📋 What You Need

   Python 3.8 is the least you need.

   PostgreSQL 12 API keys are the bare minimum.   
   
      - [Alpha Vantage] Information about stocks

     - [NewsAPI](https://newsapi.org/register)  Money News

     - [FRED API Documentation](https://fred.stlouisfed.org/docs/api/api_key.html): News on the economy

 Set up

   1. Make a copy of the repository: cd Dashboard_Project

   2. Set up the Python requirements.

   3. Set up the PostgreSQL database  To set up a database and a user, use the command createdb financial_dashboard psql -c.  CREATE USER dashboard_user;  ALTER USER dashboard_user WITH PASSWORD 'your_password'; psql -c "GIVE ALL PRIVILEGES ON DATABASE financial_dashboard TO dashboard_user;

   Run the script to make the database work.  Run psql with the command -d financial_dashboard and the file -f setup_database.sql.

   ### 4. Set up Environment Variables ```bash # Copy the environment template cp .env.example .env 

   Add your login information to the .env file:

   DATABASE_URL=postgresql://dashboard_user:your_password@localhost:5432/financial_dashboard  your_alpha_vantage_key = ALPHA_VANTAGE_API_KEY  YOUR_NEWSAPI_KEY is your newsapi key.  FRED_API_KEY=your_fred_key

   ## How to Build a Project

   Dashboard_Project/ ├── pipeline.py    The main script for the ELT pipeline    setup_database.sql for the Flask web dashboard    Using transform.sql, Requirements.txt, Python requirements, .env.example, and data update queries to set up the database schema.    A structure for environmental factors    HTML template for the pages that show errors and the dashboard    Template for an error page  └── static/ └── css/ └── style.css    Making the dashboard look better

   ## Rocket: How to Use the App

   To start the Data Pipeline, type python pipeline.py.

   This will work:

   Get stock prices for AAPL, MSFT, GOOGL, and TSLA from Alpha Vantage.

   Use NewsAPI to get the news stories from the last seven days.

   3. Get economic data from FRED, such as GDP, UNRATE, CPIAUCSL, and FEDFUNDS.

   4. Move all of the data to staging tables.

   5. Change and add to the analytics star schema.

   ### Step 2: Start the Web Dashboard by typing "bash python app.py"

   To get to the dashboard, open your browser and type "http://localhost:5000."

   ## 📊 Dashboard Features

   Summary Metrics: the total number of monitored stocks, daily gainers and decliners, news pieces, and economic data indicators.

   - Trends in Stock Prices: Line charts that show how prices have changed over the past 30 days.  - Trading Volume: Bar charts that show how many of each security were traded.   Update real-time data every five minutes  - The ability to zoom in and out

   - Current Financial News: The most recent news about money, with the ability to filter by symbol    - Economic Indicators: Important economic measures that look at trends

   ## Automatic Execution

   ### Do it by hand: ```bash python pipeline.py ```

   ### Scheduled Execution (Linux/macOS) ```bash # Change crontab crontab -e```

   Set up hourly execution    Every hour at the start of the hour    Go to /path/to/Dashboard_Project and then run /usr/bin/python3 pipeline.py.

   ### Tasks on Windows

   Set up Task Scheduler to run "pipeline.py" on a regular basis.

   ## Database Schema

   ### Staging Schema (Raw Data): `raw_stock_prices` has stock data from Alpha Vantage; `raw_news_articles` has article data from NewsAPI; and `raw_econ_data` has economic indicators from FRED.

   ### Analytics Schema (Star Schema) - Dimensions: "dim_date," "dim_security," and "dim_economic_indicator"  - -  "fact_prices," "fact_news," and "fact_economics" are all facts.

   ## Settings

   ### Changing Stock Symbols

   Change the `.env` file's `BASE_STOCK_SYMBOLS` variable to the following:  The base stock symbols are AAPL, MSFT, GOOGL, TSLA, AMZN, and NVDA.

   ### Making the Most of Economic Indicators

   Change the `ECONOMIC_SERIES` variable in the `.env` file to this: ```bash ECONOMIC_SERIES=GDP,UNRATE,CPIAUCSL,FEDFUNDS,DGS10```

   ## 📝 API Rate Limits

   - Alpha Vantage: Five calls every minute (free tier)

   - NewsAPI: 1,000 requests every day (free tier)

   - FRED API: 120 requests every minute (no API key needed)

   The pipeline automatically follows rate limits by making sure there are enough time between requests.

   ## 🔍 Fixing Problems

   ### Common Problems

   1. Database Connection Errors—check to see if PostgreSQL is working, double-check the DATABASE_URL in the .env file, and make sure the database user has the right rights.

   2. API Key Errors—Check that all API keys are still valid and have enough capacity.

   3. Pipeline Failures—To learn more about the problems, check out `pipeline.log`.

      - Make sure your internet connection is working.  - Run setup_database.sql to make sure the database tables are there.

   4. The dashboard isn't loading. Check to see if the Flask server is running on port 5000.

      - Check that the analytics tables have data in them.   - Check the browser console for any JavaScript issues.

   ###  Check the database's integrity.  Make sure that the data for staging has been loaded.

   EXECUTE COUNT(*) FROM staging.raw_stock_prices; EXECUTE COUNT(*) FROM staging.raw_news_articles;  EXECUTE COUNT(*) FROM staging.raw_econ_data;

   Make sure the change is finished.

   SELECT COUNT(*) FROM analytics.fact_prices; SELECT COUNT(*) FROM analytics.fact_news; EXECUTE COUNT(*) FROM analytics.fact_economics;

   ## 🔐 Things to Think About to Stay Safe

   - Don't put API keys in version control; instead, keep them in a `.env` file.

   - Use a database user who can only read the dashboard.

   - Allow SSL connections in production.  - Change your API keys often.

   ## 📈 Getting Better Results

   - Database indexes that speed up queries - Connection pooling for web apps - Loading data in small chunks to cut down on redundancy - Making charts easier to read for big datasets

   ## 🤝 Helping Out

   1. Make a copy of the repository.

   2. Make a branch for the feature.

      Do a lot of testing.

   Make a request to withdraw.

   ## License

   The MIT License says that this project can use it.

   ## Help

   1. Check the troubleshooting section and 2. look at `pipeline.log` for details about the problem.

   3. Check that all conditions are met.

   4. Make sure that your API key is still valid and that you haven't used up all of your quotas.
