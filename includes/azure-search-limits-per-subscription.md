Yalnızca her katmanını izin verilen hizmetler sayısı sınırlı belirli bir katman, sağlanan her biri bir abonelik içindeki birden fazla hizmet oluşturabilirsiniz. Örneğin, 12 temel katmana adresindeki ve başka bir 12 Hizmetleri aynı abonelik içindeki S1 katmanı adresindeki kadar oluşturabilirsiniz. Katmanları hakkında daha fazla bilgi için bkz: [bir SKU katmanı için Azure Search seçin veya](../articles/search/search-sku-tier.md).

Maksimum hizmet sınırları, istek üzerine yükseltilebilir. Daha fazla hizmet aynı abonelik içindeki ihtiyacınız varsa Azure desteğine başvurun.

| Kaynak | Ücretsiz | Temel | S1 | S2 | S3 | S3 HD <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| En fazla Hizmetleri |1 |12 |12 |6 |6 |6 |
| SU en fazla ölçek <sup>2</sup> |YOK <sup>3</sup> |3 SU <sup>4</sup> |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> S3 HD desteklemiyor [dizin oluşturucular](../articles/search/search-indexer-overview.md) şu anda. 

<sup>2</sup> arama birimi (SU) ya da ayrılan birimler, faturalama bir *çoğaltma* veya *bölüm*. Depolama, dizin oluşturma ve sorgu işlemleri için her iki kaynağın gerekir. Arama birimi nasıl hesaplanır plus grafikte maksimum sınırları altında kalmak geçerli kombinasyon hakkında daha fazla bilgi için bkz: [ölçeklendirme sorgu ve dizin iş yükleri için kaynak düzeylerinin](../articles/search/search-capacity-planning.md). 

<sup>3</sup> serbest birden çok abone tarafından kullanılan paylaşılan kaynakları dayanır. Bu katman tek bir abone için ayrılmış kaynak yok. Bu nedenle, en fazla ölçek uygulanamaz olarak işaretlenir.

<sup>4</sup> basic bir sabit disk bölümü vardır. Bu katman ek SUs artırılmış sorgu iş yükleri için daha fazla çoğaltmaları tahsis etmek için kullanılır.

