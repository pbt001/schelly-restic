# restic-api
This exposes the common functions of Restic with REST APIs. Originally created to be used with Schelly.

# Usage

docker-compose .yml

```
version: '3.5'

services:

  restic-api:
    image: flaviostutz/restic-api
    ports:
      - 8080:8080
    environment:
      - RESTIC_PASSWORD=123
      - LOG_LEVEL=debug
```

# REST Endpoints

  - ```POST /backups```
    - When invoked, triggers a new backup
    - Response body: json 
     
      ```
        {
           id:{alphanumeric-backup-id},
           status:{backup-status}
           message:{backy2-message}
        }
      ```
      - status must be one of:
          - 'pending' - backup is not finished yet
          - 'canceled' - backup was canceled
          - 'success' - backup has completed successfuly
          - 'error' - there was an error on backup
      
    - Status code 201 if created successfuly

  - ```GET /backups/{backup-id}```
    - Response body: json
    
       ```
         {
           id:{id},
           status:{backup-status},
           message:{backend message}
         }
       ```
    - Status code: 200 if found, 404 if not found

  - ```DELETE /backups/{backup-id}```
    - Response body: json 
     
      ```
        {
           id:{alphanumeric-backup-id},
           status:{backup-status}
           message:{backend-message}
        }
      ```
      
    - Status code 200 if deleted successfuly
