# Render Deployment - Quick Fix Guide

## The Error You Saw:
```
ValidationError: 1 validation error for ChatOpenAI
model
  Input should be a valid string [type=string_type, input_value=None, input_type=NoneType]
```

## What It Means:
The `OPENAI_MODEL` environment variable wasn't set in Render, so it defaulted to `None`.

## ✅ What I Fixed:

1. **Added default value in config.py**:
   ```python
   OPENAI_MODEL = os.getenv("OPENAI_MODEL", "gpt-4o")  # Now has default!
   ```

2. **Added validation for critical variables**:
   - App will now exit with clear error if `OPENAI_API_KEY` or `MY_API_KEY` is missing
   - You'll see: "ERROR: OPENAI_API_KEY environment variable is not set!"

3. **Updated render.yaml**:
   - All numeric values now quoted as strings
   - All environment variables properly configured

## 🚀 Next Steps in Render:

### Option 1: Redeploy (Render auto-detects push)
1. Go to your Render dashboard
2. Your service should auto-deploy with the new code
3. **IMPORTANT**: Before it starts, click "Environment" tab
4. **Add these REQUIRED variables**:
   ```
   OPENAI_API_KEY=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
   MY_API_KEY=supersecret@123A$trolzee
   CHROMADB_API_KEY=ck-8VaKLwMqsWQrxr7GYP6hNfsoUWvht7iPyr8f8aL4MSm8
   CHROMADB_TENANT=9d32821e-797a-4feb-ab2e-67a4d5a1af20
   ```

### Option 2: Manual Deploy
1. Go to Render Dashboard → Your Service
2. Click "Manual Deploy" → "Deploy latest commit"
3. Set environment variables as above

## 🔍 Check Deployment Status:

1. **Watch Logs**:
   - Render Dashboard → Your Service → Logs tab
   - Look for: `INFO: Application startup complete.`

2. **Test Health**:
   ```bash
   curl https://your-app.onrender.com/docs
   ```

3. **Test API**:
   ```bash
   curl -X POST "https://your-app.onrender.com/astro/ask/" \
     -H "x-api-key: supersecret@123A\$trolzee" \
     -H "Content-Type: application/json" \
     -d '{
       "question": "Test deployment",
       "religion": "hindu"
     }'
   ```

## 🐛 If Still Failing:

### Check Logs for:
1. **"ERROR: OPENAI_API_KEY environment variable is not set!"**
   → Add the environment variable in Render dashboard

2. **"ValidationError"**
   → Make sure you set ALL required environment variables

3. **"Module not found"**
   → Check that `requirements.txt` is being installed correctly

### Common Issues:

**Issue**: Build succeeds but app crashes on start
**Fix**: Environment variables not set → Add them in Render dashboard

**Issue**: Import errors
**Fix**: Make sure Root Directory is blank or set to project root

**Issue**: Port binding error
**Fix**: Render automatically sets $PORT - don't hardcode port 8000

## 📋 Environment Variables Checklist:

- [ ] OPENAI_API_KEY ⚠️ CRITICAL
- [ ] MY_API_KEY ⚠️ CRITICAL  
- [ ] CHROMADB_API_KEY ⚠️ CRITICAL
- [ ] CHROMADB_TENANT ⚠️ CRITICAL
- [ ] OPENAI_MODEL (optional, defaults to gpt-4o)
- [ ] TEMPERATURE (optional, defaults to 0.4)
- [ ] MAX_TOKENS (optional, defaults to 800)

## ✅ Success Indicators:

You'll know it's working when you see:
```
INFO: Started server process
INFO: Waiting for application startup.
INFO: Application startup complete.
INFO: Uvicorn running on http://0.0.0.0:XXXXX
```

Then test: `https://your-app.onrender.com/docs` should load Swagger UI!

---

**Updated Code**: Already pushed to GitHub (commit 39a4d6f)
**Changes**: Default values added, validation added, deployment guide updated
