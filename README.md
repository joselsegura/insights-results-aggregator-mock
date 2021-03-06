# insights-results-aggregator-moc
Mock service mimicking Insights Results Aggregator

## Howto build the service

```
make build
```

## Howto start the service

```
make run
```

## Accessing results

Settings for localhost:

```
ADDRESS=localhost:8080/api/v1
```

Basic endpoints:

```
curl -k -v $ADDRESS/
curl -k -v $ADDRESS/groups
curl -k -v $ADDRESS/organizations
```

Clusters per organization:

```
curl -k -v $ADDRESS/organizations/11789772/clusters
curl -k -v $ADDRESS/organizations/11940171/clusters
```

Report for organization + cluster:

```
curl -k -v $ADDRESS/report/34c3ecc5-624a-49a5-bab8-4fdc5e51a266
```

Report for cluster:

```
curl -k -v $ADDRESS/report/34c3ecc5-624a-49a5-bab8-4fdc5e51a266
```

## List of cluster IDs that can be accesses by this service

### Clusters that return 'static' rule results

```
34c3ecc5-624a-49a5-bab8-4fdc5e51a266
74ae54aa-6577-4e80-85e7-697cb646ff37
a7467445-8d6a-43cc-b82c-7007664bdf69
ee7d2bf4-8933-4a3a-8634-3328fe806e08
```

### Cluster that returns no results (ie just empty report)

```
eeeeeeee-eeee-eeee-eeee-000000000001
```

**Mnemotechnic**: `e` means "empty"

### Clusters that return rules that change every 15 minutes

```
Cluster ID                            Returns results that are similar to:

cccccccc-cccc-cccc-cccc-000000000001  34c3ecc5-624a-49a5-bab8-4fdc5e51a266
                                      74ae54aa-6577-4e80-85e7-697cb646ff37
                                      a7467445-8d6a-43cc-b82c-7007664bdf69
cccccccc-cccc-cccc-cccc-000000000002  74ae54aa-6577-4e80-85e7-697cb646ff37
                                      a7467445-8d6a-43cc-b82c-7007664bdf69
                                      ee7d2bf4-8933-4a3a-8634-3328fe806e08
cccccccc-cccc-cccc-cccc-000000000003  ee7d2bf4-8933-4a3a-8634-3328fe806e08
                                      ee7d2bf4-8933-4a3a-8634-3328fe806e08
                                      34c3ecc5-624a-49a5-bab8-4fdc5e51a266
cccccccc-cccc-cccc-cccc-000000000004  eeeeeeee-eeee-eeee-eeee-000000000001
                                      eeeeeeee-eeee-eeee-eeee-000000000001
                                      34c3ecc5-624a-49a5-bab8-4fdc5e51a266
```

**Mnemotechnic**: `c` means "changing"

### List of clusters that return improper results and/or failure

```
ffffffff-ffff-ffff-ffff-000000000xxx'
```

Returns HTTP code xxx taken directly from the last three digits of cluster ID.
It means that devels/testers could use this functionality to check the
behaviour on client side.

**Mnemotechnic**: `f` means "failure"

Example:

```
ADDRESS=localhost:8080/api/v1

clusters="ffffffff-ffff-ffff-ffff-000000000200
ffffffff-ffff-ffff-ffff-000000000201
ffffffff-ffff-ffff-ffff-000000000404
ffffffff-ffff-ffff-ffff-000000000405
ffffffff-ffff-ffff-ffff-000000000201"

for cluster in $clusters
do
    curl -k -v $ADDRESS/report/${cluster}
done
```
