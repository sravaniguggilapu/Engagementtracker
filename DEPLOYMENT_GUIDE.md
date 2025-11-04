# 🚀 Deployment Guide - Engagement Tracker

Complete guide for deploying the Engagement Tracker with Wellness Alerts to various hosting platforms.

## 📦 Prerequisites

Before deploying, ensure you have:
- Completed `yarn build` successfully
- All files in the `build/` directory
- The CSV data file in `public/data/combined_dataset.csv`

---

## 🌐 GitHub Pages Deployment

### Step 1: Prepare Your Repository

1. Create a new GitHub repository
2. Initialize git in your project (if not already done):
   ```bash
   cd /app/frontend
   git init
   git add .
   git commit -m "Initial commit - Engagement Tracker"
   ```

3. Add your GitHub repository as remote:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/engagement-tracker.git
   git branch -M main
   git push -u origin main
   ```

### Step 2: Configure package.json

Add the homepage field to your `package.json`:
```json
{
  "homepage": "https://YOUR_USERNAME.github.io/engagement-tracker"
}
```

### Step 3: Install gh-pages

```bash
yarn add --dev gh-pages
```

### Step 4: Add Deploy Scripts

Add these scripts to your `package.json`:
```json
{
  "scripts": {
    "predeploy": "yarn build",
    "deploy": "gh-pages -d build"
  }
}
```

### Step 5: Deploy

```bash
yarn deploy
```

Your site will be available at: `https://YOUR_USERNAME.github.io/engagement-tracker/`

---

## ⚡ Netlify Deployment

### Option 1: Drag & Drop

1. Build your project:
   ```bash
   yarn build
   ```

2. Go to [Netlify Drop](https://app.netlify.com/drop)

3. Drag the `build/` folder onto the page

4. Your site is live! Netlify will provide a URL like `random-name-123.netlify.app`

### Option 2: GitHub Integration

1. Push your code to GitHub

2. Go to [Netlify](https://app.netlify.com)

3. Click "New site from Git"

4. Choose GitHub and select your repository

5. Configure build settings:
   - **Build command**: `yarn build`
   - **Publish directory**: `build`

6. Click "Deploy site"

7. (Optional) Configure custom domain in Site settings

---

## 🔷 Vercel Deployment

### Option 1: CLI

1. Install Vercel CLI:
   ```bash
   npm i -g vercel
   ```

2. Deploy:
   ```bash
   cd /app/frontend
   vercel
   ```

3. Follow the prompts (select React as framework)

### Option 2: GitHub Integration

1. Push code to GitHub

2. Go to [Vercel](https://vercel.com)

3. Click "New Project"

4. Import your GitHub repository

5. Vercel auto-detects Create React App settings

6. Click "Deploy"

---

## ☁️ AWS S3 + CloudFront

### Step 1: Build Your App

```bash
yarn build
```

### Step 2: Create S3 Bucket

1. Go to AWS S3 Console
2. Create a new bucket (e.g., `engagement-tracker`)
3. Disable "Block all public access"
4. Enable static website hosting

### Step 3: Configure Bucket Policy

Add this bucket policy (replace `YOUR_BUCKET_NAME`):
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```

### Step 4: Upload Files

```bash
aws s3 sync build/ s3://YOUR_BUCKET_NAME --delete
```

### Step 5: (Optional) Setup CloudFront

1. Create CloudFront distribution
2. Set origin to your S3 bucket
3. Configure HTTPS
4. Set default root object to `index.html`

---

## 🌊 Azure Static Web Apps

### Step 1: Install Azure CLI

```bash
npm install -g @azure/static-web-apps-cli
```

### Step 2: Build Your App

```bash
yarn build
```

### Step 3: Deploy via GitHub Actions

1. Push code to GitHub

2. Go to Azure Portal → Static Web Apps

3. Create new Static Web App

4. Connect to GitHub repository

5. Configure:
   - **App location**: `/`
   - **Build location**: `build`

6. Azure will create a GitHub Action automatically

7. Commit triggers automatic deployment

---

## 📁 File Structure After Build

```
build/
├── data/
│   └── combined_dataset.csv
├── static/
│   ├── css/
│   │   └── main.[hash].css
│   ├── js/
│   │   └── main.[hash].js
│   └── media/
├── index.html
├── manifest.json
└── robots.txt
```

**Important**: Make sure the `data/` folder with `combined_dataset.csv` is included in your build!

---

## 🔧 Troubleshooting

### CSV File Not Loading

**Problem**: 404 error when loading CSV data

**Solution**: Ensure `combined_dataset.csv` is in `public/data/` before building

### Routing Issues (404 on Refresh)

**Problem**: Page refreshes result in 404 errors

**Solutions**:

- **Netlify**: Create `public/_redirects` file:
  ```
  /*    /index.html   200
  ```

- **Vercel**: Create `vercel.json`:
  ```json
  {
    "rewrites": [{ "source": "/(.*)", "destination": "/" }]
  }
  ```

- **AWS S3**: Configure error document to `index.html`

### Images Not Loading

**Problem**: Unsplash images not displaying

**Solution**: The app includes fallback images. Check your internet connection or replace with local images.

### Build Fails

**Problem**: `yarn build` fails

**Solutions**:
1. Clear cache: `rm -rf node_modules && yarn install`
2. Check Node version: Requires v16+
3. Review error logs for specific issues

---

## 🔒 Environment Variables

For production deployment, you may want to configure:

Create `.env.production`:
```bash
PUBLIC_URL=https://yourdomain.com
REACT_APP_API_URL=https://api.yourdomain.com
```

**Note**: This prototype doesn't require API keys or backend services.

---

## ✅ Pre-Deployment Checklist

- [ ] Run `yarn build` successfully
- [ ] Test build locally: `npx serve -s build`
- [ ] Verify CSV data is included in build
- [ ] Check all routes work correctly
- [ ] Test on multiple browsers
- [ ] Verify responsive design
- [ ] Check console for errors
- [ ] Update homepage URL in package.json
- [ ] Configure redirects for SPA routing
- [ ] Test deployed site thoroughly

---

## 📊 Performance Optimization

### Before Deployment

1. **Optimize Images**:
   - Use WebP format where possible
   - Compress images (TinyPNG, ImageOptim)

2. **Code Splitting**: Already handled by Create React App

3. **Enable Compression**: Ensure gzip/brotli is enabled on hosting

4. **CDN**: Use CloudFront or Netlify CDN for global distribution

### After Deployment

Monitor with:
- Google Lighthouse
- WebPageTest
- GTmetrix

Target scores:
- Performance: 90+
- Accessibility: 95+
- Best Practices: 90+
- SEO: 85+

---

## 🔄 Continuous Deployment

### Automated Deployment Workflow

Most platforms support automatic deployment on git push:

1. **Netlify/Vercel**: Auto-deploys on push to main branch
2. **GitHub Pages**: Use `gh-pages` branch
3. **AWS**: Setup CodePipeline for CI/CD
4. **Azure**: GitHub Actions workflow created automatically

---

## 📱 Custom Domain Setup

### Netlify/Vercel

1. Go to Domain Settings
2. Add custom domain
3. Update DNS records (provided by platform)
4. Enable HTTPS (automatic)

### AWS S3 + CloudFront

1. Use Route 53 for DNS
2. Create certificate in ACM
3. Configure CloudFront with custom domain
4. Update DNS CNAME records

---

## 🎯 Post-Deployment Testing

After deployment, verify:

1. **Navigation**: All pages accessible
2. **Data Loading**: CSV loads correctly
3. **Charts**: All visualizations render
4. **Search**: Student search works
5. **Filters**: Department filter functions
6. **Responsive**: Works on mobile/tablet
7. **HTTPS**: Site is secure
8. **Performance**: Page load < 3 seconds

---

## 📞 Support & Resources

- [React Deployment Docs](https://create-react-app.dev/docs/deployment/)
- [Netlify Docs](https://docs.netlify.com/)
- [Vercel Docs](https://vercel.com/docs)
- [GitHub Pages Docs](https://docs.github.com/en/pages)
- [AWS S3 Static Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)

---

**Your Engagement Tracker is now ready to help students succeed! 🎓**
