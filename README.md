#Fermions

A Meteor pacakge for writing reusable cilent-side codes for Templates. 

#QuickStart
Add it to your Meteor project
```bash
meteor add arch:fermions
```

#Basic Usage

```javascript
fermions.lepton("codeblock1", function(){
    //some code here
});
fermions.lepton("codeblock2", function(){
    //some code here
});
fermions.quark("TemplateName",["codeblock1","codeblock2"]); 
```

It would call `codeblock1` and `codeblock2` when `Template.TemplateName` is rendered.

You can also do it this way with a variable:
```javascript
var electron = fermions.lepton(function(){
    //some code here
});
fermions.quark("TemplateName",[electron]); 
```

#Dependecy
```javascript
fermions.lepton("electron", function(){
    //some schrodinger's cat here
});
fermions.lepton("muon", function(){
    //some schrodinger's cat here
});
fermions.lepton("tau", function(){
    //some schrodinger's cat here
});

fermions.quark("downQuark",["electron"]);
fermions.quark("upQuark",["muon"]).depends(["downQuark"]); //name of the quark(s) to depend on
fermions.quark("upQuark",["tau"]);
```
After `upQuark` is rendered, `muon` would not be called until `Template.downQuark` is rendered. However, `tau` would be called because it does not depend on anything.


Note: Unless an object `{TemplateName:"quarkName"}` is passed as the 1st arguement, a quark would have the same name as the Template.

```javascript
fermions.quark({TemplateName:"quarkName"},["codeblock1","codeblock2"]);
fermions.quark("upQuark",["muon"]).depends(["quarkName"]);
```


#Setting variables when Template.created
```javascript
fermions.lepton("electron", function(){
    console.log(fermions.waveFunction);
});
fermions.quark("downQuark",["electron"],{waveFunction:"collapsed"});
fermions.quark("upQuark",["electron"],{waveFunction:"psi"});
```
This would log "collapsed" after `Template.downQuark` is rendered, and "psi" after `Template.upQuark` is rendered.

#On Template.destroyed

```javascript
fermions.lepton("neutrino", function(){
    //code here would be called on Template.rendered
},function(){
   //code here would be called on Template.destroyed
});
fermions.quark("charmQuark",["neutrino"]); 
```

The second function that is passed into the `.lepton` would be called on `Template.destroyed`;

#Mobile view

```javascript
fermions.mobileView(700); //this would assume that it is a mobile device if the window's width is smaller than 700 
fermions.lepton("neutrino", function(){
    //code here would be called no matter if you are in mobile view or not
}).addToFn("mobile",function(){
   //code here would only be called if it is morible view
}).addToFn("default",function(){
  //code here would only be called if it is not mobile view
});
fermions.quark("topQuark",["neutrino"]); 
```

If `fermions.mobileView(max_width)` is not called, the function in `addToFn("default",function)` would always be called and `addToFn("mobile",function)` would never be called.

#Cleaning up the function in a lepton
```javascript
electron.reset() //reset both default and mobile
electron.reset("mobile") //only reset mobile
```

#License

"THE BEER-WARE LICENSE" (Revision 42):

As long as you retain this notice you can do whatever you want with this stuff. If we meet some day, and you think this stuff is worth it, you can buy me a beer in return