``` JavaScript
// we're setting roles
role: {
  type: DataTypes.ENUM{
    // these are keys in the ACL
    "user", 
    "writer", 
    "editor", 
    "admin"
  }
  // we're setting a default role for one isn't selected
  defaultValue: "user",
},



// we're setting which
capabilities: {
  type: DataTypes.VIRTUAL,
  get: ()=> {
    return acl[this.role];
  }
}

```

``` json
// json model for roles
{
  "writer": "",
  "admin": "",
  "editor": "", 
  "user": ""
}
```

``` javascript
// authorize.js


// curry this function, 
function authorize => (capability) => (req,res,next) =>
{
  try{
    // if the request.user object has a .capabilities and includes(capability)
    if (req.user.capabilities.includes(capability))
    {
      // call next (let them through to the next thing)
      next();
    }
    
  }
  catch (error) {
    // if they req.use.capabilities doesn't have the `capability` property, give error
    next('unauthorized');
  }
}
module.exports = authorize;
```

``` javascript
// end points: we have to specify which capability
// in this case, we want to authorize the 'read' capability
app.use('/poop', (req,res,authorize('read')
```
