# Docker-Compose to Sidecar Automation Script

## Command to run the script –
```
./update-sidecars.ps1 -subscriptionId `<subscriptionId>` -webAppName `<webAppName>` -resourceGroup `<resourceGroup>` -registryUrl `<registryUrl>` -base64DockerCompose `<base64DockerCompose>` -mainServiceName `<mainServiceName>` -targetPort `<targetPort>`
```

## Important Notes
### Backup: 
Ensure you take a backup of the site on the Azure Portal before starting this exercise. This will provide a healthy backup version in case of any issues.

### Unsupported Fields: 
The following fields are not supported and will be ignored if present in the configuration:
- build (not allowed)
- depends_on (ignored)
- networks (ignored)
- secrets (ignored)
- ports other than 80 and 8080 (ignored)
- Persistent storage using ${WEBAPP_STORAGE_HOME}
  
### Authentication: 
Currently, only user credential authentication is supported. There are some issues with slot swap for system and user-assigned identities, as these do not automatically carry over. We have added code for this, but it remains untested.

### Volumes Issue: 
There is an issue with volumes, especially with persistent storage using ${WEBAPP_STORAGE_HOME}. An error occurs when creating a compose app with a bind mount using this variable, so it is not supported.

### Ports: 
The port is taken as an input parameter directly. Customers need to specify the port on which they expect to receive the topic.

### Staging Slot: 
The script will create the app in the "staging" slot. Customers need to manually verify the site and ensure it is running correctly before performing the slot swap themselves.
