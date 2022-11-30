## Setup letsencrypt on truenas scale
  - ##### add email to root account
    - credentials>local users>root>edit
  - ##### setup acme dns authenticator
    - credentials>certificates>acme dns authenticators>add
    - name: cloudflare
    - authenticator: cloudflare
    - email: blank
    - ##### token: token from cloudflare
      - cloudflare>login>account>profile>api tokens to left>create token>create custom token
      - name: eg truenas scale
      - permissions: 
        - `Zone` `Zone` `Read`
        - `Zone` `DNS` `Edit`   
### Add csr
- ##### credentials>certificates>Certificate Signing Requests>add
  - name: eg example_com_csr
  - fill in required fields (use something like homelab for organization)
  - Subject Alternate Names: `*.example.com`
  - Save
  - ##### Create Acme Certificate
    - credentials>certificates>Certificate Signing Requests>example_com_csr>click the wrench
      - identifier: example_com_cert
      - ACME Server Directory URI: Production
      - Authenticator: Cloudflare

## Setup Traefik on Truenas Scale
- ##### Change Ports for truenas web interface to 83 and 444
  - ##### system settings>general>GUI>settings
    - Web Interface HTTP Port: `83`
    - Web Interface HTTPS Port: `444`
- ##### Add truecharts catalog to scale
  - Go to "Apps" in the left hand menu
  - Select the "Manage Catalogs" tab
  - Click "Add Catalog" and enter the required information:
    - Name: `truecharts`
    - Repository: `https://github.com/truecharts/catalog`
    - Preferred Trains: `stable`
    - Branch: `main`
- ##### Install traefik
  - Apps>Available Applications>traefik>install
    - #### web Entrypoint Configuration
      Entrypoints Port *
      ```
      80
      ```
    - #### websecure Entrypoint Configuration
      Entrypoints Port *
      ```
      443
      ```
        
## Setup Authentik behind traefik on truenas scale
- #### Configure Authentik app in truenas scale:
  - #### In the ingress section add a host with slash path for each domain that authentik will run on
    #### Configure Hosts   [Add]
    #### Host
    HostName *
    ```
    auth.example.com
    ```
    #### Configure Paths   [Add]
    #### Host
    Path *
    ```
    /
    ```
    Path Type *
    ```
    prefix
    ```
  - #### Add a catch-all for subdomains to hit the outpost path (for logouts, etc) 
    #### Configure Hosts   [Add]
    #### Host
    HostName *
    ```
    *.example.com
    ```
    #### Configure Paths   [Add]
    #### Host
    Path *
    ```
    /outpost.goauthentik.io/
    ```
    Path Type *
    ```
    prefix
    ```
  - #### Add host to tls settings
    #### Configure TLS-Settings   [Add]
    #### Host
    #### Configure Certificate Hosts [Add]
    #### Host*
    ```
    auth.exampl.com
    ```
    #### Select TrueNAS SCALE Certificate
    ```
    example_com_cert   ˅
    ```
    
## Configure Middleware in traefik on Truenas SCALE
- Apps>traefik>edit
  - Middlewares>forwardAuth>Add
  - name: `authentik`
  - address: `http://authentik-http.ix-authentik.svc.cluster.local:10230/outpost.goauthentik.io/auth/traefik`
  - Configure authResponseHeaders>Add (x11)
    - `X-authentik-username`
    - `X-authentik-groups`
    - `X-authentik-email`
    - `X-authentik-name`
    - `X-authentik-uid`
    - `X-authentik-jwt`
    - `X-authentik-meta-jwks`
    - `X-authentik-meta-outpost`
    - `X-authentik-meta-provider`
    - `X-authentik-meta-app`
    - `X-authentik-meta-version`

## Configure Proxy Provider in Authentik:
- Applications>Applications>Create
  - name, e.g.: `speedtest`
  - slug, e.g.: `speedtest`
  - provider>create provider>
    - select type>Proxy Provider>next
    - name, e.g.: `speedtest`
    - For ability to restrict app to users or groups select `Forward auth (single application)`
      - External host, eg: `https:speedtest.example.com`
    - Click `Finish`
   - Provider>Select>speedtest
   - Click `Create`
