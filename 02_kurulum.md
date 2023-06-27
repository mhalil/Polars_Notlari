# Kurulum

`Polars` bir kitaplıktır/kütüphanedir ve kurulumu, ilgili programlama dilinin paket yöneticisini çağırmak kadar basittir. Aşağıda farklı programlama dillerine göre kurulum kodları verilmiştir. Bu kodları konsoldan yazarak `polars`'ı yükleyebilirsiniz.

#### Python

```python
pip install polars
```

#### Rust

```rust
cargo add polars -F lazy

# Or Cargo.toml
[dependencies]
polars = { version = "x", features = ["lazy", ...]}
```

#### NodeJS

```js
yarn add nodejs-polars
```

## İçe Aktar / Çalışmaya Dahil Et (Import)

Kütüphaneyi kullanmak için, sisteminize kurmuş olduğunuz modülü projenize dahil etmelisiniz. Kitaplığın/Modülün farklı programlama dillerinde, kod içine nasıl dahil  edilebileceğine dair gösterim aşağıda mevcuttur.

#### Python

```python
import polars as pl
```

#### Rust

```rust
use polars::prelude::*;
```

#### NodeJS

```js
// esm
import pl from 'nodejs-polars';

// require
const pl = require('nodejs-polars'); 
```

## Özellik Bayrakları

Yukarıdaki **kurulum** komutunu kullanarak `Polars`'ın çekirdeğini sisteminize kurarsınız. Ancak kullanım durumunuza bağlı olarak, isteğe bağlı bağımlılıkları da yüklemek isteyebilirsiniz. Bunlar, kullanım alanını en aza indirmek için isteğe bağlı yapılır. Bayraklar, programlama diline bağlı olarak farklıdır. Kılavuz boyunca, ek bir bağımlılık gerektiren bir işlevsellik kullanıldığında, bu konudan bahsedeceğiz.

### Python

```python
# For example
pip install polars[numpy, fsspec]
```

| Etiket     | Açıklama                                                                                                                                                                    |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| all        | Tüm isteğe bağlı bağımlılıkları kurun (aşağıdakilerin tümü)                                                                                                                 |
| pandas     | Verileri Pandas Dataframelere/Serilere dönüştürmek için Pandas ile kurun                                                                                                    |
| numpy      | Verileri numpy dizilerine ve dizilerden (array) dönüştürmek için numpy ile yükle                                                                                            |
| pyarrow    | PyArrow kullanarak veri formatlarını okuma                                                                                                                                  |
| fsspec     | Uzak dosya sistemlerinden okuma desteği                                                                                                                                     |
| connectorx | SQL veritabanlarından okuma desteği                                                                                                                                         |
| xlsx2csv   | Excel dosyalarından okuma desteği                                                                                                                                           |
| deltalake  | Delta Lake Tablolarından okuma desteği                                                                                                                                      |
| timezone   | Saat dilimi desteği, yalnızca şu durumlarda gereklidir: <br/>1. Python < 3.9 kullanıyorsanız ve/veya <br/>2. Windows kullanıyorsanız, aksi halde hiçbir bağımlılık kurulmaz |

`Rust`bağımlılıkları hakkında bilgi almak için [bu sayfayı](https://pola-rs.github.io/polars-book/user-guide/installation/#rust) ziyaret edin.

| << Önceki Bölüm      | Sonraki Bölüm >>                                 |
|:--------------------:|:------------------------------------------------:|
| [Giriş](01_giris.md) | [ Veri Tipleri](03_Kavramlar/01_veri_tipleri.md) |
