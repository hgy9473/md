# Ajax JS库笔记


## jQuery

## Fetch API


## Axios

> Promise based HTTP client for the browser and node.js 

GitHub [Axios](https://github.com/axios/axios)

```html
<!-- 示例 -->
<!-- ... -->
    <script src="https://cdn.bootcss.com/axios/0.18.0/axios.min.js"></script>
    <script>
        axios({
            method:'get',
            url:'./demoData/data-test.json'
        })
        .then(function (response) {
            console.log("res",response);
        })
        .catch(function(err){
            console.log("err",err);
        });
    </script>
<!-- ..... -->
```

## SuperAgent

> SuperAgent is a small progressive client-side HTTP request library, and Node.js module with the same API, sporting many high-level HTTP client features. 

GitHub [SuperAgent](https://github.com/visionmedia/superagent)

```html
<!-- 示例 -->
<!-- ... -->
<script src="https://cdn.bootcss.com/superagent/3.8.3/superagent.min.js"></script>
<script>
    superagent.get('./demoData/data-test.json')
    .set('Content-Type', 'application/json')
    .then(function(msg){
        console.log("msg", msg);
    });
</script>
<!-- ... -->

```

## Request
