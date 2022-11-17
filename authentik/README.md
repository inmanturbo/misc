## Authentik behind traefik on truenas scale:

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

# Middleware
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

