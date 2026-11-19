# 07 — Kredi fiyatlama ve birim ekonomi (DRAFT)

## Örnek işlem fiyatları — satış tarifesi DEĞİL
| İşlem | Örnek kredi |
| --- | ---: |
| Basit sesli sorgu | 1 |
| Sesli fatura taslağı oluşturma | 3 |
| Standart tek sayfa OCR | 5 |
| Çok sayfalı OCR | İlk sayfa 5 + ek sayfa başına 2 |
| Karmaşık/istisnalı belge | 10+ |
| AI finansal analiz | 10 |

Gerçek fiyat **ölçülen maliyet** ve müşterinin aldığı değer üzerinden belirlenir. Kredinin TL karşılığı henüz kararlaştırılmadı.

## Bir işlem için gerçek maliyet kalemleri
`C_total = C_OCR + C_STT + C_LLM + C_infra + C_storage + C_retry + C_support + C_payment_allocated`

`UnitContribution = CreditsUsed * NetRevenuePerCredit - C_total`

`ContributionMargin = UnitContribution / (CreditsUsed * NetRevenuePerCredit)`

Ödeme sağlayıcısı komisyonu, KDV ve diğer vergilerin kârlılık analizinde doğru bazda ayrıştırılması gerekir. Yukarıdaki denklem yönetim amaçlı kaba katkı modeli; resmî finansal muhasebe değildir.

## Büyüme senaryosu
| Senaryo | Toplam tenant | Ücretli dönüşüm (%10 varsayım) |
| --- | ---: | ---: |
| Küçük pilot | 1.000 | 100 |
| Büyüme | 10.000 | 1.000 |
| Ölçek | 100.000 | 10.000 |

%10 **tahmindir**, ölçülmüş gerçek değil. Maliyet hesapları için aktif kullanıcı, belge adedi, OCR sayfa/ses dakikası, API model fiyatları, depolama ve destek yükü gerekir.

## İzlenecek KPI'lar
- Ücretsiz → ilk ücretli satın alma dönüşüm oranı
- Aktif tenant başına aylık AI job ve kredi tüketimi
- İşlem başına API/OCR/STT ve toplam işletim maliyeti
- Kredi başına net katkı, toplam contribution margin
- Failed/retry oranı, kredi iade oranı, chargeback oranı
- Kullanıcı destek zamanı, ücretsiz tenant başına altyapı maliyeti
- Cohort retention ve kredi satın alma tekrar oranı

## Ticari güvenlik
Satın alınan krediler süresiz kullanım hakkı tanıdığı için şirket açısından **ifa edilmemiş hizmet yükümlülüğü** ve gelir tanıma yaklaşımı mali müşavirle değerlendirilmelidir. Krediyi zamana bağlı sıfırlayarak maliyeti gizleme yaklaşımı benimsenmez.

## Pilot karar eşiği
İlk 100 gerçek işlem için maliyet dağılımı p50/p95, başarısızlık payı ve tüketim aşımı ölçülmeden paket satışı ve otomatik AI fiyat tablosu prod'a alınmaz.
