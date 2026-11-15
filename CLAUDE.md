# OrderFlow — ci-templates

> Bu dosya her Claude Code oturumunda otomatik okunur. Kalıcı bir kural öğrendiğinde buraya ekle.

## Proje
OrderFlow, Trendyol benzeri bir sipariş akışının açık kaynak demosu. Polyrepo, GitLab'da
`order-flow-group` altında; GitHub sadece ayna. Kararlar ve notlar: `../handbook`.

Bu repo: **ci-templates** — bütün repoların ortak GitLab CI şablonları. Uygulama kodu yok, sadece YAML.
Bir şablondaki değişiklik bütün repoların pipeline'ını etkiler; dikkatli ol.

## Geliştirici hakkında
Junior geliştiriciyim ve öğrenmek için buradayım. Bir CI kavramını (stage, include, extends, rules,
cache, artifact) kullandığında 2–3 cümleyle neden ve alternatifini açıkla.

## Kurallar
- Şablonlar `templates/` altında; her biri tek bir sorumluluk.
- Bir şablon, onu kullanan bir repoda test edilmeden eklenmez (go.yml F0-09'da, docker.yml F0-10'da).
- Image sürümlerini sabitle, `latest` kullanma.
- Job'lar `base.yml`'deki `default.tags` ile `orderflow` runner'ında çalışır; job'a ayrıca tag yazma.
- Docker image build için job'a docker.sock açma kararı F0-10'da verilecek (K-18'e bak).
- Doğrulama: `yamllint .` ve MR'daki pipeline. Pipeline yeşil olmadan merge yok.

## Git ve iş akışı
- Ana platform GitLab; GitLab işleri için `glab` kullan. Issue'lar handbook'ta: `glab ... -R order-flow-group/handbook`.
- Branch: `<issue-no>-<kısa-ad>`. Commit ve MR başlıkları İngilizce, Conventional Commits (`ci:`, `docs:`).
- handbook'taki issue'ya bağlamak için MR açıklamasına `Refs order-flow-group/handbook#N`;
  kapatmak için `Closes order-flow-group/handbook#N`.
- `main` korumalı; MR + yeşil pipeline + squash merge. Force push yok, secret commit yok.

## Bilinen tuzaklar
- Windows'ta Docker'a yol veren komutları PowerShell'de çalıştır (Git Bash yolu bozar).
