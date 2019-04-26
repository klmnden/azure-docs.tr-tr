---
title: Sanal makine boyutları için Azure bulut Hizmetleri | Microsoft Docs
description: Farklı sanal makine boyutları (ve kimlikleri) için Azure bulut hizmeti web ve çalışan rolleri listeler.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: jpconnock
editor: ''
ms.assetid: 1127c23e-106a-47c1-a2e9-40e6dda640f6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 21fbfe22901de677209b55639cd8871ab408375b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60406438"
---
# <a name="sizes-for-cloud-services"></a>Cloud Services boyutları
Bu konuda sunulan boyutlar ve bulut Hizmeti rol örnekleri (web rolleri ve çalışan rolleri) için seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken dikkat edilmesi gereken dağıtım konuları sağlar. Her boyut, içine girdiğiniz Kimliğine sahip, [Hizmet tanım dosyası](cloud-services-model-and-package.md#csdef). Her boyut için fiyatlar kullanılabilir [bulut Hizmetleri fiyatlandırması](https://azure.microsoft.com/pricing/details/cloud-services/) sayfası.

> [!NOTE]
> İlgili Azure sınırları görmek için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Web ve çalışan rolü örnekleri için boyutları
Azure'da birden fazla standart boyut seçeneği vardır. Bu boyutlarla ilgili önemli noktalardan bazıları şunlardır:

* D Serisi VM'ler, daha yüksek işlem gücüne ve geçici süreli disk performansına ihtiyaç duyan uygulamaları çalıştıracak şekilde tasarlanmıştır. D Serisi VM'ler daha hızlı işlemcilere, daha yüksek bellek-çekirdek oranına ve geçici disk için katı hal sürücüsüne (SSD) sahiptir. Ayrıntılı bilgi için Azure blogundaki [Yeni D Serisi Sanal Makine Boyutları](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) duyurusunu inceleyin.
* Dv3 serisi, Dv2 serisi, orijinal D serisinin devamı, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.
* En fazla belleği sunan G Serisi VM'ler, Intel Xeon E5 V3 ailesi işlemcilere sahip ana bilgisayarlarda çalışır.
* A serisi VM'ler, çeşitli donanım türleri ve işlemciler üzerinde dağıtılabilir. Boyutu üzerinde dağıtıldığı donanımdan bağımsız olarak çalışan örneğe tutarlı işlemci performansı sunmak için donanım, göre kısıtlanır. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın.
* A0 boyutunun fiziksel donanım üzerindeki abone sayısı planlanandan fazladır. Yalnızca bu boyutta diğer müşteri dağıtımları, çalışan iş yükünüzün performansını etkileyebilir. Göreli performans aşağıda beklenen temel düzey olarak belirtilmiştir ve %15 oranında değişiklik gösterebilir.

Sanal makinenin boyutu, fiyatlandırmayı etkiler. Boyut, sanal makinenin işlem, bellek ve depolama kapasitesini de etkiler. Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için bkz [bulut Hizmetleri fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cloud-services/) ve [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Aşağıdaki önemli noktalar boyut konusunda karar vermenize yardımcı olabilir:

* A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır. Ayrıntılı bilgi ve bu boyutları kullanırken dikkat edilmesi gereken noktalar için bkz. [yüksek performanslı işlem VM boyutları](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Dv3 serisi, Dv2 serisi, D serisi, G serisi, daha hızlı CPU'ya ihtiyaç duyan, daha iyi yerel disk performansı veya yüksek bellek taleplerine sahip uygulamalar için uygundur. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.
* Azure veri merkezlerindeki fiziksel ana bilgisayarlardan bazıları A5 – A11 gibi daha büyük sanal makine boyutlarını desteklemeyebilir. Sonuç olarak, hata iletisini görebilirsiniz **{makine adı} sanal makine yapılandırılamadı** veya **{makine adı} sanal makine oluşturulamadı** mevcut bir sanal makine için yeni boyut; yeniden boyutlandırma sırasında 16 Nisan 2013'ten önce oluşturulan bir sanal ağda yeni bir sanal makine oluşturma; veya mevcut bir bulut hizmeti için yeni bir sanal makine ekleme. Bkz: [hata: "Sanal makine yapılandırılamadı"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) her dağıtım senaryosu için geçici çözümler için destek Forumundaki.
* Ayrıca aboneliğiniz, belirli boyut ailelerinde dağıtabileceğiniz çekirdek sayısını da sınırlıyor olabilir. Artırmak istediğini kotalar için Azure Desteği'ne başvurun.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Kavram, Azure işlem birimi (bilgi işlem (CPU) performansını Azure SKU'ları arasında karşılaştırma bir yol sağlamak üzere ACU) açısından oluşturduk ve SKU performansınızı karşılamak büyük olasılıkla olan tanımlaması gerekir.  ACU şu anda Küçük (Standard_A1) VM'de 100 olarak standart haline getirilmiş ve diğer tüm SKU'lar, standart bir karşılaştırmalı testte sunabilecekleri yaklaşık hıza göre derecelendirilmiştir.

> [!IMPORTANT]
> ACU yalnızca rehberlik etme amacı taşımaktadır. İş yükünüzle aldığınız sonuçlar farklılık gösterebilir.
>
>

<br>

| SKU Ailesi | ACU/Çekirdek |
| --- | --- |
| [ExtraSmall](#a-series) |50 |
| [Küçük ExtraLarge](#a-series) |100 |
| [A5-7](#a-series) |100 |
| [A8-A11](#a-series) |225* |
| [Bir v2](#av2-series) |100 |
| [D](#d-series) |160 |
| [D v2](#dv2-series) |160 - 190* |
| [D v3](#dv3-series) |160 - 190* |
| [E v3](#ev3-series) |160 - 190* |
| [F](#f-series) |210-250*|
| [G](#g-series) |180-240* |
| [H](#h-series) |290-300* |

* işaretli ACU'lar, CPU frekansını artırmak ve performans artışı sağlamak için Intel® Turbo Boost teknolojisinden faydalanır. Performans artışının oranı VM boyutuna, iş yüküne ve aynı ana bilgisayarda çalışan iş yüklerine göre değişiklik gösterebilir.

## <a name="size-tables"></a>Boyut tabloları
Aşağıdaki tablolarda boyutlara ve sundukları kapasiteye yer verilmiştir.

* Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
* Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
* Veri diskleri önbelleğe alınmış veya alınmamış modlarda çalışabilir. Önbelleğe alınmış veri diski işlemi için ana bilgisayar önbelleği **SaltOkunur** veya **OkuYaz** moduna ayarlanır. Önbelleğe alınmamış veri diski işlemi için ana bilgisayar önbelleği **Yok** moduna ayarlanır.
* Maksimum ağ bant genişliği, VM türü başına ayrılan ve atanan toplam maksimum bant genişliğidir. Maksimum bant genişliği, yeterli ağ kapasitesinin mevcut olduğundan emin olarak doğru VM'i seçme konusunda yardımcı olur. Düşük, Orta, yüksek ve çok yüksek arasında geçiş yapıldığında aktarım hızı buna göre artar. Gerçek ağ performansı, ağ ve uygulama yüklerinin yanı sıra uygulama ağ ayarları gibi birçok faktöre bağlıdır.

## <a name="a-series"></a>A Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| ExtraSmall      | 1         | 0,768        | 20                   | 1/düşük |
| Küçük           | 1         | 1,75         | 225                  | 1/orta |
| Orta          | 2         | 3,5          | 490                  | 1/orta |
| Büyük           | 4         | 7            | 1000                 | 2/yüksek |
| ExtraLarge      | 8         | 14           | 2040                 | 4/yüksek |
| A5              | 2         | 14           | 490                  | 1/orta |
| A6              | 4         | 28           | 1000                 | 2/yüksek |
| A7              | 8         | 56           | 2040                 | 4/yüksek |

## <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler
Bilgi ve bu boyutları kullanırken dikkat edilmesi gereken noktalar için bkz. [yüksek performanslı işlem VM boyutları](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8*             |8          | 56           | 1817                 | 2/yüksek |
| A9*             |16         | 112          | 1817                 | 4/çok yüksek |
| A10             |8          | 56           | 1817                 | 2/yüksek |
| A11             |16         | 112          | 1817                 | 4/çok yüksek |

\*RDMA özellikli

## <a name="av2-series"></a>Av2 Serisi

| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1/orta                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2/orta                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4/yüksek                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8/yüksek                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2/orta                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4/yüksek                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8/yüksek                     |


## <a name="d-series"></a>D Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1     | 1         | 3,5          | 50                   | 1/orta |
| Standard_D2     | 2         | 7            | 100                  | 2/yüksek |
| Standard_D3     | 4         | 14           | 200                  | 4/yüksek |
| Standard_D4     | 8         | 28           | 400                  | 8/yüksek |
| Standard_D11    | 2         | 14           | 100                  | 2/yüksek |
| Standard_D12    | 4         | 28           | 200                  | 4/yüksek |
| Standard_D13    | 8         | 56           | 400                  | 8/yüksek |
| Standard_D14    | 16        | 112          | 800                  | 8/çok yüksek |

## <a name="dv2-series"></a>Dv2 Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_D1_v2  | 1         | 3,5          | 50                   | 1/orta |
| Standard_D2_v2  | 2         | 7            | 100                  | 2/yüksek |
| Standard_D3_v2  | 4         | 14           | 200                  | 4/yüksek |
| Standard_D4_v2  | 8         | 28           | 400                  | 8/yüksek |
| Standard_D5_v2  | 16        | 56           | 800                  | 8/aşırı yüksek |
| Standard_D11_v2 | 2         | 14           | 100                  | 2/yüksek |
| Standard_D12_v2 | 4         | 28           | 200                  | 4/yüksek |
| Standard_D13_v2 | 8         | 56           | 400                  | 8/yüksek |
| Standard_D14_v2 | 16        | 112          | 800                  | 8/aşırı yüksek |
| Standard_D15_v2 | 20        | 140          | 1000                | 8/aşırı yüksek |

## <a name="dv3-series"></a>Dv3 serisi

| Boyut            | CPU çekirdekleri | Bellek: GiB   | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------- | -------------------- | ---------------------------- |
| Standard_D2_v3  | 2         | 8             | 50                   | 2/orta |
| Standard_D4_v3  | 4         | 16            | 100                  | 2/yüksek |
| Standard_D8_v3  | 8         | 32            | 200                  | 4/yüksek |
| Standard_D16_v3 | 16        | 64            | 400                  | 8/aşırı yüksek |
| Standard_D32_v3 | 32        | 128           | 800                  | 8/aşırı yüksek |
| Standard_D64_v3 | 64        | 256           | 1600                 | 8/aşırı yüksek |

## <a name="ev3-series"></a>Ev3 serisi

| Boyut            | CPU çekirdekleri | Bellek: GiB   | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------- | -------------------- | ---------------------------- |
| Standard_E2_v3  | 2         | 16            | 50                   | 2/orta |
| Standard_E4_v3  | 4         | 32            | 100                  | 2/yüksek |
| Standard_E8_v3  | 8         | 64            | 200                  | 4/yüksek |
| Standard_E16_v3 | 16        | 128           | 400                  | 8/aşırı yüksek |
| Standard_E32_v3 | 32        | 256           | 800                  | 8/aşırı yüksek |
| Standard_E64_v3 | 64        | 432           | 1600                 | 8/aşırı yüksek |

## <a name="f-series"></a>F Serisi


| Boyut            | CPU çekirdekleri | Bellek: GiB   | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------- | -------------------- | ---------------------------- |
| Standard_F1     | 1         | 2             | 16                   | 2 / 750  |
| Standard_F2     | 2         | 4             | 32                   | 2 / 1500 |
| Standard_F4     | 4         | 8             | 64                   | 4 / 3000 |
| Standard_F8     | 8         | 16            | 128                  | 8 / 6000 |
| Standard_F16    | 16        | 32            | 256                  | 8 / 12000|


## <a name="g-series"></a>G Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1/yüksek |
| Standard_G2     | 4         | 56           | 768                  |2/yüksek |
| Standard_G3     | 8         | 112          | 1536                |4/çok yüksek |
| Standard_G4     | 16        | 224          | 3072                |8/aşırı yüksek |
| Standard_G5     | 32        | 448          | 6144                |8/aşırı yüksek |

## <a name="h-series"></a>H Serisi
Azure H Serisi sanal makineler, moleküler modelleme ve hesaplamalı akışkanlar dinamiği gibi üst düzey işlem hesaplama gereksinimlerine hitap eden yeni nesil yüksek performanslı bilgi işlem VM'leridir. Bu 8 ve 16 çekirdek VM'ler, Intel Haswell E5-2667 V3 işlemci teknolojisini DDR4 bellek ve yerel SSD tabanlı depolama oluşturulur.

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.

| Boyut            | CPU çekirdekleri | Bellek: GiB  | Geçici depolama (SSD): GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8/yüksek |
| Standard_H16    | 16        | 112          | 2000                 | 8/çok yüksek |
| Standard_H8m    | 8         | 112          | 1000                 | 8/yüksek |
| Standard_H16m   | 16        | 224          | 2000                 | 8/çok yüksek |
| Standard_H16r*  | 16        | 112          | 2000                 | 8/çok yüksek |
| Standard_H16mr* | 16        | 224          | 2000                 | 8/çok yüksek |

\*RDMA özellikli

## <a name="configure-sizes-for-cloud-services"></a>Boyutlar Cloud Services için yapılandırma
Hizmet modeli tarafından açıklanan bir parçası olarak bir rol örneği sanal makine boyutunu belirtebilirsiniz [Hizmet tanım dosyası](cloud-services-model-and-package.md#csdef). Rol boyutu CPU çekirdekleri, bellek kapasitesini ve çalışan bir örneğe ayrılmış yerel dosya sistemi boyutunu belirler. Uygulamanızın kaynak gereksinimi temel alan rol boyutu seçin.

Bir Web rol örneği için işler için standart_d2 rol boyutunu ayarlamak için bir örnek aşağıda verilmiştir:

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-the-size-of-an-existing-role"></a>Mevcut bir rolü boyutunu değiştirme

İş yükü değişiklikleri veya yeni VM boyutları kullanılabilir doğasını rolünüz boyutunu değiştirmek isteyebilirsiniz. Bunu yapmak için (yukarıda gösterildiği gibi), hizmet tanımı dosyasında VM boyutunu değiştirin, bulut hizmetinizi yeniden paketleyin ve dağıtabilirsiniz.

>[!TIP]
> (Örn. farklı VM boyutları farklı ortamlarda rolünüz için kullanmak isteyebilirsiniz. Test ve üretim). Yapmanın bir yolu bu birden fazla hizmet tanımı (.csdef) dosyaları, projenizdeki oluşturmaktır CSPack aracını kullanarak otomatik derleme sırasında'da hizmet paketleri her ortam farklı bulut oluşturun. Öğeleri bir bulut Hizmetleri paketi ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [hizmetleri modeli bulut nedir ve nasıl miyim paketi bunu?](cloud-services-model-and-package.md)
>
>

## <a name="get-a-list-of-sizes"></a>Boyutlarının listesini alın
Boyutlarının listesini almak için PowerShell veya REST API'yi kullanabilirsiniz. REST API belgelenen [burada](/previous-versions/azure/reference/dn469422(v=azure.100)). Bulut Hizmetleri için kullanılabilir boyutları listeler bir PowerShell komutu kodudur. 

```powershell
Get-AzureRoleSize | where SupportedByWebWorkerRoles -eq $true | select InstanceSize, RoleSizeLabel
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md) hakkında bilgi edinin.
* Daha fazla bilgi edinin [VM boyutları hakkında yüksek performanslı işlem](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) HPC iş yükleri için.
