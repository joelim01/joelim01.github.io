---
layout: post
title:  "AngularJS Component Bindings"
date:   2017-03-13 02:32:27 +0000
---


I learned about an interesting design pattern in AngularJS that addressed something that had always bugged me (I knew there was a better way). 

When you have something, a form let's say, that uses data that is hooked into other components of your view, I wonder how you share that data?

In the past, I'd create a data service with functions for retrieving that data, holding it, saving back to the datastore, etc. and then pass the service to multiple controllers so that they would have access to those functions and the data inside it.

While that works for me, I also found that having all of the parts of my application "know" how to manipulate the data through the datastore seemed like it might not be the best.

Enter the idea that your components shouldn't use two way binding (only the component that owns the data should manipulate it)... by using the '&' binding...

```
bindings: {
  yourData: '<' // note the one way binding here...
  onDelete: '&',
  onUpdate: '&'
}
```

...rather than directly manipulating the data, you call an output event with the changed data, which is pretty cool.

So in your child component template you might have:

```
<button ng-click="$ctrl.update()">Update</button>
```

and in your child component controller you'd have:

```
vm.update = function(){
  onUpdate({dataFromChild = yourData});
}
```

and in your parent template you'd have:

```
<child-component your-data="vm.dataForChild" on-delete="vm.delete(dataFromChild)" on-upate="vm.update(dataFromChild)"
```

And then you'd have an update and delete function in your parent component controller to actually execute the data manipulation. 

So, making mini one-way bound data streams to simplify things. Nifty, eh?
