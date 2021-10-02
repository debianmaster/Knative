# Knative

# Install kn cli
```

wget https://github.com/knative/client/releases/download/v0.26.0/kn-linux-amd64
mv kn-linux-amd64 kn
chmod +x kn
mv kn /usr/local/bin
kn version
```

# Copy your k3s.yaml file content to kn config file

```
vi ~/.kube/knts.com
```

# Install knative serving component

```
kubectl apply -f https://github.com/knative/serving/releases/download/v0.26.0/serving-crds.yaml
kubectl apply -f https://github.com/knative/serving/releases/download/v0.26.0/serving-core.yaml
```

# Install courier network layer

```
kubectl apply -f https://github.com/knative/net-kourier/releases/download/v0.26.0/kourier.yaml
```

# Configure knative serving to use kourier network

```
kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress.class":"kourier.ingress.networking.knative.dev"}}'
```

# Configure sslip.io dns

```
kubectl apply -f https://github.com/knative/serving/releases/download/v0.26.0/serving-default-domain.yaml
```

# If you want to replace with your own dns name edit config-domain configmap

```
kubectl edit cm config-domain -n knative-serving
```

# Create a service

```
kn service create helloworld-go --image gcr.io/knative-samples/helloworld-go
```



