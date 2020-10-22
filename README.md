## Builderイメージ作成
cd images
docker build -t distroless:test -f Dockerfile.parent --build-arg stack_id=shinofarastack .
docker build --build-arg "from_image=distroless:test" -t run -f Dockerfile.run .
docker build --build-arg "from_image=distroless:test" -t builder -f Dockerfile.build .


## cnb作成

pack package-buildpack python.cnb --config ./python/package.toml --format file
pack package-buildpack golang.cnb --config ./golang/package.toml --format file


## builder作成
pack create-builder shinofara:org --config ./builder.toml

## build app
pack build webapp --builder shinofara:org

参考
https://github.com/GoogleCloudPlatform/buildpacks/blob/main/builders/gcp/base/stack/build.sh
