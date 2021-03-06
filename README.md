node-reload
===========


Auto reload apps without loss connections or sessions, good for production or development

## Installation

    $ npm install relive


## Example

```javascript
global.relive = require("relive")();
var express = require('express');
var app = express();  

    
if(!relive.sessionUse) relive.sessionUse = express.session({ store: new express.session.MemoryStore()});
    
      
app.use(express.cookieParser('<your secret here>'));
app.use(relive.sessionUse);
app.use(app.router);
    
app.get("/",function(req,res){
    req.session.count = req.session.count ? req.session.count + 1 : 1;
    res.end("your count: " + req.session.count);
});

relive.app = app;

if(relive.first){
    var http = require("http");
    
    http.createServer(function(){ 
        relive.app.apply(null, arguments); 
    }).listen( process.env.PORT || 80 , process.env.IP , function(){
        console.log('server listening on  %s:%s' ,process.env.IP, process.env.PORT);
    });
}else{
    console.log("reload app");
}
```

##Options

#### global.relive = require("relive")(`options`);

* `watch` array of directories OR string of directory OR boolean `true` -> pwd  `false` -> disable watch  (default: `true`)
* `watchMatch` object Regex for filter change file (default: `/\.(js|json)$/`)
* `watchDelay` delay milliseconds to reload (for many changes) (default: `1000`)








