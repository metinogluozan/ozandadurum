# LOCK POLICY — Faz kapıları ve karar yönetimi

## Durumlar
- `DRAFT`: açık taslak, değişebilir.
- `REVIEW`: teknik ve ticari inceleme.
- `LOCKED`: sahibi tarafından onaylanmış karar; uygulama bu sürüme göre yapılır.
- `SUPERSEDED`: yeni bir ADR ile değiştirilmiştir.

## Kurallar
1. Bugünkü tüm teknik belgeler `DRAFT` kabul edilir; hiçbir faz kapanmış değildir.
2. `LOCKED` karar değişecekse gerekçe, etki (veri/API/ödeme/müşteri), migration, test ve rollback yazılır.
3. Onay sürecinin kaynağı GitHub issue/PR veya açıkça belirtilmiş karar günlüğüdür.
4. Ödeme, kredi muhasebesi ve müşteri veri izolasyonu kararları test ve gözden geçirme olmadan değiştirilemez.
5. Gate geçmeden bir sonraki fazın prod deploy'u yapılamaz.
6. İstisna için yetkili onayı, sınırlandırılmış süre ve geri alma planı gerekir.

## Başlangıç kilitleri
- Ücretsiz çekirdek temel muhasebe/ERP akışını kredi bakiyesi sıfırken de çalıştırır.
- Satın alınan krediler süreyle sıfırlanmaz.
- Finansal kayıt AI çıktısıyla kullanıcı onaysız kesinleştirilemez.
- Ödeme doğrulaması yalnız backend üzerinden yapılır, tekil `grant` idempotent olmalıdır.

Bu dört madde ürün ilkesi olarak ele alınır; teknik detayları ise Phase 0'da hâlâ taslaktır.
