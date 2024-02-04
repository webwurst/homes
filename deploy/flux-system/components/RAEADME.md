

## Webhook Receiver

```bash
kubectl -n flux-system get receiver homes -o json | jq -r '.status.webhookPath'
```

On GitHub add 
  Payload URL -> https://flux-webhook.webwur.st/hook/<id from path>
  Secreet -> $ kubectl -n flux-system get secrets homes-webhook-4dk46m97k4 -o json | jq -r '.data.token | @base64d'
