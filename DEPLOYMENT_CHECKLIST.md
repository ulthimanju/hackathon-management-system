# Deployment Checklist

Use this checklist to track your deployment progress:

## Pre-Deployment Setup

### MongoDB Atlas Setup
- [ ] Created MongoDB Atlas account
- [ ] Created a free cluster (M0 tier)
- [ ] Created database user with password
- [ ] Configured network access (0.0.0.0/0 for demo)
- [ ] Copied connection string
- [ ] Replaced `<db_password>` in connection string with actual password
- [ ] Added database name to connection string (`/hackathons`)

**My MongoDB Connection String:**
```
mongodb+srv://umanjunath2763_db_user:<PASSWORD>@hms.sateoss.mongodb.net/hackathons?retryWrites=true&w=majority
```

### Google OAuth Setup
- [ ] Created/selected project in Google Cloud Console
- [ ] Configured OAuth consent screen
- [ ] Created OAuth 2.0 Client ID
- [ ] Noted down Client ID: `_______________________________________`
- [ ] Noted down Client Secret: `_______________________________________`

### GitHub Setup
- [ ] Created GitHub repository
- [ ] Pushed code to GitHub
- [ ] Repository URL: `_______________________________________`

---

## Backend Deployment (Render)

- [ ] Signed up for Render account
- [ ] Created new Web Service
- [ ] Connected GitHub repository
- [ ] Configured service settings:
  - [ ] Runtime: Node
  - [ ] Build Command: `npm install`
  - [ ] Start Command: `npm start`
  - [ ] Instance Type: Free
- [ ] Added all environment variables:
  - [ ] `NODE_ENV=production`
  - [ ] `PORT=10000`
  - [ ] `MONGODB_URI` (with actual password)
  - [ ] `MONGODB_DB=hackathons`
  - [ ] `JWT_SECRET` (random string)
  - [ ] `GOOGLE_CLIENT_ID`
  - [ ] `GOOGLE_CLIENT_SECRET`
  - [ ] `ADMIN_API_TOKEN` (random string)
  - [ ] `SERVER_BASE_URL` (Render URL)
  - [ ] `CLIENT_ORIGIN` (will update after frontend deployment)
- [ ] Deployed successfully
- [ ] Backend is accessible at: `_______________________________________`

---

## Frontend Deployment (Vercel)

- [ ] Signed up for Vercel account
- [ ] Created new project
- [ ] Connected GitHub repository
- [ ] Configured build settings:
  - [ ] Framework: Vite
  - [ ] Build Command: `npm run build`
  - [ ] Output Directory: `dist`
- [ ] Added environment variable:
  - [ ] `VITE_API_BASE` (Render backend URL)
- [ ] Deployed successfully
- [ ] Frontend is accessible at: `_______________________________________`

---

## Post-Deployment Configuration

### Update Backend Environment
- [ ] Updated `CLIENT_ORIGIN` in Render to Vercel URL
- [ ] Saved changes (triggered redeploy)

### Update Google OAuth
- [ ] Added Vercel URL to Authorized JavaScript origins
- [ ] Added Render callback URL to Authorized redirect URIs:
  - Format: `https://your-backend.onrender.com/auth/google/callback`
- [ ] Saved OAuth client settings

---

## Testing

- [ ] Visited frontend URL
- [ ] Login with Google works
- [ ] Can create a hackathon (as organizer/admin)
- [ ] Can register for a hackathon
- [ ] Can create/join a team
- [ ] Socket.io connections work (real-time updates)
- [ ] All API calls work correctly

---

## Troubleshooting Notes

If something doesn't work, check:

### Backend Issues:
1. Render logs (Dashboard → Service → Logs tab)
2. MongoDB connection (check password, database name)
3. Environment variables are set correctly
4. CORS settings (CLIENT_ORIGIN matches Vercel URL exactly)

### Frontend Issues:
1. Vercel deployment logs
2. Browser console for errors
3. Network tab for API request failures
4. `VITE_API_BASE` is set correctly

### OAuth Issues:
1. Google OAuth settings match production URLs
2. No trailing slashes in redirect URIs
3. Using HTTPS (not HTTP) for production URLs

---

## Important URLs

Record your URLs here for quick reference:

- **Frontend (Vercel):** `_______________________________________`
- **Backend (Render):** `_______________________________________`
- **MongoDB Atlas:** `https://cloud.mongodb.com/`
- **Google Cloud Console:** `https://console.cloud.google.com/`
- **GitHub Repository:** `_______________________________________`

---

## Next Steps After Deployment

- [ ] Share your deployed app URL with others
- [ ] Test with real users
- [ ] Monitor Render logs for any errors
- [ ] Consider custom domain (optional)
- [ ] Set up monitoring/analytics (optional)

---

**Note:** Free tier services may spin down after inactivity. The first request after inactivity may take 30-60 seconds to wake up the service.
