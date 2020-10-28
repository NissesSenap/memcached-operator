# Followign operator-sdk getting started

## setup

```bash
mkdir memcached-operator
cd memcached-operator
operator-sdk init --plugins=ansible --domain=example.com

operator-sdk create api --group cache --version v1 --kind Memcached --generate-role
```

## login quay

```bash
export QUAY_ID=nissessenap
export QUAY_PASSWORD='supersecretpassword123'
podman login -u ${QUAY_ID} -p ${QUAY_PASSWORD} quay.io
```

```bash
#Change all Makefile stuff from docker to podman if you don't use docker.

export IMG="quay.io/nissessenap/memcached-operator:v0.0.2"

make docker-build docker-push IMG=quay.io/nissessenap/memcached-operator:v0.0.2

# Or ignore the makefile and just do it manually.
podman build . -t ${IMG}
podman push ${IMG}
```

## Start crd

```bash
make install
make deploy IMG=$IMG
```

The above will create a NS called memcached-operator-system

```bash
oc project memcached-operator-system

# Appl the memcached cr
kubectl apply -f config/samples/cache_v1_memcached.yaml

# look at the logs
kubectl logs -f deployment.apps/memcached-operator-controller-manager -n memcached-operator-system -c manager
```
