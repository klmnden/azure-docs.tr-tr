Depolama, disk alanı ya da dizin veya belge sayısı *üst sınırı* ile kısıtlanır (hangisi önce gelirse).

| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Hizmet Düzeyi Sözleşmesi (SLA) |Hayır <sup>1</sup> |Evet |Evet |Evet |Evet |Evet |
| Bölüm başına depolama |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Hizmet başına bölüm |Yok |1 |12 |12 |12 |3 <sup>2</sup> |
| Bölüm boyutu |Yok |2 GB |25 GB |100 GB |200 GB |200 GB |
| Çoğaltmalar |Yok |3 |12 |12 |12 |12 |
| En fazla dizin |3 |5 |50 |200 |200 |Bölüm başına 1000 veya hizmet başına 3000 |
| En fazla dizin oluşturucu |3 |5 |50 |200 |200 |Dizin oluşturucu desteği yok |
| En fazla veri kaynağı |3 |5 |50 |200 |200 |Dizin oluşturucu desteği yok |
| En fazla belge |10,000 |1 milyon |Bölüm başına 15 milyon veya hizmet başına 180 milyon |Bölüm başına 60 milyon veya hizmet başına 720 milyon |Bölüm başına 120 milyon veya hizmet başına 1.4 milyar |Dizin başına 1 milyon veya bölüm başına 200 milyon |

<sup>1</sup> ücretsiz katmanı ve önizleme özellikleri hizmet düzeyi sözleşmelerine (SLA) ile gelen değil. Hizmetiniz için yeterli artıklık sağladığınızda tüm Faturalanabilir katmanları için SLA etkili olur. İki veya daha fazla çoğaltmaları için (okuma) sorgu SLA gereklidir. Üç veya daha fazla çoğaltmalar, sorgu ve dizin oluşturma (okuma-yazma) SLA için gereklidir. Bölüm sayısı bir SLA önem verilmez. 

<sup>2</sup> S3 HD’nin sabit sınırı 3 bölümdür ve S3’ün bölüm sınırından düşüktür. S3 HD dizin sınırı çok daha yüksek olduğu için daha düşük bir bölüm sınırı uygulanmaktadır. Hem işlem kaynakları (depolama ve işleme) hem de içerik (dizinler ve belgeler) için hizmet sınırları mevcut olduğu için içerik sınırına ilk önce ulaşılır.
