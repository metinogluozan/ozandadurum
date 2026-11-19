# 10 — Test planı (DRAFT)

## Otomasyon seviyeleri
- Unit: fiyatlandırma, kredi aritmetiği, vergi/tutar kontrolleri.
- Integration: PostgreSQL transaction, RLS, webhook adapter, iş kuyruğu dedupe.
- E2E: signup → paket → sandbox ödeme → kredi → OCR → onay → ERP taslak kaydı.
- Security: tenant isolation, IDOR, injection, file upload, privilege escalation, secret leakage.
- Load/Chaos: paralel kullanım, queue tekrar teslimatı, ödeme webhook fırtınası, restart/failover.

## Başlangıç kabul matrisi
| Test | Beklenen |
| --- | --- |
| Kredi sıfır, manuel cari kaydı | Başarılı |
| Kredi sıfır, ücretli OCR | Kontrollü yetersiz bakiye |
| Aynı ödeme callback'i 100 kez | Tek kredi grant |
| Aynı idempotency key ile checkout tekrarı | Tek mantıksal sipariş |
| Aynı AI job iki kez settle | Tek tüketim |
| 100 paralel rezervasyon | Negatif bakiye yok |
| Worker iş ortasında düşer | Reservasyon doğru uzlaştırılır |
| OCR sonucu hatalı KDV | Onaysız kayıt yok, uyarı |
| Farklı tenant belge URL erişimi | 403/404, veri sızıntısı yok |
| Model tarafından sahte komut | Whitelist dışı işlem yok |
| Callback tutarı farklı | Grant yok, alarm |
| İade/chargeback | Politika uyarınca ters hareket, audit korunur |
| Yedekten geri dönüş | Ledger ve bakiye tutarlılığı doğrulanır |
| Sağlayıcı kesintisi | Başarısızlık güvenle yönetilir |

## Kanıt ve gate
Her testin kodu, çalıştırma komutu, ortamı, tarih/saat, commit SHA ve PASS/FAIL sonucu `EXECUTION_LOG.md`'ye girilir. Doküman testleri **henüz çalıştırılmış değildir**.
