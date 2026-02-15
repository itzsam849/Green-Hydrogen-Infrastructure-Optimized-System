# CORS Issue Resolution for InfraVision

## 🚨 Current Issue

You're experiencing a CORS (Cross-Origin Resource Sharing) error when trying to authenticate with Supabase from `http://localhost:5000`. This happens because Supabase's authentication endpoints don't allow requests from your local development server by default.

## ✅ Solution Implemented

I've implemented a comprehensive solution that includes:

1. **Enhanced Error Handling**: The app now provides clear error messages when CORS issues occur
2. **Development-Specific Configuration**: Disabled automatic token refresh to prevent CORS issues
3. **Auth Wrapper**: Created a wrapper that handles CORS errors gracefully

## 🔧 Required Setup

### Step 1: Configure Supabase Project (CRITICAL)

**This is the main fix - you MUST do this:**

1. Go to your Supabase Dashboard: https://supabase.com/dashboard/project/dfkiospxzdngsusofenw
2. Navigate to **Authentication** → **URL Configuration**
3. Add these URLs:
   - **Site URL**: `http://localhost:5000`
   - **Redirect URLs**: 
     - `http://localhost:5000/**`
     - `http://localhost:5173/**` (backup)

### Step 2: Set Environment Variables

Create a `.env` file in your project root:

```env
# Supabase Configuration
VITE_SUPABASE_URL=https://dfkiospxzdngsusofenw.supabase.co
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key_here

# Development Configuration
NODE_ENV=development
PORT=5000
```

### Step 3: Get Your Supabase Anon Key

1. In your Supabase Dashboard, go to **Settings** → **API**
2. Copy the **anon public** key
3. Replace `your_supabase_anon_key_here` with the actual key

## 🎯 How the Solution Works

1. **Direct Connection**: The app now connects directly to Supabase (no proxy)
2. **CORS Error Handling**: If CORS errors occur, the app shows helpful error messages
3. **Development Mode**: Automatic token refresh is disabled in development to prevent CORS issues
4. **Graceful Degradation**: The app continues to work even if some features are limited

## 🚀 Testing

After completing the setup:

1. **Restart your development server**:
   ```bash
   npm run dev
   ```

2. **Test authentication**: Try signing in - you should see either:
   - ✅ Successful authentication (if CORS is configured)
   - ❌ Clear error message explaining the CORS issue (if not configured)

## 🔍 Troubleshooting

### If you still get CORS errors:

1. **Verify Supabase Configuration**: Double-check that `http://localhost:5000` is added to your Supabase project's allowed origins
2. **Check Environment Variables**: Ensure your `.env` file is in the project root and contains the correct Supabase anon key
3. **Clear Browser Cache**: Sometimes browsers cache CORS responses
4. **Try Different Port**: The app also works on `http://localhost:5173`

### Alternative: Use Port 5173

If you can't configure Supabase CORS settings:

1. The app will automatically work on `http://localhost:5173`
2. This port is more commonly allowed by default in Supabase projects

## 📝 What Changed

- ✅ Removed problematic proxy configuration
- ✅ Added comprehensive error handling for CORS issues
- ✅ Created auth wrapper with better error messages
- ✅ Disabled automatic token refresh in development
- ✅ Added clear setup instructions

The CORS error should now be resolved once you configure your Supabase project settings!
