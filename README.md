# Polars Notları

**Polars** kütüphanesinin kullanımına dair öğrendiklerimi ve tercüme çalışmalarımı paylaştığım repo.

![polars](img/polars_github_logo.svg)

**Rust ve Python için Işık Hızında VeriÇerçevesi (DataFrame) Kütüphanesi.**

## Polars Nedir?

`Polars`, `Rust` programlama dilinde yazılmış ve temel olarak `Apache Arrow`’u kullanan bir **DataFrame kütüphanesidir**. Veri düzenleme alışkanlıklarını bilen `Polars`, okunabilir ve yüksek performanslı kod oluşturmanızı sağlayacak bir ifade dili kullanarak DataFrame'leri işlemek için tüm özellikleri içeren eksiksiz bir `Python API` 'ı sunar.

### Apache Arrow Nedir?

`Apache Arrow`, sütunlu verileri işleyen veri analitiği uygulamaları geliştirmek için dilden bağımsız bir yazılım çerçevesidir.

### Polars Kütüphanesini Ne Zaman Kullanmalıyız?

Verimiz `Pandas` için çok büyük, `Spark` için çok küçük olduğunda `Polars`  kütüphanesi iyi bir çözüm olacaktır.

## Polars - Python

`Python` programlama diline aşina olduğum için, bu repoda paylaştığım örnekler,  `polar` kütüphanesinin `python` kodları içine dahil edildiği örnekler olacaktır.

![polars-pyton](img/polars-logo-python.svg)

# KONU BAŞLIKLARI

1. [Giriş](01_giris.md)

2. [Kurulum](02_kurulum.md)

3. **Kavramlar**
   
   * [Veri Tipleri](03_Kavramlar/01_veri_tipleri.md)
   * [Veri Yapıları](03_Kavramlar/02_veri_yapilari.md)
   * [Bağlamlar](03_Kavramlar/03_baglamlar.md)
   * [İfadeler](03_Kavramlar/04_ifadeler.md)
   * Tembel / İstekli API
   * Akış API'sı

4. **İfadeler**
   
   * Temel Operatörler
   * Fonksiyonlar
   * Çevrim (Casting)
   * Diziler (Strings)
   * Kümeleme (# Aggregation)
   * Kayıp Veri
   * Pencere Fonksiyonları
   * Katlamalar / Kıvrımlar (Folds)
   * Listeler ve Diziler
   * Kullanıcı Tanımlı Fonksiyonlar
   * Numpy

5. **Dönüşümler**
   
   * Birleştirmeler (Joins)
   
   * Birleştirmeler (Concatenation)
   
   * Özet Tablolar (Pivots)
   
   * Eritmeler (Melts)
   
   * **Zaman Serileri**
     
     * Ayrıştırma (Parsing)
     
     * Filtreleme
     
     * Gruplama
     
     * Yeniden Örnekleme
     
     * Zaman dilimleri

6. **Tembel API**
   
   * Kullanım
   
   * Optimizasyon
   
   * Şema
   
   * Sorgu Planı
   
   * Sorgu yürütme

7. **IO (Girdi / Çıktı)**
   
   * CSV
   
   * Parquet
   
   * JSON dosyaları
   
   * Çoklu
   
   * Veritabanları
   
   * AWS
   
   * Google BigQuery

8. **SQL**
   
   * Giriş
   * SHOW TABLES (TABLOLARI GÖSTER)
   * SELECT (SEÇİM / SEÇMEK)
   * CREATE (OLUŞTUR)
   * Ortak Tablo İfadeleri

9. **Taşıma / Göç**
   
   * Pandas'tan Gelenler
   * Apache Spark'tan Gelenler

10. **Çeşitli**
    
    * Çoklu İşlem (Multiprocessing)
    
    * Alternatifler
    
    * Başvuru Kılavuzları
    
    * Katkı

## Kaynakça:

* [Polars Kullanım Kılavuzu - Polars User Guide ](https://www.pola.rs/)
