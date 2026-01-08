# Simple PHP8 S2I app

Deploy Simple APP PHP8 into OpenShift Cluster Using Source To Image(S2I) automation.

---

## Login as Developer and Start Deploy Simple app

```bash
oc login -u developer
oc new-project simple-s2i
oc new-app php~https://github.com/OmdaMukhtar/simple-s2i.git
oc get pods
```

---

## Expose Exteranl Access

```bash
oc expose service simple-s2i
```

---

## Trigger ImageStream for new Changes

```bash
oc start-build simple-s2i --follow
```


## Prereuiqeuests
oc new-project s2i-pipeline-demo
oc create secret generic git-ssh-key --from-file=ssh-privatekey=$HOME/.ssh/id_rsa --type=kubernetes.io/ssh-auth
oc label secret git-ssh-key tekton.dev/git-0=github.com

## Important things to re-run the pipeline after delet all image stream you must re-import the base image
oc import-image nodejs:20 \
  --from=registry.access.redhat.com/ubi9/nodejs-20 \
  --confirm \
  -n s2i-pipeline-demo
