# WASM go Filter for Envoy

## Build

```
tinygo build -o plugin.wasm -scheduler=none -target=wasi ./main.go
```
## Testing

```
envoy -c ./envoy.yaml --concurrency 2 --log-format '%v' 
```
## Optimization GC

Memory leaks exist in the Wasm plug-in for an Envoy proxy compiled by using TinyGo. The proxy-wasm-go-sdk community recommends that you use nottinygc for compilation optimization. Perform the following steps to use nottinygc for compilation optimization:

Add the following import code to the beginning of the main.go file:

```go
import _ "github.com/wasilibs/nottinygc"
```

If no dependencies are found, you can run the go mod tidy command to automatically download the dependencies.

Run the following command to compile the code:

```sh
tinygo build -o plugin.wasm -gc=custom -tags='custommalloc nottinygc_envoy'  -target=wasi -scheduler=none main.go
```

The preceding command sets the -gc and -tags parameters. For more information, see nottinygc. 
