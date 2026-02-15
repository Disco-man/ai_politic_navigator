# AI Political Navigator

An interactive web application that helps people navigate the complex world of 21st century politics through AI-powered insights, interactive maps, and comprehensive country information.

## Features

### 🌍 Interactive World Map
- Click on any country to explore its political landscape
- Visual highlighting of countries with available data
- Smooth zoom and pan interactions
- Real-time tooltips with country information

### 📊 Country Information Pages
- **Categorized Political Events**: Foreign Policy, Domestic Policy, Economy, Social Issues, Military, Environment
- **Event Details**: Date, severity, descriptions with AI-analyzed context
- **Related Information**: Automatic linking to mentioned countries and historical figures
- **Filtering System**: Browse events by category

### 👥 Historical Figures
- Biographies of key political figures from the 21st century
- Achievements and contributions
- Related countries and political contexts
- Interactive navigation between figures and countries

### 🔗 Smart Text Highlighting
- Automatic detection and highlighting of country names in text
- Interactive links to historical figures mentioned in content
- Color-coded highlighting (blue for countries, green for figures)
- Seamless navigation through interconnected political information

### 💬 AI Chat Assistant
- Context-aware AI assistant powered by Google Gemini
- Ask questions about political events, countries, or figures
- Get clarifications on complex political situations
- Chat history and conversation context
- Floating chat widget accessible from any page

### 🤖 AI-Powered Content Generation
- Generate comprehensive country information using AI
- Automatic analysis and categorization of political events
- Intelligent entity extraction from text
- Smart identification of related countries and figures

## Technology Stack

### Backend
- **FastAPI**: Modern Python web framework
- **Google Gemini AI**: Advanced language model for political analysis
- **WebSocket**: Real-time communication
- **Pydantic**: Data validation and modeling

### Frontend
- **React 18**: Modern UI library
- **React Router**: Client-side routing
- **React Simple Maps**: Interactive world map visualization
- **Zustand**: Lightweight state management
- **Axios**: HTTP client
- **Lucide React**: Beautiful icon library
- **Vite**: Lightning-fast build tool

## Getting Started

### Prerequisites
- Python 3.9 or higher
- Node.js 16 or higher
- Google Gemini API key (get it from [Google AI Studio](https://makersuite.google.com/app/apikey))

### Installation

#### 1. Clone the Repository
```bash
cd C:\Users\Lenovo\Desktop\MatrixInfo
```

#### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
# source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create .env file
copy .env.example .env

# Edit .env and add your Gemini API key
# GOOGLE_API_KEY=your_actual_api_key_here
```

#### 3. Frontend Setup

```bash
cd ..\frontend

# Install dependencies
npm install

# Create .env file (optional)
echo "VITE_API_URL=http://localhost:8000" > .env
```

### Running the Application

#### Start Backend Server
```bash
cd backend
venv\Scripts\activate  # If not already activated
python main.py
```

The API will be available at `http://localhost:8000`

#### Start Frontend Development Server
```bash
cd frontend
npm run dev
```

The application will be available at `http://localhost:3000`

### First-Time Setup

1. Open your browser and navigate to `http://localhost:3000`
2. Click the **"Initialize Sample Data"** button on the home page
3. Wait for the AI to generate comprehensive political information for 15 sample countries
4. Once complete, you can click on any country to explore!

## Usage Guide

### Exploring Countries
1. Click on any country on the world map
2. Browse events by category using the sidebar
3. Click on highlighted country names or figure names to navigate to their pages
4. Use the category filters to focus on specific policy areas

### Using the AI Chat
1. Click the chat button in the bottom-right corner
2. Ask questions about politics, countries, or historical figures
3. The AI has context about the current page you're viewing
4. Use the suggested questions to get started

### Generating Country Data
- If a country doesn't have data, you'll see a "Generate Country Data" button
- Click it to have AI create comprehensive political information
- This includes events, figures, and categorized information

## API Documentation

Once the backend is running, visit:
- Interactive API docs: `http://localhost:8000/docs`
- Alternative docs: `http://localhost:8000/redoc`

### Key Endpoints

- `POST /api/chat` - Send messages to AI assistant
- `POST /api/analyze-text` - Analyze text for countries and figures
- `GET /api/countries` - List all countries
- `GET /api/countries/{country_id}` - Get country details
- `GET /api/figures/{figure_id}` - Get figure details
- `POST /api/generate-country-info/{country_name}` - Generate AI content for a country

## Project Structure

```
MatrixInfo/
├── backend/
│   ├── main.py                 # FastAPI application
│   ├── requirements.txt        # Python dependencies
│   ├── .env.example           # Environment variables template
│   └── .gitignore
├── frontend/
│   ├── src/
│   │   ├── components/        # React components
│   │   │   ├── WorldMap.jsx
│   │   │   ├── ChatWidget.jsx
│   │   │   └── HighlightedText.jsx
│   │   ├── pages/             # Page components
│   │   │   ├── WorldMapPage.jsx
│   │   │   ├── CountryPage.jsx
│   │   │   └── FigurePage.jsx
│   │   ├── services/          # API and state management
│   │   │   ├── api.js
│   │   │   └── store.js
│   │   ├── data/              # Static data
│   │   │   └── countries-geo.js
│   │   ├── styles/            # CSS files
│   │   │   └── global.css
│   │   ├── App.jsx            # Root component
│   │   └── main.jsx           # Entry point
│   ├── package.json
│   ├── vite.config.js
│   └── index.html
└── README.md
```

## Configuration

### Environment Variables

#### Backend (.env)
```env
GOOGLE_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-2.0-flash-exp
BACKEND_URL=http://localhost:8000
```

#### Frontend (.env)
```env
VITE_API_URL=http://localhost:8000
```

## Development Tips

### Adding New Countries
Use the generate endpoint to add countries:
```bash
curl -X POST http://localhost:8000/api/generate-country-info/Japan
```

### Customizing Categories
Edit the category definitions in:
- Backend: `main.py` (CountryEvent model)
- Frontend: `CountryPage.jsx` (CATEGORY_ICONS and CATEGORY_LABELS)

### Styling
- Global styles: `frontend/src/styles/global.css`
- Component styles: Co-located CSS files (e.g., `WorldMap.css`)
- CSS variables defined in `:root` for easy theming

## Deployment

### Backend Deployment
- Can be deployed to any platform supporting Python (Heroku, Railway, Render, etc.)
- Remember to set environment variables in your hosting platform
- Use `uvicorn main:app --host 0.0.0.0 --port $PORT` for production

### Frontend Deployment
- Build: `npm run build`
- Deploy the `dist` folder to any static hosting (Vercel, Netlify, GitHub Pages)
- Update `VITE_API_URL` to point to your production backend

## Troubleshooting

### Backend Issues
- **"AI Error"**: Check if your Gemini API key is valid and has quota
- **CORS errors**: Verify the frontend URL is allowed in CORS settings
- **Port already in use**: Change the port in `main.py`

### Frontend Issues
- **White screen**: Check browser console for errors
- **API connection failed**: Ensure backend is running and VITE_API_URL is correct
- **Map not loading**: Check internet connection (map data is fetched from CDN)

### Common Issues
- **No countries showing**: Click "Initialize Sample Data" to generate content
- **Slow AI responses**: First requests may be slower; consider using a faster Gemini model
- **Text highlighting not working**: Ensure the backend analyze-text endpoint is accessible

## Future Enhancements

- [ ] User authentication and personalized feeds
- [ ] Bookmarking and note-taking features
- [ ] Timeline view of historical events
- [ ] Comparison tool for multiple countries
- [ ] Export functionality for reports
- [ ] Mobile app version
- [ ] Multi-language support
- [ ] Real-time news integration
- [ ] Data visualization charts

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

Project contributors are listed in [CONTRIBUTORS.md](CONTRIBUTORS.md).

## License

This project is licensed under the MIT License.

## Acknowledgments

- World map data from Natural Earth via [world-atlas](https://github.com/topojson/world-atlas)
- Political insights powered by Google Gemini AI
- Built for educational purposes to help people understand global politics

## Contact

For questions or support, please open an issue on the repository.

---

**Note**: This application provides AI-generated political analysis. Always verify important information from multiple authoritative sources.
