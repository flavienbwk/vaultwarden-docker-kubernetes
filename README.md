# Vaultwarden Docker Kubernetes

Docker compose and Kubernetes implementations of [Vaultwarden](https://github.com/dani-garcia/vaultwarden) for a free premium experience of one of the best credentials manager : [Bitwarden](https://bitwarden.com/).

Free 🤑 :

- ✔️ Organizations support
- ✔️ Attachments
- ✔️ Vault API support
- ✔️ Serving the static files for Vault interface
- ✔️ Website icons API
- ✔️ Authenticator and U2F support
- ✔️ YubiKey and Duo support

## Usage for docker-compose

1. Copy and edit `.env` file variables

    ```bash
    cp .env.evalifai.example .env
    ```

2. Run the project

    ```bash
    docker-compose up -d
    ```

    The web UI will be available on port `8080` (HTTP)

## Usage for Kubernetes (Helm)

It is expected from you to have a K8S instance configured and ready to be accessed by your `kubectl` CLI. You must have `helm` installed too.

I've used [Scaleway Kapsule](https://www.scaleway.com/en/kubernetes-kapsule) to perform my tests. This is an easy way to have a Kubernetes cluster quickly ready.

1. Copy and edit `./kubernetes/values.yaml` file variables

    ```bash
    cp ./kubernetes/values.example.yaml ./kubernetes/values.yaml
    ```

2. Configure secrets

    I recommend to copy the following commands in a text editor, replace values and execute it.

    ```bash
    NAMESPACE="vaultwarden"
    kubectl create ns "${NAMESPACE}"

    # SMTP credentials
    SMTP_USERNAME="example@domain.com"
    SMTP_PASSWORD="XXXXXXXXXXXXXX"
    kubectl create secret -n "${NAMESPACE}" generic globalsettings-smtp \
      --from-literal=SMTP_USERNAME="$SMTP_USERNAME" \
      --from-literal=SMTP_PASSWORD="$SMTP_PASSWORD"
    ```

3. Deploy chart

    ```bash
    NAMESPACE="vaultwarden"
    helm install vaultwarden ./kubernetes \
      -f ./kubernetes/values.yaml \
      -n "${NAMESPACE}"
    ```
