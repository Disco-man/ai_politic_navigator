# Quick Start Guide - AI Political Navigator

Get the AI Political Navigator running in 5 minutes!

## Prerequisites Check

Before starting, ensure you have:
- ✅ Python 3.9+ installed
- ✅ Node.js 16+ installed
- ✅ Google Gemini API key ([Get one here](https://makersuite.google.com/app/apikey))

## Step 1: Setup Backend (2 minutes)

Open PowerShell and run:

```powershell
# Navigate to project
cd C:\Users\Lenovo\Desktop\MatrixInfo\backend

# Create virtual environment
python -m venv venv

# Activate it
.\venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Setup environment
copy .env.example .env
```

Now edit `.env` file and add your Gemini API key:
```env
GOOGLE_API_KEY=your_actual_api_key_here
```

## Step 2: Setup Frontend (1 minute)

Open a **new** PowerShell window:

```powershell
# Navigate to frontend
cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend

# Install dependencies
npm install
```

## Step 3: Start the Application (30 seconds)

### Terminal 1 - Start Backend:
```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\backend
.\venv\Scripts\activate
python main.py
```

Wait for: `✅ Uvicorn running on http://0.0.0.0:8000`

### Terminal 2 - Start Frontend:
```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend
npm run dev
```

Wait for: `🎉 Local: http://localhost:3000`

## Step 4: Initialize Data (1 minute)

1. Open browser: `http://localhost:3000`
2. Click **"Initialize Sample Data"** button
3. Wait ~30-60 seconds while AI generates data for 15 countries
4. Start exploring! 🌍

## What's Working?

You should now be able to:

✅ **Explore Interactive Map** - Click any country  
✅ **View Country Details** - See political events by category  
✅ **Read About Figures** - Click on highlighted names  
✅ **Chat with AI** - Click the blue chat button (bottom-right)  
✅ **Smart Text Links** - Click highlighted countries/figures in text  

## Quick Test

Try these actions to test everything:

1. **Test Map**: Click on "United States" on the map
2. **Test Categories**: Click "Foreign Policy" in the sidebar
3. **Test Highlighting**: Click any blue (country) or green (figure) highlighted text
4. **Test AI Chat**: Click chat button, ask "What is NATO?"

## Troubleshooting

### Backend won't start?
```powershell
# Check if port 8000 is available
netstat -ano | findstr :8000

# If occupied, edit main.py line 202 to use different port:
# uvicorn.run(app, host="0.0.0.0", port=8001)
```

### Frontend won't start?
```powershell
# Clear cache and reinstall
rm -r node_modules
npm install
npm run dev
```

### No countries showing?
- Make sure you clicked "Initialize Sample Data"
- Check backend terminal for errors
- Verify Gemini API key is correct

### AI Chat not responding?
- Check browser console (F12) for errors
- Verify backend is running on port 8000
- Check Gemini API quota limits

## Next Steps

Once everything is running:

1. **Explore Sample Countries**: US, Russia, China, UK, France, Germany, India, Japan, Brazil, Ukraine, Israel, Iran, Saudi Arabia, Turkey, South Korea

2. **Generate More Countries**: Click any un-highlighted country → "Generate Country Data"

3. **Ask AI Questions**: 
   - "Explain the Russia-Ukraine conflict"
   - "What are the main economic challenges facing Brazil?"
   - "Who is the current leader of France?"

4. **Customize**: Edit code to add your own features!

## Common Workflows

### Adding a New Country
1. Click the country on the map
2. Click "Generate Country Data"
3. Wait 10-15 seconds
4. Explore the generated information!

### Reading About Events
1. Navigate to a country page
2. Use category filters to find specific policy areas
3. Click highlighted countries/figures to learn more
4. Use AI chat to ask follow-up questions

### Asking Complex Questions
1. Open AI chat (blue button)
2. Navigate to relevant country page (gives AI context)
3. Ask your question
4. AI will answer with context awareness

## Development URLs

- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/docs
- Alternative Docs: http://localhost:8000/redoc

## Default Port Configuration

| Service | Port | Can Change? |
|---------|------|-------------|
| Backend | 8000 | Yes (edit main.py) |
| Frontend | 3000 | Yes (edit vite.config.js) |

## Performance Tips

- First API call to Gemini may be slow (~5-10s)
- Subsequent calls are faster (~1-3s)
- Generate countries in batches during off-peak hours
- Consider using faster Gemini model for quicker responses

## Need Help?

Check the full README.md for:
- Detailed API documentation
- Architecture explanations
- Deployment guides
- Advanced customization

---

**Happy Exploring! 🌍🤖**
