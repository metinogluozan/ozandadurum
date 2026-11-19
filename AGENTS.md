# AGENTS — Mühendislik çalışma kuralları

1. Her değişiklik öncesi `README.md`, `LOCK_POLICY.md`, ilgili `docs/*` ve `EXECUTION_LOG.md` oku.
2. İlgili fazın kapsamı dışında kod yazma; değişiklik talebini önce etki analizi/ADR olarak dokümante et.
3. Belge Türkçe UTF-8; kod tanımlayıcıları ve API sözleşmesi tutarlı İngilizce olabilir.
4. Mimari kararları varsayım gibi sun; doğrulanmayan işi tamamlandı deme.
5. Satın alınan krediler zamana bağlı sıfırlanmaz; ücretsiz çekirdek AI kredisine bağımlı olmaz.
6. Wallet kayıtları append-only, eşzamanlı ve idempotent; ödeme callback'ine kör güvenme.
7. AI finansal kayıtları doğrudan kesinleştiremez; deterministik doğrulama + insan onayı zorunlu.
8. Tenant isolation, gizli anahtar güvenliği ve denetim logları için test olmadan merge yok.
9. Gerçek banka/AI API detaylarını uydurma; resmî teknik dokümana karşı doğrula.
10. Değişiklikten sonra test komutlarını çalıştır, gerçek sonuçları ve SHA'yı `EXECUTION_LOG.md`'ye ekle.
11. Mevcut dosyaları silme, üretim verisini taşıma, secrets yükleme veya prod'u deploy etme: açık yetki ve geri dönüş planı gerektirir.
12. Her faz kapanışında gate testi, security review ve kullanıcı/onay gerektiren kararı kayda geçir.

## Başlangıç iş emri
Phase 0 mimari taslağını gözden geçir → veri modeli ADR → ödeme sağlayıcısı sandbox doğrulama → küçük tenant+wallet iskeleti → testler. Kodlama sırasında commit'leri küçük ve izlenebilir tut.
