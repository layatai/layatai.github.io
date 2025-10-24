# Deployment Guide

This guide covers two deployment options for your CV website:

1. **Cloudflare Pages** (recommended for performance)
2. **GitHub Pages** (simple, integrated with GitHub)

Choose the option that best fits your needs.

---

## Option 1: Cloudflare Pages Deployment

This guide will help you set up automatic deployment of your CV website to Cloudflare Pages using GitHub Actions.

### Prerequisites

- GitHub repository with your CV website code
- Cloudflare account
- Node.js installed locally (for testing builds)

### Step 1: Create Cloudflare Pages Project

1. Log in to your [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Navigate to **Pages** in the left sidebar
3. Click **Create a project**
4. Choose **Connect to Git**
5. Select your GitHub repository
6. Configure the project:
   - **Project name**: `cv-website` (or your preferred name)
   - **Production branch**: `main`
   - **Build command**: `npm run build`
   - **Build output directory**: `dist`
7. Click **Save and Deploy**

### Step 2: Get Required Credentials

#### Get Cloudflare API Token

1. Go to [Cloudflare API Tokens](https://dash.cloudflare.com/profile/api-tokens)
2. Click **Create Token**
3. Use **Custom token** template
4. Configure permissions:
   - **Account**: `Cloudflare Pages:Edit`
   - **Zone**: `Zone:Read` (if using custom domain)
5. Click **Continue to summary** then **Create Token**
6. Copy the token (you won't see it again!)

#### Get Account ID

1. In your Cloudflare dashboard, select any domain
2. Scroll down to **API** section in the right sidebar
3. Copy your **Account ID**

### Step 3: Configure GitHub Secrets

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret** and add these secrets:

   | Secret Name | Description | Value |
   |-------------|-------------|-------|
   | `CLOUDFLARE_API_TOKEN` | Your Cloudflare API token | The token you created in Step 2 |
   | `CLOUDFLARE_ACCOUNT_ID` | Your Cloudflare Account ID | The Account ID from Step 2 |
   | `CLOUDFLARE_PROJECT_NAME` | Your Pages project name | The project name you set in Step 1 |

### Step 4: Test the Deployment

1. Make a small change to your website (e.g., update a skill percentage)
2. Commit and push to the `main` branch:
   ```bash
   git add .
   git commit -m "Test deployment"
   git push origin main
   ```
3. Go to your GitHub repository → **Actions** tab
4. Watch the deployment workflow run
5. Once complete, visit your Cloudflare Pages URL

### Step 5: Custom Domain (Optional)

1. In Cloudflare Pages dashboard, go to your project
2. Click **Custom domains**
3. Add your domain
4. Follow the DNS configuration instructions

---

## Option 2: GitHub Pages Deployment

GitHub Pages provides a simple way to host your website directly from your GitHub repository.

### Prerequisites

- GitHub repository with your CV website code
- Node.js installed locally (for testing builds)

### Step 1: Enable GitHub Pages

1. Go to your GitHub repository
2. Navigate to **Settings** → **Pages**
3. Under **Source**, select **GitHub Actions**
4. The workflow will automatically deploy when you push to main

### Step 2: Test the Deployment

1. Make a small change to your website
2. Commit and push to the `main` branch:
   ```bash
   git add .
   git commit -m "Test GitHub Pages deployment"
   git push origin main
   ```
3. Go to your GitHub repository → **Actions** tab
4. Watch the "Deploy to GitHub Pages" workflow run
5. Once complete, your site will be available at:
   `https://yourusername.github.io/your-repository-name`

### Step 3: Custom Domain (Optional)

1. In your repository **Settings** → **Pages**
2. Add your custom domain in the **Custom domain** field
3. Follow GitHub's instructions for DNS configuration

---

## Build Process

Both deployment options will automatically:

1. **Checkout** your code
2. **Install** Node.js dependencies
3. **Minify** HTML, CSS, and JavaScript files
4. **Copy** assets (like your PDF CV)
5. **Deploy** to the chosen platform

## File Structure After Build

```
dist/
├── index.html          # Minified HTML
├── styles.css          # Minified CSS
├── script.js           # Minified JavaScript
└── *.pdf # Copied PDF
```

## Troubleshooting

### Build Fails
- Check that all dependencies are in `package.json`
- Verify file paths in build scripts
- Test locally: `npm run build`

### Cloudflare Deployment Fails
- Verify GitHub secrets are correctly set
- Check Cloudflare API token permissions
- Ensure project name matches exactly

### GitHub Pages Deployment Fails
- Check that Pages is enabled in repository settings
- Verify the workflow has proper permissions
- Ensure the repository is public (required for free GitHub Pages)

### Assets Not Loading
- Verify file paths in HTML are correct
- Check that all assets are copied to `dist/` folder

## Manual Deployment

### Cloudflare Pages
```bash
# Install dependencies
npm install

# Build the project
npm run build

# Deploy using Wrangler CLI
npx wrangler pages publish dist --project-name=your-project-name
```

### GitHub Pages
```bash
# Install dependencies
npm install

# Build the project
npm run build

# Deploy using gh-pages (install first: npm install -g gh-pages)
gh-pages -d dist
```

## Comparison

| Feature | Cloudflare Pages | GitHub Pages |
|---------|------------------|--------------|
| **Performance** | Excellent (global CDN) | Good |
| **Custom Domain** | Free | Free |
| **HTTPS** | Automatic | Automatic |
| **Build Time** | Fast | Moderate |
| **Setup Complexity** | Medium | Simple |
| **Cost** | Free tier available | Free for public repos |

## Support

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Wrangler CLI Documentation](https://developers.cloudflare.com/workers/wrangler/)
