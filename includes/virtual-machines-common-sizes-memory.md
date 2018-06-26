---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/22/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 15f21fd03b0373c189f3b6c4972280d128024217
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36943530"
---
Bellek, ilişkisel veritabanı sunucuları, Orta ve büyük önbellekler ve bellek içi analizi için harika bir yüksek bellek CPU oranı VM boyutları teklif en iyi duruma getirilmiş. Bu makale Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmadaki her boyutu için depolama üretilen iş ve ağ bant sayısı hakkında bilgi sağlar. 

* M-yüksek vCPU sayısını (en fazla 128 Vcpu'lar) ve en büyük bellek (en fazla 3.8 Tıb) bulutta herhangi bir VM sunar.  Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için idealdir.

* Dv2 serisi, G-serisi ve DSv2/GS ortaklarınıza daha hızlı Vcpu, daha iyi geçici depolama performansı, talep veya daha yüksek bellek taleplerini sahip uygulamalar için idealdir.  Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.


* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Üzerinde en son oluşturma dayalı 2.4 GHz Intel Xeon® E5-2673 v3 2.4 GHz (Haswell) veya E5-2673 v4 2.3 GHz (Broadwell) işlemcileri ve Intel Turbo artırma teknolojisi 2.0 ile 3.1 GHz gidebilirsiniz. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

* Ev3-serisi özellikleri E5-2673 v4 2.3 GHz (Broadwell) işlemci en genel amaçlı iş yükleri için daha iyi bir değer teklifinde sağlayarak ve hizalamasını çoğu bulut genel amaçlı VM'ler ile Ev3 getiren bir hiper iş parçacıklı yapılandırmada.  Disk ve ağ sınırlarını hiper iş parçacığı Git hizalamak için bir çekirdek başına temelinde ayarlanmış durumdayken bellek (Başlangıç 7 Gib'den/vCPU için 8 Gib'den/vCPU) genişletilmiştir.  Ev3 D/Dv2 aileleri yüksek bellek VM boyutlarını kadar izleyin ' dir.

* Azure işlem sunar sanal makine boyutları olan Isolated belirli donanım türlerine ve tek bir müşteriye ayrılmış.  Bu sanal makine boyutlarını uyumluluk ve Mevzuat gereklilikleri gibi öğeleri içeren iş yükleri için yüksek derecede diğer müşterilerden yalıtım gerektiren iş yükleri için uygundur.  Müşterileri de seçebilirsiniz daha fazla kullanarak bu yalıtılmış sanal makinelerin kaynakları ayırabilir [iç içe geçmiş sanal makineler için Azure desteği](https://azure.microsoft.com/en-us/blog/nested-virtualization-in-azure/).  Yalıtılmış VM seçeneklerinizi için sanal makine aileleri tablolara bakın.

## <a name="esv3-series"></a>Esv3-serisi 

ACU: 160-190 <sup>1</sup>

ESv3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir ve premium depolama kullanır. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.


| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4,000 / 32 (50)                                                       | 3200/48                                | 2 / 1,000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8,000 / 64 (100)                                                      | 6400/96                                | 2 / 2,000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16,000 / 128 (200)                                                    | 12.800/192                              | 4 / 4,000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32,000 / 256 (400)                                                    | 25.600/384                              | 8 / 8,000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64,000 / 512 (800)                                                    | 51.200/768                              | 8 / 16,000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |


<sup>1</sup> Esv3-serisi VM'in özellik Intel® Hyper-Threading Teknolojisi.

<sup>2</sup> kısıtlı çekirdek boyutları kullanılabilir.

<sup>3</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.


## <a name="ev3-series"></a>Ev3 serisi 

ACU: 160-190 <sup>1</sup>

Ev3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için ESv3 boyutlarını kullanın. ESv3 boyutları için fiyatlandırma ve faturalandırma oranları Ev3 serisi ile aynıdır. 


| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1,000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8,000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16,000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |
| Standard_E64i_v3&nbsp;<sup>2</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |

<sup>1</sup> Ev3-serisi VM'in özellik Intel® Hyper-Threading Teknolojisi.

<sup>2</sup> kısıtlı çekirdek boyutları kullanılabilir. 


## <a name="m-series"></a>M serisi 

ACU: 160-180 <sup>1</sup>

| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| M8ms&nbsp;<sup>3</sup>    | 8  | 218.75 | 256  | 8  | 10.000 / 100 (793)  | 5.000 / 125 | 4 / 2.000 |
| M16ms&nbsp;<sup>3</sup>   | 16 | 437.5  | 512  | 16 | 20.000 / 200 (1,587) | 10.000/250 | 8 / 4.000 |
| Standard_M32ts | 32 | 192    | 1,024 | 32 | 40.000 / 400 (3,174) | 20.000/500 | 8 / 8,000 |
| Standard_M32ls | 32 | 256    | 1,024 | 32 | 40.000 / 400 (3,174) | 20.000/500 | 8 / 8,000 |
| M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1,024 | 32 | 40.000 / 400 (3,174) | 20.000/500 | 8 / 8,000 |
| Standard_M64s  | 64 | 1,024   | 2.048 | 64 | 80,000 / 800 (6,348)| 40.000/1000 | 8 / 16,000          |
|Standard_M64ls  | 64 | 512    | 2.048 | 64 | 80,000 / 800 (6,348) | 40.000/1000 | 8 / 16,000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1,792 | 2.048 | 64 | 80,000 / 800 (6,348)| 40.000/1000 | 8 / 160,00          |
| Standard_M128s&nbsp;<sup>2,&nbsp;3</sup> | 128  | 2.048        | 4,096  | 64 | 160,000 / 1,600 (12,696) | 80.000/2000                            | 8 / 30,000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3,892  | 4,096 | 64 | 160,000 / 1,600 (12,696) | 80.000/2000                            | 8 / 30,000          |
| Standard_M64   | 64  | 1,024 | 7,168  | 64 | 80,000 / 800 (1,228) | 40.000 / 1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1,792 | 7,168  | 64 | 80,000 / 800 (1,228) | 40.000 / 1000 | 8 / 16000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2.048 | 14,336 | 64 | 250,000 / 1,600 (2,456) | 80,000 / 2000 | 8 / 32000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3,892 | 14,336 | 64 | 250,000 / 1,600 (2,456) | 80,000 / 2000 | 8 / 32000 |



<sup>1</sup> M-serisi VM'in özelliği Intel® Hyper-Threading Teknolojisi

<sup>2</sup> 64 taneden fazla vCPU'ın bu desteklenen konuk işletim sistemleri birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya Oracle Linux 7.3 LIS 4.2.1 ile.

<sup>3</sup> kısıtlı çekirdek boyutları kullanılabilir.

<sup>4</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.
<br>

## <a name="gs-series"></a>GS serisi 

ACU: 180-240 <sup>1</sup>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 |2 |28 |56 |8 |10.000/100 (264) |5000/125 |2 / 2000 |
| Standard_GS2 |4 |56 |112 |16 |20.000/200 (528) |10.000/250 |2 / 4000 |
| Standard_GS3 |8 |112 |224 |32 |40.000/400 (1056) |20.000/500 |4 / 8000 |
| Standard_GS4&nbsp;<sup>3</sup> |16 |224 |448 |64 |80.000/800 (2112) |40.000/1000 |8 / 16000 |
| Standard_GS5&nbsp;<sup>2,&nbsp;3</sup> |32 |448 |896 |64 |160.000/1600 (4224) |80.000/2000 |8 / 20000 |

<sup>1</sup> GS serisi VM ile (IOPS veya MB/sn) mümkün sayısı ile sınırlı en fazla disk verimlilik boyutu ve bağlı diskler şeritleme. Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md). 

<sup>2</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.

<sup>3</sup> kısıtlı çekirdek boyutları kullanılabilir.

<br>

## <a name="g-series"></a>G Serisi

ACU: 180 - 240

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 8/8x500                       | 2 / 2000                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 16/16x500                       | 2 / 4000                     |
| Standard_G3  | 8         | 112         | 1536          | 24000/375/187                                        | 32/32x500                     | 4 / 8000                |
| Standard_G4  | 16        | 224         | 3072          | 48000/750/375                                        | 64/64x500                     | 8 / 16000          |
| Standard_G5&nbsp;<sup>1</sup> | 32        | 448         | 6144          | 96000/1500/750                                       | 64/64x500                     | 8 / 20000           |

<sup>1</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.
<br>


## <a name="dsv2-series-11-15"></a>DSv2 serisi 11-15

ACU: 210-250 <sup>1</sup>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000/64 (72) |6400/96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16.000/128 (144) |12.800/192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32.000/256 (288) |25.600/384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64.000/512 (576) |51.200/768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80.000/640 (720) |64.000/960 |8 / 25000&nbsp;<sup>4</sup>


<sup>1</sup> DSv2 serisi VM ile (IOPS veya MB/sn) mümkün sayısı ile sınırlı en fazla disk verimlilik boyutu ve bağlı diskler şeritleme.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md).

<sup>2</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış.

<sup>3</sup> kısıtlı çekirdek boyutları kullanılabilir.

<sup>4</sup> hızlandırılmış ağ ile 25000 MB/sn.

<br>

## <a name="dv2-series-11-15"></a>Dv2-serisi 11-15

ACU: 210 - 250

| Boyut              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Max NIC'ler / beklenen ağ bant genişliği (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 12000          |
| İçin Standard_D15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60000/937/468                                        | 64 / 64 x 500                       | 8 / 25000&nbsp;<sup>2</sup> |

<sup>1</sup> örneği için tek bir müşteriye ayrılmış donanım için ayrılmış. 

<sup>2</sup> hızlandırılmış ağ ile 25000 MB/sn.



<br>



