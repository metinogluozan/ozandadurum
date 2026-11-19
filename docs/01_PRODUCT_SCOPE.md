# 01 — Ürün kapsamı (DRAFT)

## Hedef
KOBİ'ler için ücretsiz ve günlük kullanılabilir ön muhasebe/ERP + ücretli kullanım bazlı AI otomasyonu. Ön muhasebe ile yasal defter tutma/e-belge hizmetleri aynı kapsamda değerlendirilmez.

## Ücretsiz çekirdek
- Tenant/firma ve kullanıcı yönetimi; rol bazlı erişim.
- Cari/stok/hizmet kartları; temel hareketler ve arama.
- Teklif, sipariş, satış/alış fatura **taslakları**, kasa/banka manuel hareketleri.
- Temel raporlar, CSV/Excel veri dışa aktarma ve işlenebilir veri taşınabilirliği.
- İşletmenin temel kayıtlarını görüntüleme/düzenleme AI kredisine bağlı olmaz.
- Kötüye kullanım, depolama, trafik ve destek için önceden duyurulmuş adil kullanım sınırları olabilir.

## Kredili işler
- Standart fiş/fatura OCR, çok sayfalı belge çözümleme.
- Sesli komuttan yapılandırılmış sorgu veya taslak işlem.
- Cari/stok eşleştirme önerisi, banka mutabakatı önerisi.
- Doğal dille finansal analiz ve rapor özeti.
- Yüksek karmaşıklıktaki belgeler/özel modeller, ölçülebilir tarifeyle.

## Paket kuralları
- Ön ödemeli, kullanım bazlı; 500/2.500/10.000 kredi **yalnız örnek paket boyutlarıdır**.
- **Satın alınan kredi süreye bağlı sıfırlanmaz.** Fiyat ve olası hukuki koşullar satın alma anında açıkça sunulur; yürürlükteki mevzuat ayrıca doğrulanır.
- Ücretsiz promosyon kredisi satın alınan bakiyeden ayrı tutulur; promosyon şartları ayrıca açıklanır.
- Kredi paketinin TL fiyatı, gerçek sağlayıcı ve altyapı maliyeti ölçülmeden kilitlenmez.
- Kullanıcıya ön tahmin, tavan ücret ve fiili tüketim gösterilir; ret/başarısız işlemde rezervasyon çözülür.

## MVP teslim sınırı
1. Tenant + kullanıcı + rol + cari/stok temel ekranları.
2. Satın alınan/promosyon kredi cüzdanı, append-only defter, kullanım kaydı.
3. Tek sayfalık standart fatura OCR → doğrulanmış taslak → açık onay.
4. AI iş kuyruğu, tekrar deneme ve maliyet ölçümü.
5. Ödeme sağlayıcısı sandbox'ında ödeme doğrulama + bir kez bakiye yükleme.
6. Audit log ve temel yönetim/müşteri kullanım paneli.

## İlk sürümde olmayacak
- İnsan onaysız finansal kayıt veya resmî e-belge gönderimi.
- Tam kapsamlı e-Defter/e-Beyanname veya yasal muhasebe hizmeti.
- Tüm banka POS/ERP entegrasyonları; ilk pilotta yalnız doğrulanmış bağlantılar.
- Gerçek zamanlı tüm cihaz/mobil platformları.

## Kabul ölçütleri
- Tenant A kullanıcıları tenant B belgesini, cüzdanını veya logunu göremez.
- Ücretsiz temel işler kredi bakiyesi sıfırken de çalışır.
- Tek ödeme bildirimi 100 kez gelse dahi tek yükleme olur.
- Kullanıcı onaylamadan kesin fatura/muhasebe hareketi oluşmaz.
