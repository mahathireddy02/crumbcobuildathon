# Fix Deployment Issues

## Problem
Frontend is still connecting to `localhost:5000` instead of Render backend URL.

## Solution Steps

### 1. Update Backend CORS (Already Done ✓)
Your `.env` already has:
```
FRONTEND_URL=https://calevent-frontend.onrender.com
```

### 2. Update Frontend Environment Variable
Your `.env` already has:
```
VITE_API_BASE_URL=https://calevent-backend-16pq.onrender.com/api
```

### 3. Rebuild and Redeploy Frontend

**Option A: Push to GitHub (Recommended)**
```bash
git add .
git commit -m "Update API URL for production"
git push origin main
```
Render will auto-deploy in ~5 minutes.

**Option B: Manual Rebuild**
```bash
npm run build
```
Then manually deploy the `dist` folder.

### 4. Verify Render Environment Variables

**Backend (https://calevent-backend-16pq.onrender.com):**
- `NODE_ENV` = `production`
- `PORT` = `10000`
- `MONGO_URI` = `your-mongodb-uri`
- `JWT_SECRET` = `your-jwt-secret`
- `FRONTEND_URL` = `https://calevent-frontend.onrender.com`

**Frontend (https://calevent-frontend.onrender.com):**
- `VITE_API_BASE_URL` = `https://calevent-backend-16pq.onrender.com/api`

### 5. Clear Browser Cache
After redeployment:
1. Open DevTools (F12)
2. Right-click refresh button
3. Select "Empty Cache and Hard Reload"

## Quick Test Commands

Test backend is running:
```bash
curl https://calevent-backend-16pq.onrender.com/health
```

Test CORS:
```bash
curl -H "Origin: https://calevent-frontend.onrender.com" \
  -H "Access-Control-Request-Method: POST" \
  -H "Access-Control-Request-Headers: Content-Type" \
  -X OPTIONS \
  https://calevent-backend-16pq.onrender.com/api/auth/customer/register
```

## Common Issues

### ERR_BLOCKED_BY_CLIENT
- Ad blocker is blocking requests
- Disable ad blocker or whitelist your site

### CORS Error
- Check `FRONTEND_URL` in backend environment variables
- Ensure it matches your frontend URL exactly (no trailing slash)

### Still showing localhost
- Clear browser cache
- Rebuild frontend with correct environment variable
- Check Render build logs for environment variable

## Verification Checklist
- [ ] Backend deployed and running
- [ ] Frontend environment variable set in Render
- [ ] Frontend rebuilt and redeployed
- [ ] Browser cache cleared
- [ ] Ad blocker disabled
- [ ] Registration works
- [ ] Login works
- [ ] API calls successful
