---
title: "Neon Twin Deploy Workshop Setup Instructions (Co-hosted)" # MODIFY THIS TITLE
chapter: true
weight: 1 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->

# Setting up your environment for a Neon Twin Deploy Workshop <!-- MODIFY THIS HEADING -->

If you're already running RDS in production, migrating a live database can be a real challenge. The good news is, you don’t have to move everything over to enjoy Neon's advantages in development speed and cost efficiency for non-production databases.

With a Neon Twin—a synchronized copy of your RDS data in Neon—there’s a simple, automated way to keep your development environment up-to-date. This copy updates automatically each night using ```pg_dump/restore ``` and ```GitHub Actions```, so your development data stays fresh without any heavy lifting.

## Submodule One Heading <!-- MODIFY THIS SUBHEADING -->


