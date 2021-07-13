
# Project: CI/CD with Docker and Github Actions

This is a simple project to create deployment pipeline for an app which deploys on Heroku using docker container and github actions.
'secrets' were configured in 'settings/secrets' in github repository.

Heroku Link: https://ga-pipeline.herokuapp.com/

## Deployment

.github/workflows/build.yml

```bash
name: Release DevOps with Docker

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push
        uses: docker/build-push-action@v2
        with:
          push: true
          tags: faysalmehedi/dwd-coursepage:latest

      - name: Deploy on heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
          usedocker: true
```
