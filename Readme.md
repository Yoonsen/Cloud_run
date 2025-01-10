# Cloud Run Docker Deployment Tool

A Python-based deployment tool for building Docker images locally and deploying them to Google Cloud Run. This tool leverages local Docker layer caching for faster builds while managing deployments through Google Cloud Build.

## Prerequisites

- Python 3.9+
- Docker installed and configured
- Google Cloud SDK installed
- Authenticated Google Cloud account
- Access to Google Cloud Registry (GCR)

## Setup

1. Clone the repository:
```bash
git clone <your-repository-url>
cd <repository-directory>
```

2. Authenticate Docker with Google Cloud Registry:
```bash
gcloud auth configure-docker
```

3. Ensure you're logged into the correct Google Cloud project:
```bash
gcloud config set project YOUR_PROJECT_ID
```

## Project Structure

```
.
├── deploy.py            # Deployment script
├── cloudbuild.deploy.yaml   # Cloud Run deployment configuration
├── Dockerfile           # Docker image configuration
├── requirements.txt     # Python dependencies for your app
└── app/                # Your application code
    └── app.py          # Main application file
```

## Usage

The deployment script supports several options for building and deploying your application:

### Basic Build and Deploy

```bash
python deploy.py \
    --app-path ./your-app-directory \
    --project-id your-project-id \
    --app-name your-app-name
```

### Build with Docker Cleanup

```bash
python deploy.py \
    --app-path ./your-app-directory \
    --project-id your-project-id \
    --app-name your-app-name \
    --cleanup
```

### Force Rebuild

```bash
python deploy.py \
    --app-path ./your-app-directory \
    --project-id your-project-id \
    --app-name your-app-name \
    --rebuild
```

### Full Build with Cleanup

```bash
python deploy.py \
    --app-path ./your-app-directory \
    --project-id your-project-id \
    --app-name your-app-name \
    --rebuild \
    --cleanup
```

## Command Line Arguments

- `--app-path`: Path to your application directory (required)
- `--project-id`: Google Cloud project ID (required)
- `--app-name`: Name of your application (required)
- `--rebuild`: Force a complete rebuild of the Docker image
- `--cleanup`: Clean up Docker resources after deployment

## Cleanup Features

When using the `--cleanup` flag, the script will:
- Remove stopped containers
- Remove unused images
- Clean Docker build cache
- Display current Docker disk usage

## Cloud Run Configuration

The deployment uses the following Cloud Run configuration (defined in `cloudbuild.deploy.yaml`):
- Region: europe-north1
- CPU: 2000m
- Memory: 2Gi
- Port: 8050
- Unauthenticated access enabled
- Custom root path configured

## Development

### Local Testing

Before deploying, you can build and test your Docker image locally:

```bash
# Build the image
docker build -t gcr.io/your-project-id/your-app-name:latest .

# Run the container locally
docker run -p 8050:8050 gcr.io/your-project-id/your-app-name:latest
```

### Customizing the Deployment

1. Modify `cloudbuild.deploy.yaml` to adjust Cloud Run settings
2. Update the Dockerfile to change the build process
3. Edit `deploy.py` to add additional deployment features

## Troubleshooting

If you encounter issues:

1. Ensure you're authenticated with Google Cloud:
```bash
gcloud auth login
```

2. Verify Docker is running:
```bash
docker info
```

3. Check Google Cloud project configuration:
```bash
gcloud config list
```

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License

[CC-by]
