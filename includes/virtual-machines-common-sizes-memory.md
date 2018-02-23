
Bellek, ilişkisel veritabanı sunucuları, Orta ve büyük önbellekler ve bellek içi analizi için harika bir yüksek bellek CPU oranı VM boyutları teklif en iyi duruma getirilmiş. Bu makale Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmadaki her boyutu için depolama üretilen iş ve ağ bant sayısı hakkında bilgi sağlar. 

* M-yüksek vCPU sayısını (en fazla 128 Vcpu'lar) ve en büyük bellek (en fazla 3.8 Tıb) bulutta herhangi bir VM sunar.  Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için idealdir.

* Dv2 Serisi, D Serisi, G Serisi ve bunlara karşılık gelen DS/GS modelleri daha hızlı vCPU'ya, daha iyi geçici depolama performansına veya daha yüksek belleğe ihtiyaç duyan uygulamalar için idealdir.  Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.

* D Serisi VM'ler, daha yüksek işlem gücüne ve geçici süreli disk performansına ihtiyaç duyan uygulamaları çalıştıracak şekilde tasarlanmıştır. D Serisi VM'ler daha hızlı işlemcilere, daha yüksek bellek-vCPU oranına ve geçici depolama için katı hal sürücüsüne (SSD) sahiptir. Ayrıntılı bilgi için Azure blogundaki [Yeni D Serisi Sanal Makine Boyutları](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) duyurusunu inceleyin.

* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.


## <a name="esv3-series-sup1sup"></a>Esv3-serisi <sup>1</sup>

ACU: 160-190

ESv3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir ve premium depolama kullanır. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.


| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3  | 2      | 16          | 32             | 4              | 4,000 / 32 (50)                                                       | 3200/48                                | 2 / 1,000                                   |
| Standard_E4s_v3  | 4      | 32          | 64             | 8              | 8,000 / 64 (100)                                                      | 6400/96                                | 2 / 2,000                                   |
| Standard_E8s_v3  | 8      | 64          | 128            | 16             | 16,000 / 128 (200)                                                    | 12.800/192                              | 4 / 4,000                                       |
| Standard_E16s_v3 | 16     | 128         | 256            | 32             | 32,000 / 256 (400)                                                    | 25.600/384                              | 8 / 8,000                                       |
| Standard_E32s_v3 <sup>2</sup> | 32     | 256         | 512            | 32             | 64,000 / 512 (800)                                                    | 51.200/768                              | 8 / 16,000                             |
| Standard_E64s_v3 <sup>2,3</sup> | 64     | 432         | 864            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |

<sup>1</sup> Esv3-serisi VM'in özellik Intel® Hyper-Threading Teknolojisi

<sup>2</sup> kısıtlı çekirdek boyutları kullanılabilir 

<sup>3</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.
## <a name="ev3-series-sup1sup"></a>Ev3-serisi <sup>1</sup>

ACU: 160-190 

Ev3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için ESv3 boyutlarını kullanın. ESv3 boyutları için fiyatlandırma ve faturalandırma oranları Ev3 serisi ile aynıdır. 


| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1,000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8,000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16,000                 |
| Standard_E64_v3<sup>2</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |

<sup>1</sup> Ev3-serisi VM'in özellik Intel® Hyper-Threading Teknolojisi

<sup>2</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.

## <a name="m-series-sup1sup"></a>M-serisi <sup>1</sup>

ACU: 160-180

| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M64s  | 64   | 1024        | 2048           | 64             | 80,000 / 800 (6348)       | 40.000/1000                            | 8 / 16000          |
| Standard_M64ms  | 64   | 1792        | 2048           | 64             | 80,000 / 800 (6348)       | 40.000/1000                            | 8 / 16000          |
| Standard_M128s <sup>2, 3</sup> | 128  | 2048        | 4096           | 64             | 160,000 / 1,600 (12,696) | 80.000/2000                            | 8 / 30000          |
| Standard_M128ms <sup>2, 3, 4</sup> | 128  | 3800        | 4096           | 64             | 160,000 / 1,600 (12,696) | 80.000/2000                            | 8 / 30000          |

<sup>1</sup> M-serisi VM'in özelliği Intel® Hyper-Threading Teknolojisi

<sup>2</sup> 64 taneden fazla vCPU'ın bu desteklenen konuk işletim sistemleri birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya Oracle Linux 7.3 LIS 4.2.1 ile 

<sup>3</sup> kısıtlı çekirdek boyutları kullanılabilir.

<sup>4</sup>örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.
<br>

## <a name="gs-series-sup1sup"></a>GS serisi <sup>1</sup>

ACU: 180 - 240

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10.000/100 (264) |5000/125 |2 / 2000 |
| Standard_GS2 |4 |56 |112 |16 |20.000/200 (528) |10.000/250 |2 / 4000 |
| Standard_GS3 |8 |112 |224 |32 |40.000/400 (1056) |20.000/500 |4 / 8000 |
| Standard_GS4 <sup>3</sup> |16 |224 |448 |64 |80.000/800 (2112) |40.000/1000 |8 / 16000 |
| Standard_GS5 <sup>2, 3</sup> |32 |448 |896 |64 |160.000/1600 (4224) |80.000/2000 |8 / 20000 |

<sup>1</sup> GS serisi VM ile (IOPS veya MB/sn) mümkün sayısı ile sınırlı en fazla disk verimlilik boyutu ve bağlı diskler şeritleme. Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md). 

<sup>2</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış. 

<sup>3</sup> kısıtlı çekirdek boyutları kullanılabilir 

<br>

## <a name="g-series"></a>G Serisi

ACU: 180 - 240

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 8/8x500                       | 2 / 2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 16/16x500                       | 2 / 4000                     |
| Standard_G3  | 8         | 112         | 1536          | 24000/375/187                                        | 32/32x500                     | 4 / 8000                |
| Standard_G4  | 16        | 224         | 3072          | 48000/750/375                                        | 64/64x500                     | 8 / 16000          |
| Standard_G5 <sup>1</sup> | 32        | 448         | 6144          | 96000/1500/750                                       | 64/64x500                     | 8 / 20000           |

<sup>1</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.
<br>


## <a name="dsv2-series-sup1sup"></a>DSv2 serisi <sup>1</sup>

ACU: 210 - 250

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |8 |8000/64 (72) |6400/96 |2 / 1500 |
| Standard_DS12_v2 |4 |28 |56 |16 |16.000/128 (144) |12.800/192 |4 / 3000 |
| Standard_DS13_v2 |8 |56 |112 |32 |32.000/256 (288) |25.600/384 |8 / 6000 |
| Standard_DS14_v2 |16 |112 |224 |64 |64.000/512 (576) |51.200/768 |8 / 12000 |
| Standard_DS15_v2 <sup>2</sup> |20 |140 |280 |64 |80.000/640 (720) |64.000/960 |8 / 25000 <sup>3</sup>

<sup>1</sup> DSv2 serisi VM ile (IOPS veya MB/sn) mümkün sayısı ile sınırlı en fazla disk verimlilik boyutu ve bağlı diskler şeritleme.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md).

<sup>2</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış. 

<sup>3</sup> hızlandırılmış ağ ile 25000 MB/sn.

<br>

## <a name="dv2-series"></a>Dv2 Serisi

ACU: 210 - 250

| Boyut              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 12000          |
| Standard_D15_v2 <sup>1</sup> | 20        | 140         | 1000          | 60000/937/468                                        | 64 / 64 x 500                       | 8 / 25000 <sup>2</sup> |

<sup>1</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış. 

<sup>2</sup> hızlandırılmış ağ ile 25000 MB/sn.

<br>

## <a name="ds-series-sup1sup"></a>DS serisi <sup>1</sup>

ACU: 160

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000/64 (72) |6400/64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000/128 (144) |12.800/128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000/256 (288) |25.600/256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000/512 (576) |51.200/512 |8 / 8000 |

<sup>1</sup> DS serisi VM ile (IOPS veya MB/sn) mümkün sayısı ile sınırlı en fazla disk verimlilik boyutu ve bağlı diskler şeritleme.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md).


## <a name="d-series"></a>D Serisi

ACU: 160

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 8000                |

<br>

