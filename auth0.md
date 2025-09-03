# Auth0

Api docs: https://auth0.com/docs/api/

## Setup

1. Register
2. (In dashboard) -> Applications -> Create Application -> Web Application
3. Settings Tab:
    - Allowed Callback URLs: http://localhost:3000/callback
    - Allowed Logout URLs: http://localhost:3000
    - Allowed Web Origins: http://localhost:3000
    - Show Advanced Settings -> Grant Types Tab -> Check "Password"
    - Save Changes
4. (In sidebar) -> Users & Roles -> Create User 
    - any email (no need for confirm ation)
    - Connection: Username-Password-Authentication
5. (In sidebar) -> Settings -> Default Directory
    - fill with "Username-Password-Authentication"



## [Management API](https://auth0.com/docs/api/management/v2)

1. Create new Application like in [Setup](#setup)
    - Application Type: Machine to Machine
2. Select scopes
    - read:users
    - update:users
    - delete:users
    - create:users
