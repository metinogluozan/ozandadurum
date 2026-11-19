# 04 — Ödeme ve otomatik kredi yükleme (DRAFT)

## Hedef
Müşteri paket seçer → banka/sanal POS'ta öder → sağlayıcı sunucuda doğrulanır → kredi otomatik yüklenir. Kullanıcının manuel onayı veya personelin kredi tanımlaması gerekmez.

## Sağlayıcıdan bağımsız arayüz
```csharp
public interface IPaymentProvider
{
    Task<CheckoutSession> CreateCheckoutAsync(PaymentOrder order, CancellationToken ct);
    Task<VerifiedPaymentEvent> VerifyNotificationAsync(
        string rawBody, IReadOnlyDictionary<string,string> headers, CancellationToken ct);
    Task<PaymentStatus> GetPaymentStatusAsync(string providerPaymentId, CancellationToken ct);
}
```

**Not:** Bu arayüz öneridir. QNBPay, Yapı Kredi veya başka sağlayıcının gerçek API isimleri/imza ve dönüş alanları olduğu iddia edilmez; resmî sandbox dokümanına göre adapter yazılmalıdır.

## Ödeme siparişi
`payment_orders(id, tenant_id, customer_id, package_id, package_version, amount_minor, currency_code, status, provider, merchant_order_ref, created_at, expires_at)`

`payment_events(id, provider, event_id, provider_payment_id, payload_hash, verification_status, received_at)`

## Durumlar
```text
Created -> Pending -> VerifiedPaid -> Credited
                  \-> Failed
                  \-> Expired
VerifiedPaid/Credited -> Refunded / Chargeback (ayrı dengeleme iş akışı)
```

## Kritik işlem akışı
1. Paket fiyatını backend hesaplar; istemciden gelen tutar güvenilmezdir.
2. Sipariş numarası sunucuda eşsiz üretilir; tarayıcı sağlayıcının ödeme ekranına yönlendirilir.
3. Sağlayıcı bildirimi signature/MAC veya sağlayıcının resmî mekanizmasıyla doğrulanır.
4. Şüpheli/eksik bildirimde ödeme durumu sağlayıcının sunucu API'sinden tekrar sorulur.
5. Tutar, para birimi, sipariş/tenant eşleştirmesi ve başarılı nihai durum doğrulanır.
6. Tek DB transaction içinde `payment_order` durumu, tekil kredi `GRANT` ve `ledger` yazılır.
7. Transaction sonrası outbox ile müşteri bildirimi yayınlanır.
8. Günlük reconciliation işi sağlayıcı hareketleri, siparişler ve ledger toplamını karşılaştırır.

## Sakıncalı uygulamalar
- Tarayıcının success URL parametresine bakarak kredi yüklemek.
- Sadece callback alındı diye imzasız işlem kabul etmek.
- Tam kart numarası, CVV, 3D Secure kimlik bilgisi saklamak.
- İade ve chargeback'i geçmiş kayıt silerek düzeltmek.
- Webhook retry'ında iki kez bakiye yüklemek.

## Canlıya çıkış kontrolü
- Gerçek sağlayıcı sözleşmesi, sandbox, teknik doküman ve merchant konfigürasyonu.
- İmza doğrulama, replay koruması, saat toleransı ve webhook dedupe.
- Tutar uyumsuzluğu, callback sırası değişmesi, timeout, geç yanıt, başarısız provizyon testleri.
- İşletme/tüketici mevzuatı, vergi/e-belge ve ön ödemeli hakların hukuki niteliği uzman tarafından doğrulanır.
