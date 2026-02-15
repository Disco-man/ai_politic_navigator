# Desktop App - Quick Reference

## 🚀 Running the Desktop App

### Easy Method (One Click)
Double-click: `START_DESKTOP_APP.bat`

This will show you a menu to:
1. Start both backend and desktop app
2. Start only backend
3. Start only desktop app
4. Build the desktop app

### Manual Method

**Terminal 1 - Backend:**
```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\backend
.\venv\Scripts\activate
python main.py
```

**Terminal 2 - Desktop App:**
```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend
npm run electron:dev
```

## 📦 Building the Desktop App

### Create Installer (.exe)
```powershell
cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend
npm run electron:build:win
```

Find the installer in: `frontend/release/AI Political Navigator-Setup-1.0.0.exe`

### Create Portable Version
```powershell
npm run electron:build:portable
```

Find portable app in: `frontend/release/AI Political Navigator-Portable-1.0.0.exe`

## 📁 Files Created

```
MatrixInfo/
├── START_DESKTOP_APP.bat          # 👈 CLICK THIS to start!
├── DESKTOP_APP_GUIDE.md           # Full desktop app documentation
└── frontend/
    ├── electron/
    │   ├── main.js                # Electron main process
    │   └── preload.js             # Preload script
    ├── electron-builder.json       # Build configuration
    ├── build-app.bat              # Windows build script
    └── run-desktop-app.bat        # Windows run script
```

## 🎯 What Changed

Your web app is now ALSO a desktop app:
- ✅ Runs in its own window (not browser)
- ✅ Has app icon and title bar
- ✅ Can be installed like any program
- ✅ Can be distributed as .exe file
- ✅ Works on Windows, Mac, Linux

## ⚡ Quick Commands

| Command | What It Does |
|---------|-------------|
| `npm run electron:dev` | Run desktop app in development |
| `npm run electron:build` | Build for all platforms |
| `npm run electron:build:win` | Build Windows installer |
| `npm run electron:build:portable` | Build portable .exe |

## 🐛 Troubleshooting

**"npm: command not found"**
- Install Node.js: https://nodejs.org/

**Desktop app won't start**
```powershell
cd frontend
npm install
npm run electron:dev
```

**Backend connection error**
- Make sure backend is running first!
- Check http://localhost:8000

## 📚 Documentation

- **Full Desktop Guide**: `DESKTOP_APP_GUIDE.md`
- **Original README**: `README.md`
- **Quick Start**: `QUICK_START.md`

---

**🎉 You now have both a website AND a desktop app!**
