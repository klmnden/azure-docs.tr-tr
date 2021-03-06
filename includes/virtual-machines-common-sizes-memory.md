---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/16/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 9e145bc3a6824100409a0f6215152cdf70ec6777
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67501286"
---
Bellek, ilişkisel veritabanı sunucuları, Orta veya büyük boyutlu önbellekler ve bellek içi analiz için harika olan yüksek bellek CPU oranı VM boyutları teklifi en iyi duruma getirilmiş. Bu makalede, Vcpu, veri diskleri ve NIC yanı sıra depolama aktarım hızı ve ağ bant genişliği için bu gruplandırma her boyutundaki sayısı hakkında bilgi sağlar. 

* Mv2-serisi, buluttaki herhangi bir VM en büyük bellek (en fazla 5.7 tib'a kadar) ve en yüksek vCPU sayısını (en fazla 208 Vcpu) sunar. Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için idealdir.
 
* M serisi, bir yüksek vCPU sayısını (128'e kadar Vcpu) ve çok miktarda bellek (3,8 tib'a kadar) sunar. Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için de idealdir.

* Dv2 serisi, G serisi ve DSv2/GS ortaklarınıza daha hızlı Vcpu, daha iyi geçici depolama performansı, talep ya da daha yüksek bellek taleplerine sahip uygulamalar için idealdir. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.

* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil temel 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) 2,4 GHz veya E5-2673 v4 (Broadwell) 2,3 GHz işlemcileri ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

* Ev3 serisi özellikleri E5-2673 v4 (Broadwell) 2,3 GHz işlemci en genel amaçlı iş yükleri için daha iyi bir değer önerisi sağlama yanı sıra diğer birçok bulut ile genel amaçlı sanal hizalama Ev3 getirmek hiper iş parçacıklı bir yapılandırmada.  Disk ve ağ sınırlarını hiper iş parçacıklı Git ile hizalamak için çekirdek başına temelinde ayarlanmış durumdayken (Başlangıç 7 GiB/vCPU 8 GiB/vCPU) bellek genişletildi.  Ev3, yüksek bellek VM boyutları D/Dv2 ailelerinin kadar izleme olur.

* Azure Compute, belirli bir donanım türüyle sınırlanmış ve tek bir müşteriye ayrılmış olan sanal makine boyutları sunar.  Bu sanal makine boyutları, uyumluluk ve düzenleme gereksinimleri gibi öğeler içeren iş yükleri nedeniyle diğer müşterilerden yüksek ölçüde yalıtıma ihtiyaç duyan iş yükleri için idealdir.  Müşteriler daha fazla yalıtılmış bu sanal makinelerin kaynakları kullanarak alt bölümlere ayırmak de seçebilir [iç içe sanal makineleri için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).  Lütfen yalıtılmış VM seçeneklerinizi için sanal makine aileleri tablolara bakın.

## <a name="esv3-series"></a>Esv3 serisi 

ACU: 160-190 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

ESv3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir ve premium depolama kullanır. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.


| Size             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4000 / 32 (50)                                                       | 3200 / 48                                | 2 / 1000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8000 / 64 (100)                                                      | 6400 / 96                                | 2 / 2000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16000 / 128 (200)                                                    | 12800 / 192                              | 4 / 4000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32000 / 256 (400)                                                    | 25600 / 384                              | 8 / 8000                                       |
| Standard_E20s_v3                   | 20     | 160         | 320            | 32             | 40000 / 320 (400)                                                    | 32000 / 480                              | 8 / 10000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64000 / 512 (800)                                                    | 51200 / 768                              | 8 / 16000                             |
| Standard_E48s_v3&nbsp;<sup>2</sup> | 48     | 384         | 768            | 32             | 96000/768 (1200)                                                   | 76800 / 1152                             | 8 / 24000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128000 / 1024 (1600)                                                   | 80000 / 1200                             | 8 / 30000                             |


<sup>1</sup> Esv3 serisi sanal makineler özellik Intel® Hyper-Threading Teknolojisi.

<sup>2</sup> sınırlı kullanılabilir çekirdek boyutu.

<sup>3</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.


## <a name="ev3-series"></a>Ev3 serisi 

ACU: 160 - 190 <sup>1</sup>

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

Ev3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için ESv3 boyutlarını kullanın. ESv3 boyutları için fiyatlandırma ve faturalandırma oranları Ev3 serisi ile aynıdır. 


| Size            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8 / 10000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16000                 |
| Standard_E48_v3 | 48        | 384         | 1200            | 32             | 96000/1000/500                                            | 8 / 24000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |

<sup>1</sup> Ev3 serisi sanal makineler özellik Intel® Hyper-Threading Teknolojisi.

<sup>2</sup> sınırlı kullanılabilir çekirdek boyutu.

<sup>3</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.


## <a name="mv2-series"></a>Mv2 serisi

Premium Depolama: Desteklenen

Premium depolama önbelleğe alma: Desteklenen

Hızlandırıcı yazma: [Destekleniyor](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

Mv2 serisi özellikleri yüksek aktarım hızı, düşük gecikme süresi, doğrudan bir tüm çekirdek temel sıklığını 2.5 GHz ve 3.8 GHz max turbo sıklığını ile 8180 M 2.5 GHz (Skylake) işlemci bir hiper iş parçacıklı Intel® Xeon® Platinum üzerinde çalışan yerel NVMe depolama eşlendi. Tüm Mv2 serisi sanal makine boyutları, hem standart hem de premium kalıcı diskler kullanabilir. Mv2 serisi örnekler VM boyutları, büyük bellek içi veritabanları ve ilişkisel veritabanı sunucuları, büyük boyutlu önbellekler ve bellek içi için ideal olan, yüksek bellek CPU oranı ile iş yüklerini desteklemek için eşsiz işlem performansı sağlayarak bellek için iyileştirilmiş olan Analytics. 

|Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M208ms_v2<sup>1, 2</sup> | 208 | 5700 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1, 2</sup> | 208 | 2850 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |

Mv2 serisi sanal makineler Intel® Hyper-Threading teknolojisine sahiptir  

<sup>1</sup> bu büyük sanal makinelerin bu desteklenen konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Windows Server 2019, SLES 12 SP4, SLES 15.

<sup>2</sup> Mv2 serisi VM'ler, 2. nesil yalnızca şunlardır. Linux kullanıyorsanız, bulma ve SUSE Linux görüntüsünü seçin için aşağıdaki bölüme bakın.

#### <a name="find-a-suse-image"></a>SUSE görüntüsü bulunamadı

Azure portalında uygun bir SUSE Linux görüntüsünü seçmek için: 

1. Azure portalında **kaynak oluştur** 
1. "SUSE SAP" için arama 
1. 2\. nesil görüntüleri SAP için SLES ya da Kullandıkça Öde kullanılabilir veya (BYOS) kendi aboneliğini getiren. Arama sonuçlarında istediğiniz görüntüyü kategoriyi genişletin:

    * SAP için SUSE Linux Enterprise Server (SLES)
    * (BYOS) SAP için SUSE Linux Enterprise Server (SLES)
    
1. SUSE görüntüleri Mv2 serisi ile uyumlu adı ön eki `GEN2:`. Aşağıdaki SUSE görüntüleri Mv2 serisi VM'ler için kullanılabilir:

    * GEN2: SUSE Linux Enterprise Server (SLES) 12 SP4 SAP uygulamaları için
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 SAP uygulamaları için
    * GEN2: SUSE Linux Enterprise Server (SLES) 12 SP4 (BYOS) SAP uygulamaları için
    * GEN2: SUSE Linux Enterprise Server (SLES) 15 SAP uygulamaları (BYOS) için

#### <a name="select-a-suse-image-via-azure-cli"></a>Azure CLI aracılığıyla bir SUSE görüntüsü seçme

Mv2 serisi VM'ler için şu anda kullanılabilir SLES SAP görüntü için bir listesini görmek için aşağıdakileri kullanın [ `az vm image list` ](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest#az-vm-image-list) komutu:

```azurecli
az vm image list --output table --publisher SUSE --sku gen2 --all
```

Komut şu anda kullanılabilir 2. kuşak VM'ler SUSE kullanılabilir Mv2 serisi VM'ler için çıkarır. 

Örnek çıktı:

```
Offer          Publisher  Sku          Urn                                        Version
-------------  ---------  -----------  -----------------------------------------  ----------
SLES-SAP       SUSE       gen2-12-sp4  SUSE:SLES-SAP:gen2-12-sp4:2019.05.13       2019.05.13
SLES-SAP       SUSE       gen2-15      SUSE:SLES-SAP:gen2-15:2019.05.13           2019.05.13
SLES-SAP-BYOS  SUSE       gen2-12-sp4  SUSE:SLES-SAP-BYOS:gen2-12-sp4:2019.05.13  2019.05.13
SLES-SAP-BYOS  SUSE       gen2-15      SUSE:SLES-SAP-BYOS:gen2-15:2019.05.13      2019.05.13
```

## <a name="m-series"></a>M serisi 

ACU: 160-180 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

Hızlandırıcı yazma:  [Destekleniyor](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Size            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| İşler için Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218.75 | 256  | 8  | 10000 / 100 (793)  | 5000  / 125 | 4 / 2000 |
| İşler için Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437.5  | 512  | 16 | 20000 / 200 (1587) | 10000 / 250 | 8 / 4000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40000 / 400 (3174) | 20000 / 500 | 8 / 8000 |
| İşler için standart_m64s  | 64 | 1024   | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| İşler için standart_m64ls  | 64 | 512    | 2048 | 64 | 80000 / 800 (6348) | 40000 / 1000 | 8 / 16000 |
| İşler için standart_m64ms&nbsp;<sup>3</sup>  | 64   | 1792 | 2048 | 64 | 80000 / 800 (6348)| 40000 / 1000 | 8 / 16000          |
| İşler için standart_m128s&nbsp;<sup>2</sup> | 128  | 2048        | 4096  | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| İşler için standart_m128ms&nbsp;<sup>2&nbsp;3&nbsp;4</sup> | 128  | 3892  | 4096 | 64 | 160000 / 1600 (12696) | 80000 / 2000                            | 8 / 30000          |
| Standard_M64   | 64  | 1024 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1792 | 7168  | 64 | 80000  / 800  (1228) | 40000 / 1000 | 8 / 16000 |
| Standard_M128&nbsp;<sup>2  | 128 | 2048 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32000 |
| Standard_M128m&nbsp;<sup>2 | 128 | 3892 | 14336 | 64 | 250000 / 1600 (2456) | 80000 / 2000 | 8 / 32000 |



<sup>1</sup> M serisi sanal makineler özellik Intel® Hyper-Threading Teknolojisi

<sup>2</sup> 64'ten fazla şu desteklenen konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya LIS 4.2.1 ile Oracle Linux 7.3.

<sup>3</sup> sınırlı kullanılabilir çekirdek boyutu.

<sup>4</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.
<br>


## <a name="dsv2-series-11-15"></a>DSv2-series 11-15

ACU: 210 - 250 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000 / 64 (72) |6400 / 96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16000 / 128 (144) |12800 / 192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32000 / 256 (288) |25600 / 384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64000 / 512 (576) |51200 / 768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80000 / 640 (720) |64000 / 960 |8 / 25000&nbsp;<sup>4</sup>

<sup>1</sup> maksimum disk aktarım hızı (IOPS veya MB/sn) DSv2 serisi VM ile sınırlı olabilir sayısı, boyutu ve bölümleme türüyle ekli disklerin.  Ayrıntılar için bkz [yüksek performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).  
<sup>2</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.  
<sup>3</sup> sınırlı kullanılabilir çekirdek boyutu.  
<sup>4</sup> 25.000 Mbps, hızlandırılmış ağ. 

<br>

## <a name="dv2-series-11-15"></a>Dv2 serisi 11-15

ACU: 210 - 250

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Size              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 12000          |
| İşler için standart_d15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60000/937/468                                        | 64 / 64 x 500                       | 8 / 25000&nbsp;<sup>2</sup> |

<sup>1</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.  
<sup>2</sup> 25.000 Mbps, hızlandırılmış ağ. 
