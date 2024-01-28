# [Cloudflare](https://cloudflare.com)
- [Cloudflare’s API](https://developers.cloudflare.com/fundamentals/api/)
- [Cloudflare API Token](https://dash.cloudflare.com/profile/api-tokens)

The CronJob requests the current WAN IPv4 from the UniFi Controller. If the IP is new, the [Cloudflare DDNS](https://developers.cloudflare.com/dns/manage-dns-records/how-to/managing-dynamic-ip-addresses/) record will be updated.

## UniFi Controller:
- Create a readonly user

## Cloudflare
- [Create](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/) an API token
- [Get](https://developers.cloudflare.com/api/operations/dns-records-for-a-zone-list-dns-records) `zone_id` and `dns_record_id`
- `Optional`: test [updating DNS Record](https://developers.cloudflare.com/api/operations/dns-records-for-a-zone-update-dns-record)

## env.txt
- Create the env.txt file e.g.
  - ```
    UNIFI_URL=https://unifi
    UNIFI_USER=user
    UNIFI_PASS=read:only
    ZONE_ID=023e105f4ecef8ad9ca31a8372d0c353       <= 32 characters
    DNS_RECORD_ID=023e105f4ecef8ad9ca31a8372d0c353 <= 32 characters
    TOKEN=023e105f4ecef8ad9ca31a8372d0c353
    DOMAIN=my.domain.com
- Create the [sealed secret](https://github.com/bitnami-labs/sealed-secrets)
  - `kubectl create secret generic cloudflare-ddns --dry-run=client --from-env-file=env.txt -oyaml | kubeseal --controller-name sealed-secrets -oyaml > templates/sealed-secret.yaml`

## Helm install/upgrade
`helm upgrade --install cloudflare-ddns .`

## Helm unintall
`helm uninstall cloudflare`

