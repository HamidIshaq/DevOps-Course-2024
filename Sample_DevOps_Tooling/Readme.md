# CI/CD Pipeline with GitHub Actions: Build and Push Docker Image

In this guide, we'll set up a CI/CD pipeline using GitHub Actions to build a Docker image from your repository and push it to a container registry (e.g., Docker Hub).

---

## Prerequisites

Before proceeding, ensure you have the following:

- A **GitHub repository** containing your application code and `Dockerfile`.
- A **Docker Hub account** (or another container registry) with an access token.
- Basic knowledge of GitHub Actions and Docker.

---

## Step 1: Create a Dockerfile

Ensure your project includes a `Dockerfile`. Below is a sample for a Python application:

```dockerfile
# Sample Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

Replace `requirements.txt` and `app.py` with your application dependencies and entry point.

---

## Step 2: Set Up GitHub Secrets

1. Navigate to your repository on GitHub.
2. Go to **Settings** > **Secrets and variables** > **Actions**.
3. Add the following secrets:
   - `DOCKER_USERNAME`: Your Docker Hub username.
   - `DOCKER_PASSWORD`: Your Docker Hub access token.

---

## Step 3: Create GitHub Actions Workflow

In your repository, create a folder `.github/workflows` and add a file named `docker-publish.yml` with the following content:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # Checkout code
      - name: Checkout repository
        uses: actions/checkout@v3

      # Log in to Docker Hub
      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Build Docker image
      - name: Build Docker image
        run: |
          docker build -t ${{ secrets.DOCKER_USERNAME }}/my-app:latest .

      # Push Docker image
      - name: Push Docker image
        run: |
          docker push ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

Replace `my-app` with the name of your Docker repository.

---

## Step 4: Push to Trigger Workflow

1. Commit and push your code to the `main` branch:

   ```bash
   git add .
   git commit -m "Add CI/CD pipeline with GitHub Actions"
   git push origin main
   ```

2. This will trigger the GitHub Actions workflow. You can monitor the progress in the **Actions** tab of your repository.

---

## Step 5: Verify the Docker Image

1. Log in to your Docker Hub account.
2. Confirm that the Docker image (`my-app:latest`) has been uploaded.

---

## Example Workflow Outputs

### Workflow File
![GitHub Actions Workflow File](path/to/your-workflow-file-screenshot.png)

### Workflow Execution Logs
![GitHub Actions Logs](path/to/your-logs-screenshot.png)

### Docker Hub Image
![Docker Hub Image](path/to/your-docker-hub-screenshot.png)

---

## Conclusion

With this pipeline, your Docker image is automatically built and pushed to Docker Hub every time you push changes to the `main` branch. This automation ensures consistency and saves time in deployment processes.

For further improvements, consider:

- Adding tags to Docker images based on GitHub release versions.
- Integrating with Kubernetes or cloud services for deployment.

---
