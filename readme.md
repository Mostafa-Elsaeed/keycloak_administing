# Keycloak Administration with Docker Compose

## Overview

This project uses Docker Compose to deploy a Keycloak instance integrated with PostgreSQL for user management. It features two applications: one for administrative purposes (creating users) and another for user authentication.

## Prerequisites

- Docker
- Docker Compose

## Deployment

To deploy the Keycloak server along with PostgreSQL, use the following command:

```bash
docker-compose up -d
```

This command will start all necessary containers defined in the `docker-compose.yml` file.

## Architecture

The setup includes:

- **Keycloak:** Identity and access management.
- **PostgreSQL:** A relational database to store user data.
- **Admin Application:** A client app for user management (create users, view roles, etc.).
- **User Application:** A client app for user login and authentication.

<!-- ### Architecture Diagram -->

<!-- ![Architecture Diagram](path/to/architecture_diagram.png) -->

## Docker Compose Configuration

The `docker-compose.yml` file handles the setup of the Keycloak and PostgreSQL services. Here's a snippet of the file showing the main services:

```yaml
version: "3.9"

services:
  keycloak:
    container_name: keycloak
    image: quay.io/keycloak/keycloak:26.3.3
    command: start-dev
    ports:
      - "8099:8080"
    environment:
      # Keycloak admin account
      KC_BOOTSTRAP_ADMIN_USERNAME: $username
      KC_BOOTSTRAP_ADMIN_PASSWORD: $password

      # Database settings
      KC_DB: postgres
      KC_DB_URL_HOST: postgres # container_name of postgres service
      KC_DB_URL_PORT: 5432
      KC_DB_URL_DATABASE: keycloak
      KC_DB_USERNAME: $username
      KC_DB_PASSWORD: $password

    volumes:
      - /mnt/HDD/AppsData/keycloak:/opt/keycloak/data
    networks:
      - main_network

networks:
  main_network:
    external: true
```

## keycloak Deployed on my own server

- ![keycloak Dashboard](/imgs/1.png)

- ![keycloak clients i have created](/imgs/2.png)

## API Endpoints

You can test the following endpoints:

1. **Admin Login:**

   - **Endpoint:** `/auth/realms/master/protocol/openid-connect/token`
   - **Method:** POST
   - **Description:** Obtain an access token for the admin client, required for user management.
   - ![Admin Login Request](path/to/admin_login_request.png)

2. **Create User:**

   - **Endpoint:** `/auth/admin/realms/{realm}/users`
   - **Method:** POST
   - **Description:** Use the generated token from the admin login to create users.
   - ![Create User Request](path/to/create_user_request.png)

3. **User Login:**
   - **Endpoint:** `/auth/realms/{realm}/protocol/openid-connect/token`
   - **Method:** POST
   - **Description:** Authenticate the users and obtain their access token.
   - ![User Login Request](path/to/user_login_request.png)

## Usage

Navigate to your application's frontend to start the interaction with Keycloak. Use the admin application to manage users and the user application for user login.

## Conclusion

This setup provides a powerful way to manage users and enhance the security of your applications using Keycloak's robust identity and access management capabilities.

## License

[Your License Here]
