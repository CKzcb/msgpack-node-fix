`node-msgpack-context-aware` is same to (msgpack)[https://www.npmjs.com/package/msgpack], just except that one of the bugs has been fixed to make the module context-aware;

#### issue: 


[rpo](https://github.com/msgpack/msgpack-node/issues/60)

The following erro occurred when i use msgpack in worker-thread

```shell
[2023-06-09T15:59:10.621] [ERROR] default - thread error: Error: Module did not self-register: 'xxxxxx\node_modules\msgpack\build\Release\msgpackBinding.node'.
    at Module._extensions..node (node:internal/modules/cjs/loader:1338:18)
    at Module.load (node:internal/modules/cjs/loader:1117:32)
    at Module._load (node:internal/modules/cjs/loader:958:12)
    at Module.require (node:internal/modules/cjs/loader:1141:19)       
    at require (node:internal/modules/cjs/helpers:110:18)
    at Object.<anonymous> (H:\company\sg2\tsgf\node_modules\msgpack\lib\msgpack.js:6:14)
    at Module._compile (node:internal/modules/cjs/loader:1254:14)      
    at Module._extensions..js (node:internal/modules/cjs/loader:1308:10)
    at Module.load (node:internal/modules/cjs/loader:1117:32)
    at Module._load (node:internal/modules/cjs/loader:958:12) {        
  code: 'ERR_DLOPEN_FAILED'
}
```

#### solution

(reference)[https://github.com/nodejs/node/issues/21783#issuecomment-429637117]

change this to new
```C++
// NODE_MODULE(msgpackBinding, init);
// fix: make the module context-aware that it needs to reolace NODE_MODULE
NODE_MODULE_INIT()
{
    init(exports);
}
```

thanks for all help, [Stevie](https://stackoverflow.com/users/473338/stevie), [fathyb](https://github.com/fathyb), and brother Tao

#### addition

(Context-aware addons)[https://github.com/nodejs/node/blob/main/doc/api/addons.md#context-aware-addons]
