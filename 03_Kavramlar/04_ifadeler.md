# İfadeler

`Polars`, çok hızlı performansının merkezinde yer alan, ifadeler (expressions) adı verilen güçlü bir konsepte sahiptir.

İfadeler, birçok veri bilimi işleminin merkezinde yer alır:

* bir sütundan satır örneği alma

* bir sütundaki değerleri çarpma

* tarihlerden bir yıl sütunu çıkarma

* bir dize sütununu küçük harfe dönüştürme

* ve benzeri!

Ancak ifadeler diğer işlemlerde de kullanılır:

* `groupby` işleminde bir grubun ortalamasının alınması

* `groupby` işleminde grupların boyutunu hesaplama

* toplamı, sütunlar arasında yatay olarak almak

`Polars`, bu temel veri dönüşümlerini şu yollarla çok hızlı gerçekleştirir:

* her ifadede otomatik sorgu optimizasyonu

* birçok sütundaki ifadelerin otomatik olarak paralelleştirilmesi

Polars ifadeleri, bir diziden bir diziye (veya matematiksel olarak `Fn(Series) -> Series`) eşlemedir. İfadelerin girdi olarak bir `Series` ve çıktı olarak bir `Series` sahip olması nedeniyle, bir ifade dizisi yapmak kolaydır (`Pandas`'taki yöntem zincirlemesine benzer).

## Örnekler

Aşağıdaki bir ifadedir:

```python
pl.col("foo").sort().head(2)
```

Yukarıdaki parçacığı diyor ki:

1. "foo" sütununu seçin

2. Ardından sütunu sıralayın (ters sırada değil)

3. Ardından, sıralanan çıktının ilk iki değerini alın

İfadelerin gücü, her ifadenin yeni bir ifade üretmesi ve bunların bir araya 
getirilebilmesidir. Bir ifadeyi `Polars` yürütme bağlamlarından birine 
geçirerek çalıştırabilirsiniz.

Burada `df.select`'i çalıştırarak iki ifade çalıştırıyoruz:

```python
df.select(
    [
        pl.col("foo").sort().head(2),
        pl.col("bar").filter(pl.col("foo") == 1).sum(),
    ]
)
```

Tüm  ifadeler paralel olarak çalıştırılır, yani ayrı `Polars` ifadeleri **utanç verici bir şekilde paraleldir**. Bir ifade içinde daha fazla paralelleştirme olabileceğini unutmayın.

## Çözüm

Bu, olası ifadeler açısından buzdağının görünen kısmıdır. Bir ton daha var ve bunlar çeşitli şekillerde birleştirilebilir. Bu sayfa, ifadeler kavramına aşina olmanızı amaçlamaktadır, ifadeler bölümünde daha derine ineceğiz.

| << Önceki Bölüm              | Sonraki Bölüm >>     |
|:----------------------------:|:--------------------:|
| [Bağlamlar](03_baglamlar.md) | Tembel / İstekli API |
