# Run Locally

This guide will walk you through setting up and running the portfolio site from scratch.

## Prerequisites

Ensure you have the following installed on your system:
- **Git** – for version control
- **Go** – for building Hugo (required for extended Hugo flavor)
- **Node.js & npm** – for managing JavaScript dependencies

## Step 1: Clone the Repository

```bash
cd ~/code
git clone https://github.com/prantoran/prantoran.github.io.git
cd prantoran.github.io
```

## Step 2: Install Hugo (Extended)

Install Hugo v0.120.1 with the extended flavor (required for SCSS support):

```bash
go install -tags extended github.com/gohugoio/hugo@v0.120.1
```

Verify the installation:

```bash
$(go env GOPATH)/bin/hugo version
```

## Step 3: Install Dependencies

Update Hugo module dependencies and install npm packages:

```bash
export PATH="$(go env GOPATH)/bin:$PATH"
hugo mod tidy
hugo mod npm pack
npm install
```

This will:
- Resolve and download Hugo theme dependencies
- Generate npm package requirements from the theme
- Install all npm packages

## Step 4: Start the Development Server

Start the Hugo development server:

```bash
export PATH="$(go env GOPATH)/bin:$PATH"
hugo server -D --bind 0.0.0.0 --port 1313
```

The site will be available at:
- **Local**: http://localhost:1313
- **Network**: http://<your-ip>:1313

The `-D` flag includes draft content in the build. The server will automatically rebuild and hot-reload when you make changes.

To stop the server, press `Ctrl+C` in the terminal.

## Troubleshooting

### Port Already in Use

If port 1313 is already in use, kill the process:

```bash
lsof -i :1313 -sTCP:LISTEN -n -P | awk 'NR==2 {print $2}' | xargs kill
```

Then restart the server.

### Hugo Module Issues

If you encounter module dependency issues, try:

```bash
hugo mod get -u
hugo mod tidy
```

### npm Package Issues

Clear npm cache and reinstall:

```bash
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

### assets/jsconfig.json

For local dev in Mac, had to update `CompilerOptions.paths.*`:
```json
{
 "compilerOptions": {
  "baseUrl": ".",
  "paths": {
   "*": [
    "../../../Library/Caches/hugo_cache/modules/filecache/modules/pkg/mod/github.com/hugo-toha/toha/v4@v4.4.0/assets/*"
   ]
  }
 }
}
```
