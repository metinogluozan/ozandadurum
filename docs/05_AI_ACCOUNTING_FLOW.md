# 05 — AI/OCR/Sesli muhasebe iş akışı (DRAFT)

## Kullanıcı yolculukları

**Fatura OCR**
Belge yükleme → zararlı dosya/format kontrolleri → azami kredi rezervasyonu → OCR + alan çıkarma → deterministik tutar/KDV kontrolü → cari/stok eşleştirme önerileri → taslak önizleme → insan onayı → ERP kayıt servisi → audit.

**Sesli işlem**
Mikrofon → kullanıcı izni → STT → izinli intent/parametre ayrıştırma → tenant/rol doğrulaması → şema doğrulaması → taslak veya salt okunur sorgu → işlem türüne göre onay → deterministik servis.

## Güvenlik ve doğruluk ilkeleri
- Model çıktısı güvenilmeyen veridir; doğrudan SQL, API veya muhasebe kaydına çalıştırılamaz.
- Bütün işlemler whitelist intent ve JSON şema üzerinden yürür.
- AI yalnız önerir; tutar, para birimi, KDV, belge tarihleri, cari ve hesap kodu backend tarafından kontrol edilir.
- Güven skorunun düşük olduğu alan insan tarafından tamamlanır; bilinmeyeni uydurma.
- Aynı kaynak belge hash'i ve belge numarası ile mükerrer kayıt için uyarı/kontrol sağlanır.
- Kullanıcının açık onayı olmadan tutar, stok veya finans kayıtlarını kesinleştirme yoktur.
- Okuma izinleri ve tenant filtreleri iş isteğinin sahibi üzerinden uygulanır, model üzerinden değil.
- Prompt injection içeren fatura metni işlem yetkisi oluşturmaz.
- Resmî e-belge gönderimi ayrı entegrasyon ve kullanıcı yetki kontrolüne tabidir.

## Önerilen durumlar
`Uploaded -> Processing -> NeedsReview -> Approved -> Posted`
Alternatif sonlar: `Rejected`, `Failed`, `Cancelled`. `Posted` sonrasında düzeltme, iş kurallarına uygun ters kayıt/düzeltme akışıyla yapılır.

## Örnek doğrulama kuralları
- Belge satır ara toplamı, indirim, vergi toplamı ve genel toplam mutabık olmalı.
- KDV oranları konfigürasyon + belge tarihi ve ilgili mevzuata göre kontrol edilmeli; yalnız AI yorumuna güvenilmez.
- İşletme tanımlayıcıları, tenant'a ait cari/ürün kimlikleri ile eşleştirilmeli.
- Çoklu para birimi, yuvarlama, istisna ve iade belge tipleri özel test kapsamındadır.

## Kredi tüketimi
Her işlem önce tavan ücret hesaplar; maliyet ölçümü yapılandırılmış (sayfa, saniye, token, işlem tipi) tutulur. Başarı koşulu ücretli çıktının kullanıcıya erişilebilir hâle gelmesidir; hata durumunda ayrıştırılmış politika ile kredi rezervasyonu serbest bırakılır.
