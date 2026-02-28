# Görüntü İşleme Tabanlı Optik Form Okuyucu (OMR) Sistemi - Gereksinim Dokümanı

## 1. Problem Tanımı
Geleneksel Optik İşaret Tanıma (OMR - Optical Mark Recognition) sistemleri, sınav ve anket değerlendirmeleri için özel tasarlanmış, maliyetli ve taşınabilirliği düşük donanımlara ihtiyaç duymaktadır. Optik formların okunması sürecinde yaşanan hizalama hataları, çevresel ışık değişimlerinin etkisi, düşük çözünürlüklü görüntü problemleri ve özel cihazlara olan bağımlılık, değerlendirme süreçlerini yavaşlatmakta ve maliyetleri artırmaktadır. Bu durum, eğitim kurumları ve araştırmacılar için erişilebilir, hızlı ve donanımdan bağımsız bir yazılım çözümünün geliştirilmesini zorunlu kılmaktadır.

## 2. Projenin Amacı
Bu projenin temel amacı; herhangi bir özel OMR cihazına ihtiyaç duymaksızın, standart akıllı telefon kameraları veya masaüstü tarayıcılar aracılığıyla elde edilen optik form görüntülerini işleyerek sonuçları otomatik olarak değerlendiren, açık kaynaklı kütüphaneler (OpenCV vb.) kullanılarak geliştirilmiş, donanımdan bağımsız ve yüksek doğruluk oranına sahip bir "Görüntü İşleme Tabanlı Optik Form Okuyucu Sistemi" geliştirmektir.

## 3. Kapsam
Sistem, dijital ortama aktarılmış optik form görüntülerinin alınması, görüntü işleme teknikleriyle (perspektif düzeltme, gürültü filtreleme, eşikleme) ön işlemlerden geçirilmesi, form üzerindeki işaretli alanların tespit edilmesi ve önceden tanımlanmış cevap anahtarı ile karşılaştırılarak sonuçların üretilmesi süreçlerini kapsar. Proje kapsamında örnek bir optik form şablonu tasarlanacak olup, sistem yalnızca bu standart şablon formatındaki formları işleyebilecektir. Sonuçların analiz edilebilmesi amacıyla basit bir kullanıcı arayüzü ve CSV/JSON formatında veri dışa aktarım modülü geliştirilecektir.

## 4. Fonksiyonel Gereksinimler
- Sistem, kullanıcı tarafından yüklenen form görüntülerini (JPG, PNG vb. formatlarda) kabul etmelidir.
- Sistem, görüntü üzerindeki eğrilik ve perspektif bozulmalarını otomatik olarak tespit edip düzeltmelidir.
- Sistem, form üzerindeki soru bloklarını, öğrenci numarası/kimlik alanlarını ve referans noktalarını (marker/fudicial) doğru bir şekilde bölütlemelidir (segmentasyon).
- Sistem, her bir soru satırı için dolu veya boş durumlarını belirleyebilmek adına piksel yoğunluğu veya alan analizi yapmalıdır.
- Sistem, çift işaretlenmiş (birden fazla şıkkın doldurulması) veya boş bırakılmış soruları tespit ederek ilgili satırları geçersiz veya boş olarak işaretlemelidir.
- Sistem, önceden sisteme yüklenmiş doğru cevap anahtarı ile öğrenci cevaplarını karşılaştırmalıdır.
- Sistem, karşılaştırma sonucunda öğrenci bazlı doğru, yanlış, boş sayılarını ve toplam puanı hesaplamalıdır.
- Sistem, elde edilen değerlendirme sonuçlarını raporlamak üzere dışa aktarma (CSV veya JSON) yeteneğine sahip olmalıdır.

## 5. Fonksiyonel Olmayan Gereksinimler
- **Performans:** Sistem, standart çözünürlükteki tek bir formun işlenmesi ve değerlendirilmesi sürecini makul bir sürede (örn. <2 saniye) tamamlamalıdır.
- **Doğruluk:** Farklı aydınlatma koşullarında çekilmiş net görüntülerde sistemin okuma doğruluğu (hassasiyeti) en az %95 düzeyinde olmalıdır.
- **Kullanılabilirlik:** Geliştirilecek kullanıcı arayüzü sade, anlaşılır ve teknik bilgi gerektirmeden dosya yükleme/sonuç görme işlemlerine olanak tanıyacak yapıda olmalıdır.
- **Taşınabilirlik:** Sistem, ekstra donanım gerektirmemeli ve standart bir bilgisayar ortamında Python tabanlı çalışabilmelidir.
- **Modülerlik:** Görüntü işleme, algoritma ve arayüz katmanları birbirinden bağımsız modüller halinde tasarlanmalı, gelecekte farklı form şablonlarının entegrasyonuna açık olmalıdır.

## 6. Kullanım Senaryosu (Use Case)

**Senaryo Adı:** Optik Formun Yüklenmesi ve Değerlendirilmesi
**Aktör:** Kullanıcı (Öğretmen / Değerlendirici)
**Ön Koşul:** Kullanıcının bilgisayarında değerlendirilecek form görüntüleri ve ilgili cevap anahtarı bulunmalıdır.

**Ana Akış:**
1. Kullanıcı, sistemin arayüzünü açar.
2. Kullanıcı, "Cevap Anahtarı Yükle" seçeneğini kullanarak referans cevapları sisteme tanımlar.
3. Kullanıcı, değerlendirmek istediği öğrencilere ait optik form görüntüsünü veya görüntü klasörünü sisteme yükler.
4. Sistem, yüklenen ilk görüntüyü alır, perspektif düzeltme ve eşikleme işlemlerini (ön işleme) uygular.
5. Sistem, görüntü üzerindeki işaretli alanları tespit eder ve kullanıcının cevaplarını dijital veriye dönüştürür.
6. Sistem, elde edilen cevapları, ilk adımda yüklenen cevap anahtarı ile karşılaştırır.
7. Sistem, puan hesaplamasını yapar ve ilgili form için analiz sonucunu oluşturur.
8. Sistem, tüm görüntüler için 4-7 adımlarını tekrarlar.
9. Kullanıcı, tüm işlemler bittiğinde oluşan genel paneli görüntüler veya sonuçları "Dışa Aktar" butonu ile tablo formatında (CSV) bilgisayarına kaydeder.

**Alternatif Akış (Hata Durumu):** Sistem, yüklenen görüntünün aşırı bulanık veya form formatına tamamen aykırı olduğunu tespit ederse işlemi iptal eder ve "Görüntü kalitesi yetersiz veya şablon tanınmadı" uyarısını gösterir.

## 7. Örnek Optik Form Yapısının Tanımı
Projede kullanılacak olan referans optik form, görüntü işleme algoritmalarının köşe tespiti ve perspektif düzeltme işlemlerini kolaylaştırmak amacıyla özel olarak tasarlanacaktır.
- **Referans Noktaları (Marker):** Formun dört köşesinde veya belirli kenarlarında algılamayı kolaylaştıran özel siyah kare/daire referans işaretleri (fiducial markers) bulunacaktır.
- **Öğrenci Bilgi Alanı:** Formun üst kısmında, öğrenci numarası veya kodunu belirlemek üzere, ayrı bloklar halinde oluşturulmuş (0-9 arası sayıları içeren) kodlama matrisi yer alacaktır.
- **Cevap Alanları:** Sorular belirli sütunlar halinde düzenlenecek, her soru numarasına karşılık yan yana sıralanmış 'A', 'B', 'C', 'D', 'E' (veya belirlenen standartta) seçenek baloncukları bulunacaktır. Baloncukların sınırları, arka plandan net olarak ayrılabilecek koyulukta belirlenecektir.
- **Boşluklar:** Bölümler (bilgi alanı, cevap sütunları) arası analiz hatalarını önlemek için yeterli düzeyde beyaz (boş) piksel boşluğu (margin) bırakılacaktır.
