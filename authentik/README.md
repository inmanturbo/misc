# Authentik behind traefik on truenas scale:

## Configuring Authentik app in truenas scale:

### In the ingress section add a host with slash path for each domain that authentik will run on, ie 

#### Configure Hosts   Add
#### Host
HostName *
```
auth.example.com
```


#### Configure Paths   Add
#### Host
Path *
```
/
```
Path Type *
```
prefix
```

### And also add a catchall for subdomains to hit the outpost path (for logouts, etc) 
#### Configure Hosts   Add
#### Host
HostName *
```
*.example.com
```


#### Configure Paths   Add
#### Host
Path *
```
/outpost.goauthentik.io/
```
Path Type *
```
prefix
```

## Configuring Middleware in traefik on Truenas SCALE
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

## Configuring Proxy Provider in Authentik:
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

