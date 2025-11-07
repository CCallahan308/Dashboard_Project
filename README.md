# Financial Dashboard Data Pipeline

A complete financial data pipeline that extracts data from three financial APIs (Alpha Vantage, NewsAPI, FRED), loads raw data into a PostgreSQL staging database, transforms it into a clean analytics-ready star schema, and displays results in an interactive Flask web dashboard.

## 🚀 Features

- **Data Extraction**: Fetches stock prices, news articles, and economic indicators from external APIs
- **Staging Database**: Raw data storage with text-only columns for API integration
- **Data Transformation**: SQL-based ELT process creating a proper star schema
- **Interactive Dashboard**: Web-based visualization with Plotly charts and real-time updates
- **Automated Pipeline**: Complete end-to-end data flow execution

## 📋 Prerequisites

- Python 3.8+
- PostgreSQL 12+
- API Keys:
  - [Alpha Vantage](https://www.alphavantage.co/support/#api-key) (Stock Data)
  - [NewsAPI](https://newsapi.org/register) (Financial News)
  - [FRED API](https://fred.stlouisfed.org/docs/api/api_key.html) (Economic Data)

## 🛠️ Installation

### 1. Clone Repository
```bash
git clone <repository-url>
cd Dashboard_Project
```

### 2. Install Python Dependencies
```bash
pip install -r requirements.txt
```

### 3. Setup PostgreSQL Database
```bash
# Create database and user
createdb financial_dashboard
createuser dashboard_user
psql -c "ALTER USER dashboard_user PASSWORD 'your_password';"
psql -c "GRANT ALL PRIVILEGES ON DATABASE financial_dashboard TO dashboard_user;"

# Run database setup script
psql -d financial_dashboard -f setup_database.sql
```

### 4. Configure Environment Variables
```bash
# Copy environment template
cp .env.example .env

# Edit .env with your credentials:
# DATABASE_URL=postgresql://dashboard_user:your_password@localhost:5432/financial_dashboard
# ALPHA_VANTAGE_API_KEY=your_alpha_vantage_key
# NEWSAPI_KEY=your_newsapi_key
# FRED_API_KEY=your_fred_key
```

## 🏗️ Project Structure

```
Dashboard_Project/
├── pipeline.py              # Main ELT pipeline script
├── app.py                   # Flask web dashboard
├── setup_database.sql       # Database schema setup
├── transform.sql            # Data transformation queries
├── requirements.txt         # Python dependencies
├── .env.example            # Environment variables template
├── templates/
│   ├── index.html          # Dashboard HTML template
│   └── error.html          # Error page template
└── static/
    └── css/
        └── style.css       # Dashboard styling
```

## 🚀 Running the Application

### Step 1: Execute the Data Pipeline
```bash
python pipeline.py
```

This will:
1. Extract stock data from Alpha Vantage (AAPL, MSFT, GOOGL, TSLA)
2. Extract news articles from NewsAPI (last 7 days)
3. Extract economic data from FRED (GDP, UNRATE, CPIAUCSL, FEDFUNDS)
4. Load all data into staging tables
5. Transform and populate analytics star schema

### Step 2: Start the Web Dashboard
```bash
python app.py
```

Open your browser and navigate to `http://localhost:5000` to view the dashboard.

## 📊 Dashboard Features

### Summary Metrics
- Total securities tracked
- Daily gainers/losers
- News article count
- Economic data points

### Interactive Charts
- **Stock Price Trends**: Line charts showing 30-day price history
- **Trading Volume**: Bar charts displaying volume by security
- Real-time data refresh (every 5 minutes)
- Zoom and pan capabilities

### Data Tables
- **Recent News**: Latest financial news with symbol filtering
- **Economic Indicators**: Key economic metrics with trend analysis

## 🔄 Automated Execution

### Manual Execution
```bash
python pipeline.py
```

### Scheduled Execution (Linux/macOS)
```bash
# Edit crontab
crontab -e

# Add hourly execution
0 * * * * cd /path/to/Dashboard_Project && /usr/bin/python3 pipeline.py >> pipeline.log 2>&1
```

### Scheduled Execution (Windows)
Use Task Scheduler to run `pipeline.py` at regular intervals.

## 🗄️ Database Schema

### Staging Schema (Raw Data)
- `raw_stock_prices`: Alpha Vantage stock data
- `raw_news_articles`: NewsAPI article data
- `raw_econ_data`: FRED economic indicators

### Analytics Schema (Star Schema)
- **Dimensions**: `dim_date`, `dim_security`, `dim_economic_indicator`
- **Facts**: `fact_prices`, `fact_news`, `fact_economics`

## 🔧 Configuration

### Customizing Stock Symbols
Edit the `BASE_STOCK_SYMBOLS` variable in `.env`:
```bash
BASE_STOCK_SYMBOLS=AAPL,MSFT,GOOGL,TSLA,AMZN,NVDA
```

### Customizing Economic Indicators
Edit the `ECONOMIC_SERIES` variable in `.env`:
```bash
ECONOMIC_SERIES=GDP,UNRATE,CPIAUCSL,FEDFUNDS,DGS10
```

## 📝 API Rate Limits

- **Alpha Vantage**: 5 calls/minute (free tier)
- **NewsAPI**: 1,000 requests/day (free tier)
- **FRED API**: 120 requests/minute (no API key required)

The pipeline automatically respects rate limits with appropriate delays.

## 🔍 Troubleshooting

### Common Issues

1. **Database Connection Errors**
   - Verify PostgreSQL is running
   - Check DATABASE_URL in .env file
   - Ensure database user has proper permissions

2. **API Key Errors**
   - Verify all API keys are valid
   - Check API key hasn't expired
   - Ensure API keys have sufficient quota

3. **Pipeline Failures**
   - Check `pipeline.log` for detailed error messages
   - Verify internet connectivity
   - Ensure database tables exist (run setup_database.sql)

4. **Dashboard Not Loading**
   - Check Flask server is running on port 5000
   - Verify analytics tables have data
   - Check browser console for JavaScript errors

### Database Health Check
```sql
-- Verify staging data was loaded
SELECT COUNT(*) FROM staging.raw_stock_prices;
SELECT COUNT(*) FROM staging.raw_news_articles;
SELECT COUNT(*) FROM staging.raw_econ_data;

-- Verify transformation completed
SELECT COUNT(*) FROM analytics.fact_prices;
SELECT COUNT(*) FROM analytics.fact_news;
SELECT COUNT(*) FROM analytics.fact_economics;
```

## 🔐 Security Considerations

- Store API keys in `.env` file (never commit to version control)
- Use read-only database user for dashboard
- Enable SSL connections in production
- Regularly rotate API keys

## 📈 Performance Optimization

- Database indexes included for optimal query performance
- Connection pooling for web application
- Incremental data loading to avoid duplicates
- Chart rendering optimization for large datasets

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

For issues and questions:
1. Check the troubleshooting section
2. Review `pipeline.log` for error details
3. Verify all prerequisites are met
4. Check API key validity and quotas