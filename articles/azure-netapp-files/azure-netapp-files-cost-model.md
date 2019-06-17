---
title: Azure NetApp dosyaları için maliyet modeli | Microsoft Docs
description: Expenses hizmetten yönetmek için Azure NetApp dosyaları için maliyet modeli açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: b-juche
ms.openlocfilehash: b06e3366224b90899dd3f9f9439edf897de82794
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65524228"
---
# <a name="cost-model-for-azure-netapp-files"></a>Azure NetApp Files için maliyet modeli 

Azure için NetApp dosyaları maliyet modelini anlama, harcamalarınızı hizmetten yönetmenize yardımcı olur.

## <a name="calculation-of-capacity-consumption"></a>Kapasite tüketimini hesaplama

Azure NetApp dosyaları sağlanan depolama kapasitesi faturalandırılır.  Sağlanan kapasiteyi kapasitesi havuzu oluşturarak ayrılır.  Kapasite havuzları $/ sağlanan-GiB/ay saatlik artışlarla faturalandırılır. Tek kapasitesi havuzu için minimum boyutu 4 TiB ve kapasitesi havuzu 1 TiB artışlarla sonradan genişletilebilir. Birim kapasitesi havuzu içinde oluşturulur.  Her birim bir kota atanır havuzlar tarafından sağlanan kapasiteden bu azaltır. En düşük ile 92 TiB en fazla 100 GiB, aralıkları atanabilir birimlere kotası.  

Etkin bir birim için mantıksal (etkin) kapasite kota karşı kapasite tüketimini temel alır.

Bir toplu gerçek kapasite kullanımını, depolama kotasını aşarsa, birim büyümeye devam edebilirsiniz. Gerçek birim boyutu (100 tib'a kadar) sistem sınırından az olduğu sürece yazma yine de izin verilir.  

Toplam kullanılan kapasiteyi, sağlanan miktar karşı kapasitesi havuzu toplamıdır atanan kota daha büyük ya da gerçek havuz içindeki tüm birimlerin tüketimi: 

   ![Kullanılan toplam kapasite hesaplama](../media/azure-netapp-files/azure-netapp-files-total-used-capacity.png)

Aşağıdaki diyagram bu kavramları göstermektedir.  
* 4 TiB sağlanan kapasitesinin kapasite havuzuyla sunuyoruz.  Havuz üç birimlerini içerir.  
    * Birimi 1, 2 tib'a kadar bir kotayı atanır ve tüketim 800 GiB sahiptir.  
    * 2 birim 1 tib'a kadar bir kotayı atanır ve 100 GiB tüketiminin sahiptir.  
    * Birim 3 500 GiB kotası atanır ancak 800 GiB (fazla kullanım) tüketiminin sahiptir.  
* Kapasitesi havuzu kapasitesi (sağlanan miktarı) için 4 TiB ölçülür.  
    3.8 kapasite TiB kullanılır (2 TiB ve 1 TiB kotasının birimleri 1 ve 2 ve 3 birim için gerçek tüketiminin 800 GiB). Ve kapasite 200 GiB kaldı.

   ![Üç birim kapasitesi havuzu](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-three-vols.png)

## <a name="overage-in-capacity-consumption"></a>Kapasite tüketiminde fazla kullanım  

Toplam kapasitesi havuzu kullanıldığında, sağlanan kapasiteyi aşan, veri yazma işlemlerine izin verildiği.  Kullanılan havuz kapasitesi hala sağlanan kapasitesi aşarsa sağlanan kapasiteyi kullanılan toplam kapasiteden büyük olana kadar yetkisiz kullanım süresi (bir saat) sonra havuz boyutunu otomatik olarak 1 TiB artışlarla yükseltilecektir.  Örneğin, çizimde, birim 3 büyümeye devam ve gerçek tüketim 1.2 TiB ulaştıktan sonra yetkisiz kullanım süresi havuzu otomatik olarak 5 TiB olarak boyutlandırılır.  Sağlanan havuz kapasitesi (5 TiB) kullanılan kapasitesi (4.2 tib'a kadar) aşıyor sonucudur.  

## <a name="manual-changes-of-the-pool-size"></a>Havuz boyutunu el ile yapılan değişiklikler  

Elle artırabilir veya havuz boyutunu azaltabilirsiniz. Ancak, şu kısıtlamalar uygulanır:
* Hizmet minimum ve maksimum sınırları  
    Makaleyi görüntülemek [kaynak sınırları](azure-netapp-files-resource-limits.md).
* İlk 4 TiB en düşük satın alma sonra bir 1 TiB artırma
* Bir saatlik en küçük fatura artırma
* Sağlanan havuz boyutunu toplam kapasitesi havuzu içinde kullanılan daha az için azaltılabilir değil.

## <a name="behavior-of-maximum-size-pool-overage"></a>En büyük boyut havuzu fazla kullanım davranışı   

Oluşturduğunuz veya için yeniden boyutlandır kapasitesi havuzu en büyük boyutunu 500 TiB ' dir.  Toplam Kapasite kapasitesi havuzu içinde kullanıldığında aşağıdaki durumlar ortaya çıkar 500 TiB aşıyor:
* (Birim sistem maksimum 100 TiB ise) veri yazma yine de izin verilir.
* Kullanılan toplam kapasitesi havuzu sağlanan kapasite aşana kadar bir saatlik yetkisiz kullanım süresi havuzun otomatik olarak 1 TiB artışlarla boyutlandırılır.
* Ek sağlanabilir ve havuz kapasitesi 500 aşan TiB hacmi kotası atamak için kullanılamaz olarak faturalandırılır. Ayrıca, performans QoS sınırları genişletmek için de kullanılamaz.  
    Bkz: [hizmet düzeyleri](azure-netapp-files-service-levels.md) performans sınırlarını ve QoS boyutlandırma hakkında.

Aşağıdaki diyagram bu kavramları göstermektedir:
* Premium depolama katmanı ve bir 500 TiB kapasiteye kapasite havuzuyla sunuyoruz. Havuz dokuz birimlerini içerir.
    * Birimleri 1 8 ile 60 TiB kotası atanır.  Kullanılan toplam kapasite 480 TiB ' dir.  
        Her birim 3,75 GiB/sn aktarım hızının QoS sınırı vardır (60 TiB * 64 MiB/sn).  
    * Birim 9, 20 tib'a kadar bir kotayı atanır.  
        9 biriminde QoS sınırı 1,25 GiB/sn üretilen iş (60 TiB * 64 MiB/sn).
* Birim 9 fazla kullanım bir senaryodur. 25 TiB gerçek tüketim var.  
    * Bir saatlik yetkisiz kullanım süresi kapasitesi havuzu 505 TiB olarak yeniden boyutlandırılır.  
        Diğer bir deyişle, toplam kapasite kullanılan 8 * 60 =-TiB kota birimleri 1 ila 8 ve 25 Tib'a kadar birim 9 gerçek kullanım için.
    * Faturalandırılan 505 TiB kapasitesidir.
    * Birim 9 hacmi kotası aşıldı (havuzu için atanan toplam kota 500 TiB aşamaz çünkü) yükseltilemez.
    * Ek QoS aktarım hızı (havuz için toplam QoS hala üzerinde 500 TiB dayandığından) atanamaz.

   ![Dokuz birimlerle kapasitesi havuzu](../media/azure-netapp-files/azure-netapp-files-capacity-pool-with-nine-vols.png)

## <a name="capacity-consumption-of-snapshots"></a>Kapasite tüketimini anlık görüntüleri 

Kapasite tüketimini Azure NetApp dosya anlık görüntüleri, üst birimin kotaya ücretlendirilir.  Sonuç olarak, aynı faturama yansıyan fiyat birimi ait olduğu kapasitesi havuzu paylaşır.  Ancak, bir etkin birim, anlık görüntü tüketimi tüketilen artan kapasite tabanlı ölçülür.  Doğası gereği fark Azure NetApp dosya anlık görüntüler. Veri değişikliği hızına bağlı olarak anlık görüntüleri, genellikle çok daha az kapasite etkin birim, mantıksal kapasiteden kullanır. Örneğin, yalnızca 10 GiB fark veri içeren bir 500 GiB birimin anlık görüntüsünü olduğunu varsaymaktadır. Bu anlık görüntü birimi kotası karşı ücretlendirilir kapasite 10 GiB, 500 GiB değil olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp dosyaları fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/netapp/)
* [Azure NetApp dosyaları için hizmet düzeyleri](azure-netapp-files-service-levels.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
