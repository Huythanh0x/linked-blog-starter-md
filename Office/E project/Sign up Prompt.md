# Sign up with Email/Password
The flow:
1. User input email and invoke signupTenant mutation
2. User might resend request -> use the same signupTenant mutation again
3. BE recieve the request and store the data to rootDB temp sign up table[id, email, time]
4. In case there are already 5 requests in the last 24h -> throw ERROR
- [ ] Sign up with Google or Email Password -> one mutation
- [ ] Resend email request -> basically non (reuse the sign up mutation instead)
- [ ] Actual logic on recive sign up request. Insert to a temp db to keep track of sign up
- [ ] Limit one email to send 5 emails a day. Use the same sign up table above
- [ ] Have a logic remove expired sign up data. On every new request -> remove expired sign
up data OR cron job to clean every 24h
- [ ] Then send email verification -> next task
# Send verification email
Follow the existing verification flow to send verification email (new TYPE OF token)
- [ ] Generate the signup verification and store in Redis (ttl 24h)
- [ ] Send email with temaplte and FE redirect URL
- [ ] Api verification -> next task
# On Flow set up tenant data
This flow is complex, require create tenantDB, setup data for multiple tables.
Flow On click email verification:
1. send request SetupPassword -> setup tenantdb, delete signupdate,setup coporation,setup
password owners  -> return back tenant auth data
2. user check 2 flags to go to next steps.
3. setup company infor
4. add new product
**Note** The creds of tenant owners actualy store under "tenantDB.owners"
- [ ] API setup password/login with Google (same as existing setupPassword mutation). on
Complete -> update the flag isVerified -> also execute the step to create new tenantDB. And
delete the signupData in rootDB too.
- [ ] Use existing function to create new tenant DB. The data would be inserted to: create
data at rootDB(tenantConnection, tenantOwner). And init the tenantDB. Under tenantDB, insert
coporation and owner data. (basically a new table with single row data only)
- [ ] Update auth tenant data: add two new flags: hasSetupCompanyInfor and hasSetup Product.
- [ ] Then we return the auth data for the tenant. and under this auth data would have 2
flags above to setup data
- [ ] Then create updateCompanyInfor muation
- [ ] Then create addNewProduct endpoint.We have to create a product entity here and
productService to manipulate the table
# Sign up with Google
This one a bit simplier.
On sign up with Google -> get directly to On Flow setup tenant data
- [ ] Basically check if-else to add this flow to exising sign up with Email

# Product table
| No  | Logic name | Physical name | Data type | Not Null | Unique | Default           |
| --- | ---------- | ------------- | --------- | -------- | ------ | ----------------- |
| 1   | id         | id            | varchar   | Yes      | Yes    |                   |
| 4   | title      | title         | varchar   | Yes      |        |                   |
| 5   | content    | content       | varchar   | Yes      |        |                   |
| 6   | Created At | createdAt     | datetime  | Yes      |        | Current timestamp |
| 7   | Updated At | updatedAt     | datetime  |          |        | Current timestamp |
| 8   | Deleted At | DeletedAt     | datetime  |          |        |                   |
