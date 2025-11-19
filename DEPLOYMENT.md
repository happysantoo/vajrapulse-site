# Deploying VajraPulse Website to Cloudflare Pages

This guide will walk you through deploying the VajraPulse website to Cloudflare Pages using your custom domain.

## Prerequisites

- A GitHub account with the repository pushed (already done ✅)
- A Cloudflare account (free tier works)
- A custom domain name
- DNS access to your domain (or ability to change nameservers)

## Step 1: Prepare Your Repository

The website files are already in the `docs/` folder, which is perfect for Cloudflare Pages. No additional configuration needed!

## Step 2: Connect Repository to Cloudflare Pages

1. **Log in to Cloudflare Dashboard**
   - Go to [dash.cloudflare.com](https://dash.cloudflare.com)
   - Sign in or create a free account

2. **Navigate to Pages**
   - In the left sidebar, click on **"Workers & Pages"**
   - Click **"Create application"**
   - Select **"Pages"** tab
   - Click **"Connect to Git"**

3. **Authorize Cloudflare**
   - Select **GitHub** (or GitLab/Bitbucket if you prefer)
   - Authorize Cloudflare to access your repositories
   - You may need to install the Cloudflare Pages GitHub app if prompted

4. **Select Your Repository**
   - Find and select `vajrapulse-site` (or `happysantoo/vajrapulse-site`)
   - Click **"Begin setup"**

## Step 3: Configure Build Settings

Since this is a static website, the build configuration is minimal:

1. **Project name**: `vajrapulse-site` (or your preferred name)

2. **Production branch**: `main`

3. **Build command**: Leave empty (no build needed for static site)

4. **Build output directory**: `docs`
   - ⚠️ **Important**: Set this to `docs` since your website files are in the `docs/` folder

5. **Root directory**: Leave as `/` (default)

6. **Environment variables**: Not needed for this static site

7. Click **"Save and Deploy"**

## Step 4: Wait for Initial Deployment

- Cloudflare will automatically deploy your site
- This usually takes 1-2 minutes
- You'll see a progress indicator
- Once complete, you'll get a preview URL like: `https://vajrapulse-site.pages.dev`

## Step 5: Set Up Custom Domain

### Option A: Using Cloudflare DNS (Recommended)

If your domain is already using Cloudflare DNS:

1. **In Cloudflare Pages Dashboard**
   - Go to your project → **"Custom domains"** tab
   - Click **"Set up a custom domain"**
   - Enter your domain (e.g., `vajrapulse.com` or `www.vajrapulse.com`)

2. **Add DNS Records** (if not already present)
   - Go to your domain's DNS settings in Cloudflare
   - Add a **CNAME** record:
     - **Name**: `@` (for root domain) or `www` (for www subdomain)
     - **Target**: `vajrapulse-site.pages.dev` (your Pages URL)
     - **Proxy status**: Proxied (orange cloud) ✅

3. **SSL/TLS Configuration**
   - Cloudflare automatically provisions SSL certificates
   - Go to **SSL/TLS** → **Overview**
   - Ensure it's set to **"Full"** or **"Full (strict)"**
   - Wait a few minutes for SSL to activate

### Option B: Using External DNS Provider

If your domain uses a different DNS provider:

1. **In Cloudflare Pages Dashboard**
   - Go to your project → **"Custom domains"** tab
   - Click **"Set up a custom domain"**
   - Enter your domain (e.g., `vajrapulse.com`)

2. **Add DNS Records in Your DNS Provider**
   - Log in to your domain registrar or DNS provider
   - Add a **CNAME** record:
     - **Name**: `@` (for root domain) or `www` (for www subdomain)
     - **Value/Target**: `vajrapulse-site.pages.dev`
     - **TTL**: 3600 (or default)

3. **Wait for DNS Propagation**
   - DNS changes can take 5 minutes to 48 hours
   - Usually completes within 1-2 hours
   - You can check propagation with tools like [whatsmydns.net](https://www.whatsmydns.net)

4. **SSL Certificate**
   - Cloudflare will automatically issue an SSL certificate once DNS propagates
   - This usually happens within 24 hours

## Step 6: Configure Domain Settings (Optional but Recommended)

### Redirect www to non-www (or vice versa)

1. **In Cloudflare Pages**
   - Go to **"Custom domains"**
   - Add both `vajrapulse.com` and `www.vajrapulse.com`
   - Set one as primary

2. **In Cloudflare DNS**
   - Add a **Page Rule** or use **Redirect Rules**:
     - If: `www.vajrapulse.com/*`
     - Then: Redirect to `https://vajrapulse.com/$1` (301 redirect)

### Enable Always Use HTTPS

1. Go to **SSL/TLS** → **Edge Certificates**
2. Enable **"Always Use HTTPS"**
3. This automatically redirects HTTP to HTTPS

## Step 7: Verify Deployment

1. **Visit your custom domain**
   - Open `https://yourdomain.com` in a browser
   - Verify all pages load correctly
   - Check that images, CSS, and JavaScript load properly

2. **Test Links**
   - Click through navigation links
   - Verify anchor links work (`#features`, `#advantages`, etc.)
   - Test external links (GitHub, etc.)

3. **Check Mobile Responsiveness**
   - Test on mobile devices or use browser dev tools
   - Verify the responsive design works

## Step 8: Automatic Deployments

Cloudflare Pages automatically deploys when you push to the `main` branch:

1. **Make changes** to your website files
2. **Commit and push** to GitHub:
   ```bash
   git add .
   git commit -m "Update website"
   git push origin main
   ```
3. **Cloudflare automatically builds and deploys** (usually within 1-2 minutes)
4. **Preview deployments** are created for pull requests automatically

## Troubleshooting

### Issue: 404 Errors or Blank Page

**Solution**: Verify the build output directory is set to `docs` in Cloudflare Pages settings.

### Issue: Assets Not Loading (CSS/JS/Images)

**Solution**: 
- Check that all paths in `index.html` are relative (they should be)
- Verify files exist in the `docs/assets/` folder
- Check browser console for 404 errors

### Issue: Custom Domain Not Working

**Solution**:
- Verify DNS records are correct (CNAME pointing to Pages URL)
- Wait for DNS propagation (can take up to 48 hours)
- Check SSL/TLS settings in Cloudflare
- Ensure domain is properly added in Pages custom domains

### Issue: SSL Certificate Not Issued

**Solution**:
- Wait 24 hours for automatic certificate provisioning
- Verify DNS is correctly configured
- Check SSL/TLS mode is set to "Full" or "Full (strict)"
- Manually trigger certificate in SSL/TLS → Edge Certificates

### Issue: Build Fails

**Solution**:
- This shouldn't happen for a static site, but if it does:
- Check build output directory is `docs`
- Verify no build command is needed (leave empty)
- Check Cloudflare Pages build logs for errors

## Performance Optimization

Cloudflare Pages automatically provides:

- ✅ **Global CDN**: Content served from edge locations worldwide
- ✅ **Automatic HTTPS**: Free SSL certificates
- ✅ **DDoS Protection**: Built-in protection
- ✅ **Fast Deployments**: Usually under 2 minutes
- ✅ **Preview Deployments**: Automatic previews for PRs
- ✅ **Custom Domains**: Free custom domain support
- ✅ **Bandwidth**: Unlimited on free tier

## Additional Resources

- [Cloudflare Pages Documentation](https://developers.cloudflare.com/pages/)
- [Cloudflare Pages Pricing](https://developers.cloudflare.com/pages/platform/pricing/)
- [Custom Domain Setup Guide](https://developers.cloudflare.com/pages/platform/custom-domains/)

## Support

If you encounter issues:
1. Check Cloudflare Pages build logs
2. Review Cloudflare community forums
3. Check Cloudflare status page: [status.cloudflare.com](https://www.cloudflare.com/status/)

---

**Note**: The free tier of Cloudflare Pages includes:
- Unlimited requests
- Unlimited bandwidth
- 500 builds per month
- Custom domains
- Automatic HTTPS

This is more than sufficient for hosting the VajraPulse website!

