# Trip Advisor - AI Agent - Multi-LLM Travel Assistant

A sophisticated AI agent featuring multiple LLM provider support (Google Gemini, OpenAI, Groq-DeepSeek), optimized memory management, and real-time streaming capabilities through a Flask API. The agent specializes in providing city information including weather, time, and facts.

## 🚀 Features

- **Multi-LLM Support**: Support for Google Gemini, OpenAI, and Groq-DeepSeek models
- **LLM Factory Pattern**: Easy switching between different LLM providers
- **City Information Tools**: Weather, time, facts, and city visit planning tools
- **Real-time Streaming**: Token-by-token streaming responses via Server-Sent Events (SSE)
- **Session Management**: Multiple conversation sessions with isolated memory
- **Memory Optimization**: Automatic memory cleanup to prevent memory bloat
- **Dynamic Provider Switching**: Switch between LLM providers at runtime
- **Web Interface**: Modern React-based frontend for interaction
- **API Endpoints**: RESTful API for provider management and chat

## 🏗️ Architecture

### Core Components

1. **LLM Factory**: Factory pattern for creating different LLM providers
2. **TripAgent**: Main agent class with memory management
3. **Multi-Provider Support**: Google Gemini, OpenAI, and Groq-DeepSeek integration
4. **Flask API**: RESTful API with streaming endpoints
5. **Session Management**: Thread-safe session handling
6. **City Information Tools**: Specialized tools for city-related queries

### Memory Management Strategy

- **Session-based Memory**: Each conversation session has isolated memory
- **Automatic Optimization**: Keeps only recent messages (configurable limit)
- **Thread-safe Operations**: Concurrent session handling with locks
- **Memory Status Monitoring**: Real-time memory usage tracking

## 📋 Prerequisites

- Python 3.8+
- At least one LLM provider API key:
  - Google API Key (for Gemini access)
  - OpenAI API Key (for GPT models)
  - Groq API Key (for DeepSeek models)
- Modern web browser (for the client interface)

## 🛠️ Installation

1. **Clone or navigate to the project directory**:
   ```bash
   cd /Users/davidbong/Documents/agentic_projects/memory_agentic
   ```

2. **Create a virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**:
   ```bash
   cp .env.example .env
   ```
   
   Edit `.env` and add your API keys (at least one is required):
   ```
   # LLM Provider Configuration
   GOOGLE_API_KEY=your_google_api_key_here
   OPENAI_API_KEY=your_openai_api_key_here
   GROQ_API_KEY=your_groq_api_key_here
   
   # Default provider (google_gemini, openai, or groq)
   DEFAULT_LLM_PROVIDER=google_gemini
   
   # Model configurations
   GOOGLE_MODEL=gemini-1.5-flash
   OPENAI_MODEL=gpt-3.5-turbo
   GROQ_MODEL=deepseek-r1-distill-llama-70b
   ```

## 🔑 Getting API Keys

### Google Gemini API Key
1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Copy the key and add it to your `.env` file

### OpenAI API Key
1. Go to [OpenAI Platform](https://platform.openai.com/api-keys)
2. Create a new API key
3. Copy the key and add it to your `.env` file

### Groq API Key
1. Go to [Groq Console](https://console.groq.com/keys)
2. Create a new API key
3. Copy the key and add it to your `.env` file

## 🚀 Usage

### Starting the Server

```bash
python app.py
```

The server will start on `http://localhost:5000`

### Using the Web Interface

1. Open `client.html` in your web browser
2. The interface will automatically connect to the server
3. Start chatting with the AI agent

### API Endpoints

#### Chat with Streaming
```http
POST /chat
Content-Type: application/json

{
  "message": "What's the weather like in Paris?",
  "session_id": "user123"
}
```

#### Get Available LLM Providers
```http
GET /llm/providers
```

#### Switch LLM Provider
```http
POST /llm/switch
Content-Type: application/json

{
  "provider": "openai"
}
```

#### Check Memory Status
```http
GET /memory/status/{session_id}
```

#### Clear Session Memory
```http
DELETE /memory/clear/{session_id}
```

#### Health Check
```http
GET /health
```

## 🔧 Configuration

### Memory Optimization

You can adjust memory settings in `app.py`:

```python
# Maximum messages to keep in memory per session
self._optimize_memory(session_id, max_messages=20)
```

### Model Configuration

```python
self.llm = ChatGoogleGenerativeAI(
    model="gemini-pro",
    temperature=0.7,  # Adjust creativity
    streaming=True
)
```

## 🛠️ Available Tools

The agent comes with built-in tools:

1. **Search Tool**: Simulated web search functionality
2. **Calculator Tool**: Mathematical calculations
3. **Memory Info Tool**: Conversation memory statistics

### Adding Custom Tools

Extend the `_create_tools()` method in `MemoryOptimizedAgent`:

```python
def custom_tool(input_text: str) -> str:
    """Your custom tool implementation"""
    return f"Custom result for: {input_text}"

tools.append(Tool(
    name="custom_tool",
    description="Description of what your tool does",
    func=custom_tool
))
```

## 📊 Memory Management Details

### Session Isolation
- Each session has its own `ChatMessageHistory`
- Sessions are identified by unique session IDs
- Memory is isolated between different users/sessions

### Automatic Optimization
- Prevents memory bloat by limiting message history
- Configurable message limits per session
- Maintains conversation context while managing resources

### Thread Safety
- Uses threading locks for concurrent access
- Safe for multiple simultaneous users
- Session data integrity guaranteed

## 🔍 Monitoring and Debugging

### Memory Status
Monitor memory usage through the API:
```bash
curl http://localhost:5000/memory/status/your_session_id
```

### Logs
The Flask app runs in debug mode and provides detailed logs:
- Agent reasoning steps
- Tool executions
- Memory operations
- API requests

## 🚨 Troubleshooting

### Common Issues

1. **"Google API Key not found"**
   - Ensure your `.env` file contains the correct API key
   - Verify the key is valid and has Gemini API access

2. **"Connection refused"**
   - Make sure the Flask server is running
   - Check if port 5000 is available

3. **"Streaming not working"**
   - Ensure your browser supports Server-Sent Events
   - Check browser console for JavaScript errors

4. **"Memory not persisting"**
   - Verify session IDs are consistent
   - Check server logs for memory optimization triggers

### Performance Tips

- Adjust `max_messages` based on your memory requirements
- Use shorter session IDs for better performance
- Monitor memory usage in production environments
- Consider implementing persistent storage for long-term memory

## 🔮 Future Enhancements

- [ ] Persistent memory storage (Redis/Database)
- [ ] Advanced memory summarization
- [ ] Custom tool marketplace
- [ ] Multi-modal capabilities
- [ ] Advanced session analytics
- [ ] WebSocket support for real-time updates

## 📄 License

This project is open source and available under the MIT License.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

---

**Built with ❤️ using LangChain, Google Gemini, and Flask**

---

## 📋 Assignment Evaluation & Feedback

### Assignment Evaluation Summary
This repository represents an **excellent implementation** of the City Information Assistant project requirements. The solution demonstrates sophisticated software engineering practices, comprehensive feature implementation, and production-ready architecture.

### Requirements Compliance Assessment

| Requirement | Status | Implementation Details |
|-------------|--------|----------------------|
| **Weather Information** | ✅ **Excellent** | Comprehensive WeatherTool with mock data structure ready for production APIs |
| **Current Time** | ✅ **Excellent** | TimeTool with proper timezone calculations and UTC offset handling |
| **City Facts** | ✅ **Excellent** | CityFactsTool providing demographic and location information |
| **Follow-up Questions** | ✅ **Excellent** | Context-aware conversation handling with session-based memory management |
| **Transparent Reasoning** | ✅ **Excellent** | Clear reasoning output through LangChain React Agent with step-by-step tool orchestration |

### Technical Assessment

**Architecture & Design: A+**
- Implements sophisticated Factory Pattern for multi-LLM provider support
- Clean separation of concerns with dedicated modules (config.py, llm_factory.py, output_parser.py)
- Thread-safe session management with proper memory optimization
- RESTful API design with comprehensive endpoint coverage

**Code Quality: A**
- Well-structured codebase with clear naming conventions
- Comprehensive error handling and logging
- Type hints and documentation throughout
- Minor issue: Type annotation error in `_optimize_memory` method (line 366)

**Documentation Quality: A+**
- Exceptionally comprehensive README with clear setup instructions
- Detailed API documentation with examples
- Architecture explanations and troubleshooting guides
- Multiple deployment options (Docker, manual setup)

### Strengths

1. **Multi-LLM Provider Support**: Unique implementation supporting Google Gemini, OpenAI, and Groq-DeepSeek
2. **Production-Ready Features**: 
   - Real-time streaming via Server-Sent Events
   - Session management and memory optimization
   - Rate limiting and health checks
   - Comprehensive error handling
3. **Modern Frontend**: React-based interface with professional UI/UX
4. **Scalable Architecture**: Factory pattern allows easy addition of new LLM providers
5. **Comprehensive Testing**: Multiple test files covering different scenarios
6. **Docker Support**: Complete containerization setup
7. **Memory Management**: Intelligent conversation history optimization

### Areas for Improvement

1. **Type Safety**: Fix type annotation issue in `_optimize_memory` method
2. **API Integration**: Replace mock data with real API integrations for production use
3. **Authentication**: Add user authentication for production deployment
4. **Database Integration**: Consider persistent storage for conversation history
5. **Monitoring**: Add application performance monitoring and metrics

### Overall Rating: A+ (Exceptional)

This implementation exceeds the basic project requirements and demonstrates:
- Advanced software engineering practices
- Production-ready architecture and features
- Comprehensive documentation and testing
- Innovative multi-LLM provider approach
- Professional-grade user interface

**Recommendation**: This solution serves as an excellent reference implementation for agentic AI applications and could be deployed to production with minimal additional work.

---

*Evaluation completed by: Utkarsh Saxena*  
*Date: July 29, 2025*  
*Project: City Information Assistant Assignment*ask**# 🏙️ Trip Advisor - AI Agent

An intelligent AI agent that helps users gather factual information about cities worldwide. This assistant demonstrates advanced agentic capabilities including tool orchestration, function calling, contextual dialogue handling, streaming API interface, and transparent reasoning.

## ✨ Features

### Core Capabilities
- **🌤️ Weather Information**: Get current weather conditions for any city
- **🕐 Local Time**: Check the current time in different cities worldwide
- **📍 City Facts**: Learn about city demographics, location, and interesting facts
- **🗺️ Visit Planning**: Comprehensive city visit planning using multiple tools

### Technical Features
- **Tool Orchestration**: Seamless integration of multiple specialized tools
- **Function Calling**: Structured output with reasoning transparency
- **Multi-turn Dialogue**: Context-aware conversations with memory
- **Streaming API**: Real-time response streaming
- **Transparent Reasoning**: See the agent's thinking process

## 🛠️ Architecture

### Backend (Python/Flask)
- **LangChain React Agent**: Advanced reasoning and tool orchestration
- **Google Gemini Integration**: Powered by Gemini-1.5-flash model
- **Memory Management**: Persistent conversation history
- **RESTful API**: Clean endpoints for frontend integration

### Frontend (React/Next.js)
- **Modern UI**: Beautiful, responsive interface
- **Real-time Chat**: Streaming responses with typing indicators
- **Session Management**: Multiple conversation sessions
- **Memory Visualization**: View conversation history and memory status

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- Node.js 18+
- Google API Key (for Gemini)

### Backend Setup
1. **Clone and navigate to the project**:
   ```bash
   git clone https://github.com/bitlabsdevteam/memory_agent.git
   cd memory_agent
   ```

2. **Set up environment**:
   ```bash
   cp .env.example .env
   # Edit .env and add your GOOGLE_API_KEY
   ```

3. **Install dependencies and start**:
   ```bash
   chmod +x start.sh
   ./start.sh
   ```

   The backend will be available at `http://localhost:5001`

### Frontend Setup
1. **Navigate to frontend directory**:
   ```bash
   cd frontend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Start development server**:
   ```bash
   npm run dev
   ```

   The frontend will be available at `http://localhost:3000`

## 🔧 Tools & APIs

The assistant implements the following specialized tools:

| Tool | Purpose | Implementation |
|------|---------|----------------|
| **WeatherTool** | Get current weather for a city | Mock data (production: OpenWeatherMap API) |
| **TimeTool** | Get current time in a city | Timezone calculations with UTC offsets |
| **CityFactsTool** | Get basic facts about a city | Mock data (production: GeoDB Cities API) |
| **PlanMyCityVisitTool** | Composite tool for visit planning | Orchestrates multiple tools with reasoning |

## 💬 Example Interactions

### Simple Queries
```
User: "What's the weather like in Paris?"
Assistant: Uses WeatherTool → "Current weather in Paris: 23°C, clear skies, humidity 65%"
```

### Complex Planning
```
User: "Plan my visit to Tokyo"
Assistant: Uses PlanMyCityVisitTool → Orchestrates multiple tools:
1. Gets city facts about Tokyo
2. Fetches current weather
3. Checks local time
4. Provides comprehensive visit summary
```

### Follow-up Questions
```
User: "What about the weather there?"
Assistant: Uses conversation context → Provides weather for previously mentioned city
```

## 📁 Project Structure

```
memory_agent/
├── app.py                 # Main Flask application
├── config.py             # Configuration management
├── requirements.txt      # Python dependencies
├── start.sh             # Startup script
├── .env.example         # Environment template
├── frontend/            # React/Next.js frontend
│   ├── src/app/
│   │   ├── components/  # React components
│   │   ├── page.tsx     # Main page
│   │   └── layout.tsx   # App layout
│   └── package.json     # Node dependencies
└── README.md           # This file
```

## 🔑 Configuration

Key configuration options in `config.py`:

- **GOOGLE_MODEL**: AI model (default: gemini-1.5-flash)
- **AGENT_TEMPERATURE**: Response creativity (0.0-1.0)
- **MEMORY_MAX_MESSAGES**: Conversation memory limit
- **STREAMING_ENABLED**: Real-time response streaming
- **FLASK_PORT**: Backend server port

## 🌐 API Endpoints

- `GET /health` - Health check
- `POST /chat` - Main chat endpoint (streaming)
- `GET /memory/status/<session_id>` - Memory status
- `DELETE /memory/clear/<session_id>` - Clear memory

## 🧪 Testing

Test the assistant with various queries:

1. **Weather queries**: "What's the weather in London?"
2. **Time queries**: "What time is it in Sydney?"
3. **City facts**: "Tell me about New York"
4. **Planning**: "Plan my visit to Berlin"
5. **Follow-ups**: "What about the weather there?"

## 🚀 Production Deployment

For production use:

1. **Replace mock data** with real APIs:
   - OpenWeatherMap for weather
   - World Time API for time zones
   - GeoDB Cities API for city facts

2. **Environment setup**:
   - Use production WSGI server (gunicorn)
   - Set up proper environment variables
   - Configure CORS for your domain

3. **Frontend deployment**:
   - Build optimized bundle: `npm run build`
   - Deploy to Vercel, Netlify, or similar

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is open source and available under the MIT License.
