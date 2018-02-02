<!--
Used in:
sql-database-performance-guidance.md 
sql-database-single-database-resources.md 
-->

### <a name="basic-service-tier"></a>Temel hizmet katmanı
| **Performans düzeyi** | **Temel** |
| :--- | --: |
| Maks. DTU | 5 |
| Eklenen depolama (GB) | 2 |
| En fazla depolama seçenekleri (GB) | 2 |
| Maks. bellek içi OLTP depolama alanı (GB) |Yok |
| En fazla eşzamanlı çalışan (istek sayısı) | 30 |
| Maks. eş zamanlı oturum | 30 |
| Maks. eş zamanlı oturum | 300 |
|||

### <a name="standard-service-tier"></a>Standart hizmet katmanı
| **Performans düzeyi** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Maksimum Dtu ** | 10 | 20 | 50 | 100 |
| Eklenen depolama (GB) | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) * | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |
| En fazla eşzamanlı çalışan (istek sayısı)| 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum | 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standart hizmet katmanında (devam)
| **Performans düzeyi** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Maksimum Dtu ** | 200 | 400 | 800 | 1600 | 3000 |
| Eklenen depolama (GB) | 250 | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) * | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |Yok |
| En fazla eşzamanlı çalışan (istek sayısı)| 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum | 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium hizmet katmanı 
| **Performans düzeyi** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Eklenen depolama (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| En fazla depolama seçenekleri (GB) * | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| En fazla eşzamanlı çalışan (istek sayısı)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

### <a name="premium-rs-service-tier"></a>Premium RS hizmet katmanı 
| **Performans düzeyi** | **PRS1** | **PRS2** | **PRS4** | **PRS6** |
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 |
| Eklenen depolama (GB) | 500 | 500 | 500 | 500 |
| En fazla depolama seçenekleri (GB) * | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 |
| En fazla eşzamanlı çalışan (istek sayısı)| 200 | 400 | 800 | 1600 |
| Maks. eş zamanlı oturum | 200 | 400 | 800 | 1600 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 |
|||||||

> [!IMPORTANT]
> \* Mevcut depolama alanından büyük depolama alanları önizleme aşamasındadır ve ek maliyetler uygulanır. Ayrıntılar için bkz. [SQL Veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). 
>
>\*Premium katmanındaki birden fazla 1 TB depolama alanı aşağıdaki bölgelerde şu anda kullanılabilir değil: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Merkezi, Almanya Merkezi, Japonya Doğu, Japonya Batı, Kore Merkezi Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Birleşik Krallık Güney, Birleşik Krallık Batı, ABD East2, Batı ABD, ABD kamu Virginia ve Batı Avrupa. Bkz. [P11 P15 Geçerli Sınırlamalar](../articles/sql-database/sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 
>\*\*200 Dtu'lar ve daha yüksek standart başlangıç veritabanı başına maksimum Dtu önizlemede.
>
