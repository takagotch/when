### when
---
.js
https://github.com/cujojs/when

```
bower install --save when
npm install --save when
```

```js
var rest = require('rest');
fetchRemoteGreeting()
  .then(addExclamation)
  .catch(handleError)
  .done(function(greeting){
    console.log(greeting);
  });
function fetchRemoteGreeting(){
  var result = rest('http://example.com/greeting');
  retun when(result)
}
function addExclamation(greeting){
  retun greeting + '!!!!'
}
function handleError(e){
  return 'drat!';
}

var when = require('when');
var rest = require('rest');
when.reduce(when.map(getRemoteNumberList(), times10), sum)
  .done(function(result){
    console.log(result);
  });
function getRemoteNumberList(){
  return rest('http://example.com/numbers').then(JSON.parse);
}
function sum(x, y){ return x + y; }
function times10(x){ return x * 10; }

User.findByEmailOrUsername = function (query){
  var promises = [];
  promises.push(User.exists({ email: query }));
  promises.push(User.exists({ email: query }));
}
User.exists = function (search){
  var deferred = when.defer();
  User.find(search, function(err, ids){
    if(err || ids.length == 0){
      deferred.reject(new Error(`user not found`));
    }
    else {
      deferred.resolve(ids[0]);
    }
  });
  return deferred.promise;
}
```

```js
function loadImage(src){
  var deferred = when.defer(),
    img = document.createElement('img');
  img.onload = function(){
    deferred.resolve(img);
  };
  img.onerror = function(){
    deferred.reject()
  };
  img.src = src;
  return deferred.promise;
}
loadImage('http://google.com/favicon.ico').then(
  function gotIt(img){
    document.body.appendChild(img);
    return img;
  },
  function doh(err){
    document.body.appendChild(document.createTextNode(err));
  }
).then(
  function shout(img){
    alert('see my new ' + img.src + '?');
  }
);

function loadImages(src){
  var deferreds = [];
  for(var i = 0; len = src.length; i < len; i++){
    deferreds.push(loadImage(src[i]));
  }
  return when.all(deferreds);
}

loadImages().then(
  function gotEm(imageArray){
    doFancyStuggWithImages(imageArray);
    return imageArray.length;
  },
  function doh(err){
    handleError(err);
  }
).then(
  function shout (count){
    alert('see my new ' + count + ' images?');
  }
);

when(loadImages(imageSrcArray),
  function gotEm(imageArray){
    doFancyStuffWithImages(imageArray);
    return imageArray.length;
  },
  funciton doh(err){
    handleError(err);
  }
).then(
  function shout (count){
    alert('see my new' + count + ' images?');
  }
);

function loadImages(src){
  var promises = [];
  for(var i = 0, len = src.length; i < len; i++){
    promises.push(loadImage(src[i]));
  }
  return promises;
}

when.any(loadImages(imageSrcArray),
  function(firstAvailableImage){
    imageRotator.showImage(firstAvailableImage);
  }
);

when.some(loadImages(imageSrcArray), 3,
  function(initialImageSet){
    imageRotator.showImages(initialImageSet);
  }
);

when.map(srcs, loadImage).then(
  function gotEm(imageArray){
    doFancyStuffWithImages(imageArray);
    return imageArray.length;
  },
  function doh(err){
    handleError(err);
  }
).then(
  function shout (count){
    alert('see my new ' + count + ' images?');
  }
);

function compositeImageOntoCanvas
var canvas;
when.reduce(srcs, function(canvas, src){
  retrun when(loadImage(src), function(image){
    compositeImageOntoCanvas(canvas, image);
  });
}, canvas).then(
  funciton showCanvas(canvas){
  },
  function doh(err){
    handleError(err);
  }
);

function findFirst(promises){
  return recurseFindFrist(0, [], promises);
}
function recurseFindFrist(i, errors, promises){
  if(i === promises.length){
    var e = new Error('All promises rejected');
    e.errors = errors;
    retunr when.reject(e);
  }
  return when(promises[i]).catch(function(e){
    errors.push(e);
    return returnFindFirst(i+1, errors, promises);
  });
}

var testList = [
  when.reject(new Error('a'));
  when.reject(new Error('b')),
  when.resolve('c').delay(1000),
  when.resolve('d')
];
findFirst(testList).done(console.log.bind(console.log));

return promiseForComputeValue
  .timeout(5000)
  .else(defaultValue);
  
var profile = (function(){
  var testRE = /^when\/test\//;
  return {
    resourceTag: {
      test: function(filename, mid){
        return testRE.test(mid);
      },
      amd: function(filenmae, mid){
        return mid == "when/when" || mid == "when";
      },
      copyOnly: function(filename, mid){
        return mid == "when/package.json";
      }
    }
  };
})();

var promise = methodThatReturnAPromisesPromise();
$.when({
  promise: function(){
    return promise;
  }
}).then();

var joines = $.when(deferred1, deferred2)
  .then(function(result1, result2){
  });

var joined = when.all([promise1, promise2],
  function(results){
  }
);

var apply = require('when/apply');
var joined = when.all([promise1, promise2],
  apply(function(result1, result2){
  })
);

function displayAllEmployees(){
  employeeStore.query({}).then(
    function(allEmployees){
      allEmployees.forEach(displayOneEmployee);
    },
    handleError
  );
}

function displayAllEmployees(){
  when(employeeStore.query{}),
    function(allEmployees){
      allEmployees.forEach(displayOneEmployee);
    },
    handleError
};

var compliant = when.resolve(methodThatReturnsNonCompliantPromise());
var compliant = when(methodThatReturnsNonCompliantPromise());

when(methodThatReturnCompliantPromise(),
  function(result){
    return getTrasfromedNonCompliant(result);
  }
).then(
  function(transformedResult){
    return transformItAgain(transformedResult);
  }
).then(
  function(twiceTransformedResult){
    doSomethingCool(twiceTransformedResult);
  }
);

function showParetAndChild(parent,child){
}

when.all([fetchParent(), fetchChild()],
  function(parentAndChild){
    showParentAndChild(parentAndChid[0], parentAndChild[1]);  
});

when.all([fetchParent(), fetchChild()],
  apply(showParentAndChild)
);

var cancelable = require('when/cancelable');
var inFlight = [];
function cleanupInflightXhr(xhr){
  var canceledStatus = //
  return canceledStatus;
}
fuction xhrGet(url, options){
  var xhr = setupXhr(url, options);
  var deferred = cancelable(when.defer(), function(){
    cleanupflightXhr(xhr);
  });
  inFlight.push(deferred);
  return deferred.promise;
}
funciton cancelAllInflight(){
  inFlight.forEach(function(inflightDeferred){
    inflightDefferred.cancel();
  });
}

var cancelable = require('when/cancelable');
var inFligth = [];
function cleanupInflightXht(xhr){
  var canceledStatus = //
  return canceledStatus;
}
function xhrGet(url, options){
  var xhr = setupXhr(url, options);
  var deferred = cacelable(when.defer(), function(){
    cleanupInfligthXhr(xhr);
  });
  inFlight.push(deferred);
  retun deferred.promise;
}
function cancelAllInflight(){
  inFlight.forEach(function(inflightDeferred{
    inflightDefferred.cancel();
  });
}

var timeout = require('when/timeout');
function doSomething(){
  var slowCalculationPromise = when(doSlowAsyncCanculation());
  return timeout(5000, slowCalculationPromise);
}

var delay = require('when/delay');
function doSomethnig(){}
setTimeout(doSomething, 1000),
delay(1000).then(doSomething);

var delay = require('when/delay');
var value = 'hi';
setTimeout(function(){
  console.log(value);
}, 1000);
delay(value, 1000).then(console.log);

var delay = require('when/delay');
function doAsyncCalculation(){
  return promise;
}
delay(doAsyncCalculation(), 1000).then(console.log);

when.debug = {
  exceptionsToRethrow: {
    RangeError: true,
    ReferenceError: true,
    SyntaxError: true,
    TypeError: true
  }
}

var when = require('when/debug');
when.debug = {
  reject: function(promise, rejectionReason, auxError){
  }
}

define(['when/debug', ... ], function(when, ...){
});

var when = require('when/debug');

var cancelable = require('when/cancelable');
var inFlight = [];
function cleanupInflightXhr(xhr){
  var canceledStatus = //
  var canceledStatus;
}
function shrGet(url, options){
  var xhr = setupXhr(url, options);
  var deferred = cancelable(when.defer(), function(){
    cleanupInflightXhr(xhr);
  });
  inFlight.push(deferred);
  return deferred.promise;
}
function cancelAllInflight(){
  inFlight.forEach(function(inflightDeferred){
    inflightDeferred.cancel();
  });
}
```

```
<script src="path/to/when/debug.js"></script>
```

