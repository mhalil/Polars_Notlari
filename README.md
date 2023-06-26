# Polars Notları
Polars kütüphanesinin kullanımına dair öğrendiklerimi paylaştığım repo.

## Slogan;
Olağanüstü Hızlı DataFrame Kitaplığı

## Polars Nedir?
Polars, Rust programlama dilinde yazılmış ve temel olarak Apache Arrow’u kullanan bir DataFrame kütüphanesidir. Rust dilinde de kullanılabilmesinin yanı sıra, eksiksiz bir Python API’si sunarak Python dilinde de geliştirme yapmanıza olanak sağlayan bu kütüphaneyi ister DataFrame kütüphanesi olarak ister de veri modelleriniz için backend sorgu motoru olarak kullanabilirsiniz. 

### Apache Arrow Nedir?
Kendi sitesindeki tanıma göre Apache Arrow, CPU’lar ve GPU’lar gibi modern donanımlarda verimli analitik işlemler için düzenlenmiş, düz ve hiyerarşik veriler için dilden bağımsız bir sütunlu bellek biçimi tanımlar. Arrow, DBMS, sorgu motorları, DataFrame kütüphaneleri arasında aracı görevi görür.

### Polars Kütüphanesinin Özellikleri Nelerdir ?
*    Polars, Rust dili ile sıfırdan yazılmış bir Dataframe kütüphanesidir.
*    Polars kütüphanesi, makinedeki tüm kullanılabilir çekirdekleri kullanan bir yapıya sahiptir.
*    Polars, çok fazla paralel işlemi destekler ve birçok işlemi paralel olarak çalıştırabilir.
*    Polars kütüphanesi, dataframe için bir dizin kullanmaz.
*    Polars kütüphanesi, verileri ‘’Apache Arrow” dizilerini kullanarak temsil eder. Apache Arrow dizileri, bellek kullanımı, işlem hesaplaması ve yükleme süresine göre oldukça verimlidir.
*    Polars tembel değerlendirmeyi destekler, pandaslarla çok sık çalıştıysanız farkına varmış olmalısınız ki pandaslar bir ifade ile karşılaştırdığında bu ifadeyi değerlendirip çıktı vermeye odaklanırken, polars sorguyu hedef alarak sorguyu inceler, bellek kullanımını azaltmanın yollarını arayıp, sorguyu daha optimize etmeyi amaçlar.
    
### Polars’ı Ne Zaman Kullanmalıyım?
Veriniz Pandas için çok büyük, Spark için çok küçük olduğunda Polars kesinlikle çok iyi bir çözüm.

# KONU BAŞLIKLARI
1. [Giriş](01.giris.md)
2. 





# Kaynakça:
* [Polars Kullanım Kılavuzu - Polars User Guide ](https://www.pola.rs/)
* [Fethi Tekyaygil - Medium](https://fethitekyaygil.medium.com/verimizi-hangisine-emanet-etmeliyiz-pandaya-m%C4%B1-kutup-ay%C4%B1s%C4%B1na-m%C4%B1-2260df3fc179)
* [Şule AKÇAY - Medium](https://suleakcaycs.medium.com/python-d%C3%BCnyas%C4%B1nda-yeni-bir-%C3%A7a%C4%9F-o-bi-polars-%EF%B8%8F-8f569bb9f81a)
 
