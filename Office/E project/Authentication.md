### Service Owner (Only Google)
#### Sign up - Not allow
#### Sign in - Only Google
#### Change Password - Not Allow
#### Add user (Only super admin) 
**Invite new user**: Super Admin call `addTenantOwnerUser` -> add new service owner as `not-verified` -> send email
**Accept invitation**: New service owner click into link (with token) -> update status to `verified`
**Case failed**: that new admin not accept in <mark style="background: #FF5582A6;">24h</mark> -> SuperAdmin resend invitation (check user exist but `not-vetified`)
- Api `addTenantOwnerUser`
	- User exist and `verified` -> throw Error(<mark style="background: #FF5582A6;">EMAIL_REGISTERED</mark>)
	- User exist and `not-verified` -> Send invitation again
- Api `acceptInvitation`
	- Token valid -> update to `verified`
	- Token invalid -> throw error
### Tenant Owner (Google and Email)
#### Sign up (Both)
+ Allow sign up with Google -> `set up corporation step`
+ Allow sign up with Email/Password -> need to verify email before getting into `set up corporation step` and `setupPassword`
#### Set up Corporation Step
- Step by step (keep track the progress) -> send back this progress on user login -> force user complete step before getting into dashboard
	- Corporation info (address, name, phone, ....)
	- Input a product
#### Sign in (Both)
#### Change Password (both)
- Google user -> change password with 2 fields
- Email-Password user -> change password with 3 fields
#### Add new user (send invitation)
**Invite new user**: tenant owner call `addUser` -> add new service owner as `not-verified` -> send email
**Case failed**: that new tenant user not accept in <mark style="background: #FF5582A6;">24h</mark> -> tenant owner resend invitation (check user exist but `not-vetified`)
- Api `addUser`
	- User exist and `verified` -> throw Error(<mark style="background: #FF5582A6;">EMAIL_REGISTERED</mark>)
	- User exist and `not-verified` -> Send invitation again
- Api `acceptInvitation`
	- Token valid -> update to `verified`
	- Token invalid -> throw error
### User (Google and Email)
#### Sign up - Not allow
#### Sign in (Both)
#### Change Password (NOT ALLOW)
#### FORGOT PASSWORD
<mark style="background: #BBFABBA6;">Only the Email-Password can do this</mark>
#### Add new user (accept invitation)
Click accept invitation -> navigate to `setPassword` screen
- SetPassword ->  update status to `verified`
- Login with Google ->  update status to `verified`

add field **isRootAdmin** in list **serviceOwner**
**lastLogin** by refreshToken too
