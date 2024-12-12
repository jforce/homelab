
# Bucket Secret Creation

```bash
ACCESS_KEY_ID=$(kubectl get -n openshift-logging secret loki-bucket-odf -o jsonpath='{.data.AWS_ACCESS_KEY_ID}' | base64 -d)
SECRET_ACCESS_KEY=$(kubectl get -n openshift-logging secret loki-bucket-odf -o jsonpath='{.data.AWS_SECRET_ACCESS_KEY}' | base64 -d)
```

```bash
oc create -n openshift-logging secret generic logging-loki-s3 \
--from-literal=access_key_id="${ACCESS_KEY_ID}" \
--from-literal=access_key_secret="${SECRET_ACCESS_KEY}" \
--from-literal=bucketnames="loki-bucket-odf" \
--from-literal=endpoint="https://s3.openshift-storage.svc:443"

```
