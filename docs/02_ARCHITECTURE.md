# 02 — Sistem mimarisi (öneri, DRAFT)

## Mantıksal bileşenler

```text
React Web / Flutter (sonraki faz)
      |
API Gateway / ASP.NET Core Web API
      +-- Identity & Tenant Authorization
      +-- ERP Core (Cari, Stok, Belgeler, Finans)
      +-- Pricing Catalog (versiyonlu)
      +-- Wallet + Append-only Credit Ledger
      +-- Payment Orchestrator -> Provider Adapter -> Sanal POS
      +-- AI Job API -> Durable Queue -> OCR/STT/LLM Worker
      +-- Draft Validation + Explicit Approval
      +-- Reporting / Audit / Observability
      |
PostgreSQL + şifreli nesne depolama
```

## Teknoloji kararı (ADR ile onaylanacak)
- Backend: C# / .NET 8 ASP.NET Core
- Web: React + TypeScript; mobil için Flutter opsiyonu
- PostgreSQL, transaction + gerekli indekse sahip tenant_id alanları
- Uygulama yetkilendirmesine ek PostgreSQL RLS, connection context sızıntısı testleri
- İş kuyruğu: at-least-once delivery kabul edilerek **idempotent** worker; ör. Postgres-backed queue veya ayrı broker (yük testi sonrası)
- OCR, speech-to-text ve LLM: provider adapter arayüzü; maliyet ve veri işleme koşulları ölçülür
- Obje depolama: belgeyi en az ayrı erişim kontrolü/şifreleme ile tut; TTL ve imha kuralları

## Sınırlar
- `Tenant` tüm iş verilerinin sahiplik sınırıdır; kullanıcı izni tenant + rol + kapsam birleşimidir.
- `Payment` dış sağlayıcıya; `Wallet` iç finansal hak defterine aittir. İkisi ayrıdır.
- `AIJob` bir kullanım rezervasyonu ile bağlıdır; queue teslimatı faturalama kanıtı değildir.
- ERP kayıtlarının nihai yazımı deterministik servis içinde ve kullanıcı onayı sonrasında gerçekleşir.

## Tutarlılık ilkeleri
- Para birimi tutarlarını decimal kullan; float kullanılmaz.
- Kredi bakiyesi için tek veritabanı transaction'ı, kilitleme veya güvenli koşullu update kullan.
- Dış servisi SQL transaction içinde çağırma.
- Kalıcı iş olayı için outbox + deduplication uygula.
- `correlation_id`, `tenant_id`, `actor_id` bütün loglarda bulunur; PII içeren payload loglara yazılmaz.
- Migrations sürümlenir; ileriye uyumlu dağıtım, backup ve geri dönüş planı gerekir.

## Ölçek
1.000 / 10.000 / 100.000 tenant senaryoları için ayrı performans/maliyet testi gerekir. Kapasite, tenant sayısından çok eşzamanlı aktif kullanıcı, belge hacmi, AI iş süresi ve saklanan belge boyutuyla belirlenir. Gerçek benchmark olmadan kapasite garantisi verilmez.

## Mimari karar kaydı gerektiren konular
Veritabanı RLS stratejisi, iş kuyruğu, AI sağlayıcıları, belge saklama süresi, veri bölgesi, ödeme sağlayıcısı ve kesin kredi tarifesi.
