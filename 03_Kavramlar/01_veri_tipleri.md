# Veri Tipleri

`Polars`,  tamamen `Arrow` veri türlerini temel alır ve `Arrow` bellek dizileriyle 
desteklenir. Bu, veri işlemeyi önbellek açısından verimli hale getirir ve İşlemler Arası İletişim için iyi desteklenir. Çoğu veri türü, `Utf8` (bu aslında `LargeUtf8`'dir), `Kategorik (Categorical)` ve `Nesne (Object)` (destek sınırlıdır) dışında `Arrow`'daki tam uygulamayı izler. 

Veri türleri şunlardır:

| Grup                  | Tür         | Detaylar                                                                                                                                      |
| --------------------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **Sayısal (Numeric)** | Int8        | 8 bit işaretli tamsayı.                                                                                                                       |
|                       | Int16       | 16 bit işaretli tamsayı.                                                                                                                      |
|                       | Int32       | 32 bit işaretli tamsayı.                                                                                                                      |
|                       | Int64       | 64 bit işaretli tamsayı.                                                                                                                      |
|                       | UInt8       | 8 bitlik işaretsiz tamsayı.                                                                                                                   |
|                       | UInt16      | 16 bitlik işaretsiz tamsayı.                                                                                                                  |
|                       | UInt32      | 32 bitlik işaretsiz tamsayı.                                                                                                                  |
|                       | UInt64      | 64 bitlik işaretsiz tamsayı.                                                                                                                  |
|                       | Float32     | 32 bitlik ondalıklı sayı.                                                                                                                     |
|                       | Float64     | 64 bitlik ondalıklı sayı.                                                                                                                     |
| **İçiçe (Nested)**    | Struct      | Bir yapı dizisi, bir `Vec<Series> `olarak temsil edilir ve birden çok/heterojen değeri tek bir sütunda paketlemek için kullanışlıdır.         |
|                       | List        | Bir liste dizisi, liste değerlerini ve bir ofset dizisini içeren bir alt dizi içerir. (bu aslında dahili olarak `Arrow` `LargeList`'tir).     |
| **Geçici (Temporal)** | Date        | 32 bit işaretli bir tamsayı tarafından kodlanan UNIX döneminden bu yana dahili olarak gün olarak temsil edilen tarih gösterimi.               |
|                       | Datetime    | 64 bit işaretli bir tamsayı tarafından kodlanan UNIX döneminden bu yana dahili olarak mikrosaniye olarak temsil edilen tarih-zaman gösterimi. |
|                       | Duration    | Dahili olarak mikrosaniye olarak temsil edilen bir timedelta türü. `Date/Datetime` çıkarıldığında oluşturulur.                                |
|                       | Time        | Zaman gösterimi, dahili olarak gece yarısından bu yana nanosaniye olarak temsil edilir.                                                       |
| **Diğer**             | Boolean     | Boole tipi (True/False) etkili bir şekilde paketlenmiştir.                                                                                    |
|                       | Utf8        | Dize verileri (bu aslında dahili olarak Arrow LargeUtf8'dir).                                                                                 |
|                       | Binary      | Verileri bayt olarak saklar.                                                                                                                  |
|                       | Object      | Herhangi bir değer olabilen sınırlı, desteklenen bir veri türü.                                                                               |
|                       | Categorical | Bir dizgi (string) dizisinin kategorik kodlaması.                                                                                             |

Bu veri türlerinin dahili gösterimi hakkında daha fazla bilgi edinmek için [Arrow sütun biçimini](https://arrow.apache.org/docs/format/Columnar.html) kontrol edin.

| << Önceki Bölüm             | Sonraki Bölüm >>                     |
|:---------------------------:|:------------------------------------:|
| [Kurulum](../02_kurulum.md) | [Veri Yapıları](02_veri_yapilari.md) |
