# Deployment Guide

Complete guide for deploying the AI Political Navigator to production.

## Table of Contents
1. [Backend Deployment](#backend-deployment)
2. [Frontend Deployment](#frontend-deployment)
3. [Environment Configuration](#environment-configuration)
4. [Production Checklist](#production-checklist)

---

## Backend Deployment

### Option 1: Railway (Recommended)

Railway provides easy deployment with automatic HTTPS and environment management.

#### Steps:
1. **Create Railway Account**: https://railway.app
2. **Install Railway CLI**:
   ```bash
   npm i -g @railway/cli
   ```

3. **Login**:
   ```bash
   railway login
   ```

4. **Create New Project**:
   ```bash
   cd C:\Users\Lenovo\Desktop\MatrixInfo\backend
   railway init
   ```

5. **Add Environment Variables**:
   ```bash
   railway variables set GOOGLE_API_KEY=your_api_key
   railway variables set GEMINI_MODEL=gemini-2.0-flash-exp
   ```

6. **Deploy**:
   ```bash
   railway up
   ```

7. **Get URL**:
   ```bash
   railway domain
   ```

#### Railway Configuration Files

Create `railway.json` in backend folder:
```json
{
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "uvicorn main:app --host 0.0.0.0 --port $PORT",
    "restartPolicyType": "ON_FAILURE"
  }
}
```

### Option 2: Render

1. **Create Account**: https://render.com
2. **New Web Service** → Connect GitHub repo
3. **Configure**:
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. **Environment Variables**: Add GOOGLE_API_KEY
5. **Deploy**

### Option 3: Heroku

1. **Install Heroku CLI**: https://devcenter.heroku.com/articles/heroku-cli
2. **Create Procfile** in backend folder:
   ```
   web: uvicorn main:app --host 0.0.0.0 --port $PORT
   ```

3. **Deploy**:
   ```bash
   heroku login
   heroku create ai-political-navigator-api
   heroku config:set GOOGLE_API_KEY=your_api_key
   git push heroku main
   ```

### Backend Environment Variables

Required for production:
```env
GOOGLE_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-2.0-flash-exp
```

---

## Frontend Deployment

### Option 1: Vercel (Recommended)

Perfect for React apps with automatic deployments.

#### Steps:

1. **Install Vercel CLI**:
   ```bash
   npm i -g vercel
   ```

2. **Login**:
   ```bash
   vercel login
   ```

3. **Deploy**:
   ```bash
   cd C:\Users\Lenovo\Desktop\MatrixInfo\frontend
   vercel
   ```

4. **Set Environment Variable**:
   - Go to Vercel Dashboard → Project Settings → Environment Variables
   - Add: `VITE_API_URL=https://your-backend-url.railway.app`

5. **Redeploy**:
   ```bash
   vercel --prod
   ```

#### Vercel Configuration

Create `vercel.json` in frontend folder:
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "vite",
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

### Option 2: Netlify

1. **Install Netlify CLI**:
   ```bash
   npm install -g netlify-cli
   ```

2. **Build**:
   ```bash
   cd frontend
   npm run build
   ```

3. **Deploy**:
   ```bash
   netlify deploy --prod --dir=dist
   ```

4. **Environment Variables**:
   - Netlify Dashboard → Site Settings → Environment Variables
   - Add `VITE_API_URL`

#### Netlify Configuration

Create `netlify.toml` in frontend folder:
```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### Option 3: GitHub Pages

1. **Install gh-pages**:
   ```bash
   npm install --save-dev gh-pages
   ```

2. **Update package.json**:
   ```json
   {
     "homepage": "https://yourusername.github.io/ai-political-navigator",
     "scripts": {
       "deploy": "npm run build && gh-pages -d dist"
     }
   }
   ```

3. **Deploy**:
   ```bash
   npm run deploy
   ```

---

## Environment Configuration

### Development
```env
# Backend
GOOGLE_API_KEY=your_key
GEMINI_MODEL=gemini-2.0-flash-exp

# Frontend
VITE_API_URL=http://localhost:8000
```

### Production
```env
# Backend
GOOGLE_API_KEY=your_key
GEMINI_MODEL=gemini-2.0-flash-exp

# Frontend
VITE_API_URL=https://your-api.railway.app
```

---

## Production Checklist

### Backend Checklist
- [ ] Set all environment variables
- [ ] Update CORS origins to specific domains
- [ ] Enable HTTPS
- [ ] Set up monitoring (e.g., Sentry)
- [ ] Configure rate limiting
- [ ] Add request logging
- [ ] Set up health check endpoint
- [ ] Configure auto-scaling
- [ ] Add database (PostgreSQL)
- [ ] Implement caching (Redis)

### Frontend Checklist
- [ ] Update API URL to production backend
- [ ] Enable production build optimizations
- [ ] Configure CDN for assets
- [ ] Add analytics (Google Analytics)
- [ ] Set up error tracking (Sentry)
- [ ] Configure custom domain
- [ ] Enable HTTPS
- [ ] Add robots.txt and sitemap
- [ ] Optimize images
- [ ] Test on multiple devices

### Security Checklist
- [ ] Use environment variables for secrets
- [ ] Enable CORS with specific origins
- [ ] Implement rate limiting
- [ ] Add input validation
- [ ] Enable HTTPS everywhere
- [ ] Set security headers
- [ ] Implement CSP (Content Security Policy)
- [ ] Regular dependency updates
- [ ] API key rotation policy
- [ ] Implement authentication (if needed)

---

## CORS Configuration for Production

Update `main.py` CORS settings:

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "https://your-frontend.vercel.app",
        "https://your-custom-domain.com"
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

## Custom Domain Setup

### Backend (Railway)
1. Railway Dashboard → Settings → Domains
2. Add custom domain
3. Update DNS records:
   - Type: CNAME
   - Name: api (or subdomain)
   - Value: your-app.railway.app

### Frontend (Vercel)
1. Vercel Dashboard → Settings → Domains
2. Add custom domain
3. Update DNS records as instructed
4. Wait for SSL certificate

---

## Monitoring & Logging

### Backend Monitoring

Add structured logging:
```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)
```

### Error Tracking (Sentry)

1. **Install**:
   ```bash
   pip install sentry-sdk
   ```

2. **Configure**:
   ```python
   import sentry_sdk
   
   sentry_sdk.init(
       dsn="your-sentry-dsn",
       traces_sample_rate=1.0,
   )
   ```

### Frontend Monitoring

Add to `main.jsx`:
```javascript
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: "your-sentry-dsn",
  environment: import.meta.env.MODE,
});
```

---

## Performance Optimization

### Backend
- Use connection pooling for database
- Implement caching with Redis
- Enable gzip compression
- Add CDN for static files

### Frontend
- Enable code splitting
- Lazy load components
- Optimize images (WebP format)
- Use CDN for assets
- Enable service worker for PWA

---

## Backup Strategy

### Database Backups
- Automated daily backups
- Keep 30 days of backups
- Test restore procedures

### Code Backups
- Git repository (GitHub/GitLab)
- Multiple remotes
- Tagged releases

---

## Rollback Procedure

### Backend Rollback
```bash
# Railway
railway rollback

# Heroku
heroku releases:rollback

# Render
# Use dashboard to rollback to previous deployment
```

### Frontend Rollback
```bash
# Vercel
vercel rollback

# Netlify
netlify rollback
```

---

## Cost Estimation

### Free Tier (Development/Small Scale)
- Railway: $5/month (free trial)
- Vercel: Free (hobby)
- Gemini API: Free tier available
- **Total**: ~$5/month

### Production (Medium Scale)
- Railway: $20-50/month
- Vercel: $20/month (pro)
- Gemini API: Pay-per-use
- Database: $25/month
- CDN: $10/month
- **Total**: ~$75-115/month

---

## Support & Maintenance

### Regular Tasks
- Monitor API usage
- Check error logs
- Update dependencies
- Review security alerts
- Backup verification
- Performance monitoring

### Monthly Tasks
- Review analytics
- Update content
- Security audit
- Cost optimization
- User feedback review

---

## Troubleshooting

### Common Issues

**Issue**: CORS errors in production
- **Solution**: Update CORS origins in backend

**Issue**: Environment variables not loading
- **Solution**: Verify platform-specific variable names

**Issue**: Build fails on deployment
- **Solution**: Check Node/Python versions match local

**Issue**: API rate limiting
- **Solution**: Implement caching or upgrade plan

---

## Additional Resources

- [FastAPI Deployment](https://fastapi.tiangolo.com/deployment/)
- [Vite Production Build](https://vitejs.dev/guide/build.html)
- [Railway Documentation](https://docs.railway.app/)
- [Vercel Documentation](https://vercel.com/docs)
- [Gemini API Limits](https://ai.google.dev/pricing)

---

**Ready to Deploy? Follow these steps in order:**

1. Deploy backend first
2. Get backend URL
3. Update frontend environment variable
4. Deploy frontend
5. Test thoroughly
6. Set up monitoring
7. Configure custom domains
8. Celebrate! 🎉
