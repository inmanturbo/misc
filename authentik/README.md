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
  - name: `auth`
  - address: `http://authentik-http.ix-authentik.svc.cluster.local:10230/outpost.goauthentik.io/auth/traefik`
  - Configure authResponseHeaders>Add (x11)
    1. `X-authentik-username`
    2. `X-authentik-groups`
    3. `X-authentik-email`
    4. `X-authentik-name`
    5. `X-authentik-uid`
    6. `X-authentik-jwt`
    7. `X-authentik-meta-jwks`
    8. `X-authentik-meta-outpost`
    9. `X-authentik-meta-provider`
    10. `X-authentik-meta-app`
    11. `X-authentik-meta-version`

