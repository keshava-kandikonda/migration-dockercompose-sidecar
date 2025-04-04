# Docker-Compose to Sidecar Automation Script

## Overview

This script automates the migration of Docker-Compose applications to Azure App Service with Sidecar containers. Key features include:

- Converts Docker-Compose configurations into a format compatible with Azure App Service.
- Deploys the application to the "staging" slot for testing before production deployment.
- Supports user credential authentication for deployment.
- Ignores unsupported Docker-Compose fields such as `build`, `depends_on`, `networks`, and `secrets`.
- Allows specifying the target port for the application.
- Provides error handling for unsupported persistent storage using `${WEBAPP_STORAGE_HOME}`.
- Requires manual verification of the deployed application before slot swapping.

## Command to run the script –

```powershell
./update-sidecars.ps1 -subscriptionId `<subscriptionId>` -webAppName `<webAppName>` -resourceGroup `<resourceGroup>` -registryUrl `<registryUrl>` -base64DockerCompose `<base64DockerCompose>` -mainServiceName `<mainServiceName>` -targetPort `<targetPort>`
```

## Important Notes

### Backup

Ensure you take a backup of the site on the Azure Portal before starting this exercise. This will provide a healthy backup version in case of any issues.

### Unsupported Fields

The following fields are not supported and will be ignored if present in the configuration:

- build (not allowed)
- depends_on (ignored)
- networks (ignored)
- secrets (ignored)
- ports other than 80 and 8080 (ignored)
- Persistent storage using ${WEBAPP_STORAGE_HOME}
  
### Authentication

Currently, only user credential authentication is supported. There are some issues with slot swap for system and user-assigned identities, as these do not automatically carry over. We have added code for this, but it remains untested.

### Volumes Issue

There is an issue with volumes, especially with persistent storage using ${WEBAPP_STORAGE_HOME}. An error occurs when creating a compose app with a bind mount using this variable, so it is not supported.

### Ports

The port is taken as an input parameter directly. Customers need to specify the port on which they expect to receive the topic.

### Staging Slot

The script will create the app in the "staging" slot. Customers need to manually verify the site and ensure it is running correctly before performing the slot swap themselves.
