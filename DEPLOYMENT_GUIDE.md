# 🚀 Complete MERN Stack Deployment Guide on Render

## Overview
- **Backend**: Node.js + Express on Render (Web Service)
- **Admin Panel**: React on Render (Static Site)
- **User App**: React on Render (Static Site)
- **Database**: MongoDB Atlas (Free Tier)

---

## STEP 1: Prepare GitHub Repository

### 1.1 Initialize Git (if not already done)
```bash
cd "e:\MERN Internship\Ondemand Home Services"
git init
git add .
git commit -m "Initial commit - MERN Stack"
```

### 1.2 Create GitHub Repository
1. Go to [github.com](https://github.com)
2. Click "New" to create a repository
3. Name it: `ondemand-service-platform`
4. Don't initialize README (you already have files)
5. Click "Create repository"

### 1.3 Push to GitHub
```bash
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ondemand-service-platform.git
git push -u origin main
```

**Note**: Replace `YOUR_USERNAME` with your actual GitHub username.

---

## STEP 2: Set Up MongoDB Atlas

### 2.1 Create MongoDB Account
1. Go to [mongodb.com/cloud/atlas](https://mongodb.com/cloud/atlas)
2. Click "Create Account"
3. Fill details and verify email
4. Sign in

### 2.2 Create Free Cluster
1. Click "Build a Cluster"
2. Choose "FREE" tier
3. Select region closest to you (e.g., US East)
4. Click "Create Cluster" (wait 2-3 minutes)

### 2.3 Create Database User
1. In left sidebar, go to "Database Access"
2. Click "Add New Database User"
3. Enter username: `admin`
4. Generate secure password (copy it!)
5. Click "Add User"

### 2.4 Get Connection String
1. In left sidebar, go to "Deployment" → "Database"
2. Click "Connect" button on your cluster
3. Choose "Connect your application"
4. Copy the connection string
5. Replace `<password>` with your password
6. Replace `myFirstDatabase` with `ondemand_db`

**Example:**
```
mongodb+srv://admin:YourPassword123@cluster0.xyz.mongodb.net/ondemand_db?retryWrites=true&w=majority
```

### 2.5 Whitelist IP Addresses
1. Go to "Network Access" in left sidebar
2. Click "Add IP Address"
3. Select "Allow access from anywhere" (for Render)
4. Click "Confirm"

---

## STEP 3: Create Environment Variables

### 3.1 Backend .env file
Create `backend/.env`:
```
MONGODB_URI=mongodb+srv://admin:YourPassword123@cluster0.xyz.mongodb.net/ondemand_db?retryWrites=true&w=majority
PORT=8000
JWT_SECRET=your_super_secret_key_min_32_chars_long_change_this!
RAZORPAY_KEY_ID=your_razorpay_key_id_here
RAZORPAY_KEY_SECRET=your_razorpay_secret_here
```

### 3.2 Admin .env file
Create `admin/.env`:
```
REACT_APP_API_URL=https://YOUR_BACKEND_URL.onrender.com
```

### 3.3 User .env file
Create `user/.env`:
```
REACT_APP_API_URL=https://YOUR_BACKEND_URL.onrender.com
```

**Note**: You'll get the actual URLs after creating Render services.

---

## STEP 4: Deploy Backend on Render

### 4.1 Sign Up on Render
1. Go to [render.com](https://render.com)
2. Click "Get Started"
3. Choose "Sign up with GitHub"
4. Authorize Render to access your GitHub

### 4.2 Create Backend Service
1. Click "New +" → "Web Service"
2. Select your GitHub repository
3. Configure as follows:
   - **Name**: `ondemand-backend`
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `node index.js`
   - **Plan**: `Free`

### 4.3 Add Environment Variables
1. Scroll to "Environment" section
2. Add these variables:
   ```
   MONGODB_URI=mongodb+srv://admin:PASSWORD@cluster0.xyz.mongodb.net/ondemand_db?retryWrites=true&w=majority
   JWT_SECRET=your_super_secret_key_min_32_chars_long_change_this!
   RAZORPAY_KEY_ID=your_razorpay_key_id
   RAZORPAY_KEY_SECRET=your_razorpay_secret
   ```

### 4.4 Deploy
1. Click "Create Web Service"
2. Wait for deployment (5-10 minutes)
3. Once done, you'll see a URL like: `https://ondemand-backend.onrender.com`
4. **COPY THIS URL** - you'll need it for frontend

### 4.5 Verify Backend
1. Open `https://ondemand-backend.onrender.com/` in browser
2. You should see: "Welcome to On-Demand Service Platform API!"

---

## STEP 5: Deploy Admin Panel on Render

### 5.1 Create Admin Static Site
1. On Render dashboard, click "New +" → "Static Site"
2. Select your GitHub repository
3. Configure as follows:
   - **Name**: `ondemand-admin`
   - **Branch**: `main`
   - **Build Command**: `cd admin && npm run build`
   - **Publish Directory**: `admin/build`
   - **Plan**: `Free`

### 5.2 Add Environment Variable
1. Go to Environment section
2. Add:
   ```
   REACT_APP_API_URL=https://ondemand-backend.onrender.com
   ```
   (Replace with your actual backend URL from Step 4.4)

### 5.3 Deploy
1. Click "Create Static Site"
2. Wait for build and deployment (5-7 minutes)
3. Once done, you'll see a URL like: `https://ondemand-admin.onrender.com`

### 5.4 Verify Admin Panel
1. Open the admin URL
2. You should see the login page

---

## STEP 6: Deploy User App on Render

### 6.1 Create User Static Site
1. On Render dashboard, click "New +" → "Static Site"
2. Select your GitHub repository
3. Configure as follows:
   - **Name**: `ondemand-user`
   - **Branch**: `main`
   - **Build Command**: `cd user && npm run build`
   - **Publish Directory**: `user/build`
   - **Plan**: `Free`

### 6.2 Add Environment Variable
1. Go to Environment section
2. Add:
   ```
   REACT_APP_API_URL=https://ondemand-backend.onrender.com
   ```

### 6.3 Deploy
1. Click "Create Static Site"
2. Wait for deployment
3. You'll see a URL like: `https://ondemand-user.onrender.com`

### 6.4 Verify User App
1. Open the user URL
2. You should see the home page

---

## STEP 7: Update Backend CORS Configuration

### 7.1 Update index.js
In `backend/index.js`, update the CORS section:

```javascript
app.use(cors({
  origin: [
    "http://localhost:3000",
    "http://localhost:3001",
    "https://ondemand-admin.onrender.com",
    "https://ondemand-user.onrender.com",
  ],
  credentials: true,
  methods: ["GET", "POST", "PUT", "DELETE"],
}));
```

### 7.2 Commit and Push
```bash
git add backend/index.js
git commit -m "Update CORS for production"
git push
```

**Render will automatically redeploy your backend** ✅

---

## STEP 8: Final Configuration & Testing

### 8.1 Update Frontend API Endpoints
Check that `services/api.js` in both admin and user apps has:

```javascript
const API_BASE_URL = process.env.REACT_APP_API_URL || "http://localhost:8000";

export const axiosInstance = axios.create({
  baseURL: API_BASE_URL,
  withCredentials: true,
});
```

### 8.2 Update All Frontend .env Files
Make sure both `admin/.env` and `user/.env` have:
```
REACT_APP_API_URL=https://ondemand-backend.onrender.com
```

### 8.3 Commit and Deploy
```bash
git add .
git commit -m "Production configuration"
git push
```

This will trigger automatic rebuilds on Render.

---

## STEP 9: Complete Testing Checklist

- [ ] **Backend URL**: `https://ondemand-backend.onrender.com/`
  - Should show welcome message

- [ ] **Admin URL**: `https://ondemand-admin.onrender.com/`
  - Should load login page
  - Try login with admin credentials
  - Navigate to dashboard

- [ ] **User URL**: `https://ondemand-user.onrender.com/`
  - Should load home page
  - Try register/login
  - Browse services
  - Book a service

- [ ] **API Connectivity**:
  - Open browser DevTools → Network tab
  - Check if API calls go to your backend URL
  - No CORS errors should appear

- [ ] **Database**:
  - Ensure data persists across page reloads
  - Check MongoDB Atlas for new documents

---

## STEP 10: Optional - Custom Domain

### 10.1 Add Custom Domain (Optional)
1. On Render service, click "Settings"
2. Scroll to "Custom Domains"
3. Add your domain (e.g., `admin.yourdomain.com`)
4. Follow DNS setup instructions

---

## Troubleshooting

### Issue: "Frontend not connecting to backend"
**Solution**: 
1. Check `REACT_APP_API_URL` environment variable on Render
2. Verify backend CORS includes your frontend URL
3. Clear browser cache and reload

### Issue: "Build failing on Render"
**Solution**:
1. Check build logs (click "Logs" in Render)
2. Ensure `package.json` exists in the directory
3. Verify build command is correct

### Issue: "Database connection error"
**Solution**:
1. Verify MongoDB URI in `.env`
2. Check IP whitelist in MongoDB Atlas
3. Confirm username/password in connection string

### Issue: "File uploads not working"
**Solution**:
1. Use AWS S3 or Cloudinary instead of local uploads
2. Free tier on Render doesn't persist file uploads

---

## Useful Render Dashboard Links

- **Manage Services**: https://dashboard.render.com/
- **View Logs**: Click on your service → "Logs" tab
- **Monitor Activity**: Click on your service → "Activity" tab

---

## Summary of Your Live URLs

After deployment:
- **Admin Panel**: `https://ondemand-admin.onrender.com`
- **User App**: `https://ondemand-user.onrender.com`
- **Backend API**: `https://ondemand-backend.onrender.com`
- **Database**: MongoDB Atlas (Cloud)

Your website is now LIVE! 🎉

---

## Next Steps (Optional)

1. Set up CI/CD pipeline for automated testing
2. Add monitoring and error tracking (Sentry)
3. Set up email notifications for alerts
4. Configure backup for MongoDB
5. Add custom domain
6. Scale up to paid Render plan if needed

