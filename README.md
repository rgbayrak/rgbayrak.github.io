# roza gunes bayrak

## Setup

To get started with development, install dependencies:

```bash
npm install
```

### Docker Development

You can run the project using Docker for a consistent development environment.

**Using the standard Docker setup:**

To start this:

  1. Start Docker Desktop - Open Docker Desktop application on your Mac
  2. Wait for Docker Desktop to fully start (you'll see the Docker icon in your menu bar change from animated to static)
  3. Then run the below command.

```bash
docker compose up
```

This will build the image from the Dockerfile and start the Jekyll server.

**Accessing the site:**
- The site will be available at `http://localhost:8080`
- Live reload is enabled on port 35729

**Stopping the container:**
```bash
docker compose down
```

## Maintenance

### Code Formatting

This project uses [Prettier](https://prettier.io/) to maintain consistent code formatting across YAML, Markdown, and other files.

**Check formatting:**
```bash
npx prettier . --check
```

**Fix formatting issues:**
```bash
npx prettier . --write
```

The Prettier configuration is defined in `.prettierrc` and uses the `@shopify/prettier-plugin-liquid` plugin for Liquid template files.

### Dependencies

Keep dependencies up to date by running:

```bash
npm install
```

This will install packages defined in `package.json`.
