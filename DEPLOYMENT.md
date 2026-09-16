# Deployment Guide

This project includes automated GitHub Actions workflows for testing and deployment.

## GitHub Actions Workflows

### 1. Tests Workflow (`.github/workflows/tests.yml`)
- Runs on every push to `main` and `develop` branches
- Runs on pull requests
- **Steps:**
  - Checks out code
  - Sets up Node.js 18
  - Installs dependencies
  - Runs linter (if configured)
  - Runs tests (if configured)

### 2. Deploy Workflow (`.github/workflows/deploy.yml`)
- Runs on every push to `main` branch
- Automatically deploys to Render
- **Steps:**
  - Checks out code
  - Sets up Node.js 18
  - Installs dependencies
  - Runs tests
  - Triggers Render deployment webhook

## Setup Instructions

### Option 1: Deploy to Render (Recommended - Free Tier Available)

1. **Create Render Account**
   - Go to [render.com](https://render.com)
   - Sign up with your GitHub account

2. **Create New Web Service**
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Select `muntasirul/Home-services-provider-platform-Proyojon-`

3. **Configure Service**
   - **Name:** proyojon-api
   - **Environment:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Plan:** Free or Paid

4. **Set Environment Variables**
   - Go to "Environment" section
   - Add the following variables:
     ```
     MONGODB_URI=your_mongodb_atlas_connection_string
     GEMINI_API_KEY=your_gemini_api_key
     NODE_ENV=production
     PORT=5050
     ```

5. **Get Deploy Hook**
   - Go to "Settings" → "Deploy Hook"
   - Copy the hook URL
   - Add it to GitHub Secrets as `RENDER_DEPLOY_HOOK`

6. **Add GitHub Secret**
   - Go to your repo → Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - **Name:** `RENDER_DEPLOY_HOOK`
   - **Value:** Paste your Render deploy hook URL
   - Click "Add secret"

### Option 2: Deploy to Railway

1. **Create Railway Account**
   - Go to [railway.app](https://railway.app)
   - Sign up with GitHub

2. **Create New Project**
   - Click "Create New Project"
   - Select "Deploy from GitHub repo"
   - Choose your repository

3. **Configure Environment**
   - Add variables in Railway dashboard:
     - `MONGODB_URI`
     - `GEMINI_API_KEY`
     - `NODE_ENV=production`

4. **Deploy**
   - Railway automatically deploys on push to main
   - No additional webhook setup needed

### Option 3: Deploy to GitHub Pages (Frontend Only)

1. Create a separate workflow for frontend static files
2. Push only `index.html`, `app.js`, and `style.css` to `gh-pages` branch
3. Access at: `https://muntasirul.github.io/Home-services-provider-platform-Proyojon-/`

## Environment Variables Required

| Variable | Description | Example |
|----------|-------------|---------|
| `MONGODB_URI` | MongoDB connection string | `mongodb+srv://user:pass@cluster.mongodb.net/proyojon` |
| `GEMINI_API_KEY` | Google Gemini API key | `AIzaSyD...` |
| `NODE_ENV` | Environment | `production` |
| `PORT` | Server port | `5050` |
| `BCRYPT_ROUNDS` | Password hash rounds | `10` |
| `CORS_ORIGIN` | CORS allowed origins | `*` or specific domain |

## Monitoring Deployments

1. Go to your repo → "Actions" tab
2. View workflow runs in real-time
3. Check logs for any errors
4. Each workflow shows:
   - Build status ✅ or ❌
   - Deployment link (after successful deploy)
   - Detailed logs for debugging

## Troubleshooting

**Workflow not triggering?**
- Verify branch protection rules aren't blocking it
- Check that `.github/workflows/` files are properly committed

**Deploy hook failing?**
- Verify `RENDER_DEPLOY_HOOK` secret is set correctly
- Check Render dashboard for service status

**Build failures?**
- Check workflow logs for specific errors
- Ensure all dependencies are in `package.json`
- Verify environment variables are correctly set

## Accessing Your Application

After successful deployment:

**Render:** `https://proyojon-api.onrender.com` (or your chosen name)

**Railway:** Check your Railway dashboard for the assigned URL

**GitHub Pages:** `https://muntasirul.github.io/Home-services-provider-platform-Proyojon-/`

## Testing Locally Before Deploy

```bash
# Install dependencies
npm install

# Set up .env file with your variables
cp .env.example .env
# Edit .env with your actual values

# Start development server
npm start

# Access at http://localhost:5050
```
