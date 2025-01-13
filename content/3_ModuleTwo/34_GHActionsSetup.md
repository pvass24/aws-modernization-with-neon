---
title: "Creating the GitHub Actions Workflow"
chapter: true
weight: 34
---

# Creating the GitHub Actions Workflow

In this section, we'll create the GitHub Actions workflow that syncs your RDS database to Neon.

## 🚀 Creating the Workflow File

1. In your repository, navigate to:
   - Click on "Actions"
   - Click "New workflow"
   - Choose "set up a workflow yourself"

![Create Workflow](/images/create-workflow.png)

2. Name your file `sync.yml` and paste the following content:

```yaml
name: Create Neon Twin
on:
  schedule:
    - cron: '0 0 * * *' # Runs every day at midnight
  workflow_dispatch: # Allows manual trigger
env:
  PROD_DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
  DEV_DATABASE_URL: ${{ secrets.DEV_DATABASE_URL }}
  PG_VERSION: '16'
jobs:
  dump-and-restore:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v3
      - name: Install PostgreSQL Client
        run: |
          sudo apt update
          yes '' | sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
          sudo apt install -y postgresql-client-${{ env.PG_VERSION }}
      - name: Verify PostgreSQL Client Installation
        run: |
          pg_dump --version
          pg_restore --version
      - name: Dump from RDS
        env:
          PROD_DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
        run: |
          echo "Dumping data from RDS to file..."
          /usr/lib/postgresql/${{ env.PG_VERSION }}/bin/pg_dump "${PROD_DATABASE_URL}" -Fc -f "${{ github.workspace }}/prod-dump-file.dump"
      - name: Restore to Neon
        env:
          DEV_DATABASE_URL: ${{ secrets.DEV_DATABASE_URL }}
        run: |
          echo "Restoring data to Neon..."
          /usr/lib/postgresql/${{ env.PG_VERSION }}/bin/pg_restore -d "${DEV_DATABASE_URL}" --clean --no-owner --no-acl --if-exists "${{ github.workspace }}/prod-dump-file.dump"
      - name: Cleanup
        run: |
          echo "Cleaning up dump file..."
          rm -f "${{ github.workspace }}/prod-dump-file.dump"
```

![Edit Workflow](/images/edit-workflow.png)

3. Click "Commit changes" to save the workflow

## ✅ Initial Sync

The workflow will automatically start running once you commit the file:
1. Watch the progress in the "Actions" tab
2. The initial sync will create your Neon twin database
3. Future syncs will run daily at midnight

![Initial Run](/images/initial-run.png)

## 🚀 Manual Triggers

You can also trigger the workflow manually:
1. Go to the "Actions" tab
2. Click on "Create Neon Twin"
3. Click "Run workflow"

![Manual Run](/images/manual-run.png)

## 🎯 Next Steps

Your automated database sync is now set up and running! Let's move on to testing your Neon twin database.