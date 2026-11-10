# kahanit/charts

Helm charts published to Docker Hub under `oci://registry-1.docker.io/kahanit`.

| Folder | Published as | Version |
|---|---|---|
| `redis-cluster/` | `oci://registry-1.docker.io/kahanit/redis-cluster` | 1.0.1 |

The images the charts run are built in
[kahanit/images](https://github.com/kahanit/images).

## redis-cluster

A Redis Cluster chart: Bitnami's `redis-cluster` chart `13.0.5`, the last one
Bitnami released (`github.com/bitnami/charts`, Apache-2.0, no template change
since August 2025), running images built under `kahanit/`.

The folder is the unpacked `1.0.1` package (published 2026-01-16), with the
`common` library chart in `charts/common/` as packaged.

Compared with Bitnami's chart on 2026-11-10: all 22 files in `templates/` are
identical. The differences are:

- `Chart.yaml`: `version` 1.0.1, `appVersion` 8.4.0, the three images under
  `kahanit/`, and the `common` dependency from
  `oci://registry-1.docker.io/kahanit` pinned at 2.31.7
- `values.yaml`: the image repositories and tags (`kahanit/redis-cluster`,
  `kahanit/redis-exporter`, `kahanit/os-shell`)
- `charts/common`: 2.31.7 (Bitnami's chart shipped 2.31.4); its `_labels.tpl`
  replaces `+` with `_` in the version label
- README wording

The chart's default image tags (`8.4.0-r1`, `1.80.1`, `12`) are older than
the images built in `kahanit/images`; an installation sets the current tags
in its own values:

```yaml
image:
  tag: 8.10.3-r1
metrics:
  image:
    tag: 1.97.0
```

## Releasing a chart

1. Edit the chart and raise `version` in its `Chart.yaml`.
2. `helm lint redis-cluster`
3. `helm package redis-cluster`
4. `helm push redis-cluster-<version>.tgz oci://registry-1.docker.io/kahanit`
5. Update the version in the table above.
