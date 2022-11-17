## Authentik behind traefik on truenas scale:

### In the ingress section add a host with slash path for each domain that authentik will run on, ie 

#### Configure Hosts   Add
#### Host
HostName *
```
auth.example.com                   x
```


#### Configure Paths   Add
#### Host
Path *
```
/                 x
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
*.example.com                   x
```


#### Configure Paths   Add
#### Host
Path *
```
/outpost.goauthentik.io/                  x
```
Path Type *
```
prefix
```
