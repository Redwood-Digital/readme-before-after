# IntelliBot - AI-Powered Conversational Framework

A flexible, production-ready chatbot framework built with TensorFlow and Flask, designed for easy customization and deployment.

![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![TensorFlow](https://img.shields.io/badge/tensorflow-2.12+-orange.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Tests](https://img.shields.io/badge/tests-passing-brightgreen.svg)

## Quick Start

Get a chatbot running in under 5 minutes:

```bash
# Clone and setup
git clone https://github.com/example/intellibot.git
cd intellibot
pip install -r requirements.txt

# Train on sample data
python train.py --intents data/sample_intents.json

# Start the server
python app.py

# Test it out!
curl -X POST http://localhost:5000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello!"}'
```

## Features

### Core Capabilities
- 🧠 **Intent Recognition** - Understands user intentions with 95%+ accuracy
- 💬 **Context Management** - Maintains conversation state across messages
- 🔧 **Easy Customization** - Simple JSON format for training data
- 📈 **Scalable** - Handles 1000+ concurrent conversations
- 🚀 **Production Ready** - Includes monitoring, logging, and error handling

### Technical Features
- Neural network architecture with attention mechanism
- Pre-trained word embeddings (Word2Vec/GloVe)
- RESTful API with comprehensive documentation
- WebSocket support for real-time chat
- Multi-language support (English, Spanish, French)
- Sentiment analysis integration
- Admin dashboard for analytics

## Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Frontend  │────▶│   API       │────▶│   NLU       │
│   (React)   │◀────│   (Flask)   │◀────│ (TensorFlow)│
└─────────────┘     └─────────────┘     └─────────────┘
                          │                     │
                          ▼                     ▼
                   ┌─────────────┐     ┌─────────────┐
                   │   Context    │     │  Training   │
                   │   Manager    │     │   Pipeline  │
                   └─────────────┘     └─────────────┘
```

## Installation

### Prerequisites
- Python 3.8 or higher
- 4GB RAM minimum
- CUDA-capable GPU (optional, for faster training)

### Detailed Setup

1. **Create Virtual Environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Download Pre-trained Models**
   ```bash
   python scripts/download_models.py
   ```

4. **Configure Environment**
   ```bash
   cp .env.example .env
   # Edit .env with your settings
   ```

## Training Your Chatbot

### Intent File Format

Create a JSON file with your intents:

```json
{
  "intents": [
    {
      "tag": "greeting",
      "patterns": [
        "Hi", "Hello", "Good morning", "Hey there"
      ],
      "responses": [
        "Hello! How can I help you?",
        "Hi there! What can I do for you?"
      ]
    },
    {
      "tag": "product_inquiry",
      "patterns": [
        "Tell me about your products",
        "What do you sell?",
        "Show me your catalog"
      ],
      "responses": [
        "We offer a wide range of products. What category interests you?",
        "I'd be happy to help you explore our products. Any specific area?"
      ],
      "context": ["show_categories"]
    }
  ]
}
```

### Training Command

```bash
python train.py \
  --intents data/intents.json \
  --epochs 200 \
  --batch-size 32 \
  --learning-rate 0.001
```

### Training Tips
- Start with 50-100 intents for good coverage
- Include 5-10 pattern variations per intent
- Use the validation set to prevent overfitting
- Monitor training with TensorBoard: `tensorboard --logdir logs/`

## API Documentation

### Chat Endpoint

**POST** `/api/v1/chat`

```json
{
  "message": "What products do you have?",
  "session_id": "user123",
  "context": {}
}
```

**Response:**
```json
{
  "response": "We offer a wide range of products. What category interests you?",
  "intent": "product_inquiry",
  "confidence": 0.95,
  "context": {
    "expecting": "category_selection"
  }
}
```

### Other Endpoints

- `GET /api/v1/health` - Health check
- `POST /api/v1/feedback` - Submit conversation feedback
- `GET /api/v1/analytics` - Get usage analytics (requires auth)
- `WebSocket /ws/chat` - Real-time chat connection

## Deployment

### Docker Deployment

```bash
# Build image
docker build -t intellibot .

# Run container
docker run -p 5000:5000 -e ENV=production intellibot
```

### Cloud Deployment

#### AWS
```bash
# Using AWS Copilot
copilot app init intellibot
copilot env init --name production
copilot deploy
```

#### Heroku
```bash
heroku create your-bot-name
git push heroku main
heroku ps:scale web=1
```

### Production Checklist

- [ ] Set environment to production
- [ ] Configure SSL/TLS
- [ ] Set up monitoring (Prometheus/Grafana)
- [ ] Configure rate limiting
- [ ] Enable request logging
- [ ] Set up error tracking (Sentry)
- [ ] Configure auto-scaling
- [ ] Set up backup strategy

## Customization

### Adding Custom Actions

```python
# actions/weather.py
from chatbot.actions import Action

class WeatherAction(Action):
    def execute(self, parameters):
        city = parameters.get('city', 'New York')
        weather = get_weather_api(city)
        return f"The weather in {city} is {weather['temp']}°F"

# Register in config
ACTIONS = {
    'get_weather': WeatherAction()
}
```

### Custom NLU Pipeline

```python
# Extend the NLU pipeline
from chatbot.nlu import Pipeline

class CustomPipeline(Pipeline):
    def add_sentiment_analysis(self):
        # Add your custom component
        pass
```

## Performance

- **Response Time**: < 100ms average
- **Accuracy**: 95%+ on trained intents
- **Throughput**: 1000+ requests/second
- **Memory Usage**: ~500MB base + model size

## Troubleshooting

### Common Issues

1. **High Memory Usage**
   - Reduce batch size during training
   - Use model quantization
   - Enable swap space

2. **Slow Response Times**
   - Enable model caching
   - Use GPU inference
   - Implement response caching

3. **Low Accuracy**
   - Add more training examples
   - Balance your dataset
   - Tune hyperparameters

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Development Setup

```bash
# Install dev dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Run linting
flake8 chatbot/
black chatbot/
```

## License

MIT License - see [LICENSE](LICENSE) for details.

## Support

- 📚 [Documentation](https://intellibot.readthedocs.io)
- 💬 [Discord Community](https://discord.gg/intellibot)
- 🐛 [Issue Tracker](https://github.com/example/intellibot/issues)
- 📧 Email: support@intellibot.ai

---

*Building better conversations with AI* 🤖