<!--
Used in:
sql-database-elastic-pool.md 
-->

 
### <a name="basic-elastic-pool-limits"></a>Temel esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Eklenen depolama havuzu (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz (GB) başına en fazla depolama seçenekleri | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına en fazla veritabanı | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Havuz başına maks. eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Havuz başına maks. eş zamanlı oturum | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Veritabanı başına minimum Edtu seçenekleri | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Veritabanı başına maksimum Edtu seçenekleri | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Veritabanı başına maks. depolama alanı (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standart esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Eklenen depolama havuzu (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Havuz (GB) başına en fazla depolama seçenekleri * | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | Yok | 
| Havuz başına en fazla veritabanı | 100 | 200 | 500 | 500 | 500 | 500 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 |
| Havuz başına maks. eş zamanlı oturum | 100 | 200 | 400 | 600 | 800 | 1600 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı ** başına Min Edtu'lar seçenekleri | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı ** başına maksimum Edtu seçenekleri | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Veritabanı başına maks. depolama alanı (GB)* | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Standart esnek havuz limitleri (devam) 

| Havuz başına eDTU | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Eklenen depolama havuzu (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Havuz (GB) başına en fazla depolama seçenekleri * | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | 
| Havuz başına en fazla veritabanı | 500 | 500 | 500 | 500 | 500 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 2400 | 3200 | 4000 | 5000 | 6000 |
| Havuz başına maks. eş zamanlı oturum | 2400 | 3200 | 4000 | 5000 | 6000 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı ** başına Min Edtu'lar seçenekleri | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı ** başına maksimum Edtu seçenekleri | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Veritabanı (GB) başına en fazla depolama seçenekleri * | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Premium esnek havuz sınırları

| Havuz başına eDTU | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Havuz (GB) başına en fazla depolama seçenekleri * | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| En fazla bellek içi OLTP depolama havuzu (GB) | 1 | 2 | 4 | 10 | 12 | 
| Havuz başına en fazla veritabanı | 50 | 100 | 100 | 100 | 100 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 200 | 400 | 800 | 1600 | 2400 | 
| Havuz başına maks. eş zamanlı oturum | 200 | 400 | 800 | 1600 | 2400 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına Min. eDTU | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Veritabanı başına Maks. eDTU | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Veritabanı başına maks. depolama alanı (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Premium esnek havuz limitleri (devam) 

| Havuz başına eDTU | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Havuz (GB) başına en fazla depolama seçenekleri * | 2048 | 2560 | 3072 | 3548 | 4096|
| En fazla bellek içi OLTP depolama havuzu (GB) | 16 | 20 | 24 | 28 | 32 |
| Havuz başına en fazla veritabanı | 100 | 100 | 100 | 100 | 100 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 3200 | 4000 | 4800 | 5600 | 6400 |
| Havuz başına maks. eş zamanlı oturum | 3200 | 4000 | 4800 | 5600 | 6400 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına minimum Edtu seçenekleri | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Veritabanı başına maksimum Edtu seçenekleri | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Veritabanı başına maks. depolama alanı (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Premium RS elastik havuz sınırları

| Havuz başına eDTU | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 250 | 500 | 750 | 750 |
| Havuz (GB) başına en fazla depolama seçenekleri * | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 
| En fazla bellek içi OLTP depolama havuzu (GB) | 1 | 2 | 4 | 10 |
| Havuz başına en fazla veritabanı | 50 | 100 | 100 | 100 |
| Havuz başına maks. eş zamanlı çalışan (istek) | 200 | 400 | 800 | 1600 |
| Havuz başına maks. eş zamanlı oturum | 200 | 400 | 800 | 1600 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına minimum Edtu seçenekleri | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| Veritabanı başına maksimum Edtu seçenekleri | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Veritabanı başına maks. depolama alanı (GB)* | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/). Önizlemede bulunan depolama alanı miktarını büyük depolama boyutları ve ek maliyetlerden uygulayın. Ayrıntılar için bkz [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).
>
> \*Premium katmanındaki birden fazla 1 TB depolama alanı aşağıdaki bölgelerde şu anda kullanılabilir değil: Avustralya Doğu, Avustralya Güneydoğu, Orta Kanada, Doğu Kanada, Fransa Merkezi, Almanya merkezi Japonya, Doğu merkezi Kore, Güney Orta ABD, Güney Doğu Asya, BİZE East2 , Batı ABD, ABD kamu Virginia ve Batı Avrupa. 
>
>\*\*Veritabanı 200 Edtu ve daha yüksek başlayarak başına min/max Edtu'lar **standart** havuzları olan önizlemede.
>
