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
