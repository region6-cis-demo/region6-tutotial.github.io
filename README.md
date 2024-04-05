## climate-services-webportal

The Climate Services Web Portal will serve several climate web application services for agriculture accessible for public viewing and usage. The project aims  to empower farmers and decision-makers by giving them reliable crop activity advisories backed by organized crop recommendations research data.

The portal also aims to be a one-stop hub for consolidating and organizing expert-reviewed data that will be consumed by its web application services, and viewed by public viewers.

### ACAP Cloud Components Overview

![acap components](/server/src/scripts/data/components-overview-v2-small.png)

### ACAP Data Sources

![acap datasources](/server/src/scripts/data/diagrams/acap_1.0_arch_datasources.png)

## Requirements

NodeJS LTS v16.14.2

## Content

- [climate-services-webportal](#climate-services-webportal)
  - [ACAP Cloud Components Overview](#acap-cloud-components-overview)
- [Requirements](#requirements)
- [Content](#content)
- [Installation](#installation)
- [Usage](#usage)
  - [Run the Client and Server Apps on Localhost](#run-the-client-and-server-apps-on-localhost)
  - [Run the Client and Server Apps Using Docker](#run-the-client-and-server-apps-using-docker)
- [Code Repository](#code-repository)
- [Backend Server URLs](#backend-server-urls)
- [Client Website URLs](#client-website-urls)
- [Github Actions Environment Variables](#github-actions-environment-variables)
  - [Firebase](#firebase)
  - [GitHub Pages](#github-pages)
  - [Render (render.com)](#render-rendercom)
  - [Heroku](#heroku)
  - [MapBox](#mapbox)
  - [Openweather API](#openweather-api)
  - [Miscellaneous](#miscellaneous)

## Installation

1. Clone this repository.
`git clone https://github.com/ciatph/climate-services-webportal.git`

2. Install the **client** and **server** dependencies.
   - client
      ```
      cd climate-services-webportal/client
      npm install
      ```
   - server
      ```
      cd climate-services-webportal/server
      npm install
      ```

3. Set the **client** and **server** environment variables.
   - client
      - Create a **.env** file inside the `/client` directory
      - Copy the contents of `/client/.env.example` into the `.env` file created from the previous step.
      - Provide the appropriate values for each variable as needed.
   - server
      - Create (1) new file: **.env** inside the `/server` directory
      - Copy the contents of `/server/.env.example` into the new file created from the previous step.
      - Take note to supply the appropriate values for `FIREBASE_SERVICE_ACC` and `FIREBASE_PRIVATE_KEY`.
4. Read the instructions on the **README** files inside the `/server` and `/client` directories for detailed information on configuring and using the client and backend.

## Usage

Run the client and server apps on localhost after following the set-up steps in the [Installation](#installation) section, and the detailed **client** and **server** configuration steps.

### Run the Client and Server Apps on Localhost

1. Run the app for local development.
   - client
      ```
      cd climate-services-webportal/client
      npm run dev
      ```
   - server
      ```
      cd climate-services-webportal/server
      npm run dev
      ```
2. Launch the local website on:
`http://localhost:3000`

### Run the Client and Server Apps Using Docker

We can use Docker to run dockerized client and server apps for local development and production mode. The following methods require Docker and Docker compose correctly installed and set up on your development machine, and set-up of the required `.env` files in the **client** and **server** directories discussed in the Installation](#installation) section.

<b>Docker Dependencies</b>

The following dependencies are used to build and run the image. Please feel feel free to use other OS and versions as needed.

1. Ubuntu 22.04.1
2. Docker version 23.0.3, build 3e7cbfd

<b>Docker for Localhost Development</b>

1. Verify that ports 3000 and 3001 are free because the client and server containers will use these ports.
2. Stop current-running my-phonebook containers, if any.

   ```
   docker compose -f docker-compose.dev.yml down
   docker compose -f docker-compose.prod.yml up
   ```
3. Stop and delete all docker instances for a fresh start.
   - > **NOTE:** Running this script will delete all docker images, containers, volumes, and networks. Run this script if you feel like everything is piling but do not proceed if you have important work on other running Docker containers.
   - ```
     sudo chmod u+x scripts/docker-cleanup.sh
     ./scripts/docker-cleanup.sh
     # Answer all proceeding prompts
     ```
4. Edit any of the files under the `/client` or `/server` directory after running step no. 4.2 and wait for their live reload on `http://localhost:3000` (client) and `http://localhost:3001` (server).

   ```
   # 4.1. Build the client and server containers for localhost development.
   docker compose -f docker-compose.dev.yml build

   # 4.2. Create and start the development client and server containers
   docker compose -f docker-compose.dev.yml up

   # 4.3. Stop and remove the development containers, networks, images and volumes
   docker compose -f docker-compose.dev.yml down
   ```

<b>Docker for Production Deployment</b>

The following docker-compose commands build a small client image targeted for creating optimized dockerized apps running on self-managed production servers. An Nginx service serves the frontend client on port 3000. Hot reload is NOT available when editing source codes from the `/client` and `/server` directories.

1. Follow step numbers 1 - 4 in the Docker for Localhost Development section.
2. Build the client and server containers for production deployment.
   - > **NOTE:** Run this step only once or as needed when housekeeping docker images or if there are new source code updates in the /client or /server directories.
   - `docker compose -f docker-compose.prod.yml build`
3. Load the production mode apps on `http://localhost:3000` (client) and `http://localhost:3001` (server) after running step no. 3.1.

   ```
   # 3.1. Create and start the production client and server containers.
   docker compose -f docker-compose.prod.yml up

   # 3.2. Stop and remove the production containers, networks, images and volumes
   docker compose -f docker-compose.prod.yml down
   ```

## Code Repository

The **climate-services-webportal** repository is a monorepo that contains source codes for the front-end in the `/client` directory, and backend in the `/server` directory. Set-up and usage instructions are available in each respective directory.

## Backend Server URLs

- **Production**
  - Server: https://amia-cis.onrender.com/api
  - API Documentation: https://amia-cis.onrender.com/docs
- **Development**
  - Server: https://amia-cis-xj2o.onrender.com/api
  - API Documentation: https://amia-cis-xj2o.onrender.com/docs
- **Localhost Development**:
  - Server: http://localhost:3001/api

## Client Website URLs

- Production (Live) Website: https://amia-cis.github.io/
- Development (Live) Website: https://amia-cis-dev.web.app/
- Localhost Development Website: http://localhost:3000

## Github Actions Environment Variables

The following GitHub secrets are required for GitHub Actions.

Refer to the **README** inside `/client` and `/server` for more informaation on their required environment variables.

### Firebase

| Variable Name                         | Description                                                                                                                                                                                                                                                                                                               |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FIREBASE_TOKEN                        | Firebase CI token used for deploying the `/client` app to Firebase Hosting.<br>This is obtained by signing-in to the firebase cli.                                                                                                                                                                                        |
| FIREBASE_SERVICE_ACC_DEV              | The (Development) Firebase project's service account JSON contents, condensed into one line and minus all whitespace characters.<br><br>The service account JSON file is generated from the Firebase project's **Project Settings** page, on **Project Settings** -> **Service accounts** -> **Generate new private key** |
| FIREBASE_PRIVATE_KEY_DEV              | The (Development) `private_key` entry from the `FIREBASE_SERVICE_ACC_DEV` service account JSON file.<br><br><blockquote>**NOTE:** Take note to make sure that the value starts and ends with a double-quote on WINDOWS OS localhost. Some systems may or may not require the double-quotes (i.e., Ubuntu).</blockquote>   |
| FIREBASE_SERVICE_ACC_PROD             | The (Production) Firebase project's service account JSON contents, condensed into one line and minus all whitespace characters.<br><br>The service account JSON file is generated from the Firebase project's **Project Settings** page, on **Project Settings** -> **Service accounts** -> **Generate new private key**  |
| FIREBASE_PRIVATE_KEY_PROD             | The (Production) `private_key` entry from the `FIREBASE_SERVICE_ACC_PROD` service account JSON file.<br><br><blockquote>**NOTE:** Take note to make sure that the value starts and ends with a double-quote on WINDOWS OS localhost. Some systems may or may not require the double-quotes (i.e., Ubuntu).</blockquote>   |
| FIREBASE_WEB_API_KEY_DEV              | Firebase web API key from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                                 |
| FIREBASE_WEB_AUTHDOMAIN_DEV           | Firebase web auth domain key from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                         |
| FIREBASE_WEB_PROJECT_ID_DEV           | Firebase web project ID from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                              |
| FIREBASE_WEB_STORAGE_BUCKET_DEV       | Firebase web storage bucket key from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                      |
| FIREBASE_WEB_MESSAGING_SENDER_ID_DEV  | Firebase web messaging sender ID from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                     |
| FIREBASE_WEB_APP_ID_DEV               | Firebase web web app key from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                             |
| FIREBASE_WEB_MEASUREMENT_ID_DEV       | Firebase web measurement ID from the (Development) Firebase Project Settings configuration file.                                                                                                                                                                                                                          |
| FIREBASE_WEB_API_KEY_PROD             | Firebase web API key from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                                  |
| FIREBASE_WEB_AUTHDOMAIN_PROD          | Firebase web auth domain key from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                          |
| FIREBASE_WEB_PROJECT_ID_PROD          | Firebase web project ID from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                               |
| FIREBASE_WEB_STORAGE_BUCKET_PROD      | Firebase web storage bucket key from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                       |
| FIREBASE_WEB_MESSAGING_SENDER_ID_PROD | Firebase web messaging sender ID from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                      |
| FIREBASE_WEB_APP_ID_PROD              | Firebase web web app key from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                              |
| FIREBASE_WEB_MEASUREMENT_ID_PROD      | Firebase web measurement ID from the (Production) Firebase Project Settings configuration file.                                                                                                                                                                                                                           |

### GitHub Pages

ACAP uses another GitHub repository's GitHub Pages for deploying it's client website. The remote GitHub repository is public, and only contains ACAP's built `/client` app exported using `npm run export`.

| Variable Name | Description                                                                                                              |
| ------------- | ------------------------------------------------------------------------------------------------------------------------ |
| GH_EMAIL      | Email associated with the target (remote) GitHub account on which Github Pages the production `/client` app is deployed. |
| GH_USERNAME   | GitHub username associated with `GH_EMAIL`                                                                               |
| GH_ORG        | GitHub organization name from the GitHub account (linked to `GH_EMAIL`)                                                  |
| GH_PUSH_TOKEN | GitHub Personal Access Token associated with `GH_EMAIL`                                                                  |
| GH_REPOSITORY | Public GitHub repository name on GH_ORG on which Github Pages the production `/client app` is deployed.                  |

### Render (render.com)

| Variable Name           | Description                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------- |
| RENDER_DEPLOY_HOOK_DEV  | The (development) server's private Deploy Hook URL to trigger a deploy for it's server on render.com |
| RENDER_DEPLOY_HOOK_PROD | The (production) server's private Deploy Hook URL to trigger a deploy for it's server on render.com  |
| API_BASE_URL_DEV        | /server app's API base url on the Render (development) app<br>https://amia-cis-xj2o.onrender.com/api |
| API_BASE_URL_PROD       | /server app's API base url on the Render (production) app<br>https://amia-cis.onrender.com/api       |

### Heroku

ACAP's server initially used Heroku for hosting and migrated to Render as of [v8.1.1](https://github.com/ciatph/climate-services-webportal/releases/tag/v8.1.1), after Heroku shut down it's Free plan.<br><br>
Use the following Heroku environment variables for reference to deploy on Heroku's new paid plans.<br>
Check out the archived Heroku yml files on `.github/workflows/archive` for reference.

| Variable Name     | Description                                                                                                             |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| HEROKU_EMAIL      | Heroku email of a Heroku account on which to deploy the **/server**                                                     |
| HEROKU_API_KEY    | Heroku account API key associated with `HEROKU_EMAIL`                                                                   |
| HEROKU_APP_DEV    | Heroku (development) app name inside a Heroku account associated with `HEROKU_EMAIL` on which to deploy the **/server** |
| HEROKU_APP_PROD   | Heroku (production) app name inside a Heroku account associated with `HEROKU_EMAIL` on which to deploy the **/server**  |
| API_BASE_URL_DEV  | /server app's API base url on the Heroku (development) app<br>https://amia-cis.herokuapp.com/api                        |
| API_BASE_URL_PROD | /server app's API base url on the Heroku (production) app<br>https://amia-cis-dev.herokuapp.com/api                     |

### MapBox

ACAP uses MapBox to host the AMIA Villages GeoJSON file used on the Home Page's map visualization.

| Variable Name           | Description                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| MAPBOX_USERNAME         | Mapbox username                                                                              |
| MAPBOX_BASEMAP_STYLE_ID | Mapbox style ID of a Style containing a blank map                                            |
| MAPBOX_DATASET_ID       | Mapbox dataset ID of a GeoJSON Dataset, containing the Bicol region's provinces GeoJSON file |
| MAPBOX_API_KEY          | Mapbox API key to use for querying the Mapbox API                                            |

### Openweather API

| Variable Name        | Description                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OPENWEATHERMAP_APPID | (Optional) Openweather API key, required to query the Openweather API.<br><br><blockquote>**NOTE:** ACAP stopped using the Openweather API since [v8.4.6](https://github.com/ciatph/climate-services-webportal/releases/tag/v8.4.6) but let's keep this variable for reference if there will be a need<br>to use non-PAGASA weather data in the future. We can use real or random values for this variable in the meantime.</blockquote> |

### Miscellaneous

| Variable Name               | Description                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FIREBASE_APP_DEV            | Name of the development Firebase project                                                                                                                                                                                                                                                                                                                                                        |
| EMAIL_WHITELIST             | Comma-separated emails of ACAP's Firebase Auth users that are not allowed to be updated.                                                                                                                                                                                                                                                                                                        |
| LIVE_ORIGIN_DEV             | The (development) domain/origin where the client is running, which will be used by the server's `LIVE_ORIGIN` variable.<br>Use https://climate-webservices-dev.web.app                                                                                                                                                                                                                          |
| LIVE_ORIGIN_PROD            | The (production) domain/origin where the client is running, which will be used by the server's `LIVE_ORIGIN` variable.<br>Use https://amia-cis.github.io.                                                                                                                                                                                                                                       |
| REGION_NAME                 | Region name. Default value is `"bicol"`                                                                                                                                                                                                                                                                                                                                                         |
| PROVINCES                   | Comma-separated province names belonging to `REGION_NAME`.<br>These should match the province names found in the 10-day weather forecast excel files.                                                                                                                                                                                                                                           |
| DEFAULT_PROVINCE            | Default province name to use. Default value is `"Albay"`.                                                                                                                                                                                                                                                                                                                                       |
| DEFAULT_MUNICIPALITY        | One of the municipalities under `DEFAULT_PROVINCE`. Default value is `"Tiwi"`.                                                                                                                                                                                                                                                                                                                  |
| SORT_ALPHABETICAL           | Arranges the municipality names in alphabetical order.<br>Default value is `1`. Set to `0` to use the ordering as read from the Excel file.                                                                                                                                                                                                                                                     |
| AXIOS_SSL_REJECT_INVALID    | Flag to ignore SSL/TLS certificate verification errors and accept self-signed or invalid certificates in the `AxiosInstance` object axios settings, used for web scraping PAGASA's severe tropical cyclone and El Nino / La Nina weather forecast pages. Defaults to `'1'` (recommended for security). Set to `'0'` as an extreme workaround to ignore SSL/TLS certificate verification errors. |
| PAGASA_10DAY_EXCEL_BASE_URL | Download URL of PAGASA's 10-day weather forecast excel files, minus the excel file names.<br>`https://pubfiles.pagasa.dost.gov.ph/pagasaweb/files/climate/tendayweatheroutlook`                                                                                                                                                                                                                 |
| NEXT_PUBLIC_GEOJSON_URL     | URL of the remote GeoJSON map layer containing municipalities boundaries used in the Home page<br>Loads the MapBox-hosted GeoJSON file if value is left to `"default"`.                                                                                                                                                                                                                         |
| REGION_CODE                 | Region code number                                                                                                                                                                                                                                                                                                                                                                              |
| REGION_LAT_AND_LNG          | Center longitude (lon) and latitude (lat) of a region                                                                                                                                                                                                                                                                                                                                           |
| REGIONAL_FIELD_OFFICE       | Regional field office name                                                                                                                                                                                                                                                                                                                                                                      |
| REGION_URL                  | Regional field office website URL                                                                                                                                                                                                                                                                                                                                                               |

@ciatph
20220405
