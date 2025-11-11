# Deploying Astrolozee-AI to Render

## Quick Deploy

1. **Push to GitHub** (Already done ✅)
   ```bash
   git push astrologer main
   ```

2. **Sign up/Login to Render**
   - Go to https://render.com
   - Sign up or login with your GitHub account

3. **Create New Web Service**
   - Click "New +" → "Web Service"
   - Connect your GitHub repository: `msmahatha/Astrologer`
   - Select the repository

4. **Configure the Service**
   - **Name**: `astrolozee-ai` (or your preferred name)
   - **Region**: Oregon (US West) or closest to you
   - **Branch**: `main`
   - **Root Directory**: Leave blank (or specify `Astrolozee-AI` if deploying entire repo)
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`

5. **Set Environment Variables**
   Click "Advanced" → "Add Environment Variable" and add:
   
   ```
   PYTHON_VERSION=3.11.8
   OPENAI_API_KEY=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
   CHROMADB_API_KEY=ck-8VaKLwMqsWQrxr7GYP6hNfsoUWvht7iPyr8f8aL4MSm8
   CHROMADB_TENANT=9d32821e-797a-4feb-ab2e-67a4d5a1af20
   CHROMADB_DB_NAME=Astrolozee
   COLLECTION_NAME=knowledge_base
   EMBED_MODEL=text-embedding-3-small
   OPENAI_MODEL=gpt-4o
   TEMPERATURE=0.4
   MAX_TOKENS=800
   TOP_K=4
   MY_API_KEY=supersecret@123A$trolzee
   LANGSMITH_TRACING=false
   ```

6. **Select Plan**
   - Choose "Free" plan (or paid for better performance)
   - Free plan includes: 512 MB RAM, auto-sleep after 15 min inactivity

7. **Deploy**
   - Click "Create Web Service"
   - Render will automatically build and deploy
   - Wait 5-10 minutes for first deployment

8. **Access Your API**
   - Your API will be available at: `https://astrolozee-ai.onrender.com`
   - Test with: `https://astrolozee-ai.onrender.com/docs`

## Test Your Deployed API

```bash
curl -X POST "https://astrolozee-ai.onrender.com/astro/ask/" \
  -H "x-api-key: supersecret@123A\$trolzee" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What does Jupiter in 10th house mean for career?",
    "religion": "hindu"
  }'
```

## Alternative: Using render.yaml

If you prefer Infrastructure as Code:

1. The `render.yaml` file is already included
2. Go to Render Dashboard → "Blueprints"
3. Click "New Blueprint Instance"
4. Connect your GitHub repo
5. Render will automatically detect `render.yaml` and configure everything

## Troubleshooting

### Build Fails
- Check that `requirements.txt` is in root directory
- Verify Python version compatibility
- Check Render logs for specific errors

### API Not Responding
- Check environment variables are set correctly
- Verify OPENAI_API_KEY and CHROMADB credentials
- Check Render logs for runtime errors

### Timeout Issues
- Free tier may spin down after inactivity
- First request after spin-down takes 30-60 seconds
- Consider upgrading to paid plan for always-on service

### ChromaDB Connection Issues
- Verify CHROMADB_API_KEY is correct
- Check CHROMADB_TENANT matches your account
- Ensure ChromaDB collection exists

## Files Included for Deployment

- ✅ `render.yaml` - Render configuration file
- ✅ `Procfile` - Process file for start command
- ✅ `build.sh` - Build script
- ✅ `requirements.txt` - Python dependencies
- ✅ `.gitignore` - Excludes .env and test files

## Important Notes

1. **Don't commit .env file** - Already in .gitignore
2. **API Key Security** - Set as environment variable in Render dashboard
3. **Free Tier Limitations**:
   - Spins down after 15 min inactivity
   - 512 MB RAM
   - Shared CPU
   - Consider paid plan for production use

4. **Cold Start** - First request after sleep takes 30-60 seconds
5. **Logs** - Available in Render dashboard under "Logs" tab

## Update Deployed App

After making changes:
```bash
cd /Users/madhusudanmahatha/Downloads/Astrology/Astrolozee-AI
git add .
git commit -m "Your update message"
git push astrologer main
```

Render will automatically detect the push and redeploy!

## Custom Domain (Optional)

1. Go to Render Dashboard → Your Service
2. Click "Settings" → "Custom Domain"
3. Add your domain (e.g., `api.astrolozee.com`)
4. Update DNS records as instructed by Render

## Monitoring

- **Health Check**: `https://your-app.onrender.com/docs`
- **Logs**: Available in Render dashboard
- **Metrics**: CPU, Memory usage in dashboard
- **Alerts**: Configure in Render settings

---

🚀 **Your API is now ready for production!**

Base URL: `https://your-app-name.onrender.com`
Docs: `https://your-app-name.onrender.com/docs`
