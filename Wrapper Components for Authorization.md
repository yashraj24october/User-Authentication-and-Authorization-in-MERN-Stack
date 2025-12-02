<h2>Wrapper Components on frontend for Authorization</h2>

1. **ProtectRoute:** for logged-in users
   
**==== Will check user authentication using token, 2 layer check using authCheck API and then give access to page**

2.**AdminRoute:** for admin pages only

**==== Will check user authentication using token, 2 layer check using authCheck API , user role is admin or not and then if token correct, user exist and role is admin then give access to page**
