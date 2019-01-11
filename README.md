# Basic CRUD App with Angular 7.0 and Spring Boot 2.1, with Docker!

This is a modified version of [an existing Okta sample app](https://github.com/oktadeveloper/okta-spring-boot-2-angular-7-example). ([See article for details](https://developer.okta.com/blog/2018/08/22/basic-crud-angular-7-and-spring-boot-2))
 
Modifications have been made to further the following goals:

- Add Docker and Docker Compose support
- Simplify configuration by extracting all references to hostnames/ports to configuration files

**Prerequisites:** [Docker](https://www.docker.com/get-started)

## Getting Started

To install this example application, run the following commands:

```bash
git clone https://github.com/MikePulcini/okta-spring-boot-2-angular-7-example.git
cd okta-spring-boot-2-angular-7-example
```

This will get a copy of the project installed locally. 

To build the docker images and start the application, run the following command:

```
docker-compose up --build
```

### Create a New OIDC App in Okta

To create a new OIDC app on Okta:

1. Log in to your developer account, navigate to **Applications**, and click on **Add Application**.
2. Select **Single-Page App** and click **Next**. 
3. Give the application a name, change all instances of `localhost:8080` to `{your_desired_hostname}:{your_desired_webapp_port}` (e.g. `localhost:4200`) and click **Done**.

### Server Configuration

Set the following cconfiguration values in `server/src/main/resources/application.yml`. 

**NOTE:** The value of `{yourOktaDomain}` should be something like `dev-123456.oktapreview`. Make sure you don't include `-admin` in the value!

```yaml
okta:
  oauth2:
    client-id: {clientId}
    issuer: https://{yourOktaDomain}/oauth2/default
    origin: http://{your_desired_hostname}:{your_desired_webapp_port} # ex: http://localhost:4200
```

### Client Configuration

For the client, update `client/src/environments/environment.prod.ts` with the appropriate settings as noted below. 

```typescript
export const environment = {
  production: true,
  hostname: '{your_desired_hostname}', 
  apihostname: '{your_desired_hostname}',
  apiport: 8080, // The default port for the API; leave as-is
  issuer: 'https://{yourOktaDomain}.oktapreview.com/oauth2/default',
  redirectUri: 'http://{your_desired_hostname}:{your_desired_webapp_port}/implicit/callback', // ex: http://localhost:4200/implicit/callback
  clientId: '{clientId}'
};
```

### Docker Compose Configuration

`docker-compose.yml` only needs to be updated if you want to change the default ports of the client and server (which default to 4200 and 8080, respectively).

To update the ports in `docker-compose.yml`, locate the client or server section of the file and update the `ports` section as follows. (Note that the port number after the colon should stay as the default port.)

```yml
version: '3'
services:
  oidc-web:
    build:
      context: ./client
    image: oidc-web
    ports: 
      - {your_desired_port}:4200
    networks:
      - network1
  oidc-api:
    build: 
      context: ./server
    image: oidc-api
    ports:
      - {your_desired_port}:8080
    networks:
      - network1

networks:
  network1:
```

## License

Apache 2.0, see [LICENSE](LICENSE).
