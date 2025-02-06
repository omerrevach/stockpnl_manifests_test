## Make sure to setup terraform first


## Setup
1. **Store Database Credentials in AWS Secrets Manager:**
    ```
    aws secretsmanager create-secret --name db-secret --secret-string '{
    "DB_USER": "trading_user",
    "DB_PASSWORD": "trading_pass",
    "DB_DATABASE": "trading_db",
    "ROOT_PASSWORD": "securepassword"
    }' --region us-east-1
    ```

2. **Create ClusterSecretStore to Allow Kubernetes to Read AWS Secrets:**
    ```
    📂 stockpnl_manifests/external-secrets/cluster-secret-store.yaml

    apiVersion: external-secrets.io/v1beta1
    kind: ClusterSecretStore
    metadata:
      name: global-secret-store
    spec:
      provider:
        aws:
          service: SecretsManager
          region: eu-north-1
          auth:
            jwt:
              serviceAccountRef:
                name: external-secrets-sa
                namespace: external-secrets
    ```

    ```
    kubectl apply -f stockpnl_manifests/external-secrets/cluster-secret-store.yaml
    ```

3. **Sync AWS Secrets into Kubernetes:**
    ```
    📂 stockpnl_manifests/external-secrets/db-external-secret.yaml

    apiVersion: external-secrets.io/v1beta1
    kind: ExternalSecret
    metadata:
      name: db-secret
      namespace: default
    spec:
      refreshInterval: 1m
      secretStoreRef:
        name: global-secret-store
        kind: ClusterSecretStore
      target:
        name: db-secret
        creationPolicy: Owner
      dataFrom:
      - extract:
        key: db-secret
    ```

    ```
    kubectl apply -f stockpnl_manifests/external-secrets/db-external-secret.yaml
    ```