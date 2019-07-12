---
title: Azure'da Red Hat Enterprise Linux görüntüleri | Microsoft Docs
description: Microsoft azure'da Red Hat Enterprise Linux görüntüleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 6/6/2019
ms.author: borisb
ms.openlocfilehash: f7ae82b0376489e21b35e4e94dce32805bea69c6
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708375"
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
Şu anda yayımlanan RHEL görüntüleri tüm Kullandıkça Öde modeli kullanır ve bağlı [azure'da Red Hat Update Infrastructure'a (RHUI)](https://aka.ms/rhui-update). RHEL 7 ailesi görüntüler, disk bölümleme şeması için yeni bir adlandırma kuralı tarafından benimsenmiştir (ham LVM) sürümü yerine SKU belirtilir. RHEL görüntüsü sürümü ya da içerecek 7 ham veya 7 LVM. Şu anda ailesi adlandırma değiştirilmediği RHEL 6.

Bu adlandırma SKU'ları görüntüde RHEL 7 2 tür olacaktır: Alt sürüm listesi SKU'ları ve SKU'ları yok. Bir 7 RAW kullanın veya 7 LVM SKU istiyorsanız, RHEL podverze sürümünde dağıtmak istediğinizi belirtebilirsiniz. "Son" sürümünü seçerseniz, olmayacak RHEL küçük en son sürümünü sağlandı.

>[!NOTE]
> Görüntü SAP kümesi için RHEL, RHEL sürüm sabit kalır. Bu nedenle, kendi adlandırma kuralı belirli bir sürümü SKU'da içerir.

>[!NOTE]
> Görüntü RHEL 6 kümesi taşınmadı yeni adlandırma kuralı için.

## <a name="extended-update-support-eus"></a>Genişletilmiş güncelleştirme desteği (EUS)
RHEL görüntüleri Nisan 2019 ' kullanılabilir olduğu gibi genişletilmiş güncelleştirme desteği (EUS) depolar için varsayılan olarak eklenir. RHEL EUS hakkında daha fazla ayrıntı kullanılabilir [Red Hats belgeleri](https://access.redhat.com/articles/rhel-eus).

Sanal makinenizin EUS ve EUS destek ömrü son tarihleri hakkında daha fazla ayrıntı için geçiş yapmak yönergeler kullanılabilir [burada](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> EUS RHEL ek özellikler üzerinde desteklenmiyor. Bu, RHEL ek özellikler kanaldan genellikle kullanılabilir olan bir paket yüklüyorsanız, EUS sırada üzerinde bunu mümkün olmayacağını anlamına gelir. Red Hat ek özellikler ürün yaşam döngüsü ayrıntılı [burada](https://access.redhat.com/support/policy/updates/extras/).

### <a name="for-customers-that-want-to-use-eus-images"></a>EUS görüntüleri kullanmak istediğiniz müşterileri için:
EUS depolara bağlı görüntüleri kullanmak istediğiniz müşteriler sku'daki RHEL küçük versiyon numarasını içeren RHEL görüntüsü kullanmanız gerekir. Bu görüntüleri bölümlenmiş ham (yani değil LVM).

Örneğin, aşağıdaki 2 RHEL 7.4 sağlanan görüntülerin görebilirsiniz:
```bash
RedHat:RHEL:7-RAW:7.4.2018010506
RedHat:RHEL:7.4:7.4.2019041718
```
Bu durumda, `RedHat:RHEL:7.4:7.4.2019041718` EUS depolar için varsayılan olarak eklenir ve `RedHat:RHEL:7-RAW:7.4.2018010506` EUS olmayan depolar için varsayılan olarak eklenir.

### <a name="for-customers-that-dont-want-to-use-eus-images"></a>EUS görüntüleri kullanmak istemiyorsanız müşteriler için:
Varsayılan olarak EUS için bağlı bir görüntüyü kullanmak istemiyorsanız, küçük versiyon numarasını sku'daki içermeyen bir görüntü kullanarak dağıtın.

#### <a name="rhel-images-with-eus"></a>RHEL görüntüleri EUS ile
Aşağıdaki tabloda, bir ikincil sürüm sku'daki içeren RHEL görüntüleri için geçerli olur.

>[!NOTE]
> Makalenin yazıldığı sırada, yalnızca RHEL 7.4 ve sonraki ikincil sürümleri desteği EUS sahip. EUS RHEL için artık desteklenmeyen < 7.3 =.

Alt sürüm |EUS resim örneği              |EUS durumu                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7.4      |RedHat:RHEL:7.4:7.4.2019041718 | Görüntüleri Nisan 2019 yayımlanan ve daha sonra varsayılan olarak EUS olacaktır|
RHEL 7.5      |RedHat:RHEL:7.5:7.5.2019060305 | Görüntüleri Haziran 2019 yayımlanan ve daha sonra varsayılan olarak EUS olacaktır |
RHEL 7.6      |RedHat:RHEL:7.6:7.6.2019052206 | Görüntüleri Mayıs 2019 yayımlanan ve daha sonra varsayılan olarak EUS olacaktır  |
RHEL 8.0      |Yok                            | Hiçbir EUS şu anda sunulmakta olan görüntülerin                 |


## <a name="list-of-rhel-images-available"></a>RHEL görüntüleri kullanılabilir
SKU'ları şu anda genel kullanıma açık aşağıdaki teklifler şunlardır:

Sunduğu| SKU | Bölümleme | Sağlama | Notlar
:----|:----|:-------------|:-------------|:-----
RHEL          | HAM 7    | RAW    | Linux Aracısı | RHEL 7 ailesi görüntüler. <br> EUS depolar için varsayılan olarak bağlı değil.
|             | 7-LVM    | LVM    | Linux Aracısı | RHEL 7 ailesi görüntüler. <br> EUS depolar için varsayılan olarak bağlı değil.
|             | 7-RAW-CI | HAM CI | Cloud-init  | RHEL 7 ailesi görüntüler. <br> EUS depolar için varsayılan olarak bağlı değil.
|             | 6.7      | RAW    | Linux Aracısı | RHEL 6.7 görüntüleri, eski adlandırma kuralı
|             | 6.8      | RAW    | Linux Aracısı | RHEL 6,8 için yukarıdaki gibi aynı
|             | 6.9      | RAW    | Linux Aracısı | RHEL 6.9 için yukarıdaki gibi aynı
|             | 6.10     | RAW    | Linux Aracısı | RHEL 6.10 için yukarıdaki gibi aynı
|             | 7.2      | RAW    | Linux Aracısı | RHEL 7.2 için yukarıdaki gibi aynı
|             | 7.3      | RAW    | Linux Aracısı | RHEL 7.3 için yukarıdaki gibi aynı
|             | 7.4      | RAW    | Linux Aracısı | RHEL 7.4 için yukarıdaki gibi aynı. <br> Varsayılan olarak Nisan 2019 tarihinde EUS depolara bağlı
|             | 7.5      | RAW    | Linux Aracısı | RHEL 7.5 için yukarıdaki gibi aynı. <br> Varsayılan olarak Haziran 2019 tarihinde EUS depolara bağlı
|             | 7.6      | RAW    | Linux Aracısı | RHEL 7.6 için yukarıdaki gibi aynı. <br> Varsayılan olarak Mayıs 2019 tarihinde EUS depolara bağlı
RHEL-SAP      | 7.4      | LVM    | Linux Aracısı | SAP HANA ve iş kolu uygulamaları için RHEL 7.4
|             | 7.5      | LVM    | Linux Aracısı | SAP HANA ve iş kolu uygulamaları için RHEL 7.5
RHEL-SAP-HANA | 6.7      | RAW    | Linux Aracısı | SAP HANA için RHEL 6.7
|             | 7.2      | LVM    | Linux Aracısı | SAP HANA için RHEL 7.2
|             | 7.3      | LVM    | Linux Aracısı | SAP HANA için RHEL 7.3
RHEL-SAP-APPS | 6.8      | RAW    | Linux Aracısı | 6,8 RHEL for SAP Business Applications
|             | 7.3      | LVM    | Linux Aracısı | 7\.3 RHEL for SAP Business Applications

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
