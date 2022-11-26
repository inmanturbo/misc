## Setup letsencrypt on truenas scale
  - add email to root account
    - credentials>local users>root>edit
    
  - setup acme dns authenticator
    - credentials>certificates>acme dns authenticators>add
    - name: cloudflare
    - authenticator: cloudflare
    - email: blank
    - token: token from cloudflare
      - cloudflare>login>account>profile>api tokens to left>create token>create custom token
      - name: eg truenas scale
      - permissions: 
        - `Zone` `Zone` `Read`
        - `Zone` `DNS` `Edit`
        
### Add csr
- credentials>certificates>Certificate Signing Requests>add
  - name: eg example_com_csr
  - fill in required fields (use something like homelab for organization)
  - Subject Alternate Names: `*.example.com`
  - Save
  - Create Acme Certificate
    - credentials>certificates>Certificate Signing Requests>example_com_csr>click the wrench
      - identifier: example_com_cert
      - ACME Server Directory URI: Production
      - Authenticator: Cloudflare
           
           
## Setup Traefik on Truenas Scale

  - Change Ports for truenas web interface to 83 and 444
    - system settings>general>GUI>settings
      - Web Interface HTTP Port: `83`
      - Web Interface HTTPS Port: `444`
  - Add truecharts catalog to scale
    - Go to "Apps" in the left hand menu
    - Select the "Manage Catalogs" tab
    - Click "Add Catalog" and enter the required information:
      - Name: `truecharts`
      - Repository: `https://github.com/truecharts/catalog`
      - Preferred Trains: `stable`
      - Branch: `main`
  - Install traefik
    - Apps>Available Applications>traefik>install
    
