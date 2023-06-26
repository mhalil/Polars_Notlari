# Bağlamlar

Polars,  verileri dönüştürmek için kendi Etki Alanına Özgü Dili (DSL - Domain Specific Language) geliştirmiştir. Dilin kullanımı çok kolaydır ve insan tarafından okunabilen karmaşık sorgulara izin verir. Dilin iki temel bileşeni, sonraki bölümde ele alacağımız Bağlamlar ve İfadelerdir.

Adından da anlaşılacağı gibi bir bağlam, bir ifadenin değerlendirilmesi gereken bağlamı ifade eder. Üç ana bağlam vardır. (*Bu  kılavuzda daha sonra ele alınan ek Liste ve SQL bağlamları vardır. Ancak basit olması için bunları şimdilik kapsam dışında bırakıyoruz.* ):

1. Seçim: `df.select([..])`, `df.with_columns([..])`
2. Filtreleme: `df.filter()`
3. Grouplama / Kümeleme: `df.groupby(..).agg([..])`

Aşağıdaki örnekler aşağıdaki DataFrame üzerinde gerçekleştirilir:

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

`select` (seçim) bağlamında seçim, ifadeleri sütunlar üzerinden uygular. Bu bağlamdaki ifadeler, tümü aynı uzunlukta veya 1 birim uzunluğunda olan Seriler (`series`) üretmelidir.

`DataFrame`'in  yüksekliğine uyması için 1 birim uzunluğunda bir Seri (`series`) yayınlanacaktır. Bir seçimin kümeler (toplamlar), ifade kombinasyonları veya hazır değerler olan yeni sütunlar üretebileceğini unutmayın.

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

Sorgudan  görebileceğiniz gibi, `select` bağlamı çok güçlüdür ve birbirinden 
bağımsız (ve paralel olarak) rasgele ifadeler gerçekleştirmenize izin verir.

`select` ifadesine benzer şekilde, aynı zamanda seçim bağlamına bir giriş olan `with_columns` ifadesi de vardır. Temel fark, `with_columns` öğesinin orijinal sütunları tutması ve `select` orijinal sütunları bırakırken yenilerini eklemesidir.

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

## Filter

`filter` bağlamında, mevcut veri çerçevesini, `Boolean` veri türü olarak değerlendirilen keyfi ifadeye göre filtrelersiniz.

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

## Groupby / Aggregation

`Groupby` bağlamında, ifadeler gruplar üzerinde çalışır ve bu nedenle herhangi 
bir uzunlukta sonuçlar verebilir (bir grubun birçok üyesi olabilir).

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

Sonuçtan da görebileceğiniz gibi, tüm ifadeler `groupby` bağlamı tarafından 
tanımlanan gruba uygulanır. Standart `groupby`'nin yanı sıra, `groupby_dynamic` ve `groupby_rolling` de `groupby` bağlamına girişlerdir.

| << Önceki Bölüm                      | Sonraki Bölüm >>           |
|:------------------------------------:|:--------------------------:|
| [Veri Yapıları](02_veri_yapilari.md) | [İfadeler](04_ifadeler.md) |
