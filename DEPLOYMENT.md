# Deployment Guide

This guide will help you deploy your Hackathon Management System to the cloud for free.

## Overview
- **Frontend**: Deploy to Vercel (free tier)
- **Backend**: Deploy to Render (free tier)
- **Database**: MongoDB Atlas (free tier)

## Step 1: Set Up MongoDB Atlas (Database)

1. Go to https://www.mongodb.com/cloud/atlas/register
2. Create a free account and sign in
3. Create a new project (e.g., "Hackathon-Management")
4. Click "Build a Database" → Choose "M0 FREE" tier
5. Select a cloud provider (AWS, Google Cloud, or Azure) and region close to you
6. Click "Create Cluster"
7. **Create a Database User**:
   - Click "Database Access" in the left sidebar
   - Click "Add New Database User"
   - Choose "Password" authentication
   - Username: `umanjunath2763_db_user`
   - Password: Set a strong password (save it!)
   - Database User Privileges: "Atlas admin"
   - Click "Add User"
8. **Configure Network Access**:
   - Click "Network Access" in the left sidebar
   - Click "Add IP Address"
   - Click "Allow Access from Anywhere" (0.0.0.0/0) - for demo purposes
   - Click "Confirm"
9. **Get Connection String**:
   - Click "Database" in the left sidebar
   - Click "Connect" on your cluster
   - Choose "Connect your application"
   - Copy the connection string: `mongodb+srv://umanjunath2763_db_user:<db_password>@hms.sateoss.mongodb.net/?appName=HMS`
   - Replace `<db_password>` with your actual password
   - Add your database name: `mongodb+srv://umanjunath2763_db_user:<db_password>@hms.sateoss.mongodb.net/hackathons?retryWrites=true&w=majority`

**Your MongoDB Connection String** (replace `<db_password>` with actual password):
```
mongodb+srv://umanjunath2763_db_user:<db_password>@hms.sateoss.mongodb.net/hackathons?retryWrites=true&w=majority
```

---

## Step 2: Set Up Google OAuth (Authentication)

1. Go to https://console.cloud.google.com/
2. Create a new project or select an existing one
3. Go to "APIs & Services" → "Credentials"
4. Click "Create Credentials" → "OAuth 2.0 Client ID"
5. Configure the OAuth consent screen if prompted:
   - User Type: External
   - App name: "Hackathon Management System"
   - User support email: your email
   - Developer contact: your email
   - Save and continue through the scopes and test users
6. Create OAuth Client ID:
   - Application type: "Web application"
   - Name: "Hackathon Management System"
   - Authorized JavaScript origins:
     - `http://localhost:5173` (for development)
     - `https://your-app.vercel.app` (add this after deploying to Vercel)
   - Authorized redirect URIs:
     - `http://localhost:5000/auth/google/callback` (for development)
     - `https://your-backend.onrender.com/auth/google/callback` (add this after deploying to Render)
   - Click "Create"
7. **Copy your Client ID and Client Secret** - you'll need these!

---

## Step 3: Deploy Backend to Render

### 3.1: Push Code to GitHub

1. Initialize git (if not already done):
   ```powershell
   git init
   git add .
   git commit -m "Initial commit - ready for deployment"
   ```

2. Create a new repository on GitHub:
   - Go to https://github.com/new
   - Repository name: `hackathon-management-system`
   - Make it public or private
   - Don't initialize with README (you already have one)
   - Click "Create repository"

3. Push your code:
   ```powershell
   git remote add origin https://github.com/YOUR_USERNAME/hackathon-management-system.git
   git branch -M main
   git push -u origin main
   ```

### 3.2: Deploy on Render

1. Go to https://dashboard.render.com/ and sign up (use GitHub to sign in)
2. Click "New" → "Web Service"
3. Connect your GitHub repository
4. Configure the service:
   - **Name**: `hackathon-backend` (or any name)
   - **Region**: Choose closest to you
   - **Branch**: `main`
   - **Root Directory**: Leave blank (root of repo)
   - **Runtime**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
   - **Instance Type**: `Free`

5. **Add Environment Variables** (click "Advanced" → "Add Environment Variable"):
   ```
   NODE_ENV=production
   PORT=10000
   MONGODB_URI=mongodb+srv://umanjunath2763_db_user:<YOUR_PASSWORD>@hms.sateoss.mongodb.net/hackathons?retryWrites=true&w=majority
   MONGODB_DB=hackathons
   JWT_SECRET=your-super-secret-jwt-key-change-this-to-random-string
   GOOGLE_CLIENT_ID=your-google-client-id-from-step-2
   GOOGLE_CLIENT_SECRET=your-google-client-secret-from-step-2
   ADMIN_API_TOKEN=your-admin-token-change-this-to-random-string
   SERVER_BASE_URL=https://hackathon-backend.onrender.com
   CLIENT_ORIGIN=https://your-app.vercel.app
   ```
   
   **Note**: You'll update `CLIENT_ORIGIN` after deploying the frontend in Step 4.

6. Click "Create Web Service"
7. Wait for deployment to complete (5-10 minutes)
8. **Copy your backend URL**: `https://hackathon-backend.onrender.com` (or whatever Render assigns)

---

## Step 4: Deploy Frontend to Vercel

### 4.1: Deploy on Vercel

1. Go to https://vercel.com/signup and sign up (use GitHub to sign in)
2. Click "Add New" → "Project"
3. Import your GitHub repository
4. Configure the project:
   - **Framework Preset**: Vite
   - **Root Directory**: `./` (root of repo)
   - **Build Command**: `npm run build` (should be auto-detected)
   - **Output Directory**: `dist` (should be auto-detected)
   - **Install Command**: `npm install`

5. **Add Environment Variables**:
   - Click "Environment Variables"
   - Add:
     ```
     VITE_API_BASE=https://hackathon-backend.onrender.com
     ```
     (Use your actual Render backend URL from Step 3)

6. Click "Deploy"
7. Wait for deployment to complete (2-5 minutes)
8. **Copy your frontend URL**: `https://your-app.vercel.app`

### 4.2: Update Backend Environment Variables

1. Go back to Render dashboard
2. Select your backend service
3. Go to "Environment" tab
4. Update `CLIENT_ORIGIN` to your Vercel URL: `https://your-app.vercel.app`
5. Save changes (this will trigger a redeploy)

### 4.3: Update Google OAuth Redirect URIs

1. Go back to Google Cloud Console (https://console.cloud.google.com/)
2. Go to "APIs & Services" → "Credentials"
3. Click on your OAuth 2.0 Client ID
4. Add your production URLs:
   - **Authorized JavaScript origins**:
     - Add: `https://your-app.vercel.app`
   - **Authorized redirect URIs**:
     - Add: `https://hackathon-backend.onrender.com/auth/google/callback`
5. Click "Save"

---

## Step 5: Test Your Deployment

1. Visit your Vercel URL: `https://your-app.vercel.app`
2. Try logging in with Google OAuth
3. Test creating a hackathon, team, etc.

---

## Troubleshooting

### Backend Issues:
- Check Render logs: Dashboard → Your Service → "Logs" tab
- Common issues:
  - MongoDB connection string incorrect (check password and database name)
  - Environment variables not set correctly
  - CORS errors (check `CLIENT_ORIGIN` matches your Vercel URL exactly)

### Frontend Issues:
- Check Vercel deployment logs: Dashboard → Your Project → "Deployments" → Click on deployment
- Common issues:
  - `VITE_API_BASE` not set or incorrect
  - API requests failing (check Network tab in browser DevTools)

### OAuth Issues:
- Check Google Cloud Console OAuth settings
- Make sure redirect URIs match exactly (no trailing slashes, correct protocol https)
- Check that OAuth consent screen is configured

---

## Notes for Free Tiers

- **Render Free Tier**: 
  - Service spins down after 15 minutes of inactivity
  - First request after spin-down takes 30-60 seconds
  - 750 hours/month free (enough for one service running 24/7)

- **Vercel Free Tier**:
  - Unlimited deployments
  - 100GB bandwidth/month
  - Automatic HTTPS

- **MongoDB Atlas Free Tier**:
  - 512MB storage
  - Shared resources
  - Enough for demo/student projects

---

## Updating Your Deployment

### Backend Updates:
```powershell
git add .
git commit -m "Update backend"
git push
```
Render will automatically redeploy.

### Frontend Updates:
```powershell
git add .
git commit -m "Update frontend"
git push
```
Vercel will automatically redeploy.

---

## Summary of URLs to Track

After deployment, you'll have:
- **Frontend (Vercel)**: `https://your-app.vercel.app`
- **Backend (Render)**: `https://hackathon-backend.onrender.com`
- **MongoDB**: `mongodb+srv://...`

Make sure these are all configured correctly in:
1. Render environment variables (`CLIENT_ORIGIN`)
2. Vercel environment variables (`VITE_API_BASE`)
3. Google OAuth console (authorized origins and redirect URIs)

Good luck with your deployment! 🚀
