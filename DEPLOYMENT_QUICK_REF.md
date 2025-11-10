# Quick Deployment Reference

## Your MongoDB Connection String
Replace `<db_password>` with your actual MongoDB password:
```
mongodb+srv://umanjunath2763_db_user:<db_password>@hms.sateoss.mongodb.net/hackathons?retryWrites=true&w=majority
```

## Backend Environment Variables for Render

When deploying to Render, set these environment variables:

```
NODE_ENV=production
PORT=10000
MONGODB_URI=mongodb+srv://umanjunath2763_db_user:<db_password>@hms.sateoss.mongodb.net/hackathons?retryWrites=true&w=majority
MONGODB_DB=hackathons
JWT_SECRET=<generate-a-random-string-here>
GOOGLE_CLIENT_ID=<your-google-client-id>
GOOGLE_CLIENT_SECRET=<your-google-client-secret>
ADMIN_API_TOKEN=<generate-a-random-string-here>
SERVER_BASE_URL=https://<your-service-name>.onrender.com
CLIENT_ORIGIN=https://<your-app-name>.vercel.app
```

## Frontend Environment Variables for Vercel

When deploying to Vercel, set this environment variable:

```
VITE_API_BASE=https://<your-service-name>.onrender.com
```

## Build Configuration

### Backend (Render)
- **Build Command**: `npm install`
- **Start Command**: `npm start`

### Frontend (Vercel)
- **Build Command**: `npm run build`
- **Output Directory**: `dist`

## Deployment Steps Summary

1. ✅ **Setup MongoDB Atlas** - Get your connection string (already done!)
2. ⬜ **Setup Google OAuth** - Get Client ID and Secret from Google Cloud Console
3. ⬜ **Push to GitHub** - Create a repository and push your code
4. ⬜ **Deploy Backend to Render** - Connect GitHub repo and set environment variables
5. ⬜ **Deploy Frontend to Vercel** - Connect GitHub repo and set environment variable
6. ⬜ **Update OAuth URIs** - Add production URLs to Google OAuth settings
7. ⬜ **Test** - Visit your Vercel URL and test the app

---

For detailed step-by-step instructions, see `DEPLOYMENT.md`
