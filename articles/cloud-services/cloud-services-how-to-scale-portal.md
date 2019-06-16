---
title: Otomatik olarak portalda bir bulut hizmeti ölçeklendirin | Microsoft Docs
description: Azure'daki bir bulut hizmeti web rolü veya çalışan rolü için otomatik ölçeklendirme kurallarını yapılandırmak için portalı kullanmayı öğrenin.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeconnoc
ms.openlocfilehash: f5597773b3127852481d5e14844bed889c4d6f83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61435330"
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a>Otomatik ölçeklendirme portalda bir bulut hizmeti için yapılandırma

Bir ölçek daraltma veya genişletme işlemi tetikleyen bir bulut hizmeti çalışan rolü için koşulları ayarlanabilir. Rolü için koşulları CPU, disk veya ağ yükü rolünün temel alabilir. Ayrıca, bir ileti kuyruğu veya aboneliğinizle ilişkilendirilmiş diğer bazı Azure kaynak ölçüsü bağlı bir koşul ayarlayabilirsiniz.

> [!NOTE]
> Bu makalenin bulut hizmeti web ve çalışan rolleri üzerinde odaklanır. Bir sanal makine (Klasik) doğrudan oluşturduğunuzda, onu bir bulut hizmetinde barındırılır. Standart bir sanal makine ile ilişkilendirerek ölçeklendirilebilir bir [kullanılabilirlik kümesi](../virtual-machines/windows/classic/configure-availability-classic.md) ve bunları el ile açıp kapatabilir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Uygulamanız için ölçeklendirme yapılandırmadan önce aşağıdakileri dikkate almanız gerekir:

* Ölçeklendirme çekirdek kullanımı etkilenir.

    Daha fazla çekirdek büyük rol örneği kullanın. Aboneliğiniz için bir uygulama yalnızca çekirdek sınırına içinde ölçeklendirebilirsiniz. Örneğin, 20 çekirdek sınırı aboneliğiniz olduğunu düşünelim. İki orta ölçekli bulut Hizmetleri ile (4 çekirdek toplam) bir uygulama çalıştırıyorsanız, yalnızca diğer bulut hizmeti dağıtımlarını ayarlayan aboneliğinizde kalan 16 çekirdek ölçeklendirebilirsiniz. Boyutları hakkında daha fazla bilgi için bkz. [bulut hizmeti boyutları](cloud-services-sizes-specs.md).

* Ölçeklendirebileceğiniz bir kuyruk iletisi eşiği temel alarak. Kuyrukları kullanma hakkında daha fazla bilgi için bkz. [kuyruk depolama hizmetini kullanma](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Aboneliğinizle ilişkili diğer kaynaklar da ölçekleyebilirsiniz.

* Uygulamanızın yüksek kullanılabilirliği etkinleştirmek için iki veya daha fazla rol örneğini ile dağıtıldığından emin olmak. Daha fazla bilgi için [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

* Otomatik ölçeklendirme yalnızca tüm roller olduğunda gerçekleşir **hazır** durumu.  


## <a name="where-scale-is-located"></a>Ölçek bulunduğu
Bulut hizmetinizin seçtikten sonra bulut hizmeti dikey görünür olmalıdır.

1. Bulut hizmeti dikey üzerinde **roller ve örnekler** kutucuğunda, bulut hizmeti adını seçin.   
   **ÖNEMLİ**: Bulut hizmeti rolü, rolün rol örneği tıkladığınızdan emin olun.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Seçin **ölçek** Döşe.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Otomatik ölçeklendirme
Bir rol için ölçek ayarları ya da iki mod ile yapılandırabileceğiniz **el ile** veya **otomatik**. Beklediğiniz gibi el ile mutlak bir örnek sayısına ayarlayın. Otomatik, nasıl ve ne kadar yöneten kuralları kümesi ölçeklendirmeniz gerekir ancak izin verir.

Ayarlama **ölçeklendirilmesine** seçeneğini **zamanlama ve performans kuralları**.

![Bulut Hizmetleri ölçek ayarları profili ve kural](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Mevcut bir profili.
2. Üst profili için bir kural ekleyin.
3. Başka bir profili ekleyin.

Seçin **profili ekleme**. Ölçek için kullanmak istediğiniz hangi modda profil belirler: **her zaman**, **yinelenme**, **sabit tarihi**.

Kuralları ve profili yapılandırdıktan sonra seçin **Kaydet** simgesine tıklayın.

#### <a name="profile"></a>Profil
Minimum ve maksimum örnekleri için ölçek, profilin ayarlar ve ayrıca bu ölçek aralığı etkinken.

* **Her zaman**

    Her zaman bu aralığı kullanılabilir örneklerinin tutun.  

    ![Her zaman ölçeklenebilen bulut hizmeti](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Yineleme**

    Bir dizi ölçeklendirmek için haftanın günlerini seçin.

    ![Bir yineleme zamanlaması ile bulut hizmeti ölçeklendirme](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Sabit tarih**

    Rolü'nü ölçeklendirmek için bir sabit tarih aralığı.

    ![Bir sabit tarih ile bulut hizmeti ölçeklendirme](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Profil yapılandırdıktan sonra seçin **Tamam** profili dikey pencerenin alt kısmındaki düğmesi.

#### <a name="rule"></a>Kural
Kuralları profil eklenir ve ölçek tetikleyen bir koşulu temsil eder.

Kural tetikleyici koşullu değeri ekleyebileceğiniz bir bulut hizmetinin (CPU kullanımı, disk etkinliği yok veya ağ etkinliği) bir ölçüme göre temel alır. Ayrıca, bir ileti kuyruğu veya aboneliğinizle ilişkilendirilmiş diğer bazı Azure kaynak ölçü göre tetikleyici olabilir.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Kural yapılandırdıktan sonra seçin **Tamam** kural dikey pencerenin alt kısmındaki düğmesi.

## <a name="back-to-manual-scale"></a>El ile ölçeklendirme
Gidin [ölçeklendirme ayarları](#where-scale-is-located) ayarlayıp **ölçeklendirilmesine** seçeneğini **el ile girdiğim bir örnek sayısı**.

![Bulut Hizmetleri ölçek ayarları profili ve kural](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Bu ayar, otomatik ölçeklendirme rolünden kaldırır ve ardından örnek sayısını doğrudan ayarlayabilirsiniz.

1. Ölçek (el ile veya otomatik) seçeneği.
2. Rol örneği örnekleri ölçeklendirilebilecek şekilde ayarlamak için kaydırıcıyı.
3. Ölçeklendirme için rol örnekleri.

Ölçek ayarları yapılandırdıktan sonra seçin **Kaydet** simgesine tıklayın.
