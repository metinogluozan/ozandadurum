# 06 — Güvenlik, KVKK ve uyum (DRAFT)

## Yetkilendirme ve veri ayrımı
- Her kaynak sorgusu tenant + actor + role ile yetkilendirilir; yalnız JWT içindeki tenant id'sine kör güvenilmez.
- PostgreSQL RLS ve uygulama yetkileri birlikte kullanılır; cross-tenant testleri zorunludur.
- Admin/support erişimi ayrı yetki, gerekçe, kayıt ve süre sınırı ile denetlenir.
- Hassas işlemlerde MFA/TOTP; token rotation, oturum/cihaz yönetimi ve brute-force koruması.

## Veri koruma
- TLS ve at-rest şifreleme; KMS/secret manager, güvenli backup.
- Belgeler güvenli obje depolamada; kısa ömürlü erişim URL'leri.
- PII, vergi numarası, kart bilgisi ve banka detayları loglardan ve gereksiz AI promptlarından çıkarılır.
- AI sağlayıcısına veri aktarımı için sözleşme, veri işleme bölgesi, saklama ve eğitim amaçlı kullanım koşulları doğrulanır.
- KVKK aydınlatma, veri işleme envanteri, saklama/imha, ilgili kişi talepleri ve gerekirse yurt dışı aktarım hukuki incelemeden geçer.
- Veri ihlali ve olay müdahale prosedürü, yedek geri yükleme testi.

## Finansal güvenlik
- Webhook MAC/imza doğrulama ve tekrar oynatma engeli.
- Kart verileri uygulamaya girmemeli; ödeme sağlayıcısı hostlu checkout tercih edilmeli.
- Operasyonel audit append-only; yönetici kredi düzeltmeleri çift kontrol ve gerekçeli ledger olayı.
- Fatura/fiş numaraları ve veri düzeltme mevzuata uygun tasarlanmalı; resmî uyum varsayılmamalı.

## Kalite kapıları
SAST, dependency/license tarama, secret scanning, DAST, penetrasyon testi; tenant izolasyonu, IDOR, ödeme ve kredi concurrency testleri. Kritik ve yüksek önem düzeyinde açıklar kapanmadan canlıya çıkış yok.

> Hukuki, vergi ve e-belge uyumu uygulama geliştiricinin tek başına beyanıyla sağlanmış sayılmaz; güncel mevzuat ve uzman değerlendirmesi gereklidir.
