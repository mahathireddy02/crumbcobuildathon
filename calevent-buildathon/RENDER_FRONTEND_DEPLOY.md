# Deploy CALEVENT Frontend to Render

## Prerequisites
- GitHub account with your code pushed
- Render account (sign up at https://render.com)

## Step 1: Prepare Your Frontend for Deployment

### 1.1 Update API Base URL
Create/update environment configuration for production:

**File: `src/services/api.js`**
Update the base URL to use environment variable:
```javascript
const API_BASE_URL = import.meta.env.VITE_API_URL || 'http://localhost:5000/api';
```

### 1.2 Create Build Script
Your `package.json` should have (already exists):
```json
{
  "scripts": {
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

## Step 2: Deploy to Render

### 2.1 Login to Render
1. Go to https://render.com
2. Sign up or login with GitHub

### 2.2 Create New Static Site
1. Click **"New +"** button
2. Select **"Static Site"**

### 2.3 Connect Repository
1. Connect your GitHub account if not already connected
2. Select repository: `mahathireddy02/crumbcobuildathon`
3. Click **"Connect"**

### 2.4 Configure Build Settings

**Basic Settings:**
- **Name:** `calevent-frontend` (or your preferred name)
- **Branch:** `main`
- **Root Directory:** `calevent-buildathon`
- **Build Command:** `npm install && npm run build`
- **Publish Directory:** `dist`

**Environment Variables:**
Click **"Advanced"** and add:
- Key: `VITE_API_URL`
- Value: `https://your-backend-url.onrender.com/api`
  (Replace with your actual backend URL once deployed)

### 2.5 Deploy
1. Click **"Create Static Site"**
2. Wait for build to complete (5-10 minutes)
3. Your site will be live at: `https://calevent-frontend.onrender.com`

## Step 3: Configure Custom Domain (Optional)

1. Go to your static site dashboard
2. Click **"Settings"**
3. Scroll to **"Custom Domain"**
4. Add your domain and follow DNS instructions

## Step 4: Update Backend CORS

After deployment, update your backend's allowed origins:

**File: `calevent-backend/middleware/security.js`**
```javascript
const allowedOrigins = [
  'http://localhost:5173',
  'http://localhost:3000',
  'https://calevent-frontend.onrender.com', // Add your Render URL
  process.env.FRONTEND_URL
];
```

## Step 5: Test Your Deployment

1. Visit your Render URL
2. Test key features:
   - Homepage loads
   - Search functionality
   - Login/Register
   - Event browsing
   - API calls work

## Troubleshooting

### Build Fails
- Check build logs in Render dashboard
- Ensure all dependencies are in `package.json`
- Verify Node version compatibility

### API Calls Fail
- Check CORS settings in backend
- Verify `VITE_API_URL` environment variable
- Check browser console for errors

### 404 on Page Refresh
Add `_redirects` file in `public` folder:
```
/*    /index.html   200
```

Or create `vercel.json` if using Vercel:
```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

## Environment Variables Reference

| Variable | Value | Description |
|----------|-------|-------------|
| `VITE_API_URL` | `https://your-backend.onrender.com/api` | Backend API URL |

## Auto-Deploy Setup

Render automatically deploys when you push to `main` branch:
1. Make changes locally
2. Commit: `git commit -m "update"`
3. Push: `git push origin main`
4. Render auto-deploys in ~5 minutes

## Performance Optimization

### Enable Compression
Already handled by Vite build

### Add Cache Headers
In Render dashboard:
- Settings → Headers
- Add: `Cache-Control: public, max-age=31536000`

### Monitor Performance
- Use Render's built-in analytics
- Check build times and deploy logs

## Cost
- **Free Tier:** 
  - 100 GB bandwidth/month
  - Global CDN
  - Auto SSL certificate
  - Perfect for this project!

## Next Steps
1. Deploy backend to Render (see RENDER_BACKEND_DEPLOY.md)
2. Update frontend `VITE_API_URL` with backend URL
3. Test full application flow
4. Set up custom domain (optional)

## Support
- Render Docs: https://render.com/docs/static-sites
- Vite Docs: https://vitejs.dev/guide/build.html
