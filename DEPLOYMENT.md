# Student Management System - Vercel Deployment

## Setup Instructions

### Note about databases
This project has been prepared for a Serverless deployment on Vercel using *mock* in-memory data for development/testing. The original MongoDB integration was removed and replaced with temporary mock data so the serverless functions run without an external DB.

If you later want to use a real database in production, you can reintroduce a cloud DB (MongoDB Atlas or other) and update the API functions accordingly.

### 2. Deploy to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Login to Vercel
vercel login

# Deploy
vercel

# During deployment, add this environment variable if you use JWTs:
# JWT_SECRET = any random string (e.g., "my-secret-key-123")
```

### 3. After Deployment

Once deployed, you'll get a URL like `https://your-project.vercel.app`

- Frontend: `https://your-project.vercel.app`
- APIs: `https://your-project.vercel.app/api/*`

The frontend will automatically use the new API URLs.

## Testing Locally (before deployment)

1. Update `public/app.js` and `public/auth.js`:
   - Change `const API_URL = '/api'` to `const API_URL = 'http://localhost:3000/api'`

2. (Optional) If you want to connect to a real database later, install MongoDB locally or use a cloud connection string

3. Run:
```bash
npm install
node server.js
```

## File Structure
```
app/
├── api/                    # Vercel serverless functions
│   ├── db.js              # MongoDB connection & schemas
│   ├── auth-utils.js      # JWT utilities
│   ├── auth/
│   │   ├── register.js
│   │   ├── login.js
│   │   └── verify.js
│   ├── students/
│   │   ├── index.js       # GET all, POST create
│   │   ├── [id].js        # GET one, PUT update, DELETE
│   │   └── search/
│   │       └── [query].js # Search students
│   └── statistics.js      # Dashboard stats
├── public/                # Frontend files
│   ├── index.html
│   ├── login.html
│   ├── app.js
│   ├── auth.js
│   └── styles.css
├── server.js              # Local development server
├── package.json
└── vercel.json           # Vercel config
```

## Important Notes

- **sessionStorage** is used for tokens (cleared on browser close)
- The API automatically includes CORS headers
- All routes require JWT authentication (except register/login)
- MongoDB Atlas free tier has limits (512MB storage, shared resources)
