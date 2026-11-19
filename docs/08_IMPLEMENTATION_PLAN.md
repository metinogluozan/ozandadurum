# 08 — Faz planı ve kapı kriterleri

## Phase 0 — Discovery / kilit kararlar
- Hedef müşteri ve ücretsiz/ücretli ürün sınırlarını netleştir.
- Gerçek banka/ödeme sağlayıcısı sözleşmesini ve sandbox'ını doğrula.
- OCR/STT/LLM sağlayıcılarını gizlilik, fiyat, doğruluk bazında test et.
- Wallet muhasebesi, kredi satışı, iadeler ve ilgili mevzuatı incele.
- Mimari ADR, tehdit modeli, veri sözlüğü ve test kriterlerini kilitle.
**Gate-0:** belgeler onaylanmadan uygulama kodu prod'a yaklaşmaz.

## Phase 1 — Kimlik + ücretsiz ERP çekirdeği
- Tenant/user/role + temel cari/stok tabloları.
- Manuel veri giriş ekranları, audit ve tenant RLS.
**Gate-1:** cross-tenant entegrasyon testi ve temel veri doğruluğu geçer.

## Phase 2 — Kredi motoru
- Wallet, append-only ledger, rezervasyon/settlement/release.
- Versiyonlu fiyat kataloğu ve limitler.
**Gate-2:** concurrency, idempotency, negatif bakiye ve crash recovery testleri geçer.

## Phase 3 — AI belge ve sesli taslak
- OCR + STT/LLM adaptörleri, durable queue, maliyet telemetrisi.
- Taslak önizleme, onay, deterministik ERP kayıt servisi.
**Gate-3:** modelin doğrudan kayıt yazamadığı doğrulanır; örnek veri doğruluk testleri geçer.

## Phase 4 — Banka sanal POS
- Hostlu checkout, webhook validation, status query, otomatik kredi grant.
- İade/chargeback ve reconciliation.
**Gate-4:** replay/double-pay/retry/failover testleri geçer.

## Phase 5 — Pilot ve üretim
- 10–30 pilot tenant ile izleme, yük testleri ve gerçek birim ekonomi analizi.
- Backup/restore, DAST/pentest, operasyonel runbook ve müşteri desteği.
**Gate-5:** kritik açık ve mali kayıt tutarsızlığı sıfır; canlıya çıkış kararı yazılı.

## Her faz için teslim standardı
- Kod + otomatik test + migration + operasyon notu + değişiklik günlüğü.
- `EXECUTION_LOG.md`'ye gerçek test sonucu/SHA eklenir.
- Başarısız gate kapatılmaz; geçilmemiş çalışma tamamlandı diye sunulmaz.
