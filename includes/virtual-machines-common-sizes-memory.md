---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 7984172c2b66f2b09e31c646b111e4b9d04fce2b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344283"
---
Bellek, ilişkisel veritabanı sunucuları, Orta veya büyük boyutlu önbellekler ve bellek içi analiz için harika olan yüksek bellek CPU oranı VM boyutları teklifi en iyi duruma getirilmiş. Bu makalede, Vcpu, veri diskleri ve NIC yanı sıra depolama aktarım hızı ve ağ bant genişliği için bu gruplandırma her boyutundaki sayısı hakkında bilgi sağlar. 

* M serisi, en yüksek vCPU sayısını (128'e kadar Vcpu) ve bulutta herhangi bir VM en büyük bellek (3,8 tib'a kadar) sunar.  Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için idealdir.

* Dv2 serisi, G serisi ve DSv2/GS ortaklarınıza daha hızlı Vcpu, daha iyi geçici depolama performansı, talep ya da daha yüksek bellek taleplerine sahip uygulamalar için idealdir. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.

* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil temel 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) 2,4 GHz veya E5-2673 v4 (Broadwell) 2,3 GHz işlemcileri ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

* Ev3 serisi özellikleri E5-2673 v4 (Broadwell) 2,3 GHz işlemci en genel amaçlı iş yükleri için daha iyi bir değer önerisi sağlama yanı sıra diğer birçok bulut ile genel amaçlı sanal hizalama Ev3 getirmek hiper iş parçacıklı bir yapılandırmada.  Disk ve ağ sınırlarını hiper iş parçacıklı Git ile hizalamak için çekirdek başına temelinde ayarlanmış durumdayken (Başlangıç 7 GiB/vCPU 8 GiB/vCPU) bellek genişletildi.  Ev3, yüksek bellek VM boyutları D/Dv2 ailelerinin kadar izleme olur.

* Azure Compute, belirli bir donanım türüyle sınırlanmış ve tek bir müşteriye ayrılmış olan sanal makine boyutları sunar.  Bu sanal makine boyutları, uyumluluk ve düzenleme gereksinimleri gibi öğeler içeren iş yükleri nedeniyle diğer müşterilerden yüksek ölçüde yalıtıma ihtiyaç duyan iş yükleri için idealdir.  Müşteriler daha fazla yalıtılmış bu sanal makinelerin kaynakları kullanarak alt bölümlere ayırmak de seçebilir [iç içe sanal makineleri için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).  Lütfen yalıtılmış VM seçeneklerinizi için sanal makine aileleri tablolara bakın.

## <a name="esv3-series"></a>Esv3 serisi 

ACU: 160-190 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

ESv3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir ve premium depolama kullanır. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.


| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4,000 / 32 (50)                                                       | 3200/48                                | 2 / 1,000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8,000 / 64 (100)                                                      | 6400/96                                | 2 / 2,000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16,000 / 128 (200)                                                    | 12.800/192                              | 4 / 4,000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32,000 / 256 (400)                                                    | 25.600/384                              | 8 / 8,000                                       |
| Standard_E20s_v3                   | 20     | 160         | 320            | 32             | 40,000 / 320 (400)                                                    | 32,000 / 480                              | 8 / 10,000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64,000 / 512 (800)                                                    | 51.200/768                              | 8 / 16,000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |


<sup>1</sup> Esv3 serisi sanal makineler özellik Intel® Hyper-Threading Teknolojisi.

<sup>2</sup> sınırlı kullanılabilir çekirdek boyutu.

<sup>3</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.


## <a name="ev3-series"></a>Ev3 serisi 

ACU: 160 - 190 <sup>1</sup>

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

Ev3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için ESv3 boyutlarını kullanın. ESv3 boyutları için fiyatlandırma ve faturalandırma oranları Ev3 serisi ile aynıdır. 


| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1,000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8,000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8 / 10,000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16,000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |

<sup>1</sup> Ev3 serisi sanal makineler özellik Intel® Hyper-Threading Teknolojisi.

<sup>2</sup> sınırlı kullanılabilir çekirdek boyutu.

<sup>3</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.


## <a name="m-series"></a>M serisi 

ACU: 160-180 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

Hızlandırıcı yazma:  [Destekleniyor](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| İşler için Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218.75 | 256  | 8  | 10,000 / 100 (793)  | 5,000  / 125 | 4 / 2,000 |
| İşler için Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437.5  | 512  | 16 | 20,000 / 200 (1,587) | 10.000/250 | 8 / 4,000 |
| Standard_M32ts | 32 | 192    | 1,024 | 32 | 40,000 / 400 (3,174) | 20.000/500 | 8 / 8,000 |
| Standard_M32ls | 32 | 256    | 1,024 | 32 | 40,000 / 400 (3,174) | 20.000/500 | 8 / 8,000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1,024 | 32 | 40,000 / 400 (3,174) | 20.000/500 | 8 / 8,000 |
| İşler için standart_m64s  | 64 | 1,024   | 2,048 | 64 | 80,000 / 800 (6,348)| 40.000/1000 | 8 / 16,000          |
| İşler için standart_m64ls  | 64 | 512    | 2,048 | 64 | 80,000 / 800 (6,348) | 40.000/1000 | 8 / 16,000 |
| İşler için standart_m64ms&nbsp;<sup>3</sup>  | 64   | 1,792 | 2,048 | 64 | 80,000 / 800 (6,348)| 40.000/1000 | 8 / 16,000          |
| İşler için standart_m128s&nbsp;<sup>2</sup> | 128  | 2,048        | 4,096  | 64 | 160,000 / 1,600 (12,696) | 80.000/2000                            | 8 / 30,000          |
| İşler için standart_m128ms&nbsp;<sup>2&nbsp;3&nbsp;4</sup> | 128  | 3,892  | 4,096 | 64 | 160,000 / 1,600 (12,696) | 80.000/2000                            | 8 / 30,000          |
| Standard_M64   | 64  | 1,024 | 7,168  | 64 | 80,000  / 800  (1,228) | 40.000/1000 | 8 / 16,000 |
| Standard_M64m  | 64  | 1,792 | 7,168  | 64 | 80,000  / 800  (1,228) | 40.000/1000 | 8 / 16,000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2,048 | 14,336 | 64 | 250,000 / 1,600 (2,456) | 80.000/2000 | 8 / 32,000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3,892 | 14,336 | 64 | 250,000 / 1,600 (2,456) | 80.000/2000 | 8 / 32,000 |



<sup>1</sup> M serisi sanal makineler özellik Intel® Hyper-Threading Teknolojisi

<sup>2</sup> 64'ten fazla şu desteklenen konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya LIS 4.2.1 ile Oracle Linux 7.3.

<sup>3</sup> sınırlı kullanılabilir çekirdek boyutu.

<sup>4</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.
<br>

## <a name="gs-series"></a>GS serisi 

ACU: 180 - 240 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10.000/100 (264) |5000/125 |2 / 2000 |
| Standard_GS2 |4 |56 |112 |16 |20.000/200 (528) |10.000/250 |2 / 4000 |
| Standard_GS3 |8 |112 |224 |32 |40.000/400 (1056) |20.000/500 |4 / 8000 |
| Standard_GS4&nbsp;<sup>3</sup> |16 |224 |448 |64 |80.000/800 (2112) |40.000/1000 |8 / 16000 |
| Standard_GS5&nbsp;<sup>2&nbsp;3</sup> |32 |448 |896 |64 |160.000/1600 (4224) |80.000/2000 |8 / 20000 |

<sup>1</sup> maksimum disk aktarım hızı (IOPS veya MB/sn) GS serisi VM ile sınırlı olabilir sayısı, boyutu ve bölümleme türüyle ekli disklerin. Ayrıntılar için bkz [yüksek performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>2</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.

<sup>3</sup> sınırlı kullanılabilir çekirdek boyutu.

<br>

## <a name="g-series"></a>G Serisi

ACU: 180 - 240

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 8/8x500                       | 2 / 2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 16/16x500                       | 2 / 4000                     |
| Standard_G3  | 8         | 112         | 1536          | 24000/375/187                                        | 32/32x500                     | 4 / 8000                |
| Standard_G4  | 16        | 224         | 3072          | 48000/750/375                                        | 64/64x500                     | 8 / 16000          |
| Standard_G5&nbsp;<sup>1</sup> | 32        | 448         | 6144          | 96000/1500/750                                       | 64/64x500                     | 8 / 20000           |

<sup>1</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.
<br>

## <a name="dsv2-series-11-15"></a>DSv2-series 11-15

ACU: 210 - 250 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000/64 (72) |6400/96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16.000/128 (144) |12.800/192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32.000/256 (288) |25.600/384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64.000/512 (576) |51.200/768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80.000/640 (720) |64.000/960 |8 / 25000&nbsp;<sup>4</sup>

<sup>1</sup> maksimum disk aktarım hızı (IOPS veya MB/sn) DSv2 serisi VM ile sınırlı olabilir sayısı, boyutu ve bölümleme türüyle ekli disklerin.  Ayrıntılar için bkz [yüksek performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).  
<sup>2</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.  
<sup>3</sup> sınırlı kullanılabilir çekirdek boyutu.  
<sup>4</sup> 25.000 Mbps, hızlandırılmış ağ. 

<br>

## <a name="dv2-series-11-15"></a>Dv2 serisi 11-15

ACU: 210 - 250

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Boyut              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 12000          |
| İşler için standart_d15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60000/937/468                                        | 64 / 64 x 500                       | 8 / 25000&nbsp;<sup>2</sup> |

<sup>1</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.  
<sup>2</sup> 25.000 Mbps, hızlandırılmış ağ. 