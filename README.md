# Durum AI Plan — Ücretsiz ERP + Kullanım Bazlı Yapay Zekâ

**Durum AI Plan**, işletmelerin temel ön muhasebe/ERP işlerini ücretsiz yürütebildiği; fatura OCR, sesli işlem, yapay zekâ destekli eşleştirme ve analiz için **ön ödemeli kredi** tükettiği çok kiracılı (multi-tenant) SaaS ürün taslağıdır.

> **Proje durumu:** Phase 0 — Discovery / Tasarım. Bu depo henüz çalışan uygulama içermez. Belgelerdeki mimari ve fiyatlar taslak/öneridir; tamamlanmış özellik veya kesinleşmiş ticari koşul değildir.

## Kesinleşen iş modeli ilkeleri

- **Temel yazılım ücretsiz:** Cari, stok, sipariş, manuel fatura taslağı, finans hareketleri ve standart raporlar kredi gerektirmez. Makul adil kullanım/güvenlik kotaları ayrıca tanımlanır.
- **AI kullanımı ücretli:** OCR, sesli komut, AI mutabakat ve gelişmiş analizler kredi tüketir.
- **Satın alınan krediler süreyle sıfırlanmaz:** Müşteri 1 hafta veya aylar içinde kullanabilir; deneme/promosyon kredileri ayrı tutulur.
- **Ödeme sonrası otomatik yükleme:** Banka/sanal POS sağlayıcısı sunucuda doğrulanır; aynı ödeme asla iki kez kredi eklemez.
- **Finansal işlemlerde insan onayı:** AI yalnız taslak hazırlar; kalıcı kayıtlar kullanıcı onayı ve deterministik iş kurallarıyla oluşur.
- **Şeffaf ücretlendirme:** İşlem öncesi tahmini ve işlem sonrası kesin kredi kullanımı görünür.
- **Tenant izolasyonu:** Kredi bakiyesi, muhasebe verileri ve erişimler tenant bazında ayrılır.

## Doküman haritası

| Belge | Kapsam |
| --- | --- |
| [01_PRODUCT_SCOPE](docs/01_PRODUCT_SCOPE.md) | Ücretsiz/ücretli sınırı ve MVP |
| [02_ARCHITECTURE](docs/02_ARCHITECTURE.md) | Modüller, veri akışı ve teknoloji önerisi |
| [03_CREDIT_LEDGER](docs/03_CREDIT_LEDGER.md) | Cüzdan, ledger, rezervasyon, idempotency |
| [04_PAYMENT_INTEGRATION](docs/04_PAYMENT_INTEGRATION.md) | Banka sanal POS, webhook ve mutabakat |
| [05_AI_ACCOUNTING_FLOW](docs/05_AI_ACCOUNTING_FLOW.md) | OCR/sesli işlem, onay ve muhasebe güvenliği |
| [06_SECURITY_COMPLIANCE](docs/06_SECURITY_COMPLIANCE.md) | KVKK, güvenlik, denetim ve erişim |
| [07_PRICING_UNIT_ECONOMICS](docs/07_PRICING_UNIT_ECONOMICS.md) | Örnek kredi tarifesi, kârlılık denklemleri |
| [08_IMPLEMENTATION_PLAN](docs/08_IMPLEMENTATION_PLAN.md) | Fazlar ve kabul kapıları |
| [09_API_CONTRACTS](docs/09_API_CONTRACTS.md) | Taslak endpoint ve veri sözleşmeleri |
| [10_TEST_PLAN](docs/10_TEST_PLAN.md) | Kritik uçtan uca test senaryoları |
| [AGENTS.md](AGENTS.md) | Ajan çalışma kuralları |
| [LOCK_POLICY.md](LOCK_POLICY.md) | Faz ve karar kilitleme ilkeleri |
| [EXECUTION_LOG.md](EXECUTION_LOG.md) | Doğrulanabilir ilerleme kaydı |

## Önerilen teknik yığın (onay bekliyor)

- Backend: .NET 8 / ASP.NET Core Web API
- Veritabanı: PostgreSQL, satır düzeyi tenant koruması ve uygulama yetkilendirmesi
- Web: React + TypeScript
- Mobil: Flutter (sonraki faz)
- AI işleri: durable queue + worker; sağlayıcıdan bağımsız OCR/LLM/STT adaptörleri
- Ödeme: banka veya yetkili ödeme kuruluşu adapter'ı; sağlayıcı seçimi gerçek sözleşme ve sandbox doğrulamasından sonra
- Dağıtım: Docker tabanlı ortamlar, ayrı test/production, gizli anahtarlar yalnız secret manager'da

## Geliştirme sırası

1. Phase 0 kararlarının ve ürün sınırlarının netleşmesi.
2. Tenant, yetki ve ücretsiz ERP çekirdeği.
3. Atomik kredi defteri ve kullanım ölçümleme.
4. OCR/sesli taslak oluşturma ve kullanıcı onay akışı.
5. Test ortamında ödeme doğrulama ve otomatik kredi yükleme.
6. Güvenlik, yük, maliyet ve finansal doğruluk testleri.
7. Pilot müşteri ve kontrollü canlıya çıkış.

**Uyarı:** Bu depoya gerçek müşteri verisi, API anahtarı, banka şifresi veya ödeme kartı bilgisi commit edilmez. Özellikle depo herkese açıksa yalnızca genel tasarım ve örnek içerik paylaşılır.
