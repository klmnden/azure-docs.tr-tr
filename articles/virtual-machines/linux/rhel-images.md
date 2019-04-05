---
title: Azure'da Red Hat Enterprise Linux görüntüleri | Microsoft Docs
description: Microsoft azure'da Red Hat Enterprise Linux görüntüleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/18/2019
ms.author: borisb
ms.openlocfilehash: fb3c0e46324a22bdd95bf7d93c28e69c195927e8
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045431"
---
# <a name="red-hat-enterprise-linux-images-in-azure"></a>Azure'da Red Hat Enterprise Linux görüntüleri
Bu makalede Azure Market'te kendi adlandırma ve saklama ilkeleri ile birlikte kullanılabilir Red Hat Enterprise Linux (RHEL) görüntüler.

RHEL tüm sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi bulunabilir [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.

>[!Important]
> RHEL görüntüleri şu anda Azure Marketi'nde Getir Your-kendi-abonelik (BYOS) ya da Kullandıkça Öde (PAYG) lisans modelleri destekler. [Azure hibrit kullanım teklifi](../windows/hybrid-use-benefit-licensing.md) ve PAYG BYOS arasında dinamik geçiş desteklenmez. Geçiş lisans modu, karşılık gelen görüntüden sanal makine yeniden dağıtıldığında gerektirir.

>[!Note]
> Azure Market galerisinde RHEL görüntüleri için ilgili tüm sorunlarınız için lütfen Microsoft ile bir destek bileti dosya.

## <a name="images-available-in-the-ui"></a>Kullanıcı Arabiriminde kullanılabilen görüntüleri
Market'te veya Azure portalı kullanıcı arabirimini bir kaynak oluştururken "Red Hat" için arama yaptığınızda kullanılabilir RHEL görüntülerinin ve ilgili Red Hat ürünlerini bir alt kümesini görürsünüz. Her zaman eksiksiz bir listesi kullanılabilir VM görüntüleri Azure CLI/PowerShell/API'sini kullanarak elde edebilirsiniz.

Azure'da kullanılabilen Red Hat görüntülerini tam kümesini görmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az vm image list --publisher RedHat --all
```

## <a name="naming-convention"></a>Adlandırma kuralı
Azure'da VM görüntüleri, yayımcı, teklif, SKU ve sürümü tarafından düzenlenir. SKU yayımcı: Teklif: version'a birleşimi URN görüntü ve kullanılacak resmi benzersiz şekilde tanımlar.

Örneğin, `RedHat:RHEL:7-RAW:7.6.2018103108` 31 Ekim 2018'de yerleşik RHEL 7.6 bölümlenmiş ham görüntüsüne başvuruyor.

RHEL 7.6 VM oluşturmak nasıl bir örnek aşağıda gösterilmiştir.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>"Son" takma
"Son" belirli sürümü yerine sürüm için bilinen ad, Azure REST API izin verir. "Son" kullanarak en son kullanılabilir görüntüyü belirtilen yayımcı, teklif ve SKU için sağlanır.

Örneğin, `RedHat:RHEL:7-RAW:latest` son RHEL 7 ailesi bölümlenmiş ham görüntü için kullanılabilir ifade eder.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:latest --no-wait
```

>[!NOTE]
> Genel olarak, en son belirlemek için sürümleri karşılaştırma, kurallara [CompareTo yöntemi](https://msdn.microsoft.com/library/a5ts8tb6.aspx).

### <a name="current-naming-convention"></a>Geçerli adlandırma kuralları
Şu anda yayımlanan RHEL görüntüleri tüm Kullandıkça Öde modeli kullanır ve bağlı [azure'da Red Hat Update Infrastructure'a (RHUI)](https://aka.ms/rhui-update). RHUI bir tasarım sınırlamaları nedeniyle, RHEL 7 ailesi görüntüler için yeni bir adlandırma kuralı tarafından benimsenmiştir. Şu anda ailesi adlandırma değiştirilmediği RHEL 6.

Aslında sınırlamasıdır olduğunda olmayan-Seçici `yum update` karşı bir VM çalıştırma RHUI bağlı RHEL sürüm en son geçerli ailesindeki güncelleştirilir. Daha fazla bilgi için [bu bağlantıyı](https://aka.ms/rhui-update). Sağlanan bir RHEL 7.2 görüntüsü RHEL 7.6 sonra bir güncelleştirme olduğunda bu içinde karışıklığa neden olabilir. Gerekli sürümü açıkça belirterek örneklerde gösterildiği gibi daha eski bir görüntüden sağlayabilirsiniz. Gerekli sürümü yeni bir RHEL 7 görüntüsü sağlanırken belirtilmezse, en son görüntü sağlanır.

>[!NOTE]
> Görüntü SAP kümesi için RHEL, RHEL sürüm sabit kalır. Bu nedenle, kendi adlandırma kuralı belirli bir sürümü SKU'da içerir.

>[!NOTE]
> Görüntü RHEL 6 kümesi taşınmadı yeni adlandırma kuralı için.

SKU'ları şu anda genel kullanıma açık aşağıdaki teklifler şunlardır:

Sunduğu| SKU | Bölümleme | Sağlama | Notlar
:----|:----|:-------------|:-------------|:-----
RHEL | HAM 7 | HAM | Linux Aracısı | Görüntüleri RHEL 7 ailesi
| | 7-LVM | LVM | Linux Aracısı | Görüntüleri RHEL 7 ailesi
| | 7-RAW-CI | HAM CI | Cloud-init | Görüntüleri RHEL 7 ailesi
| | 6.7 | HAM | Linux Aracısı | RHEL 6.7 görüntüleri, eski adlandırma kuralı
| | 6.8 | HAM | Linux Aracısı | RHEL 6,8 için yukarıdaki gibi aynı
| | 6.9 | HAM | Linux Aracısı | RHEL 6.9 için yukarıdaki gibi aynı
| | 6.10 | HAM | Linux Aracısı | RHEL 6.10 için yukarıdaki gibi aynı
| | 7.2 | HAM | Linux Aracısı | RHEL 7.2 için yukarıdaki gibi aynı
| | 7.3 | HAM | Linux Aracısı | RHEL 7.3 için yukarıdaki gibi aynı
| | 7.4 | HAM | Linux Aracısı | RHEL 7.4 için yukarıdaki gibi aynı
| | 7.5 | HAM | Linux Aracısı | RHEL 7.5 için yukarıdaki gibi aynı
RHEL-SAP | 7.4 | LVM | Linux Aracısı | SAP HANA ve iş kolu uygulamaları için RHEL 7.4
| | 7.5 | LVM | Linux Aracısı | SAP HANA ve iş kolu uygulamaları için RHEL 7.5
RHEL-SAP-HANA | 6.7 | HAM | Linux Aracısı | SAP HANA için RHEL 6.7
| | 7.2 | LVM | Linux Aracısı | SAP HANA için RHEL 7.2
| | 7.3 | LVM | Linux Aracısı | SAP HANA için RHEL 7.3
RHEL-SAP-APPS | 6.8 | HAM | Linux Aracısı | 6,8 RHEL for SAP Business Applications
| | 7.3 | LVM | Linux Aracısı | 7.3 RHEL for SAP Business Applications

### <a name="old-naming-convention"></a>Eski adlandırma kuralı
Adlandırma kuralı değişiklik yukarıda açıklanan kadar belirli sürümler görüntüleri RHEL 7 ailesi ve görüntüleri RHEL 6 ailesi içinde SKU'larını kullanılır.

Sayısal SKU'ları tam görüntü listesinde bulabilirsiniz. Yeni bir alt yayın sürümü çıktığında Microsoft ve Red Hat yeni sayısal SKU'ları oluşturun.

### <a name="other-available-offers-and-skus"></a>Diğer kullanılabilir teklif ve SKU'ları
Kullanılabilir teklif ve SKU'ları tam listesini ne Örneğin, yukarıdaki tabloda listelenen ötesinde ek görüntüleri içerebilir `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`. Bu teklif, belirli Market çözümlerini desteği sağlamak için kullanılabilir veya Önizleme ve test amacıyla yayınlanabilir. Bunlar değiştirilemez veya uyarısı olmadan herhangi bir zamanda kaldırıldı. Varlıklarını herkese açık şekilde Microsoft veya Red Hat tarafından belgelenmiştir sürece bunları kullanmayın.

## <a name="publishing-policy"></a>Yayımlama İlkesi
Microsoft ve Red Hat belirli CVEs gidermenin gerekli olarak ya da bazen yapılandırma değişikliklerinin/güncelleştirmelerinin için yeni ikincil sürümleri yayımlanan görüntüleri güncelleştirin. Olabildiğince çabuk - sürüm veya CVE düzeltme kullanılabilirliğini izleyen üç iş günü içinde güncelleştirilmiş görüntüleri sağlamak elimizden.

Yalnızca belirtilen görüntü ailesi küçük geçerli sürümde güncelleştiriyoruz. Küçük bir sürüme'nın yayınlanmasıyla birlikte, eski ikincil sürümü güncelleştirme durdurun. Örneğin, RHEL 7.6'ın yayınlanmasıyla birlikte, 7.5 RHEL görüntüleri artık güncelleştirilmesi oluşturacaksınız.

>[!NOTE]
> RHEL görüntüleri için Azure RHUI bağlı ve Red Hat tarafından sunulan ve Azure RHUI (genellikle, 24 saatten daha az resmi sürümden Red Hat aşağıdaki) çoğaltılan hemen sonra güncelleştirme ve düzeltme alabilir Kullandıkça Öde'den sağlanan etkin Azure Vm'leri . Bu sanal makineler güncelleştirmeleri almak için yeni yayımlanan görüntüsünü gerektirmez ve müşterilerin güncelleştirmeyi başlatmak ne zaman üzerinde tam denetime sahip.

## <a name="image-retention-policy"></a>Görüntü bekletme ilkesi
Geçerli ilkemizi tüm daha önce yayımlanmış görüntülerin tutmaktır. Herhangi bir türde sorunlara neden bilinen görüntüleri kaldırma hakkını saklı tutarız. Örneğin, sonraki platform veya bileşen güncelleştirmeleri nedeniyle yanlış yapılandırmaları görüntülerle kaldırılabilir. 30 gün önce resim kaldırma yukarı bildirimleri sağlamak için geçerli Market ilke kaldırılabilir görüntüleri izler.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Red Hat güncelleştirme altyapısı hakkında daha fazla bilgi [burada](https://aka.ms/rhui-update).
* RHEL tüm sürümleri için Red Hat destek ilkeleri hakkında daha fazla bilgi bulunabilir [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfası.
