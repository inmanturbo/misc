# Configuring Active Directory
- Fist add Alternative UPN for your external domain
  - mmc>snap-ins>AD Domains & Trusts>Properties
    - Alternative UPN suffixes: `example.com` (for login like john@example.com)
- Create user for authentik syncing
  - mmc>AD Users & Computers>%Domain%>Users>Right Click>New>User
    - first name: `authentik`
    - last name: `Service`
    - full name: `authentik Service`
    - User logon name: `_service_authentik@example.com`
    - User logon name (pre-Windows 2000): `AD.INTERNAL.SOMETHING\_service_authentik`
    - Click `Next`
      - Give a strong unique random password
      - Check `User cannot change pasword`
      - Click Finish
- Delegate Control for passwords 
  - mmc>AD Users & Computers>%Domain%>Right Click>All Tasks>Delegate Control
    - Click Add under users and groups
      - type `_service_authentik`
      - click check names
      - click next
      - click next
        - [x] Create, delete, and manage user accounts
        - [x] Reset user password and force password chagne at next logon
        - [x] Read all user information
      - click next
      - Finish
 
# Configuring Authentik
- Directory>Federation & Social login>Select Type>LDAP Source
  - Name, eg: `company_name_active_directory`
  - Server URI: `ldaps://samba-domain-controller-ip-or-resolvable-hostname`
  - Uncheck `Enable StartTLS`
  - Bind CN: `_service_authentik@example.com`
  - Bind Password: password for `_service_authentik`
  - Base DN: `CN=Users,DC=<domain>,DC=<tld>`
    > Note: This will be your actual domain and not your custom UPN suffix!
    > Can be retrieved by running `dsquery user -name <any user name>` from windows cmd prompt
  - Property mappings: Control/Command-select all Mappings which start with "authentik default LDAP" and "authentik default Active Directory"
  - Group property mappings: Select "authentik default LDAP Mapping: Name"

  
