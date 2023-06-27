# Veri Yapıları

`Polars` tarafından sağlanan temel veri yapıları `Series (Seriler)` ve `DataFrame (VeriÇerçeveleri)`'dir.

# Series (Seriler)

Seriler, 1-boyutlu bir veri yapısıdır. Bir dizi içindeki tüm öğeler aynı [Veri 
Türüne/Tipine](01_veri_tipleri.md) sahiptir. Aşağıdaki kod parçacığı, basit bir `Series` nesnesinin 
nasıl oluşturulacağını gösterir.

```python
import polars as pl

s = pl.Series("a", [1, 2, 3, 4, 5])
print(s)
```

```python
shape: (5,)
Series: 'a' [i64]
[
    1
    2
    3
    4
    5
]
```

# DataFrame (VeriÇerçevesi)

VeriÇerçevesi (DataFrame), bir `Seri (Series)` tarafından desteklenen 2-boyutlu bir veri yapısıdır ve bir `Series` koleksiyonunun (ör. liste) soyutlaması olarak görülebilir. Bir `DataFrame` üzerinde yürütülebilen işlemler, `SQL` benzeri bir sorguda  yapılanlara çok benzer. `GROUP BY`, `JOIN`, `PIVOT` yapabilir, aynı zamanda özel fonksiyonlar da tanımlayabilirsiniz.

Aşağıda, Sütun bilgileri ve veri türleri / tipleri sırasıyla, Tamsayı (`integer`), Tarih (`datetime`), ve Ondalıklı Sayı (`float`)  olan bir VeriÇerçevesi (`DataFrame`) tanımlanıyor ve çıktısı gösterilyor.

```python
df = pl.DataFrame(
    {
        "integer": [1, 2, 3, 4, 5],
        "date": [
            datetime(2022, 1, 1),
            datetime(2022, 1, 2),
            datetime(2022, 1, 3),
            datetime(2022, 1, 4),
            datetime(2022, 1, 5),
        ],
        "float": [4.0, 5.0, 6.0, 7.0, 8.0],
    }
)

print(df)
```

```python
shape: (5, 3)
┌─────────┬─────────────────────┬───────┐
│ integer ┆ date                ┆ float │
│ ---     ┆ ---                 ┆ ---   │
│ i64     ┆ datetime[μs]        ┆ f64   │
╞═════════╪═════════════════════╪═══════╡
│ 1       ┆ 2022-01-01 00:00:00 ┆ 4.0   │
│ 2       ┆ 2022-01-02 00:00:00 ┆ 5.0   │
│ 3       ┆ 2022-01-03 00:00:00 ┆ 6.0   │
│ 4       ┆ 2022-01-04 00:00:00 ┆ 7.0   │
│ 5       ┆ 2022-01-05 00:00:00 ┆ 8.0   │
└─────────┴─────────────────────┴───────┘
```

### Verileri Görüntülemek

Bu  bölüm, bir `DataFrame`'deki verileri görüntülemeye odaklanır. Yukarıdaki 
örnekteki oluşturulan VeriÇerçevesini (DataFrame'i) başlangıç ​​noktası olarak kullanacağız ve aşağıda anlatılan komutlar / metotlar, bu VeriÇerçevesi ve içeriğindeki verielr kullanılarak anlatılacak.

#### Head

`head` metodu varsayılan olarak bir `VeriÇerçevesi (DataFrame)`'in <mark>ilk 5 satırını</mark> görüntüler.  İsterseniz, görmek istediğiniz satır sayısını metod içinde parametre olarak belirtebilirsiniz (ör. `df.head(10)`).

```python
print(df.head(3))
```

```python
shape: (3, 3)
┌─────────┬─────────────────────┬───────┐
│ integer ┆ date                ┆ float │
│ ---     ┆ ---                 ┆ ---   │
│ i64     ┆ datetime[μs]        ┆ f64   │
╞═════════╪═════════════════════╪═══════╡
│ 1       ┆ 2022-01-01 00:00:00 ┆ 4.0   │
│ 2       ┆ 2022-01-02 00:00:00 ┆ 5.0   │
│ 3       ┆ 2022-01-03 00:00:00 ┆ 6.0   │
└─────────┴─────────────────────┴───────┘
```

#### Tail

`tail` metodu, bir `VeriÇerçevesi (DataFrame)`'in <mark>son 5 satırını</mark> gösterir. `head` metoduna benzer şekilde görmek istediğiniz satır sayısını da belirtebilirsiniz. (ör. `df.tail(8)`)

```python
print(df.tail(3))
```

```python
shape: (3, 3)
┌─────────┬─────────────────────┬───────┐
│ integer ┆ date                ┆ float │
│ ---     ┆ ---                 ┆ ---   │
│ i64     ┆ datetime[μs]        ┆ f64   │
╞═════════╪═════════════════════╪═══════╡
│ 3       ┆ 2022-01-03 00:00:00 ┆ 6.0   │
│ 4       ┆ 2022-01-04 00:00:00 ┆ 7.0   │
│ 5       ┆ 2022-01-05 00:00:00 ┆ 8.0   │
└─────────┴─────────────────────┴───────┘
```

#### Sample

VeriÇerçevenizin (DataFrame'inizin) verileri hakkında bir izlenim edinmek istiyorsanız, `sample` metodunu da kullanabilirsiniz. `sample` metodu ile, `DataFrame`'den `n` sayıda rasgele satır alırsınız. Bu metodu, `random` modülündeki `sample` metoduna benzetebilirsiniz.

```python
print(df.sample(2))
```

```python
shape: (2, 3)
┌─────────┬─────────────────────┬───────┐
│ integer ┆ date                ┆ float │
│ ---     ┆ ---                 ┆ ---   │
│ i64     ┆ datetime[μs]        ┆ f64   │
╞═════════╪═════════════════════╪═══════╡
│ 1       ┆ 2022-01-01 00:00:00 ┆ 4.0   │
│ 5       ┆ 2022-01-05 00:00:00 ┆ 8.0   │
└─────────┴─────────────────────┴───────┘
```

#### Describe

`Describe` metodu, VeriÇerçevenizin (DataFrame'inizin) özet istatistiklerini döndürür. VeriÇerçevenizde sayısal değerler varsa, bu metot size birkaç hızlı istatistik bilgisi sağlayacaktır. `Pandas`kütüphanesinin `describe` metodu gibi düşünebilirsiniz.

```python
print(df.describe())
```

```python
shape: (9, 4)
┌────────────┬──────────┬─────────────────────┬──────────┐
│ describe   ┆ integer  ┆ date                ┆ float    │
│ ---        ┆ ---      ┆ ---                 ┆ ---      │
│ str        ┆ f64      ┆ str                 ┆ f64      │
╞════════════╪══════════╪═════════════════════╪══════════╡
│ count      ┆ 5.0      ┆ 5                   ┆ 5.0      │
│ null_count ┆ 0.0      ┆ 0                   ┆ 0.0      │
│ mean       ┆ 3.0      ┆ null                ┆ 6.0      │
│ std        ┆ 1.581139 ┆ null                ┆ 1.581139 │
│ min        ┆ 1.0      ┆ 2022-01-01 00:00:00 ┆ 4.0      │
│ max        ┆ 5.0      ┆ 2022-01-05 00:00:00 ┆ 8.0      │
│ median     ┆ 3.0      ┆ null                ┆ 6.0      │
│ 25%        ┆ 2.0      ┆ null                ┆ 5.0      │
│ 75%        ┆ 4.0      ┆ null                ┆ 7.0      │
└────────────┴──────────┴─────────────────────┴──────────┘
```

**count** : sütunlarda kaç adet veri olduğunu,

**null_count** : sütunlarda kaç adet kayıp veri olduğunu,

**mean** : sütunlardaki verilerin ortalamasını,

**std** : sütunlardaki verilerin standart sapmasını,

**min**: sütunlardaki verilerin en küçük değerini

**max** : sütunlardaki verilerin en büyük değerini,

**median** : sütunlardaki verilerin Ortanca medyanını,

**%25** : sütunlardaki verilerin medyanın alt çeyreğini (dörttebirliğini),

**%75** : sütunlardaki verilerin medyanın üst çeyreğini (dörttebirliğini) tanımlar

| << Önceki Bölüm                    | Sonraki Bölüm >>             |
|:----------------------------------:|:----------------------------:|
| [Veri Tipleri](01_veri_tipleri.md) | [Bağlamlar](03_baglamlar.md) |
