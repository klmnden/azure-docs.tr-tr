---
title: "Otomatik ölçeklendirme bir bulut hizmeti Portalı'nda | Microsoft Docs"
description: "Portal ile Azure bulut hizmeti web rolü veya çalışan rolü için otomatik ölçeklendirme kurallarını yapılandırmak için nasıl kullanılacağını öğrenin."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 0eea38cdb9827ab6e322025ff344ebbab0e83da3
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a>Bir bulut hizmeti Portalı'nda ölçeklendirme otomatik yapılandırma

Koşullar bir ölçekte veya işlemi uzaklaştırma tetikleyen bir bulut hizmeti çalışan rolü için ayarlayabilirsiniz. Rolü için koşulları CPU, disk veya ağ yükü rolünün temel alabilir. Ayrıca, bir ileti sırası veya aboneliğinizle ilişkilendirilmiş diğer bazı Azure kaynak ölçümü temel alarak bir koşul ayarlayabilirsiniz.

> [!NOTE]
> Bu makalede bulut hizmeti web ve çalışan rolleri odaklanır. Bir sanal makine (Klasik) doğrudan oluşturduğunuzda, bir bulut hizmetinde barındırılır. Standart bir sanal makine ile ilişkilendirerek ölçeklenebilen bir [kullanılabilirlik kümesi](../virtual-machines/windows/classic/configure-availability-classic.md) ve bunları el ile açın veya kapatın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Uygulamanız için ölçeklendirme yapılandırmadan önce aşağıdaki bilgileri dikkate alın:

* Ölçeklendirme Çekirdek kullanımını etkilenir.

    Daha fazla sayıda çekirdek büyük rol örneği kullanın. Aboneliğiniz için bir uygulama yalnızca çekirdek sınırının içinde ölçeklendirebilirsiniz. Örneğin, aboneliğiniz 20 olarak belirlenen çekirdek sınırına sahip söyleyin. Bir uygulamayı iki orta ölçekli bulut hizmetleriyle (4 çekirdeğe toplamı) çalıştırırsanız, kalan 16 çekirdek tarafından aboneliğinizde diğer bulut hizmet dağıtımları yalnızca ölçeklendirebilirsiniz. Boyutları hakkında daha fazla bilgi için bkz: [bulut hizmeti boyutları](cloud-services-sizes-specs.md).

* Ölçeklendirmek bir kuyruk iletisi eşiğine dayalı. Kuyruklarının nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [kuyruk depolama hizmetini kullanmayı](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Ayrıca, aboneliğinizle ilişkilendirilmiş diğer kaynakları ölçeklendirebilirsiniz.

* Yüksek kullanılabilirlik, uygulamanızın etkinleştirmek için iki veya daha fazla rol örneği ile dağıtılan emin olmalısınız. Daha fazla bilgi için bkz: [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).

* Otomatik ölçek yalnızca olur tüm rolleri içinde olduğunda **hazır** durumu.  


## <a name="where-scale-is-located"></a>Ölçek bulunduğu
Bulut hizmeti seçtikten sonra bulut hizmeti dikey görünür olması gerekir.

1. Bulut hizmeti dikey üzerinde **roller ve örnekler** döşeme, bulut hizmeti adını seçin.   
   **Önemli**: Bulut hizmet rolü değil rolü rol örneği tıklattığınızdan emin olun.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Seçin **ölçek** döşeme.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Otomatik ölçeklendirme
Bir rol için ölçek ayarları ya da iki modlarıyla yapılandırabilirsiniz **el ile** veya **otomatik**. Beklediğiniz gibi el ile örnekleri mutlak sayısını ayarlayın. Ancak otomatik nasıl ve nasıl tarafından çok yöneten ayarlama kuralları ölçeklendirmek izin verir.

Ayarlama **göre Ölçeklendirmeniz** için seçenek **zamanlama ve performans kuralları**.

![Bulut Hizmetleri ölçek ayarları profili ve kural](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Varolan bir profili.
2. Üst profil için bir kural ekleyin.
3. Başka bir profili ekleyin.

Seçin **profili eklemek**. Ölçek için kullanmak istediğiniz hangi modun profil belirler: **her zaman**, **yineleme**, **sabit tarihi**.

Kuralları ve profil yapılandırdıktan sonra Seç **kaydetmek** simgesine tıklayın.

#### <a name="profile"></a>Profil
Ölçek için minimum ve maksimum örnek profilin ayarlar ve aynı zamanda bu ölçek aralığı etkinken.

* **Her zaman**

    Her zaman bu aralıktaki kullanılabilir örneklerinin tutun.  

    ![Her zaman ölçeklendirme bulut hizmeti](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Yineleme**

    Bir dizi ölçeklendirmek için haftanın günlerini seçin.

    ![Bulut hizmeti ölçekli bir yineleme zamanlaması](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Sabit tarih**

    Rol ölçeklendirmek için sabit bir tarih aralığı.

    ![Bulut hizmeti ölçekli sabit tarih](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Profil yapılandırdıktan sonra Seç **Tamam** profili dikey penceresinin altındaki düğmesini.

#### <a name="rule"></a>Kural
Kurallar bir profile eklenir ve ölçek tetikleyen bir koşul temsil eder.

Kural tetikleyici koşullu değeri ekleyebileceğiniz bulut hizmeti (CPU kullanımı, disk etkinliği veya ağ etkinliği) ölçüsü üzerinde temel alır. Ayrıca, bir ileti sırası veya aboneliğinizle ilişkilendirilmiş diğer bazı Azure kaynak ölçümü temel alarak tetikleme olabilir.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Kural yapılandırdıktan sonra Seç **Tamam** kural dikey penceresinin altındaki düğmesini.

## <a name="back-to-manual-scale"></a>El ile Ölçeklendir
Gidin [ölçek ayarı](#where-scale-is-located) ve **göre Ölçeklendirmeniz** için seçenek **el ile girdiğim bir örnek sayısı**.

![Bulut Hizmetleri ölçek ayarları profili ve kural](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Bu ayar otomatik ölçeklendirme rolden kaldırır ve ardından örnek sayısı doğrudan ayarlayabilirsiniz.

1. Ölçek (el ile veya otomatik) seçeneği.
2. İçin ölçeklendirmek için örnekleri ayarlamak için bir rol örneği kaydırıcı.
3. İçin ölçeklendirmek için rol örnekleri.

Ölçek ayarları yapılandırdıktan sonra Seç **kaydetmek** simgesine tıklayın.
