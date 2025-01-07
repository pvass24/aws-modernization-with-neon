---
title: "Neon Twin Deploy Workshop Setup Instructions (Co-hosted)" # MODIFY THIS TITLE
chapter: true
weight: 21 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->

# Setting up your environment for a Neon Twin Deploy Workshop

If you're already running RDS in production, migrating a live database can be a challenge. The good news is, you don’t have to move everything over to enjoy Neon's advantages in development speed and cost efficiency for non-production databases.

![Neon Twin](/images/neontwin.jpg)

With a Neon Twin—a synchronized copy of your RDS data in Neon—there’s a simple, automated way to keep your development environment up-to-date. This copy updates automatically each night using ```pg_dump/restore ``` and ```GitHub Actions```, so your development data stays fresh without any heavy lifting.

## Create a Neon Twin in your GitHub Actions workflow

### 1. Create a workflow YAML file

Add a new workflow file in your GitHub repository at ```.github/workflows/create-neon-twin.yml.``` This file will define how the synchronization happens. 


```yaml
name: Create Neon Twin

on:
  schedule:
    - cron: '0 0 * * *' # Runs every day at midnight
  workflow_dispatch:  # Allows manual trigger

env:
  PROD_DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}  # RDS connection string
  DEV_DATABASE_URL: ${{ secrets.DEV_DATABASE_URL }}  # Neon connection string
  PG_VERSION: '16'

jobs:
  dump-and-restore:
    runs-on: ubuntu-latest

    steps:
      - name: Install PostgreSQL
        run: |
          sudo apt update
          yes '' | sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
          sudo apt install -y postgresql-${{ env.PG_VERSION }}

      - name: Dump from RDS and Restore to Neon
        run: |
          /usr/lib/postgresql/${{ env.PG_VERSION }}/bin/pg_dump "${{ env.PROD_DATABASE_URL }}" -Fc -f "${{ github.workspace }}/prod-dump-file.dump"
          /usr/lib/postgresql/${{ env.PG_VERSION }}/bin/pg_restore -d "${{ env.DEV_DATABASE_URL }}" --clean --no-owner --no-acl --if-exists "${{ github.workspace }}/prod-dump-file.dump"

```

### 2. Understanding the Workflow Steps

#### Workflow Components

| Component | Description |
|-----------|-------------|
| **Cron** | Runs nightly at midnight (Eastern Time) using POSIX cron syntax |
| **workflow_dispatch** | Enables manual workflow triggering during development |

#### Environment Variables

| Variable | Description |
|----------|-------------|
| **PROD_DATABASE_URL** | Connection string for production database (stored in GitHub Secrets) |
| **DEV_DATABASE_URL** | Connection string for Neon Twin (stored in GitHub Secrets) |
| **PG_VERSION** | Specifies the Postgres version to install |

#### Database Operations

| Command | Flag | Description |
|---------|------|-------------|
| **pg_dump** | `-Fc` | Uses custom format for dump file |
| | `-f` | Specifies dump file path and name |
| **pg_restore** | `--clean` | Removes existing objects before restore |
| | `--if-exists` | Prevents errors for non-existent objects |
| | `--no-owner` | Skips original ownership restoration |
| | `--no-acl` | Skips original permissions restoration |
| | `-d` | Specifies target database using DEV_DATABASE_URL |

> **Note**: Environment variables mentioned above must be configured in your GitHub repository's secrets for the Action to work properly.

### 2. Configuring secrets in GitHub Actions

In the workshop prerequisites module you will have created a GitHub repository and added secrets to the repository.

- In your GitHub Actions workflow (e.g., in `.github/workflows/create-neon-twin.yml`), reference these secrets like this:
    
    ```yaml
    yaml
    Copy code
    env:
      PROD_DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
      DEV_DATABASE_URL: ${{ secrets.DEV_DATABASE_URL }}
    
    ```
This syntax ensures that the sensitive values, such as database credentials or API keys, are securely injected into your workflow at runtime without hardcoding them into the file.