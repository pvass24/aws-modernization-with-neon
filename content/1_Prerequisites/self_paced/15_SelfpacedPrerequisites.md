---
title: "Self-Paced Workshop Prerequisites"
chapter: true
weight: 15
---
# Self-Paced Workshop Prerequisites

## AWS Prerequisites
- ### AWS account
- Must have an AWS Account [Click Here to Register for an Account](https://aws.amazon.com/free/?gclid=CjwKCAiApY-7BhBjEiwAQMrrEQvPVHrROjm_VPCmPQKxuQ5MDb45z8R8_aYf9qnh9YTa2K88EwxLoRoCZcoQAvD_BwE&trk=78b916d7-7c94-4cab-98d9-0ce5e648dd5f≻_channel=ps&ef_id=CjwKCAiApY-7BhBjEiwAQMrrEQvPVHrROjm_VPCmPQKxuQ5MDb45z8R8_aYf9qnh9YTa2K88EwxLoRoCZcoQAvD_BwE:G:s&s_kwcid=AL!4422!3!432339156165!e!!g!!aws%20account!9572385111!102212379047&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)

- Github Account [Click Here to Register for an Account](https://github.com/join)

- ### RDS connection string (Provided by AWS for this workshop)
[AWS DIRECTIONS ON HOW TO FIND A CONNECTION STRING FOR THE SELF-PACED USER STAGING DATABASE - Patrick/Kruthi]
- ### AWS deployment region (us-west-2)
[AWS DIRECTIONS ON HOW TO FIND THE REGION FOR THE SELF-PACED USER STAGING RDS DATABASE - Patrick/Kruthi]

## Neon Prerequisites <!-- MODIFY THIS SUBHEADING -->

### 1. A Neon account
**Directions for creating a Neon account** 

If you’re new to Neon, the first step is to [sign up](https://console.neon.tech/signup) and create your initial project in Neon using the steps detailed in the section below.

### 2. A Neon database 

**Creating a Neon database** 

**Make sure your Neon database is deployed to the same AWS region as your RDS database (us-west-2).**

After you have signed up for a Neon account, follow our [getting started guide](https://neon.tech/docs/get-started-with-neon/signing-up) to create your initial project

![Neon Project Creation](/images/neon-project-creation.png)

You will be introduced to these key Neon terms:

![Neon Object Hierarchy](/images/Neondatabasedr.png)

- **Project:** The top-level container for your Neon databases—the logical equivalent to an “instance” in RDS.
- **Branch:** A versioned copy of your database environment. Each Neon project can have multiple branches, as we will see later.
- **Database:** The actual database instance where your data resides. In Neon, databases live inside branches.

Neon is built on an innovative [branch-based architecture](https://neon.tech/blog/architecture-decisions-in-neon):

- The **main branch** will be your **primary development branch**, where you’ll load the data from your RDS database every night.
- Once everything is set up, you’ll be able to create additional branches from the main branch to [duplicate your development environment in a second](https://neon.tech/blog/how-to-copy-large-postgres-databases-in-seconds), without additional storage costs, as many times as you need.
- Every engineer on your team can have their own dev branch, facilitating parallel development without adding overheads in database management or costs.

🚨 Once you’ve set up your Neon account and your project, **create a database in the main branch, and make a note of the connection string.** You will need this in the next step.

#### Select Your Project -> Connection Details -> Connection String

- On the dashboard, you will see a list of your projects.
- Click on the project for which you want to find the connection string. 
- In the left-hand menu, locate and click on "Connection Details" or "Connect" (this may vary slightly depending on the Neon Console's version).

![Neon Connection Details](/images/neon-connection-details.png)

- In the Connection Details section, you will see the connection string displayed. Your connection string will be customized with your Neon credentials. Copy the connections string based on the frameworks you are using.
-  View or Generate a Password 
-  If you need a password, look for a button to "Reveal Password" or "Reset Password" in the Connection Details section. Follow the prompts to view or reset it as needed.

### 3. Setting up GitHub Actions
- First you will need to **make sure you have a GitHub Account**, if you do not have one already, you can sign up for one [here.](https://github.com/signup)
- **Create a GitHub repository** if you don't have one already, you can create on by following the instructions [here.](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository). This is needed to store the automation scripts and workflows for the synchronization. 
- **Accessing GitHub Actions**
    - **Navigate to your repository**: Go to the GitHub repository where you want to enable or configure Actions.
    - **Go to the Actions tab**: On the repository’s main page, click on the “Actions” tab in the top menu. This is where you can view and manage existing workflows, or set up new ones.
    - **Create or Edit a Workflow**: If you want to create a new workflow, click "New workflow" or "Set up a workflow yourself". If you're editing an existing one, select the workflow you want to modify. You will be editing or creating .yml files under .github/workflows in your repository.
- **Accessing GitHub Secrets**
    - **Go to the repository settings**: From your repository page, click on the "Settings" tab (next to the "Insights" tab).
    - **Navigate to Secrets**: In the left sidebar, scroll down and click on "Secrets and variables" and then "Actions". This will bring you to a section where you can add and manage secrets for use in GitHub Actions.
    - **Add Secrets**: To add a new secret, click on the "New repository secret" button. Provide a name (e.g., PROD_DATABASE_URL) and the corresponding secret value (e.g., your RDS connection string). You can add as many secrets as needed for your workflows, ensuring they are securely stored and can be used in your GitHub Actions.
- **Using Secrets in GitHub Actions**
In your GitHub Actions workflow (e.g., in .github/workflows/create-neon-twin.yml), reference these secrets like this:
    ```yaml
    env:
    PROD_DATABASE_URL: ${{ secrets.PROD_DATABASE_URL }}
    DEV_DATABASE_URL: ${{ secrets.DEV_DATABASE_URL }}  
    ```
    This syntax ensures that the sensitive values, such as database credentials or API keys, are securely injected into your workflow at runtime without hardcoding them into the file.
- **Permissions and Security**
    - **Repository Access**: Ensure that the account or team running the workflow has the appropriate permissions to access the repository and secrets.
    - **Secrets Access Control**: Only repository collaborators or those with administrative rights typically have permission to add or modify secrets. For enhanced security, avoid exposing sensitive data by logging secrets directly in the Actions output.




