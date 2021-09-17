## Usage

[Helm](https://helm.sh) must be installed to use the charts. Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```
helm repo add akash https://ovrclk.github.io/helm-charts
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages. You can then run `helm search repo akash` to see the charts.

### Example

To install the provider chart make sure you have the following setup:

- Place a [provider-cert.pem](https://docs.akash.network/operations/provider#creating-the-provider-on-the-blockchain) in the current directory
- Place your private key in a file named `key.pem` in the current directory
- Set the `AKASH_KEY_NAME` and `AKASH_PASSWORD` env vars
- Install the [Akash network CRD](https://github.com/ovrclk/akash/blob/master/pkg/apis/akash.network/v1/crd.yaml)
- Install the [Provider host CRD](https://github.com/ovrclk/akash/blob/troian/storage/pkg/apis/akash.network/v1/provider_hosts_crd.yaml)

```
export AKASH_KEY_NAME=<mykeyname>

export AKASH_PASSWORD=<mykeypassword>

export AKASH_ACCOUNT_ADDRESS="$(akash keys show $AKASH_KEY_NAME -a)"

helm install provider akash/provider --set akash_client.from=$AKASH_ACCOUNT_ADDRESS --set akash_client.keysecret="$(echo $AKASH_PASSWORD | base64)" --set akash_client.key="$(cat ./key.pem | base64)" --set akash_provider.providercert="$(cat ./provider-cert.pem | base64)"
```

To uninstall the chart:

```
helm delete provider
```
