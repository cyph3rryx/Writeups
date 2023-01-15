# Local Authority  →  100Pts.

1. So we have to enter the username and password to get the access of flag.
2. Tried admin= username and admin=password -> Revert back that Log In Failed.
3. So checked the files and sources and in the JS file it was indicated that

if( username === 'admin' && password === 'strongPassword098765' )
{
return true;
}
else
{
return false;
}

4. So logged in with username = admin & password = strongPassword098765
5. Got the flag.
