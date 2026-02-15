# Supabase Project Setup Guide

## 🚨 Current Issue

The Supabase project URL `https://dfkiospxzdngsusofenw.supabase.co` doesn't exist (DNS resolution failed). This means either:
- The project was deleted
- The URL is incorrect
- The project was never created

## ✅ Solutions

### Option 1: Create a New Supabase Project (Recommended)

1. **Go to Supabase**: https://supabase.com
2. **Sign up/Login** with your account
3. **Create New Project**:
   - Click "New Project"
   - Choose your organization
   - Enter project name: "InfraVision"
   - Choose a strong database password
   - Select a region close to you
   - Click "Create new project"

4. **Get Your Project Details**:
   - Wait for the project to be created (2-3 minutes)
   - Go to **Settings** → **API**
   - Copy your **Project URL** (it will look like `https://your-project-id.supabase.co`)
   - Copy your **anon public** key

5. **Update Your Configuration**:
   - Create a `.env` file in your project root:
   ```env
   VITE_SUPABASE_URL=https://your-new-project-id.supabase.co
   VITE_SUPABASE_ANON_KEY=your_new_anon_key_here
   ```

6. **Configure Authentication**:
   - Go to **Authentication** → **URL Configuration**
   - Add these URLs:
     - **Site URL**: `http://localhost:5000`
     - **Redirect URLs**: `http://localhost:5000/**`

### Option 2: Use Mock Authentication (Quick Fix)

If you want to test the app without setting up Supabase:

1. **Update the auth wrapper** to use mock authentication
2. **Disable real Supabase calls** temporarily
3. **Test the app functionality** without authentication

### Option 3: Find Existing Project

If you think you have an existing Supabase project:

1. **Check your Supabase dashboard**: https://supabase.com/dashboard
2. **Look for existing projects** in your account
3. **Get the correct URL and API key** from the project settings

## 🚀 Next Steps

1. **Choose one of the options above**
2. **Update your environment variables**
3. **Restart your development server**: `npm run dev`
4. **Test authentication**

## 📝 Environment File Template

Create a `.env` file in your project root:

```env
# Supabase Configuration
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=your_anon_key_here

# Development Configuration
NODE_ENV=development
PORT=5000
```

## 🔍 Verification

After setting up, verify your configuration:

1. **Check the URL**: Visit `https://your-project-id.supabase.co` in your browser
2. **Test API**: The URL should show a Supabase API response
3. **Check authentication**: Try signing in to your app

The CORS issue will be resolved once you have a valid Supabase project configured!
