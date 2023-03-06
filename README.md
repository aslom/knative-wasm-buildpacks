# knative-wasm-buildpacks

## Build build and run stacks

```
cd tinygo-wasi-wagi-stack
docker build . -t aslom/tinygo-wasi-wagi-stack-run:v1
docker build . -t aslom/tinygo-wasi-wagi-stack-build:v1
docker push aslom/tinygo-wasi-wagi-stack-run:v1
docker push aslom/tinygo-wasi-wagi-stack-build:v1
```

## Build builder and buildpack 

Required: install pack from buildpacks.io

```
cd ..

pack builder create docker.io/aslom/tinygo-wasi-wagi-builder  --config  ./knative-wasm-buildpacks/tinygo-wasi-wagi/builder.toml --publish

docker push docker.io/aslom/tinygo-wasi-wagi-builder

pack buildpack package docker.io/aslom/tinygo-wasi-wagi-buildpack --config ./knative-wasm-buildpacks/tinygo-wasi-wagi/package.toml 

docker push docker.io/aslom/tinygo-wasi-wagi-buildpack
```

Test buildpack:

```
pack build --clear-cache -v aslom/knative-wasm-tinygo-wasi-wagi --path ./func-wasm/tinygo/http-wagi  --buildpack docker.io/aslom/tinygo-wasi-wagi-buildpack --builder docker.io/aslom/tinygo-wasi-wagi-builder
```

Test builpack dev mode:

```
pack build --clear-cache -v aslom/knative-wasm-tinygo-wasi-wagi --path ./func-wasm/tinygo/http-wagi  --buildpack ./knative-wasm-buildpacks/tinygo-wasi-wagi --builder docker.io/aslom/tinygo-wasi-wagi-builder
```
