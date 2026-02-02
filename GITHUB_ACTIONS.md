# GitHub Actions Deployment

## 🚀 Automated Deployment Setup

The repository now uses **GitHub Actions** for automated deployment to GitHub Pages.

## 📋 Workflow Details

### File Location
`.github/workflows/deploy.yml`

### Trigger Events
- **Push to main branch**: Automatically deploys on every push
- **Manual trigger**: Can be triggered manually via GitHub UI

### Workflow Steps

#### 1. Build Job
1. **Checkout code**: Clones the repository
2. **Setup Node.js**: Installs Node.js 20
3. **Install dependencies**: Runs `npm ci`
4. **Validate OpenAPI**: Runs `npm run validate`
5. **Validate AsyncAPI**: Runs `npm run validate:asyncapi`
6. **Build documentation**: Creates `docs/` directory with all files
7. **Upload artifact**: Prepares files for deployment

#### 2. Deploy Job
1. **Deploy to GitHub Pages**: Publishes the `docs/` artifact

## 📁 Deployed Files

The workflow deploys the following files to GitHub Pages:

- ✅ `landing.html` - Beautiful landing page with navigation
- ✅ `index.html` - Redoc documentation viewer
- ✅ `swagger.html` - Swagger UI interactive explorer
- ✅ `openapi.yaml` - Complete OpenAPI specification
- ✅ `websocket-api.yaml` - Complete AsyncAPI specification
- ✅ `README.md` - Project documentation
- ✅ `QUICKSTART.md` - Quick start guide

## 🌐 Live URLs

After deployment completes (usually 1-2 minutes):

### Main Pages
- **Landing Page**: https://bowen31337.github.io/hyperliquid-openapi/landing.html
- **Redoc**: https://bowen31337.github.io/hyperliquid-openapi/
- **Swagger UI**: https://bowen31337.github.io/hyperliquid-openapi/swagger.html

### Downloadable Files
- **OpenAPI Spec**: https://bowen31337.github.io/hyperliquid-openapi/openapi.yaml
- **AsyncAPI Spec**: https://bowen31337.github.io/hyperliquid-openapi/websocket-api.yaml

## 🔄 Deployment Process

### Automatic Deployment
```bash
# Make changes to any file
git add .
git commit -m "Your commit message"
git push

# GitHub Actions automatically:
# 1. Validates the OpenAPI and AsyncAPI specs
# 2. Builds the documentation
# 3. Deploys to GitHub Pages
# 4. Site is live in ~1-2 minutes
```

### Manual Deployment
1. Go to: https://github.com/bowen31337/hyperliquid-openapi/actions
2. Click "Deploy to GitHub Pages"
3. Click "Run workflow"
4. Select branch (main)
5. Click "Run workflow" button

## 📊 Monitoring Deployments

### View Workflow Runs
```bash
# List recent runs
gh run list --limit 5

# Watch latest run
gh run watch

# View run details
gh run view
```

### Via GitHub UI
1. Go to: https://github.com/bowen31337/hyperliquid-openapi/actions
2. View all workflow runs
3. Click on a run to see details
4. View logs for each step

## ✅ Validation

The workflow includes automatic validation:

### OpenAPI Validation
- Runs `npm run validate`
- Uses Redocly CLI
- Checks for spec compliance
- Fails build if errors found

### AsyncAPI Validation
- Runs `npm run validate:asyncapi`
- Uses AsyncAPI CLI
- Validates message schemas
- Continues even if validation fails (optional)

## 🔧 Workflow Configuration

### Permissions
```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

### Concurrency
```yaml
concurrency:
  group: "pages"
  cancel-in-progress: false
```

Ensures only one deployment runs at a time.

### Environment
```yaml
environment:
  name: github-pages
  url: ${{ steps.deployment.outputs.page_url }}
```

## 🎨 Features

### Landing Page
- Beautiful gradient design
- Navigation to Redoc and Swagger UI
- Statistics display (endpoints, schemas, etc.)
- Feature highlights
- Resource links

### Redoc Page
- Custom header with navigation
- Links to Swagger UI and landing page
- Beautiful, responsive documentation
- Advanced search functionality

### Swagger UI Page
- Custom header with navigation
- Interactive API testing
- "Try it out" functionality
- Request/response examples
- Syntax highlighting

## 🔄 Update Workflow

### Update Documentation
1. Edit `openapi.yaml` or `websocket-api.yaml`
2. Run validation locally: `npm run validate`
3. Commit and push
4. GitHub Actions deploys automatically

### Update UI
1. Edit `index.html`, `swagger.html`, or `landing.html`
2. Test locally: `npm start`
3. Commit and push
4. GitHub Actions deploys automatically

### Update Workflow
1. Edit `.github/workflows/deploy.yml`
2. Commit and push
3. Workflow updates automatically

## 🐛 Troubleshooting

### Deployment Failed
1. Check workflow logs: `gh run view`
2. Look for validation errors
3. Fix errors and push again

### Pages Not Updating
1. Check workflow status: `gh run list`
2. Wait 1-2 minutes for deployment
3. Clear browser cache
4. Check GitHub Pages settings

### Validation Errors
1. Run locally: `npm run validate`
2. Fix reported errors
3. Commit and push

## 📈 Deployment History

View all deployments:
```bash
# Via CLI
gh run list --workflow="Deploy to GitHub Pages"

# Via GitHub UI
https://github.com/bowen31337/hyperliquid-openapi/actions/workflows/deploy.yml
```

## 🔐 Security

### Permissions
- Workflow has minimal required permissions
- Read access to repository contents
- Write access to GitHub Pages only
- No access to secrets or sensitive data

### Validation
- All specs validated before deployment
- Build fails if validation errors found
- Prevents deploying broken documentation

## 📝 Best Practices

### Before Pushing
1. ✅ Validate locally: `npm run validate`
2. ✅ Test locally: `npm start`
3. ✅ Check all links work
4. ✅ Review changes in browser

### Commit Messages
Use clear, descriptive commit messages:
- ✅ "Update OpenAPI spec: Add new endpoint"
- ✅ "Fix: Correct schema definition for User"
- ✅ "Docs: Update README with new examples"

### Testing
1. Test locally before pushing
2. Check workflow logs after push
3. Verify live site after deployment
4. Test all navigation links

## 🎯 Next Steps

### Enhancements
- [ ] Add custom domain
- [ ] Enable GitHub Discussions
- [ ] Add issue templates
- [ ] Create contributing guidelines
- [ ] Add changelog automation

### Monitoring
- [ ] Set up status badges
- [ ] Add analytics
- [ ] Monitor deployment times
- [ ] Track validation failures

## 📊 Workflow Status

Current status: ✅ **Active and Running**

View live status: https://github.com/bowen31337/hyperliquid-openapi/actions

## 🎉 Success!

Your documentation is now:
- ✅ Automatically deployed on every push
- ✅ Validated before deployment
- ✅ Available at multiple URLs
- ✅ Easy to update and maintain
- ✅ Professional and production-ready

---

**Last Updated**: February 2, 2026
**Workflow Version**: 1.0.0
**Status**: ✅ Active
