

- A Serisi ve Av2 Serisi VM'ler, farklı donanım türleri ve işlemciler üzerinde dağıtılabilir. Dağıtıldığı donanımdan bağımsız olarak, çalışan örneğe tutarlı işlemci performansı sunmak için boyut donanıma göre genişletilir. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın.

- D Serisi VM'ler, daha yüksek işlem gücüne ve geçici süreli disk performansına ihtiyaç duyan uygulamaları çalıştıracak şekilde tasarlanmıştır. D Serisi VM'ler daha hızlı işlemcilere, daha yüksek bellek-vCPU oranına ve geçici disk için katı hal sürücüsüne (SSD) sahiptir. Ayrıntılı bilgi için Azure blogundaki [Yeni D Serisi Sanal Makine Boyutları](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) duyurusunu inceleyin.

- Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- Temel katman boyutları genelde geliştirme iş yükleri ve yük dengeleme, otomatik ölçeklendirme veya bellek kullanımı yoğun sanal makineler gerektirmeyen diğer uygulamalar içindir. Üretim uygulamaları için daha uygun VM boyutları hakkında daha fazla bilgi için bkz. (Sanal makine boyutları)[virtual-machines-size-specs.md] ve VM fiyatlandırma bilgileri için bkz. [Sanal Makine Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="b-series-preview"></a>B-serisi (Önizleme)

> [!NOTE] 
> Kullanım koşullarını kabul ediyorum koşuluyla önizlemeleri için kullanılabilir hale getirilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> Bu önizleme aşağıdaki bölgeler için sınırlı olacaktır:
> - ABD - Batı 2
> - ABD - Doğu
> - Europe - Batı
> - Asya Pasifik - Southeast

B-serisi burstable VM'ler CPU tam performansını sürekli olarak, web sunucuları gibi küçük veritabanları ve geliştirme gerekir ve ortamlarında test iş yükleri için idealdir. Bu iş yükleri genellikle burstable performans gereksinimleri vardır. B-serisi bu müşterilerin VM boyutu VM'i temel performansını sayısından az kullanırken kredilerinizin tamamını oluşturmak VM örneğine izin veren bir fiyat conscience temel performans satın almanıza olanak sağlar. VM kredi birikmiş, VM uygulamanız daha yüksek CPU performans gerektirdiğinde % 100 CPU kullanarak VM'ın temel veri bloğu.


| Boyut             | Sanal işlemci  | Bellek: GiB | Yerel SSD: GiB | Bir çekirdek, temel performans | Bankaya nakledilen krediler / saat | Max krediler Bankaya nakledilen | Maksimum veri diskleri | En fazla yerel disk perf: IOP / MB/sn | En büyük disk perf Histogramı: IOP / MB/sn | En fazla NIC |          
|---------------|-------------|----------------|----------------------------|-----------------------|--------------------|----------------|----------------------------------------|-------------------------------------------|-------------------------------------------|----------|
| Standard_B1s  | 1           | 1              | 4                          | %10                   | 6                  | 144            | 2                                      | 400 / 10                                  | 320 / 10                                  | 2  |
| Standard_B1ms | 1           | 2              | 4                          | %20                   | 12                 | 288            | 2                                      | 800 / 10                                  | 640 / 10                                  | 2  |
| Standard_B2s  | 2           | 4              | 8                          | 40%                   | 24                 | 576            | 4                                      | 1600 / 15                                 | 1280 / 15                                 | 3  |
| Standard_B2ms | 2           | 8              | 16                         | 60%                   | 36                 | 864            | 4                                      | 2400 / 22.5                               | 1920 / 22.5                               | 3  |
| Standard_B4ms | 4           | 16             | 32                         | 90%                   | 54                 | 1296           | 8                                      | 3600 / 35                                 | 2880 / 35                                 | 4  |
| Standard_B8ms | 8           | 32             | 64                         | 135%                  | 81                 | 1944           | 16                                     | 4320 / 50                                 | 4320 / 50                                 | 4  |


## <a name="dsv3-series"></a>Dsv3 serisi

ACU: 160-190

Dsv3 serisi boyutları, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir ve premium depolama kullanır. Dsv3 serisi boyutları, üretim iş yüklerinin çoğu için uygun bir vCPU, bellek ve geçici depolama kombinasyonu sunar.


| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / Beklenen ağ performansı (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4,000 / 32 (50)                                                       | 3200/48                                | 2/orta                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8,000 / 64 (100)                                                      | 6400/96                                | 2/orta                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16,000 / 128 (200)                                                    | 12.800/192                              | 4/yüksek                                       |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32,000 / 256 (400)                                                    | 25.600/384                              | 8/yüksek                                       |
| Standard_D32s_v3 | 32     | 128          | 256            | 32             | 64,000 / 512 (800)                                                    | 51.200/768                              | 8 / çok yüksek                                       |
| Standard_D64s_v3 | 64     | 256          | 512            | 32             | 128,000 / 1024 (1600)                                                    | 80,000 / 1200                              | 8 / çok yüksek                                       |


## <a name="dv3-series"></a>Dv3 serisi

ACU: 160-190

Dv3 serisi boyutları, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Dv3 serisi boyutları, üretim iş yüklerinin çoğu için uygun bir vCPU, bellek ve geçici depolama kombinasyonu sunar.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için Dsv3 boyutlarını kullanın. Dsv3 boyutları için fiyatlandırma ve faturalandırma oranları Dv3 serisi ile aynıdır. 


| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3000/46/23                                               | 2/orta                 |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6000/93/46                                               | 2/orta                 |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12000/187/93                                             | 4/yüksek                     |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24000/375/187                                            | 8/yüksek                     |
| Standard_D32_v3 | 32        | 128          | 800            | 32             | 48000/750/375                                            | 8 / çok yüksek                     |
| Standard_D64_v3 | 64        | 256          | 1600            | 32             | 96000/1000/500                                            | 8 / çok yüksek                     |


## <a name="dsv2-series"></a>DSv2 serisi

ACU: 210-250

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / Beklenen ağ performansı (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4000/32 (43) |3200/48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |4 |8000/64 (86) |6400/96 |2 / 1500 |
| Standard_DS3_v2 |4 |14 |28 |8 |16.000/128 (172) |12.800/192 |4 / 3000 |
| Standard_DS4_v2 |8 |28 |56 |16 |32.000/256 (344) |25.600/384 |8 / 6000 |
| Standard_DS5_v2 |16 |56 |112 |32 |64.000/512 (688) |51.200/768 |8 / 6000 - 12000 &#8224;|



## <a name="dv2-series"></a>Dv2 Serisi

ACU: 210-250

| Boyut              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3,5         | 50             | 3000/46/23                                           | 2/2x500                         | 2 / 750                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000/93/46                                           | 4/4x500                         | 2 / 1500                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12000/187/93                                         | 8/8x500                         | 4 / 3000                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24000/375/187                                        | 16/16x500                       | 8 / 6000                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48000/750/375                                        | 32/32x500                       | 8 / 6000 - 12000 &#8224;          |


<br>

## <a name="ds-series"></a>DS serisi

ACU: 160

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / Beklenen ağ performansı (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4000/32 (43) |3200/32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |4 |8000/64 (86) |6400/64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |8 |16.000/128 (172) |12.800/128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |16 |32.000/256 (344) |25.600/256 |8 / 4000 |

<br>

## <a name="d-series"></a>D Serisi 

ACU: 160

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000/46/23                                           | 2/2x500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000/93/46                                           | 4/4x500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12000/187/93                                         | 8/8x500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24000/375/187                                        | 16/16x500                       | 8 / 4000                     |

<br>


## <a name="av2-series"></a>Av2 Serisi

ACU: 100

| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000/20/10                                           | 2/2x500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000/40/20                                           | 4/4x500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000/80/40                                           | 8/8x500               | 4 / 1000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000/160/80                                          | 16/16x500             | 8 / 2000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000/40/20                                           | 4/4x500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000/80/40                                           | 8/8x500               | 4 / 1000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000/160/80                                          | 16/16x500             | 8 / 2000                     |

<br>

## <a name="a-series"></a>A Serisi

ACU: 50-100

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maks NIC / Beklenen ağ performansı (Mbps)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0,768 |20 |1 |1x500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2x500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4x500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8x500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16x500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4x500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8x500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16x500 |4 / 2000 |
<br>

*A0 boyutunun fiziksel donanım üzerindeki abone sayısı planlanandan fazladır. Yalnızca bu boyutta diğer müşteri dağıtımları, çalışan iş yükünüzün performansını etkileyebilir. Göreli performans aşağıda beklenen temel düzey olarak belirtilmiştir ve %15 oranında değişiklik gösterebilir.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4, CLI ve PowerShell kullanarak
Klasik dağıtım modelinde bazı VM boyutu adları CLI ve PowerShell'dekilerden biraz farklıdır:

* Standard_A0 = Çok Küçük 
* Standard_A1 = Küçük
* Standard_A2 = Orta
* Standard_A3 = Büyük
* Standard_A4 = Çok Büyük

## <a name="basic-a"></a>Temel A

|Boyut – Boyut\Ad | Sanal işlemci |Bellek|NIC'ler (Maks.)|En fazla geçici disk boyutu |En çok, veri disklerinin her biri 1.023 GB’tır)|En çok, IOPS (disk başına 300)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1x300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2x300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4x300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8x300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16x300|
