# 03 — Kredi cüzdanı, rezervasyon ve muhasebeleştirme (DRAFT)

## Değişmez kurallar
1. Satın alınan krediler zamanla silinmez/sıfırlanmaz.
2. Satın alınan ve deneme/promosyon kredileri **ayrı bakiyelerde** izlenir.
3. Kredi defteri append-only'dir; bakiyeyi geçmiş kayıt silerek düzeltme yoktur.
4. Her hak ediş ve harcamanın benzersiz iş anahtarı vardır.
5. Başarısız AI işinde rezervasyon bırakılır; tamamlanan işin tüketimi **en fazla bir kez** kesinleşir.
6. Kredi yetersizken AI job başlatılmaz (ücretsiz ERP etkilenmez).
7. Kullanıcıya tarifeye göre hesaplanan maksimum tutar harcamadan önce gösterilir.

## Önerilen tablolar
- `wallets(id, tenant_id, currency='CREDIT', purchased_available, promo_available, purchased_reserved, promo_reserved, row_version)`
- `credit_ledger(id, tenant_id, wallet_id, event_type, source_type, source_id, purchased_delta, promo_delta, created_at, metadata_json, correlation_id)`
- `credit_packages(id, version, credits, price_minor, currency_code, active_from, active_to)`
- `credit_purchases(id, tenant_id, package_version, credits, amount_minor, status, payment_order_id)`
- `usage_jobs(id, tenant_id, operation, status, pricing_version, reserved_credits, settled_credits, idempotency_key)`
- `usage_metering(id, job_id, provider, model, units, provider_cost_minor, currency_code, timestamp)`
- `pricing_rules(id, version, operation, formula, min_credits, max_credits, valid_from)`

Şema bir başlangıç taslağıdır; migrations öncesi CHECK, UNIQUE, FK ve tenant bileşik anahtarları tanımlanmalıdır. Satın alınan bakiye negatif olamaz; promo rezervasyonu kaynağıyla birlikte izlenmelidir.

## İşlem durum makinesi
```text
Created -> Reserved -> Running -> Succeeded -> Settled
                 \-> Failed    -> Released
                 \-> Cancelled -> Released
```
Zaman aşımı durumunda job sorgulanır/uzlaştırılır; körlemesine iki kez tahsil edilmez.

## Ödeme ile kredi kazanımı
- `grant_key = provider + ':' + provider_payment_id + ':credit_grant'` benzersizdir.
- Doğrulanmış başarı durumu ve beklenen sipariş tutarı/para birimi kontrol edilmeden `GRANT` olmaz.
- `GRANT` ledger kaydı ve wallet artışı **aynı transaction** içinde yapılır.
- Çift callback veya yeniden oynatma unique constraint tarafından engellenir.
- Chargeback/iade durumları negatif bakiye/borç politikasıyla ayrıca ele alınır; geçmiş ledger silinmez.

## Tüketim işlemi (önerilen)
1. Yetki, tenant, fiyat versiyonu, kullanıcı kota/limit ve idempotency kontrolü.
2. Transaction içinde azami kredi rezervasyonu + ledger `RESERVE`.
3. Worker sağlayıcıyı çağırır; ham maliyet ve tüketim birimleri kaydedilir.
4. Başarılı işte `SETTLE`, varsa fark için `RELEASE`; hatada sadece `RELEASE`.
5. Her geçiş retry'a güvenli, tekil iş anahtarlı ve denetlenebilir olur.

## Eşzamanlılık testleri
- Aynı cüzdandan 100 paralel rezervasyon negatif bakiye oluşturamaz.
- Aynı job 10 kez sonuç döndürse tek settlement olur.
- Aynı ödeme 100 kez gönderilse tek grant oluşur.
- Transaction ortasında servis düşerse kredi defteri ve bakiye ayrışmaz.
