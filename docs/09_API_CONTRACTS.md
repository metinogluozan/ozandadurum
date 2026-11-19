# 09 — API sözleşmeleri (öneri, DRAFT)

Base: `/api/v1`. Tüm isteklerde kimlik ve tenant yetkisi kontrol edilir. Tenant seçimi sunucunun yetkilendirilmiş üyeliklerinden doğrulanır.

| Method | Path | Amaç |
| --- | --- | --- |
| GET | /wallet | Kullanılabilir/rezerve kredi bakiyesi |
| GET | /wallet/ledger | Sayfalanmış kredi hareketleri |
| GET | /credit-packages | Güncel paketler ve fiyat versiyonu |
| POST | /checkout/orders | Ödeme siparişi + provider checkout |
| GET | /checkout/orders/{id} | Backend doğrulamalı durum |
| POST | /payment-callbacks/{provider} | Sağlayıcı webhook'u (public ingress, **imza doğrulamalı**) |
| POST | /ai/jobs | İdempotent AI job + tavan rezervasyon |
| GET | /ai/jobs/{id} | İşin durumu, tüketim, taslak ref |
| POST | /drafts/{id}/approve | İş kuralı ve yetki kontrolüyle kesinleştirme |
| POST | /drafts/{id}/reject | Taslak reddi |
| GET | /usage | Zaman aralıklı kullanım raporu |

## Örnek AI isteği
```json
{
  "operation": "invoice_ocr",
  "documentId": "doc_uuid",
  "idempotencyKey": "generated-uuid",
  "maxCreditsAccepted": 5
}
```
Yanıt: `202 Accepted` + `jobId, pricingVersion, reservedCredits, status`. Sunucunun fiyat hesabı istemcinin gönderdiği değerden bağımsızdır.

## Örnek hata gövdesi
```json
{
  "code": "INSUFFICIENT_CREDITS",
  "message": "İşlem için yeterli kredi yok.",
  "correlationId": "request-uuid"
}
```

## API güvenlik kuralları
- `Idempotency-Key` gerektiren mutasyonlarda benzersizlik tenant + route + key üzerinde.
- Rate limit, izin kontrolü, payload ve MIME doğrulama, dosya boyutu limitleri.
- Finansal mutasyonlar ETag/sürüm veya transaction ile yarış durumlarını yönetir.
- Webhook endpoint JWT gerektirmez; sağlayıcı doğrulama mekanizması ve replay kontrolü gerektirir.
- AI input metni veri olarak işlenir; SQL veya yüksek yetkili komuta dönüşmez.

## Açık konular
Paket para birimi, iade API'si, ses yükleme şekli, job sonucu erişim süresi, fatura kayıt formatı ve toplu belge işleme kotası ADR ile belirlenecek.
