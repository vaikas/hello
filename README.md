# hello
Hello World in go

# Different SBOMs

So, in the testdata directory there are 4 different SBOMs demonstrating
different SBOMs for it.

## Produced by ko

The first one [produced by ko](./testdata/hello-sbom.json) is produced by ko,
by running this from the root of this repo:

```shell
export KO_DOCKER_REPO=registry.local:5001/chainguard-local
ko publish
```

For me, it produced the following sha (so grab the SBOM for it produced by ko):
```shell
cosign download sbom registry.local:5001/chainguard-local/hello-df6843247fa9144cdc6e9692a8059ed9@sha256:3048c694c096dbb15e63a1f8bdf8f0646f894287d569c355ee3e65484a353c68 --allow-insecure-registry  > ./testdata/hello-sbom.json
```

## Produced by syft in SPDX format:

```shell
syft -o spdx-json registry.local:5001/chainguard-local/hello-df6843247fa9144cdc6e9692a8059ed9@sha256:3048c694c096dbb15e63a1f8bdf8f0646f894287d569c355ee3e65484a353c68 > ./testdata/hello-sbom-syft-spdx.json
```

## Produced by syft in CycloneDX format:

```shell
syft -o cyclonedx-json registry.local:5001/chainguard-local/hello-df6843247fa9144cdc6e9692a8059ed9@sha256:3048c694c096dbb15e63a1f8bdf8f0646f894287d569c355ee3e65484a353c68 > ./testdata/hello-sbom-syft-cdx.json
```

### Inspecting them.

#### ko packages:

```shell
vaikas@vaikas-MBP hello % cat ./testdata/hello-sbom.json | jq '.packages[].name'
"sha256:3048c694c096dbb15e63a1f8bdf8f0646f894287d569c355ee3e65484a353c68"
"distroless.dev/static@sha256:9efcaa8163c0e35e1f13e98582beaf07ab434678e90408624cf32315f4ae940e"
"github.com/puerco/hello"
"github.com/sirupsen/logrus"
"golang.org/x/sys"
```

#### syft spdx packages:

```shell
vaikas@vaikas-MBP hello % cat ./testdata/hello-sbom-syft-spdx.json| jq '.packages[].name'
"alpine-baselayout-data"
"alpine-keys"
"alpine-release"
"ca-certificates-bundle"
"github.com/puerco/hello"
"github.com/sirupsen/logrus"
"golang.org/x/sys"
"tzdata"
```

#### syft cyclonedx packages:

```shell
vaikas@vaikas-MBP hello % cat ./testdata/hello-sbom-syft-cdx.json | jq '.components[].name'
"alpine-baselayout-data"
"alpine-keys"
"alpine-release"
"ca-certificates-bundle"
"github.com/puerco/hello"
"github.com/sirupsen/logrus"
"golang.org/x/sys"
"tzdata"
"alpine"
```

#### Hand crafted cyclonedx:

```shell
vaikas@vaikas-MBP hello % cat ./testdata/bom.json | jq '.components[].name'
"github.com/sirupsen/logrus"
"golang.org/x/sys"
"go.mod"
"go.sum"
"LICENSE"
"main.go"
"README.md"
```
