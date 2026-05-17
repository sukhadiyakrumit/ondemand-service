# 📋 Quick Deployment Checklist

## Pre-Deployment (Local Setup)
- [ ] Create `.gitignore` file
- [ ] Create `.env` files in backend, admin, and user folders
- [ ] Update `backend/index.js` CORS with Render URLs
- [ ] Verify `services/api.js` uses environment variables
- [ ] Push code to GitHub

## MongoDB Atlas Setup
- [ ] Create MongoDB Account
- [ ] Create Free Cluster
- [ ] Create Database User (username: admin, password: _____)
- [ ] Get Connection String: _________________________
- [ ] Whitelist all IPs (0.0.0.0/0)

## Render Deployment

### Backend Service
- [ ] Create Render Account (GitHub OAuth)
- [ ] Create Web Service
  - Name: `ondemand-backend`
  - Build: `npm install`
  - Start: `node index.js`
- [ ] Add Environment Variables:
  - MONGODB_URI: _________________________
  - JWT_SECRET: _________________________
  - RAZORPAY_KEY_ID: _________________________
  - RAZORPAY_KEY_SECRET: _________________________
- [ ] Deploy and wait for success
- [ ] Copy Backend URL: https://______.onrender.com

### Admin Panel
- [ ] Create Static Site
  - Name: `ondemand-admin`
  - Build: `cd admin && npm run build`
  - Publish: `admin/build`
- [ ] Add REACT_APP_API_URL: ________________
- [ ] Deploy
- [ ] Admin URL: https://______.onrender.com

### User App
- [ ] Create Static Site
  - Name: `ondemand-user`
  - Build: `cd user && npm run build`
  - Publish: `user/build`
- [ ] Add REACT_APP_API_URL: ________________
- [ ] Deploy
- [ ] User URL: https://______.onrender.com

## Post-Deployment Testing
- [ ] Backend loads (shows welcome message)
- [ ] Admin panel loads and login works
- [ ] User app loads and signup/login works
- [ ] Services load from database
- [ ] Bookings can be created
- [ ] Payments process
- [ ] Admin dashboard shows stats
- [ ] All API calls show correct backend URL in DevTools

## Live URLs
- 🔗 Admin: https://______.onrender.com
- 🔗 User: https://______.onrender.com
- 🔗 Backend: https://______.onrender.com
