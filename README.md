# ci-templates

OrderFlow repolarının ortak GitLab CI şablonları. Job'lar burada bir kez yazılır, her repo `include` ile kullanır.

## Kullanım

Bir reponun `.gitlab-ci.yml` dosyasının en başına:

```yaml
include:
  - project: order-flow-group/platform/ci-templates
    ref: main
    file:
      - templates/base.yml          # her zaman ilk sırada
      - templates/secret-scan.yml
```

## Şablonlar

| Dosya | Ne yapar | Stage | Durum |
| --- | --- | --- | --- |
| `templates/base.yml` | Ortak stage sırası; bütün job'lar `orderflow` runner'ında | — | Hazır |
| `templates/secret-scan.yml` | gitleaks ile bütün git geçmişinde gizli bilgi taraması | security | Hazır |
| `templates/spec-lint.yml` | Spectral ile OpenAPI + AsyncAPI lint; uyarı da hata (`$SPEC_OPENAPI_GLOB`, `$SPEC_ASYNCAPI_GLOB`) | lint | Hazır (contracts) |
| `templates/go.yml` | golangci-lint + go test | lint, test | F0-09 |
| `templates/docker.yml` | Image build + Container Registry'ye push | build, publish | F0-10 |

Stage sırası: `lint → test → security → build → publish → deploy`

## Kurallar

- Bir şablon, onu gerçekten kullanan bir repoda test edilmeden eklenmez.
- Image sürümleri sabitlenir (`gitleaks:v8.21.2`, `latest` değil).
- Bu reponun kendi pipeline'ı (`.gitlab-ci.yml`) şablonları kendisi kullanır: yamllint + secret-scan.
- Şimdilik `ref: main` kullanılıyor. Şablonlar oturunca sürüm etiketleri (`v1`) ile sabitlenecek.
