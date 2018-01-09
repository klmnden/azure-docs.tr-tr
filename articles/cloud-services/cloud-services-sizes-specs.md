---
title: "Sanal makine boyutları için Azure bulut hizmetlerini | Microsoft Docs"
description: "Başka bir sanal makine boyutları (ve kimlikler) için Azure bulut hizmeti web ve çalışan rolleri listeler."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1127c23e-106a-47c1-a2e9-40e6dda640f6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: a5ac8c46f17d2d1c2f20ed2cc2348f50b7739ddf
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="sizes-for-cloud-services"></a>Cloud Services boyutları
Bu konu, kullanılabilir boyutları ve bulut Hizmeti rol örnekleri (web rolleri ve çalışan rolleri) için seçenekleri açıklar. Ayrıca, bu kaynakları kullanmayı planlarken dikkat edilmesi gereken dağıtımında dikkat edilecek noktalar sağlar. İçine bir kimliği her boyutuna sahip, [hizmet tanımı dosyası](cloud-services-model-and-package.md#csdef). Fiyatlar her boyutu için kullanılabilir [Cloud Services fiyatlandırması](https://azure.microsoft.com/pricing/details/cloud-services/) sayfası.

> [!NOTE]
> İlgili Azure sınırları görmek için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md)
>
>

## <a name="sizes-for-web-and-worker-role-instances"></a>Web ve çalışan rolü örnekleri için Boyutlar
Azure'da birden fazla standart boyut seçeneği vardır. Bu boyutlarla ilgili önemli noktalardan bazıları şunlardır:

* D Serisi VM'ler, daha yüksek işlem gücüne ve geçici süreli disk performansına ihtiyaç duyan uygulamaları çalıştıracak şekilde tasarlanmıştır. D Serisi VM'ler daha hızlı işlemcilere, daha yüksek bellek-çekirdek oranına ve geçici disk için katı hal sürücüsüne (SSD) sahiptir. Ayrıntılı bilgi için Azure blogundaki [Yeni D Serisi Sanal Makine Boyutları](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) duyurusunu inceleyin.
* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Yeni nesil 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.
* En fazla belleği sunan G Serisi VM'ler, Intel Xeon E5 V3 ailesi işlemcilere sahip ana bilgisayarlarda çalışır.
* A-series VM'ler çeşitli donanım türleri ve işlemciler üzerinde dağıtılabilir. Çalışan örneğin üzerinde dağıtılmış donanım bağımsız olarak tutarlı işlemci performans sağlamak için donanımı, temel boyutu azaltılır. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın.
* A0 boyutunun fiziksel donanım üzerindeki abone sayısı planlanandan fazladır. Yalnızca bu boyutta diğer müşteri dağıtımları, çalışan iş yükünüzün performansını etkileyebilir. Göreli performans aşağıda beklenen temel düzey olarak belirtilmiştir ve %15 oranında değişiklik gösterebilir.

Sanal makinenin boyutu, fiyatlandırmayı etkiler. Boyut, sanal makinenin işlem, bellek ve depolama kapasitesini de etkiler. Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için bkz [Cloud Services fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cloud-services/) ve [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

Aşağıdaki önemli noktalar boyut konusunda karar vermenize yardımcı olabilir:

* A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır. Ayrıntılı bilgi ve bu boyutları kullanma hakkında dikkat edilecek noktalar için bkz: [yüksek performanslı işlem VM boyutları](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Dv2 serisi, D-serisi, G serisi, daha hızlı CPU talep uygulamaları için ideal, daha iyi yerel disk performans ya da daha yüksek bellek taleplerini sahip. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.
* Azure veri merkezlerindeki fiziksel ana bilgisayarlardan bazıları A5 – A11 gibi daha büyük sanal makine boyutlarını desteklemeyebilir. Sonuç olarak, hata iletisi görebilirsiniz **sanal makine {makine adı} yapılandırılamadı** veya **sanal makine {makine adı} oluşturulamadı** zaman mevcut bir sanal makine yeni bir boyuta yeniden boyutlandırma; 16 Nisan 2013 önce; oluşturulan bir sanal ağda yeni bir sanal makine oluşturma veya var olan yeni bir sanal makine ekleme bulut hizmeti. Bkz: [hata: "sanal makine yapılandırma başarısız oldu"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) her dağıtım senaryosu için geçici çözümler için Destek Forumu üzerinde.
* Ayrıca aboneliğiniz, belirli boyut ailelerinde dağıtabileceğiniz çekirdek sayısını da sınırlıyor olabilir. Artırmak istediğini kotalar için Azure Desteği'ne başvurun.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Kavram, Azure işlem birimi (Azure SKU'ları üzerinde işlem (CPU) performans karşılaştırma bir yol sağlamak üzere ACU) oluşturduk ve SKU performansınızı yetecek kadar büyük olasılıkla olan tanımlamak için gerekiyor.  ACU şu anda Küçük (Standard_A1) VM'de 100 olarak standart haline getirilmiş ve diğer tüm SKU'lar, standart bir karşılaştırmalı testte sunabilecekleri yaklaşık hıza göre derecelendirilmiştir.

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
| [Standard_A1-8v2](#av2-series) |100 |
| [Standard_A2m-8mv2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1-14](#d-series) |160 |
| [D1-15v2](#dv2-series) |210-250* |
| [G1-5](#g-series) |180-240* |
| [H](#h-series) |290-300* |

* işaretli ACU'lar, CPU frekansını artırmak ve performans artışı sağlamak için Intel® Turbo Boost teknolojisinden faydalanır. Performans artışının oranı VM boyutuna, iş yüküne ve aynı ana bilgisayarda çalışan iş yüklerine göre değişiklik gösterebilir.

## <a name="size-tables"></a>Boyut tabloları
Aşağıdaki tablolarda boyutlara ve sundukları kapasiteye yer verilmiştir.

* Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
* Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
* Veri diskleri önbelleğe alınmış veya alınmamış modlarda çalışabilir. Önbelleğe alınmış veri diski işlemi için ana bilgisayar önbelleği **SaltOkunur** veya **OkuYaz** moduna ayarlanır. Önbelleğe alınmamış veri diski işlemi için ana bilgisayar önbelleği **Yok** moduna ayarlanır.
* Maksimum ağ bant genişliği, VM türü başına ayrılan ve atanan toplam maksimum bant genişliğidir. Maksimum bant genişliği, yeterli ağ kapasitesinin mevcut olduğundan emin olarak doğru VM'i seçme konusunda yardımcı olur. Düşük, Orta, yüksek ve çok yüksek arasında taşırken uygun şekilde verimliliğini artırır. Gerçek ağ performansı, ağ ve uygulama yüklerinin yanı sıra uygulama ağ ayarları gibi birçok faktöre bağlıdır.

## <a name="a-series"></a>A Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel HDD: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| ExtraSmall      | 1         | 0,768        | 20                   | 1/düşük |
| Küçük           | 1         | 1,75         | 225                  | 1/orta |
| Orta          | 2         | 3,5 GB       | 490                  | 1/orta |
| Büyük           | 4         | 7            | 1000                 | 2/yüksek |
| ExtraLarge      | 8         | 14           | 2040                 | 4/yüksek |
| A5              | 2         | 14           | 490                  | 1/orta |
| A6              | 4         | 28           | 1000                 | 2/yüksek |
| A7              | 8         | 56           | 2040                 | 4/yüksek |

## <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler
Bilgi ve bu boyutları kullanma hakkında dikkat edilecek noktalar için bkz: [yüksek performanslı işlem VM boyutları](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel HDD: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| A8 *             |8          | 56           | 1817                 | 2/yüksek |
| A9 *             |16         | 112          | 1817                 | 4/çok yüksek |
| A10             |8          | 56           | 1817                 | 2/yüksek |
| A11             |16         | 112          | 1817                 | 4/çok yüksek |

\*RDMA özellikli

## <a name="av2-series"></a>Av2 Serisi

| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel SSD: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_A1_v2  | 1         | 2            | 10                   | 1/orta                 |
| Standard_A2_v2  | 2         | 4            | 20                   | 2/orta                 |
| Standard_A4_v2  | 4         | 8            | 40                   | 4/yüksek                     |
| Standard_A8_v2  | 8         | 16           | 80                   | 8/yüksek                     |
| Standard_A2m_v2 | 2         | 16           | 20                   | 2/orta                 |
| Standard_A4m_v2 | 4         | 32           | 40                   | 4/yüksek                     |
| Standard_A8m_v2 | 8         | 64           | 80                   | 8/yüksek                     |


## <a name="d-series"></a>D Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel SSD: GiB       | Maksimum NIC/Ağ bant genişliği |
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
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel SSD: GiB       | Maksimum NIC/Ağ bant genişliği |
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
| İçin Standard_D15_v2 | 20        | 140          | 1000                | 8/aşırı yüksek |

## <a name="g-series"></a>G Serisi
| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel SSD: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_G1     | 2         | 28           | 384                  |1/yüksek |
| Standard_G2     | 4         | 56           | 768                  |2/yüksek |
| Standard_G3     | 8         | 112          | 1536                |4/çok yüksek |
| Standard_G4     | 16        | 224          | 3072                |8/aşırı yüksek |
| Standard_G5     | 32        | 448          | 6144                |8/aşırı yüksek |

## <a name="h-series"></a>H Serisi
Azure H Serisi sanal makineler, moleküler modelleme ve hesaplamalı akışkanlar dinamiği gibi üst düzey işlem hesaplama gereksinimlerine hitap eden yeni nesil yüksek performanslı bilgi işlem VM'leridir. Bu 8 ve 16 çekirdek VM'ler DDR4 bellek ve yerel SSD tabanlı depolama özelliklerine sahip Intel Haswell E5-2667 V3 işlemci teknolojisi oluşturulmuştur.

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.

| Boyut            | CPU çekirdekleri | Bellek: GiB  | Yerel SSD: GiB       | Maksimum NIC/Ağ bant genişliği |
|---------------- | --------- | ------------ | -------------------- | ---------------------------- |
| Standard_H8     | 8         | 56           | 1000                 | 8/yüksek |
| Standard_H16    | 16        | 112          | 2000                 | 8/çok yüksek |
| Standard_H8m    | 8         | 112          | 1000                 | 8/yüksek |
| Standard_H16m   | 16        | 224          | 2000                 | 8/çok yüksek |
| Standard_H16r*  | 16        | 112          | 2000                 | 8/çok yüksek |
| Standard_H16mr* | 16        | 224          | 2000                 | 8/çok yüksek |

\*RDMA özellikli

## <a name="configure-sizes-for-cloud-services"></a>Bulut Hizmetleri için boyutlarını yapılandırma
Tarafından açıklanan hizmet modeli bir parçası olarak bir rol örneği için sanal makine boyutu belirtebilirsiniz [hizmet tanımı dosyası](cloud-services-model-and-package.md#csdef). Rol boyutu CPU çekirdekleri, bellek kapasitesi ve çalışan bir örneğine ayrılan yerel dosya sistemi boyutunu sayısını belirler. Uygulamanızın kaynak gereksinimi temel alan rol boyutunu seçin.

Rol boyutunu ayarlama örneği [Standard_D2](#general-purpose-d) Web rol örneği için:

```xml
<WorkerRole name="Worker1" vmsize="Standard_D2">
...
</WorkerRole>
```

## <a name="changing-the-size-of-an-existing-role"></a>Mevcut bir rolü boyutunu değiştirme

İş yükü değişiklikleri veya kullanılabilir hale yeni VM boyutları yapısını rolünüze boyutunu değiştirmek isteyebilirsiniz. Bunu yapmak için VM boyutu, hizmet tanımı dosyasında (yukarıda gösterildiği gibi) değiştirme, bulut hizmetinizin yeniden paketleyin ve dağıtın. VM boyutları doğrudan portal veya PowerShell değiştirmek mümkün değil.

>[!TIP]
> (Ör. farklı VM boyutları farklı ortamlarda rolünüz için kullanmak isteyebilirsiniz VS üretim test). Yapmanın bir yolu bu birden çok hizmet tanımı (.csdef) dosyaları, projenizdeki oluşturmaktır sonra farklı bir bulut ortamı başına hizmet paketleri CSPack aracını kullanarak, otomatik derleme sırasında oluşturun. Öğeleri bir bulut Hizmetleri paketi ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [hizmetleri modeli bulut nedir ve nasıl ı paket onu?](cloud-services-model-and-package.md)
>
>

## <a name="get-a-list-of-sizes"></a>Boyutlarının listesini al
Boyutlarının listesini almak için PowerShell veya REST API'sini kullanabilirsiniz. REST API belgelenen [burada](https://msdn.microsoft.com/library/azure/dn469422.aspx). Verilen bir konuma ait tüm boyutları listeler bir PowerShell komut kodudur. 

```powershell
Get-AzureRmVMSize -Location 'West Europe'
```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md) hakkında bilgi edinin.
* Daha fazla bilgi edinin [VM boyutları hakkında yüksek performanslı işlem](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) HPC iş yükleri için.
