# GİRİŞ
Bu Kullanım Kılavuzu, [Polars DataFrame kitaplığına](https://github.com/pola-rs/polars) bir giriş niteliğindedir. Amacı, örnekler üzerinden geçerek ve diğer çözümlerle karşılaştırarak size `Polars`'ı tanıtmaktır. Bazı tasarım seçenekleri burada tanıtılmaktadır. Kılavuz ayrıca size Polars'ın optimum kullanımını tanıtacaktır.

`Polars` tamamen [Rust](https://www.rust-lang.org/)'ta yazılmış olsa da ve temel olarak Arrow'u ([yerel arrow2 Rust uygulaması](https://github.com/jorgecarleitao/arrow2)) kullanıyor olsa da, bu kılavuzda sunulan örneklerde çoğunlukla onun üst düzey dil bağlamaları kullanılacaktır. Daha yüksek düzey bağlamalar, çekirdek kitaplıkta uygulanan işlevsellik için yalnızca ince bir sarmalayıcı görevi görür.

[Pandas](https://pandas.pydata.org/) kullanıcıları için, [Python paketimiz](https://pypi.org/project/polars/) `Polars`'ı kullanmaya başlamanın en kolay yolunu sunacaktır.

## Felsefe
`Polars`'ın amacı, aşağıdaki özelliklere sahip ışık hızında bir `VeriÇerçevesi (DataFrame)` kitaplığı sağlamaktır:
* Makinenizdeki tüm kullanılabilir çekirdekleri kullanır.
* Gereksiz iş/bellek ayırmalarını azaltmak için sorguları optimize eder.
* Kullanılabilir RAM'inizden çok daha büyük veri kümelerini işler.
* Tutarlı ve öngörülebilir bir API'ye sahiptir.
* Kesin bir şeması vardır (sorguyu çalıştırmadan önce veri türleri bilinmelidir)

Polars, kendisine C/C++ performansı sağlayan ve bir sorgu motorundaki kritik performans parçalarını tam olarak kontrol etmesine izin veren Rust'ta yazılmıştır.

Bu nedenle `Polars`, aşağıdakiler için büyük çaba sarf eder:
* Gereksiz kopyaları azaltın.
* Bellek önbelleğini verimli bir şekilde çaprazlayın.
* Paralellikte çekişmeyi en aza indirin.
* Verileri parçalar halinde işleyin.
* Bellek ayırmalarını yeniden kullanın.
