# Desktop App Guide - AI Political Navigator

Complete guide for running and building the desktop application.

## 🖥️ What Is This?

Your AI Political Navigator is now a **standalone Windows desktop application** that:
- ✅ Runs without a browser
- ✅ Has its own window and icon
- ✅ Can be installed like any Windows app
- ✅ Works offline (once data is loaded)
- ✅ Can be distributed as a `.exe` file

## 🚀 Quick Start

### Prerequisites
- Python 3.9+ (for backend)
- Node.js 16+ (for building the app)
- Your Gemini API key

### Step 1: Setup Backend (Same as before)

```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\backend
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
# Edit .env and add your Gemini API key
python main.py
```

Keep this terminal running!

### Step 2: Install Desktop App Dependencies

Open a **new** PowerShell window:

```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend
npm install
```

This will install Electron and all required packages.

### Step 3: Run Desktop App in Development Mode

```powershell
# Make sure you're in the frontend folder
npm run electron:dev
```

This will:
1. Start the Vite dev server
2. Wait for it to be ready
3. Launch the Electron desktop app

**The app window will open automatically!** 🎉

## 📦 Building the Desktop App (Installer)

To create a distributable `.exe` file:

### Build Installer

```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend
npm run electron:build:win
```

This creates:
- **Setup Installer**: `release/AI Political Navigator-Setup-1.0.0.exe`
  - Full installer with uninstaller
  - Can be installed to Program Files
  - Creates desktop and start menu shortcuts

### Build Portable Version

```powershell
npm run electron:build:portable
```

This creates:
- **Portable App**: `release/AI Political Navigator-Portable-1.0.0.exe`
  - No installation needed
  - Run directly from USB drive or any folder
  - Perfect for distribution

## 📁 Output Files

After building, find your apps in:
```
frontend/release/
├── AI Political Navigator-Setup-1.0.0.exe     # Installer (60-100 MB)
└── AI Political Navigator-Portable-1.0.0.exe  # Portable (60-100 MB)
```

## 🎮 Using the Desktop App

### Development Mode
```powershell
# Terminal 1: Backend
cd backend
.\venv\Scripts\activate
python main.py

# Terminal 2: Desktop App
cd frontend
npm run electron:dev
```

### Production Mode (Built App)
1. Start the backend:
   ```powershell
   cd backend
   .\venv\Scripts\activate
   python main.py
   ```

2. Run the built `.exe` file from `release/` folder

## 🔧 Desktop App Features

### Window Controls
- Minimize, Maximize, Close buttons
- Resizable window (min: 1024x768)
- Dark theme interface
- Custom app icon

### Keyboard Shortcuts
- `Ctrl+R` - Reload app
- `Ctrl+Shift+I` - Open DevTools (development)
- `Ctrl+Q` - Quit app
- `Alt+F4` - Close window

### Menu Bar
- File menu with standard options
- Help menu with about info
- View menu for zoom controls

## 📋 Available NPM Scripts

```bash
npm run dev                    # Web development mode
npm run build                  # Build web version
npm run electron              # Run Electron (after build)
npm run electron:dev          # Run desktop app in dev mode
npm run electron:build        # Build for all platforms
npm run electron:build:win    # Build Windows installer
npm run electron:build:portable # Build portable Windows app
npm run pack                  # Test build without packaging
npm run dist                  # Full distribution build
```

## 🎯 Distribution

### Sharing the App

**Option 1: Setup Installer** (Recommended)
- Share `AI Political Navigator-Setup-1.0.0.exe`
- Users run installer
- App gets installed to Program Files
- Creates desktop shortcut

**Option 2: Portable Version**
- Share `AI Political Navigator-Portable-1.0.0.exe`
- Users can run directly
- No installation needed
- Great for USB drives

### Important Note
Users must **also run the backend separately** or you need to bundle it. See "Bundling Backend" section below.

## 🔄 Updating the App

1. Make changes to your code
2. Rebuild:
   ```powershell
   npm run electron:build:win
   ```
3. New `.exe` files will be in `release/` folder
4. Increment version in `package.json` before rebuilding

## 🐛 Troubleshooting

### App won't start
```powershell
# Clear cache and rebuild
rm -r node_modules
rm -r dist
rm -r release
npm install
npm run electron:build:win
```

### "Cannot find module" error
```powershell
# Reinstall dependencies
npm install
```

### Backend connection error
- Make sure backend is running on port 8000
- Check backend terminal for errors
- Verify `.env` has correct API key

### Build fails
```powershell
# Check Node.js version (needs 16+)
node --version

# Update electron-builder
npm install electron-builder@latest --save-dev
```

## 📦 Bundling Backend (Optional)

To create a **truly standalone app** that doesn't need Python:

### Using PyInstaller

1. Install PyInstaller:
   ```powershell
   cd backend
   pip install pyinstaller
   ```

2. Create executable:
   ```powershell
   pyinstaller --onefile --name "PoliticalNavigatorAPI" main.py
   ```

3. Copy `dist/PoliticalNavigatorAPI.exe` to frontend

4. Update Electron to start backend automatically:
   ```javascript
   // In electron/main.js
   const { spawn } = require('child_process')
   const backend = spawn('path/to/PoliticalNavigatorAPI.exe')
   ```

## 🎨 Customization

### Change App Icon
1. Create `public/globe.png` (256x256 or larger)
2. Update `electron-builder.json`:
   ```json
   {
     "win": {
       "icon": "public/globe.png"
     }
   }
   ```

### Change App Name
Update in `package.json`:
```json
{
  "name": "your-app-name",
  "productName": "Your App Display Name"
}
```

### Window Size
Edit `electron/main.js`:
```javascript
new BrowserWindow({
  width: 1600,    // Change width
  height: 1000,   // Change height
  // ...
})
```

## 🚢 Production Checklist

- [ ] Backend `.env` configured
- [ ] Frontend built: `npm run build`
- [ ] Desktop app built: `npm run electron:build:win`
- [ ] Test installer on clean Windows machine
- [ ] Test portable version
- [ ] Verify all features work
- [ ] Create user documentation
- [ ] Package backend if needed

## 📊 File Sizes

Typical sizes:
- **Setup Installer**: 80-120 MB
- **Portable App**: 80-120 MB
- **Unpacked App**: 200-250 MB

These include:
- Electron runtime (~70 MB)
- Your React app (~5-10 MB)
- Node modules and dependencies

## 🔐 Code Signing (Optional)

For professional distribution:
1. Get a code signing certificate
2. Update `electron-builder.json`:
   ```json
   {
     "win": {
       "certificateFile": "path/to/cert.pfx",
       "certificatePassword": "password"
     }
   }
   ```

## 🌟 Advanced Features

### Auto-Updates
Add electron-updater for automatic updates:
```bash
npm install electron-updater
```

### System Tray
Add app to system tray instead of taskbar:
```javascript
const { Tray } = require('electron')
const tray = new Tray('icon.png')
```

### Native Notifications
Show Windows notifications:
```javascript
new Notification('Title', {
  body: 'Message'
})
```

## 📚 Resources

- [Electron Documentation](https://www.electronjs.org/docs)
- [Electron Builder](https://www.electron.build/)
- [React + Electron](https://github.com/electron-react-boilerplate)

## ✅ Summary

You now have:
- ✅ Desktop app that runs in development mode
- ✅ Scripts to build Windows installer
- ✅ Scripts to build portable `.exe`
- ✅ Professional app window with controls
- ✅ Ability to distribute to users

**Next Step**: Run `npm run electron:dev` to see your desktop app! 🎉

---

**Questions?** Check `README.md` for full documentation or `QUICK_START.md` for setup help.
