# chrome extension messaging library


## Basics
### receiver registration
```js
let message = new Message({
  foo1: data=>{...},
  foo2: data=>{...}
});
message.setReceiver("foo3", data=>{
  // todo..
  return 'testdata';
  // you can use Promise
  return new Promise(resolve=>{
    setTimeout(()=>{
      resolve('testdata');
    }, 2000);
  })
  // axios example
  return axios.post(someurl, data);
});
```

### send message
```js
message.send("bar", data);
```
### send message and receive
```js
message.send("bar", data).then((result, send, tabId)=>{
  console.error(result);
});
```
### send message and receive and send again
```js
message.send("bar", data).then((result, send, tabId)=>{
  send("bar2", "ok");
});
```

## examples

### inject.js
```js
let message = new Message({
  saveComplete: result=>{
    console.error("complete", result);
  }
});
message.send("save", data);
```

### background.js
```js
let message = new Message({
  save: (data, send)=>{
    // todo..
    send("saveComplete", "resultData");    
  }
});

```


### inject2.js
```js
let message = new Message();
message.send("save", data).then(result=>{
  console.error("complete", result);
})
```

### background2.js
```js
let message = new Message({
  save: data=>{
    // todo..
    return "resultData";    
  }
});

```


## License

MIT
