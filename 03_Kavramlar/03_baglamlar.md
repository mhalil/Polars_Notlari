# Bağlamlar

Polars,  verileri dönüştürmek için kendi EtkiAlanına Özgü Dil (DSL - Domain Specific Language) geliştirmiştir. Dilin kullanımı çok kolaydır ve insan tarafından okunabilen karmaşık sorgulara izin verir. Dilin iki temel bileşeni,  Bağlamlar ve İfadelerdir (sonraki bölümde ele alacağız).

Adından da anlaşılacağı gibi bir bağlam, bir ifadenin değerlendirilmesi gereken bağlamı ifade eder. Üç ana bağlam vardır. (*Bu  kılavuzda daha sonra ele alınacak olan ek Liste ve SQL bağlamları vardır. Ancak basit olması için bunları şimdilik kapsam dışında bırakıyoruz.* ):

1. Seçim: `df.select([..])`, `df.with_columns([..])`
2. Filtreleme: `df.filter()`
3. Gruplama / Kümeleme: `df.groupby(..).agg([..])`

Örnekler, aşağıdaki DataFrame üzerinden gerçekleştirilir:

```python
df = pl.DataFrame(
    {
        "nrs": [1, 2, 3, None, 5],
        "names": ["foo", "ham", "spam", "egg", None],
        "random": np.random.rand(5),
        "groups": ["A", "A", "B", "C", "B"],
    }
)
print(df)
```

```python
shape: (5, 4)
┌──────┬───────┬──────────┬────────┐
│ nrs  ┆ names ┆ random   ┆ groups │
│ ---  ┆ ---   ┆ ---      ┆ ---    │
│ i64  ┆ str   ┆ f64      ┆ str    │
╞══════╪═══════╪══════════╪════════╡
│ 1    ┆ foo   ┆ 0.154163 ┆ A      │
│ 2    ┆ ham   ┆ 0.74005  ┆ A      │
│ 3    ┆ spam  ┆ 0.263315 ┆ B      │
│ null ┆ egg   ┆ 0.533739 ┆ C      │
│ 5    ┆ null  ┆ 0.014575 ┆ B      │
└──────┴───────┴──────────┴────────┘
```

## Select

`select` bağlamı ile seçim yapılırken, ifadeler, sütunlar üzerinden uygulanır. Yani Sütunlar baz alınarak seçim yapılır. Bu bağlamdaki ifadeler, tümü aynı uzunlukta veya 1 birim uzunluğunda olan Serileri (`series`) üretmelidir. Bu şu demek, select bağlamı sonrası, seçim yapılan veri çerçevesinin uzunluğuna (satır sayısına) eşit yeni seri(ler) üretilir.

`DataFrame`'in  yüksekliğine (satır sayısın) uyması için 1 birim uzunluğunda bir Seri (`series`) yayınlanacaktır. `select` bağlamı ile bir seçimin kümeleri, ifade kombinasyonları veya hazır değerleri olan yeni sütunlar üretebileceğini unutmayın.

```python
out = df.select(
    [
        pl.sum("nrs"),
        pl.col("names").sort(),
        pl.col("names").first().alias("first name"),
        (pl.mean("nrs") * 10).alias("10xnrs"),
    ]
)
print(out)
```

```python
shape: (5, 4)
┌─────┬───────┬────────────┬────────┐
│ nrs ┆ names ┆ first name ┆ 10xnrs │
│ --- ┆ ---   ┆ ---        ┆ ---    │
│ i64 ┆ str   ┆ str        ┆ f64    │
╞═════╪═══════╪════════════╪════════╡
│ 11  ┆ null  ┆ foo        ┆ 27.5   │
│ 11  ┆ egg   ┆ foo        ┆ 27.5   │
│ 11  ┆ foo   ┆ foo        ┆ 27.5   │
│ 11  ┆ ham   ┆ foo        ┆ 27.5   │
│ 11  ┆ spam  ┆ foo        ┆ 27.5   │
└─────┴───────┴────────────┴────────┘
```

***Şimdi kodları satır satır açıklamaya çalışayım;***

```python
pl.sum("nrs"),
```

* `sum` metodu ile, "**nrs**" sütununda bulunan sayıların toplamını hesapla ve "**nrs**" sütununa yaz. Veri Çerçevesi 5 satırdan oluştuğu için elde edilen seri de 5 elemanlı olacak. Böylece her satıra aynı değer yazılacak.

```python
pl.col("names").sort(),
```

* `col` metodu (column yani satır anlamına gelir) ile "**names**" isimli satırı seç, 

* `sort` metodu ile de değerleri alfabetik olarak sırala. Öncelik `null` yani boş değerlere verilir.

```python
pl.col("names").first().alias("first name"),
```

* `col` metodu ile "**names**" isimli satırı seç,

* `first()` metodu ile ilk değeri al,

* `alias()` metodu ile yeni sütuna "**first name**" başlığını / adını ver. (**alias** : takma ad, başka ad, sahte isim, namı diğer anlamlarına gelir) 

```python
pl.mean("nrs") * 10).alias("10xnrs")
```

* `mean()`metodu ile "**nrs**" sütununun ortalamasını al [(1+2+3+5) / 4] (**mean** : "ortalama" anlamına gelir.)

* `* 10 ` ile ortalama değeri 10 ile çarp,

* `alias()` metodu ile yeni sütun ismini "**10xnrs**" olarak belirle.

Sorgudan  görebileceğiniz gibi, `select` bağlamı çok güçlüdür ve birbirinden 
bağımsız (ve paralel olarak) rasgele ifadeler gerçekleştirmenize izin verir.

`select` ifadesine benzer, `with_columns` ifadesi de vardır. Temel fark, `with_columns`  bağlamı kullanıldığında veri çerçevesindeki **orijinal sütunlar tutulur**, yeni sütunlar veri çerçevesine eklenir, `select` bağlamı kullanıldığında ise **orijinal sütunlar tutulmaz**, yeni sütunlar eklenir.

```python
df = df.with_columns(
    [
        pl.sum("nrs").alias("nrs_sum"),
        pl.col("random").count().alias("count"),
    ]
)
print(df)
```

```python
shape: (5, 6)
┌──────┬───────┬──────────┬────────┬─────────┬───────┐
│ nrs  ┆ names ┆ random   ┆ groups ┆ nrs_sum ┆ count │
│ ---  ┆ ---   ┆ ---      ┆ ---    ┆ ---     ┆ ---   │
│ i64  ┆ str   ┆ f64      ┆ str    ┆ i64     ┆ u32   │
╞══════╪═══════╪══════════╪════════╪═════════╪═══════╡
│ 1    ┆ foo   ┆ 0.154163 ┆ A      ┆ 11      ┆ 5     │
│ 2    ┆ ham   ┆ 0.74005  ┆ A      ┆ 11      ┆ 5     │
│ 3    ┆ spam  ┆ 0.263315 ┆ B      ┆ 11      ┆ 5     │
│ null ┆ egg   ┆ 0.533739 ┆ C      ┆ 11      ┆ 5     │
│ 5    ┆ null  ┆ 0.014575 ┆ B      ┆ 11      ┆ 5     │
└──────┴───────┴──────────┴────────┴─────────┴───────┘
```

***Kodları açıklayalım***;

```python
 pl.sum("nrs").alias("nrs_sum"),
```

* `sum()` metodu ile "**nrs**" isimli sütunun toplamı alınır. (**sum**, toplam anlamına gelir)

* `.alias()` metodu ile toplamı alınan sütunun değeri "**nrs_sum**" isimli yeni sütuna eklensin.

```python
pl.col("random").count().alias("count")
```

* `col` metodu ile "**random**" isimli satırı seç,

* `count()` metodu ile **random** sütununda kaç adet veri bulunduğunu tespit et,

* `alias()` metodu ile, tespit edilen veri serisini "**count**" ismi ile yeni sütuna ekle,

## Filter

`filter` bağlamında, mevcut veri çerçevesini, `Boolean` veri türü (`True / False`) olarak değerlendirilen keyfi ifadeye göre filtrelersiniz.

```python
out = df.filter(pl.col("nrs") > 2)
print(out)
```

```python
shape: (2, 6)
┌─────┬───────┬──────────┬────────┬─────────┬───────┐
│ nrs ┆ names ┆ random   ┆ groups ┆ nrs_sum ┆ count │
│ --- ┆ ---   ┆ ---      ┆ ---    ┆ ---     ┆ ---   │
│ i64 ┆ str   ┆ f64      ┆ str    ┆ i64     ┆ u32   │
╞═════╪═══════╪══════════╪════════╪═════════╪═══════╡
│ 3   ┆ spam  ┆ 0.263315 ┆ B      ┆ 11      ┆ 5     │
│ 5   ┆ null  ┆ 0.014575 ┆ B      ┆ 11      ┆ 5     │
└─────┴───────┴──────────┴────────┴─────────┴───────┘
```

***Kodları açıklayalım;***

```python
df.filter(pl.col("nrs") > 2)
```

* `df.filter()` metodu ile df isimli veri çerçevesine (Data Frame'e) filtre uygula,

* `col()` metou ile "**nrs**" isimli sütunu seç,

* `> 2` karşılaştırma operatörü ile, "**nrs**" isimli sütunda değeri 2'den büyük olanları seç (2'den büyük olanlar `True` değeri döndürür)

## Groupby / Aggregation

`Groupby` bağlamında, ifadeler gruplar üzerinde çalışır ve bu nedenle farklı 
uzunlukta sonuçlar verebilir (bir grubun birçok üyesi olabilir).

```python
out = df.groupby("groups").agg(
    [
        pl.sum("nrs"),  # sum nrs by groups
        pl.col("random").count().alias("count"),  # count group members
        # sum random where name != null
        pl.col("random").filter(pl.col("names").is_not_null()).sum().suffix("_sum"),
        pl.col("names").reverse().alias(("reversed names")),
    ]
)
print(out)
```

```python
shape: (3, 5)
┌────────┬──────┬───────┬────────────┬────────────────┐
│ groups ┆ nrs  ┆ count ┆ random_sum ┆ reversed names │
│ ---    ┆ ---  ┆ ---   ┆ ---        ┆ ---            │
│ str    ┆ i64  ┆ u32   ┆ f64        ┆ list[str]      │
╞════════╪══════╪═══════╪════════════╪════════════════╡
│ B      ┆ 8    ┆ 2     ┆ 0.263315   ┆ [null, "spam"] │
│ C      ┆ null ┆ 1     ┆ 0.533739   ┆ ["egg"]        │
│ A      ┆ 3    ┆ 2     ┆ 0.894213   ┆ ["ham", "foo"] │
└────────┴──────┴───────┴────────────┴────────────────┘
```

***Kodları açıklayalım;***

```python
df.groupby("groups").agg()
```

* `groupby()` metodu ile `df` isimli veri çerçevesini, "**groups**" isimli (etiketi, başlıklı) sütuna göre grupla,

* `.agg()`metodu ile, kodun devamında yazılan kriterlere göre **kümeleme** yap,

```python
pl.sum("nrs"),  # sum nrs by groups
```

* `sum() ` metodu ile "**nrs**" sütunun değerlerini, gruplarına göre topla, (ör. **"B"** grubuna dahil "**nrs**" sütunu değerlerinin toplamı 3+5 = 8'dir)

```python
pl.col("random").count().alias("count"),  # count group members
```

* `col()` metou ile "**random**" isimli sütunu seç,

* `count()` metodu ile **random** sütununda her grup için kaç adet veri bulunduğunu tespit et,

* `alias()` metodu ile, tespit edilen değerleri "**count**" ismi ile yeni sütuna ekle,



```python
pl.col("random").filter(pl.col("names").is_not_null()).sum().suffix("_sum"),
```

* `col()` metodu ile "**random**" isimli sütunu seç,

* `filter()`  metodu ile **random** sütununa **filtre** uygula,

* `col("names").is_not_null())` ile, "**names**" sütununda **boş olmayan değerleri** tespit et,

* `sum()` ile boş olmadığı tespit edilen satırların "**random**" sütunu değerlerini topla,

* `suffix()` metodu ile toplamı alınan "**random**" sütunun değerlerinin ekleneceği yeni sütunun isminin sonuna "**_sum**" ifadesini ekle.



```python
pl.col("names").reverse().alias(("reversed names")),
```

* `col()` metodu ile "**names**" isimli sütunu seç,

* `reverse()` metodu ile (aynı gruba sahip) "**names**" sütun isimlerini ters çevir yani Z'den A'ya doğru sırala, 

* `alias()` metodu ile sütun ismini "**reversed names**" olarak belirle,



Sonuçtan da görebileceğiniz gibi, tüm ifadeler `groupby` bağlamı tarafından 
tanımlanan gruba uygulanır. Standart `groupby`'nin yanı sıra, `groupby_dynamic` ve `groupby_rolling` de `groupby` bağlamına giriştir.

| << Önceki Bölüm                      | Sonraki Bölüm >>           |
|:------------------------------------:|:--------------------------:|
| [Veri Yapıları](02_veri_yapilari.md) | [İfadeler](04_ifadeler.md) |
