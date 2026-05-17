# 🏗️ Deployment Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         USERS' DEVICES                          │
│  (Browser: Chrome, Firefox, Safari, Mobile)                     │
└────────────┬─────────────────────────────────┬──────────────────┘
             │                                  │
      ┌──────▼──────┐                   ┌──────▼──────┐
      │  ADMIN APP  │                   │   USER APP  │
      │  React/Vite │                   │  React/Vite │
      │  (Static)   │                   │  (Static)   │
      │   Hosted on │                   │   Hosted on │
      │   Render    │                   │   Render    │
      └──────┬──────┘                   └──────┬──────┘
             │                                  │
             │         ┌──────────────┐         │
             └────────►│   BACKEND    │◄────────┘
                       │  Node/Exp    │
                       │   on Render  │
                       │   Port 8000  │
                       └──────┬───────┘
                              │
                       ┌──────▼───────┐
                       │   MongoDB    │
                       │    Atlas     │
                       │   Database   │
                       │  (Cloud DB)  │
                       └──────────────┘
```

---

## Deployment Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    YOUR LOCAL MACHINE                           │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Source Code                                             │  │
│  │  ├── backend/                                            │  │
│  │  ├── admin/                                              │  │
│  │  ├── user/                                               │  │
│  │  └── .env files                                          │  │
│  └────────────────────┬─────────────────────────────────────┘  │
│                       │                                         │
│                       │ git push                                │
│                       ▼                                         │
└───────────────────────┬─────────────────────────────────────────┘
                        │
        ┌───────────────┴───────────────┐
        │   GITHUB REPOSITORY           │
        │   (Your code backup)          │
        └───────────────┬───────────────┘
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
   ┌────────┐      ┌────────┐      ┌────────┐
   │ RENDER │      │ RENDER │      │ RENDER │
   │Backend │      │ Admin  │      │ User   │
   │Service │      │  Site  │      │  Site  │
   └────┬───┘      └────┬───┘      └────┬───┘
        │               │               │
        │    ┌──────────┴──────────┐   │
        └───►│   LIVE URLS        │◄──┘
             ├─ backend.render.com│
             ├─ admin.render.com  │
             └─ user.render.com   │
```

---

## Environment Variables Mapping

```
LOCAL DEVELOPMENT:
┌─────────────────────────┐
│ backend/.env            │
├─────────────────────────┤
│ MONGODB_URI=local_url   │
│ JWT_SECRET=dev_secret   │
└─────────────────────────┘

┌─────────────────────────┐    ┌─────────────────────────┐
│ admin/.env              │    │ user/.env               │
├─────────────────────────┤    ├─────────────────────────┤
│ REACT_APP_API_URL=      │    │ REACT_APP_API_URL=      │
│ http://localhost:8000   │    │ http://localhost:8000   │
└─────────────────────────┘    └─────────────────────────┘


PRODUCTION (Render):
┌──────────────────────────────────┐
│ Backend Service (Render)         │
├──────────────────────────────────┤
│ MONGODB_URI=production_url       │
│ JWT_SECRET=production_secret     │
│ RAZORPAY_KEY_ID=key             │
│ RAZORPAY_KEY_SECRET=secret      │
└──────────────────────────────────┘

┌──────────────────────────────────┐  ┌──────────────────────────────────┐
│ Admin Site (Render)              │  │ User Site (Render)               │
├──────────────────────────────────┤  ├──────────────────────────────────┤
│ REACT_APP_API_URL=               │  │ REACT_APP_API_URL=               │
│ https://backend.onrender.com     │  │ https://backend.onrender.com     │
└──────────────────────────────────┘  └──────────────────────────────────┘
```

---

## API Communication Flow

### User booking flow:
```
1. User logs in on User App
   ↓
2. Frontend calls: https://backend.onrender.com/login
   ↓
3. Backend validates in MongoDB
   ↓
4. Returns JWT token to User App
   ↓
5. User App stores token in cookie
   ↓
6. User browses services: /services API call
   ↓
7. Books service with payment
   ↓
8. Backend processes with Razorpay
   ↓
9. Booking stored in MongoDB
   ↓
10. Admin App can see booking via: /admin/bookings
```

### Admin dashboard flow:
```
1. Admin logs in on Admin App
   ↓
2. Admin App calls: https://backend.onrender.com/admin/dashboardStats
   ↓
3. Backend queries MongoDB for stats
   ↓
4. Returns aggregated data
   ↓
5. Dashboard displays charts/metrics
```

---

## Data Storage Structure

```
MongoDB Atlas (Cloud)
├── Users Collection
│   ├── _id
│   ├── name
│   ├── email
│   ├── phone
│   ├── profile_image
│   ├── createdAt
│   └── role (User/Admin)
│
├── Categories Collection
│   ├── _id
│   ├── name
│   ├── image
│   ├── description
│   └── createdAt
│
├── Services Collection
│   ├── _id
│   ├── name
│   ├── category_id
│   ├── image
│   ├── price
│   ├── description
│   └── createdAt
│
├── Bookings Collection
│   ├── _id
│   ├── user_id
│   ├── service_id
│   ├── booking_date
│   ├── price
│   ├── status
│   └── payment_id
│
└── Payments Collection
    ├── _id
    ├── order_id
    ├── payment_id
    ├── amount
    ├── status
    └── createdAt
```

---

## Deployment Timeline

```
Hour 1:
├─ 0:00 - Start
├─ 0:05 - Create .env files
├─ 0:10 - Set up MongoDB Atlas
├─ 0:15 - Push code to GitHub
└─ 0:20 - (Ready for deployment)

Hour 2:
├─ 0:20 - Deploy Backend on Render
├─ 0:35 - Deploy Admin on Render
├─ 0:45 - Deploy User on Render
├─ 1:05 - Update CORS in backend
└─ 1:15 - Everything is LIVE! 🎉

Testing:
├─ Check backend endpoint
├─ Test admin login
├─ Test user signup
├─ Create booking
└─ Verify admin dashboard
```

---

## Performance Considerations

### Free Tier Limitations:
- ⏱️ Backend sleeps after 15 min inactivity (wakes on first request)
- 💾 File uploads not persistent (use S3 or Cloudinary)
- 🚀 Build time: 5-10 minutes per deployment
- 🔄 Auto-redeploy on every git push

### Optimization Tips:
1. **Use CDN**: Render includes built-in CDN for static assets
2. **Database**: MongoDB Atlas free tier: 512MB
3. **Images**: Compress before upload
4. **Frontend**: React production build is already optimized

---

## Monitoring & Troubleshooting

### View Logs:
```
Render Dashboard → Your Service → Logs
```

### Common Issues & Fixes:

| Issue | Cause | Fix |
|-------|-------|-----|
| 502 Bad Gateway | Backend crashed | Check logs, verify env vars |
| CORS Error | Frontend URL not in CORS | Update index.js CORS list |
| MongoDB Error | Wrong connection string | Verify URI, check whitelist |
| Build Failed | Missing dependencies | Check package.json |
| File Upload Failed | Free tier limitation | Use AWS S3 instead |

---

## Update Flow After Deployment

```
1. Make code changes locally
   ↓
2. Test locally (npm start)
   ↓
3. Commit: git commit -m "message"
   ↓
4. Push: git push
   ↓
5. Render auto-detects push
   ↓
6. Auto-redeploys (5-10 min)
   ↓
7. Your changes are LIVE
```

**No manual deployment needed after initial setup!**

---

## Next Level: Custom Domain

```
Your Custom Domain (optional)
│
├─ admin.yourdomain.com ──► Render Admin Site
├─ yourdomain.com ────────► Render User Site
└─ api.yourdomain.com ────► Render Backend
```

To set up:
1. Go to Render service → Settings
2. Add custom domain
3. Update DNS records (Render will guide you)

---

## Architecture Checklist

- [x] Frontend static hosting (Render)
- [x] Backend API server (Render)
- [x] Database (MongoDB Atlas)
- [x] Environment variables configured
- [x] CORS configured
- [x] JWT authentication
- [x] File upload ready
- [x] Payment gateway integrated
- [x] Auto-redeployment on push

Your complete MERN stack is ready! 🚀
