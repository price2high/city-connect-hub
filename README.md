# People's Plaza - City Connect Hub

**AI-powered civic engagement platform for discovering events, resources, and staying informed about your city.**

## 🌟 Project Overview

People's Plaza is a modern web application designed to bridge the gap between citizens and their communities. The platform provides real-time local information, AI-powered assistance, and seamless access to civic resources across multiple market areas.

### Key Features

- **🤖 AI Assistant (Penny)**: Intelligent civic assistant powered by Hugging Face Spaces that helps users find events, resources, and answers questions about their city
- **📰 Live News Feed**: Real-time local news articles from NewsAPI.org, filtered by selected market area
- **🌤️ Weather Integration**: Current weather conditions using WeatherAPI.com, Azure Maps, and OpenWeatherMap with automatic location detection
- **📅 Events Discovery**: Community events and activities sourced from Azure Cognitive Search and Eventbrite API
- **📍 Location Services**: Automatic geolocation detection with manual market area selection (7 supported cities)
- **💬 Chat History**: Persistent conversation history with Penny, including clear all functionality
- **📱 Responsive Design**: Modern, mobile-first UI built with React and Tailwind CSS

### Supported Market Areas

- Norfolk, Virginia
- El Paso, Texas
- Atlanta, Georgia
- Providence, Rhode Island
- Birmingham, Alabama
- Chesterfield, Virginia
- Seattle, Washington

## 🛠️ Technology Stack

### Frontend
- **React 18** with TypeScript
- **Vite** - Build tool and dev server
- **Tailwind CSS** - Utility-first CSS framework
- **shadcn/ui** - Component library
- **React Router DOM** - Client-side routing
- **TanStack Query** - Data fetching and caching
- **Lucide React** - Icon library

### Backend & Infrastructure
- **Azure Static Web Apps** - Hosting and serverless functions
- **Azure Functions (Python)** - Serverless API endpoints
- **Azure Cognitive Search** - Search service for events, documents, and resources
- **Azure Maps** - Geocoding and location services
- **Azure Blob Storage** - File storage (configured)

### External Services
- **Hugging Face Spaces** - AI agent (Penny) deployment
- **NewsAPI.org** - Live news articles
- **WeatherAPI.com** - Weather data (primary)
- **OpenWeatherMap** - Weather data (fallback)
- **Eventbrite API** - Event discovery

### Development Tools
- **GitHub Actions** - CI/CD pipeline
- **Node.js 22** - Runtime environment
- **Python 3.x** - Backend functions
- **TypeScript** - Type safety

## 🚀 Getting Started

### Prerequisites

- Node.js 22+ and npm
- Python 3.x (for local Azure Functions development)
- Azure account (for deployment)
- API keys for external services (see Environment Variables)

### Installation

```bash
# Clone the repository
git clone <YOUR_GIT_URL>
cd city-connect-hub

# Install dependencies
npm install

# Start development server
npm run dev
```

### Environment Variables

Create a `.env` file in the root directory (for local development):

```env
VITE_PENNY_BACKEND_URL=https://pythonprincess-penny-v2-2.hf.space
VITE_API_URL=/api
VITE_NEWS_API_KEY=your_news_api_key
```

**Note**: For production, environment variables are configured in Azure Portal → Static Web App → Configuration.

## 📦 Project Structure

```
city-connect-hub/
├── api/                    # Azure Functions (Python)
│   ├── agent.py           # Penny AI proxy function
│   ├── news.py            # News API function
│   ├── weather.py         # Weather API function
│   ├── events.py          # Events API function
│   ├── search.py          # Azure Search function
│   └── requirements.txt   # Python dependencies
├── src/
│   ├── components/        # React components
│   ├── hooks/            # Custom React hooks
│   ├── lib/              # Utilities and API clients
│   ├── pages/            # Page components
│   └── assets/           # Images and static files
├── docs/                 # Contributor documentation (see below)
└── scripts/              # Utility scripts
```

## 🏗️ Architecture

### Frontend Architecture
- **Component-based**: Modular React components with TypeScript
- **Context API**: Global state management for location, auth, and chat
- **Custom Hooks**: Reusable logic (`useLocation`, `usePennyChat`, `useAuth`)
- **Error Boundaries**: Graceful error handling with fallback UI

### Backend Architecture
- **Serverless Functions**: Python Azure Functions for API endpoints
- **Proxy Pattern**: Azure Function acts as proxy to Hugging Face Space
- **Fallback Mechanisms**: Multiple API providers with automatic fallback
- **CORS Configuration**: Proper CORS headers for cross-origin requests

### Data Flow
1. User interacts with frontend (React app)
2. Frontend calls Azure Functions (`/api/*`)
3. Azure Functions proxy to external APIs or Azure services
4. Responses cached and displayed in UI

## 🎯 Challenges & Solutions

### Challenge 1: AI Agent Integration
**Problem**: Connecting the Hugging Face Space (Penny) to the frontend was complex due to Gradio API format differences and CORS issues.

**Solution**: 
- Created Azure Function proxy (`api/agent.py`) to handle format conversion
- Implemented fallback mechanism (Azure Function → Direct Hugging Face)
- Added proper error handling and queue management for Gradio API
- Configured CORS headers in Azure Functions

### Challenge 2: Location-Based Content
**Problem**: Ensuring news, weather, and events update correctly when users change their market area.

**Solution**:
- Built `useLocation` context hook for centralized location management
- Implemented automatic geolocation detection with manual override
- Added location persistence in localStorage
- Created location-aware components that react to market changes

### Challenge 3: Azure Function Deployment
**Problem**: Functions returning 404/500 errors due to structure issues and missing environment variables.

**Solution**:
- Standardized function structure (flat `.py` files matching other functions)
- Added comprehensive error handling and logging
- Created deployment checklist for environment variables
- Implemented graceful fallbacks when functions aren't deployed

### Challenge 4: News Feed Location Mismatch
**Problem**: News articles showing incorrect city names (e.g., showing "Norfolk" when location was "El Paso").

**Solution**:
- Fixed mock data to use dynamic city variable in all articles
- Added proper URL encoding for city parameters
- Ensured API calls include correct city parameter
- Improved error handling with location-aware fallback data

### Challenge 5: Weather API Reliability
**Problem**: Weather data not loading consistently across all market areas.

**Solution**:
- Implemented multi-provider fallback (WeatherAPI.com → Azure Maps → OpenWeatherMap)
- Added coordinate-based weather fetching (more accurate than city names)
- Integrated user's detected geolocation for precise weather
- Added mock data fallback for development/testing

### Challenge 6: Custom Domain Configuration
**Problem**: Ensuring all services work correctly after adding custom domain to Azure Static Web App.

**Solution**:
- Verified all Azure Functions use `"*"` for CORS (works with any domain)
- Confirmed frontend uses relative paths (domain-agnostic)
- Updated Hugging Face Space CORS settings (if using direct access)
- Documented custom domain requirements

## 📚 Documentation

### For Contributors

Detailed technical documentation, troubleshooting guides, and deployment instructions are available in the [`docs/`](./docs/) folder. **These documents are intended for project contributors and developers only.**

**Key Documentation Categories:**
- **Deployment & Setup**: Azure Function deployment, custom domain configuration
- **API Integration**: Hugging Face, Azure services, external APIs
- **Troubleshooting**: Common issues and their solutions
- **Azure Services**: Search, Maps, Storage configuration
- **Testing & Verification**: Testing procedures and verification steps

See [`docs/README.md`](./docs/README.md) for a complete documentation index.

### Public Documentation

- **About Page**: Visit `/about` in the application for user-facing information
- **API Documentation**: See individual function files in `api/` for endpoint documentation

## 🚢 Deployment

### Azure Static Web Apps

The application is deployed to Azure Static Web Apps with automatic CI/CD via GitHub Actions.

**Deployment Process:**
1. Push to `main` branch
2. GitHub Actions builds the React app
3. Azure Static Web Apps deploys frontend and functions
4. Environment variables configured in Azure Portal

**Required Environment Variables:**
- `HF_TOKEN` - Hugging Face API token
- `NEWS_API_KEY` - NewsAPI.org key
- `WEATHERAPI_KEY` - WeatherAPI.com key
- `AZURE_MAPS_KEY` - Azure Maps subscription key
- `AZURE_SEARCH_ENDPOINT` - Azure Cognitive Search endpoint
- `AZURE_SEARCH_KEY` - Azure Cognitive Search API key
- `AZURE_SEARCH_INDEX_*` - Search index names (events, documents, etc.)

## 🤝 Contributing

This project uses a structured development workflow. For detailed contribution guidelines, see the documentation in the `docs/` folder.

## 📄 License

[Add your license information here]

## 🙏 Acknowledgments

- **Penny AI**: Developed by Cyber-Shawties-LLC
- **Hugging Face**: For hosting the AI agent
- **Azure**: Cloud infrastructure and services
- **shadcn/ui**: Component library
- **NewsAPI.org**: News data provider

---

**Built with ❤️ for civic engagement**
