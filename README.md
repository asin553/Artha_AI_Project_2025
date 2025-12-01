# Airbnb Listing Intelligence Platform

A comprehensive data pipeline and AI-powered analytics platform for Airbnb listings, featuring real-time data streaming, graph database storage, GraphQL API, and conversational AI interface.

## 🏗️ Architecture Overview

<img width="2516" height="1098" alt="image" src="https://github.com/user-attachments/assets/3388a968-a98b-41a6-aea3-4749c782d66b" />


## 🚀 Quick Start

### Prerequisites
- **Python 3.12**
- **Docker Desktop** (running)
- **Conda** (recommended for environment management)
- **Apache Kafka 3.5.1** (with Zookeeper)
- **Neo4j Aura** cloud database account

### 1. Environment Setup

```bash
# Clone the repository
git clone <your-repo-url>
cd AirbnbListingLLM

# Create conda environment
conda create -n airbnb-platform python=3.12
conda activate airbnb-platform

# Install dependencies
pip install -r requirements.txt
```

### 2. Configuration Setup

#### Airflow Configuration
```bash
cp .astro/config.yaml.template .astro/config.yaml
# Edit .astro/config.yaml with your settings
```

#### Streamlit Secrets
```bash
cp .streamlit/secrets.toml.template .streamlit/secrets.toml
# Edit .streamlit/secrets.toml with your API keys and database credentials
```

### 3. Infrastructure Startup

#### Option A: Docker Compose (Recommended)
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f
```

#### Option B: Manual Setup
Follow the manual setup steps below for individual service startup.

## 🐳 Docker Compose Services

The `docker-compose.yml` orchestrates the following services:

| Service | Port | Description |
|---------|------|-------------|
| **Airflow** | 8080 | Workflow orchestration (user: admin, pass: admin) |
| **Kafka UI** | 8081 | Kafka cluster visualization |
| **Zookeeper** | 2181 | Kafka coordination service |
| **Kafka** | 9092 | Message streaming platform |
| **GraphQL API** | 8000 | Strawberry Apollo Sandbox |
| **MLflow** | 5000 | ML experiment tracking |
| **Streamlit** | 8501 | AI chat interface |

### Service Dependencies
```
Zookeeper → Kafka → Airflow DAGs
Neo4j Aura (External) → GraphQL API → Streamlit App
MLflow → Streamlit (experiment tracking)
```

## 🔧 Manual Setup (Alternative)

### 1. Start Apache Kafka
```bash
# Terminal 1: Start Zookeeper
cd /path/to/kafka
bin/zookeeper-server-start.sh config/zookeeper.properties

# Terminal 2: Start Kafka Server
bin/kafka-server-start.sh config/server.properties

# Terminal 3: Start Kafka UI (Docker)
docker run -it -p 8081:8080 -e DYNAMIC_CONFIG_ENABLED=true provectuslabs/kafka-ui
```

### 2. Start Airflow
```bash
# In project directory
astro dev start
```
Access: http://localhost:8080 (admin/admin)

### 3. Start GraphQL API
```bash
python -m api.main
```
Access: http://127.0.0.1:8000

### 4. Start MLflow
```bash
mlflow ui
```
Access: http://127.0.0.1:5000

### 5. Start Streamlit App
```bash
streamlit run chat/bot.py
```
Access: http://localhost:8501

## 📊 Monitoring & Visualization

### Spark UI
- **URL**: http://localhost:4040
- **Available**: When SparkSession is initialized
- **Purpose**: Monitor streaming job stages and performance

### Kafka Monitoring
- **Kafka UI**: http://localhost:8081
- **Features**: View topics, messages, consumer groups, brokers

### Neo4j Visualization
- **Neo4j Aura Console**: https://console.neo4j.io
- **Neo4j Desktop**: Connect using credentials from secrets.toml

### MLflow Experiment Tracking
- **URL**: http://127.0.0.1:5000
- **Purpose**: Track GenAI agent interactions and model performance


## 🔑 Required Configuration

### Environment Variables (.env)
```bash
# Kafka Configuration
KAFKA_BROKER=localhost:9092
KAFKA_TOPIC=airbnb-usa-listings

# Neo4j Configuration  
NEO4J_URI=neo4j+s://your-instance.databases.neo4j.io
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your-password

# OpenAI Configuration
OPENAI_API_KEY=your-openai-api-key
OPENAI_MODEL=gpt-4o-mini

# Spark Configuration
CHECKPOINT_LOCATION=/tmp/kafka-checkpoint
```

### Streamlit Secrets (.streamlit/secrets.toml)
```toml
OPENAI_API_KEY = "your-openai-api-key"
OPENAI_MODEL = "gpt-4o-mini"

NEO4J_URI = "neo4j+s://your-instance.databases.neo4j.io"
NEO4J_USERNAME = "neo4j"  
NEO4J_PASSWORD = "your-password"

KAFKA_BROKER = "localhost:9092"
KAFKA_TOPIC = "airbnb-usa-listings"
CHECKPOINT_LOCATION = "/tmp/kafka-checkpoint"

EDGE_DRIVER_PATH = "/path/to/msedgedriver.exe"
AIRBNB_URL = "https://www.airbnb.com/airbnb-friendly"
```

## 🚦 Usage Workflow

### 1. Data Collection & Streaming
1. **Scraper** extracts Airbnb listings using Selenium
2. **Kafka** queues the extracted data
3. **Spark Streaming** processes data in real-time
4. **Neo4j** stores the graph-structured data

### 2. API & Analysis
1. **GraphQL API** provides flexible data access
2. **Apollo Sandbox** enables interactive API exploration
3. **LangChain Agents** perform intelligent data analysis

### 3. AI-Powered Interaction
1. **Streamlit Interface** provides conversational UI
2. **OpenAI GPT Models** power natural language understanding
3. **MLflow** tracks experiment metrics and performance

## 🎯 Key Features

### Data Pipeline
- ✅ **Real-time streaming** with Apache Kafka & Spark
- ✅ **Graph database** storage with relationship modeling
- ✅ **Scalable architecture** with containerized services

### API Layer  
- ✅ **GraphQL API** with type-safe schema
- ✅ **Complex queries** with filtering and aggregation
- ✅ **ROI calculations** and investment analysis

### AI Interface
- ✅ **Natural language** property search
- ✅ **Intelligent recommendations** based on criteria
- ✅ **Market analysis** and trend insights
- ✅ **Investment opportunity** identification

## 🛠️ Development

### Adding New Features
1. **Data Sources**: Extend `scraper.py` for new listing sources
2. **API Endpoints**: Add resolvers in `api/query.py` or `api/mutation.py`
3. **AI Tools**: Create new tools in `chat/tools/` directory
4. **UI Components**: Enhance `chat/bot.py` interface

### Testing
```bash
# Run API tests
python -m pytest api/tests/

# Test GraphQL queries
# Visit http://127.0.0.1:8000 and use Apollo Sandbox

# Test Streamlit app
streamlit run chat/bot.py
```

## 🏷️ Version Information

- **Python**: 3.12+
- **Apache Kafka**: 3.5.1
- **Apache Spark**: 3.4.1  
- **Neo4j**: 5.27+
- **Airflow**: 2.10+ (Astro Runtime 12.0.0)
- **Streamlit**: 1.35+
- **OpenAI**: 1.56+

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Troubleshooting

### Common Issues

#### Kafka Connection Errors
```bash
# Check if Kafka is running
docker ps | grep kafka

# Restart Kafka services
docker-compose restart kafka zookeeper
```

#### Neo4j Connection Issues
- Verify credentials in `.streamlit/secrets.toml`
- Check firewall settings for Neo4j Aura
- Ensure IP whitelist includes your address

#### Airflow DAG Not Showing
```bash
# Check DAG syntax
python dags/streamer.py

# Restart Airflow
astro dev restart
```

#### Streamlit Memory Issues
```bash
# Reduce model token limits in chat/llm.py
# Use gpt-4o-mini instead of gpt-4o for lower costs
```

### Support
- 📧 Create an issue in the GitHub repository
- 📖 Check the project documentation
- 💬 Review MLflow experiment logs for debugging AI interactions

---


**Built with ❤️ using Apache Kafka, Spark, Neo4j, GraphQL, LangChain, and Streamlit**



# Artha_AI_Project_2025
