---
title: "Co-hosted Workshop Prerequisites" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# Co-hosted Workshop Prerequisites <!-- MODIFY THIS HEADING IF APPLICABLE -->

## AWS Prerequisites <!-- MODIFY THIS SUBHEADING -->
- ### AWS account (Provided by AWS for this workshop if you do not have a staging RDS database)
[AWS DIRECTIONS HERE - Patrick/Kruthi]  
- ### RDS connection string (Provided by AWS for this workshop)
[AWS DIRECTIONS HERE - Patrick/Kruthi]
- ### AWS deployment region (us-west-2)
[AWS DIRECTIONS HERE - Patrick/Kruthi]


## Neon Prerequisites <!-- MODIFY THIS SUBHEADING -->

### 1. A Neon account
**Directions for creating a Neon account** <br>

If you’re new to Neon, the first step is to [sign up](https://console.neon.tech/signup) and create your initial project in Neon using the steps detailed in the section below.


### 2. A Neon database 

**Creating a Neon database** <br>

{{% notice warning %}}
**Make sure your Neon database is deployed to the same AWS region as your RDS database (us-west-2).**
{{% /notice %}}

After you have signed up for a Neon account, follow our [getting started guide](https://neon.tech/docs/get-started-with-neon/signing-up) to create your initial project, you will be introduced to these key Neon terms:

![Neon Object Hierarchy](/images/Neondatabasedr.png)


- **Project:** The top-level container for your Neon databases—the logical equivalent to an “instance” in RDS.
- **Branch:** A versioned copy of your database environment. Each Neon project can have multiple branches, as we will see later.
- **Database:** The actual database instance where your data resides. In Neon, databases live inside branches.


Neon is built on an innovative [branch-based architecture](https://neon.tech/blog/architecture-decisions-in-neon):

- The **main branch** will be your **primary development branch**, where you’ll load the data from your RDS database every night.
- Once everything is set up, you’ll be able to create additional branches from the main branch to [duplicate your development environment in a second](https://neon.tech/blog/how-to-copy-large-postgres-databases-in-seconds), without additional storage costs, as many times as you need.
- Every engineer on your team can have their own dev branch, facilitating parallel development without adding overheads in database management or costs.

<br>    
🚨 Once you’ve set up your Neon account and your project, **create a database in the main branch, and make a note of the connection string.** You will need this in the next step.
<br>

#### Select Your Project -> Connection Details -> Connection String

- On the dashboard, you will see a list of your projects.
- Click on the project for which you want to find the connection string. 
- In the left-hand menu, locate and click on "Connection Details" or "Connect" (this may vary slightly depending on the Neon Console's version).
- In the Connection Details section, you will see the connection string displayed. Your connection string will be customized with your Neon credentials. Copy the connections string based on the frameworks you are using.

-  View or Generate a Password 
<br> -  If you need a password, look for a button to "Reveal Password" or "Reset Password" in the Connection Details section. Follow the prompts to view or reset it as needed.


### 3. GitHub repository access to Actions and Secrets 
- **Directions for preparing the GitHub Repository** <br>
    - **Create a GitHub repository** if you don't have one already, as this will store the automation scripts and workflows for the synchronization. <br>

    [GitHub Directions For Creating a Repository Here]

    - **Add necessary secrets** to GitHub Secrets: You’ll need the connection URLs for both your RDS production database and your Neon database (these can be added to GitHub Secrets under `PROD_DATABASE_URL` and `DEV_DATABASE_URL`).

    [GitHub Directions For Adding Secrets]