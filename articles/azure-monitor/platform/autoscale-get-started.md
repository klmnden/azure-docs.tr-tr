---
title: Azure'da otomatik ölçeklendirme ile çalışmaya başlama
description: Web uygulaması kaynağınıza ölçeklendirmeyi öğrenin, Azure'da bulut hizmeti, sanal makine veya sanal makine ölçek kümesi.
author: rajram
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: rajram
ms.component: autoscale
ms.openlocfilehash: 0535c84a8ee0776c2c35a46d3c7510a2cd615cf6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60788625"
---
# <a name="get-started-with-autoscale-in-azure"></a>Azure'da otomatik ölçeklendirme ile çalışmaya başlama
Bu makalede, Microsoft Azure portalında kaynağınıza, otomatik ölçeklendirme ayarlarını ayarlamak açıklar.

Azure İzleyici otomatik ölçeklendirme için yalnızca geçerlidir [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="discover-the-autoscale-settings-in-your-subscription"></a>Otomatik ölçeklendirme ayarları, aboneliğinizdeki keşfedin
Otomatik ölçeklendirme'nin Azure İzleyici'de geçerli olduğu tüm kaynakları bulabilir. Adım adım bir kılavuz için aşağıdaki adımları kullanın:

1. Açık [Azure portalı.][1]
1. Sol bölmede Azure İzleyici simgesine tıklayın.
  ![Azure İzleyicisi'ni Aç][2]
1. Tıklayın **otomatik ölçeklendirme** otomatik ölçeklendirme olduğu yanı sıra geçerli otomatik ölçeklendirme durumlarıyla ilgili tüm kaynakları görüntülemek için.
  ![Azure İzleyici otomatik ölçeklendirme keşfedin][3]

Kaynakları belirli bir kaynak grubu, belirli kaynak türlerine veya belirli bir kaynak seçmek için üst kapsamını listesini daraltmak için Filtre bölmesini kullanabilirsiniz.

Her bir kaynak için geçerli örnek sayısına ve otomatik ölçeklendirme durumu bulabilirsiniz. Otomatik ölçeklendirme durumu olabilir:

- **Yapılandırılmamış**: Otomatik ölçeklendirme henüz bu kaynak için etkinleştirmediniz.
- **Etkin**: Bu kaynak için otomatik ölçeklendirmeyi etkinleştirdiniz.
- **Devre dışı bırakılmış**: Bu kaynak için otomatik ölçeklendirme devre dışı bırakıldı.

## <a name="create-your-first-autoscale-setting"></a>İlk uygulamanızı otomatik ölçeklendirme ayarı oluşturma

Şimdi, ilk otomatik ölçeklendirme ayarı oluşturmak için basit adım adım kılavuz şimdi gidin.

1. Açık **otomatik ölçeklendirme** dikey penceresinde Azure İzleyici ve ölçek istediğiniz bir kaynak seçin. (Bir web uygulaması ile ilişkili bir App Service planı aşağıdaki adımları kullanın. Yapabilecekleriniz [5 dakika içinde Azure'da ilk ASP.NET web uygulamanızı oluşturun.] [4])
1. Geçerli örnek sayısını 1 olduğuna dikkat edin. Tıklayın **etkinleştirmek otomatik ölçeklendirme**.
  ![Yeni web uygulaması için ölçek ayarı][5]
1. Ölçek ayarı için bir ad belirtin ve ardından **alınabilecek**. Bir bağlam bölmesi sağ tarafındaki açık ölçek kuralı seçeneklerini dikkat edin. Varsayılan olarak, bu yüzde 70 kaynak CPU yüzdesini aşarsa, örnek sayınız 1 ile ölçeklendirme seçeneği ayarlar. Kendi varsayılan değerlerinde bırakın ve tıklayın **Ekle**.
  ![Ölçek ayarı için bir web uygulaması oluşturma][6]
1. İlk, Ölçek kuralı oluşturdunuz. Not UX en iyi yöntemler önerir ve durumları ", en az bir adet ölçek kuralı olması önerilir." Bunu yapmak için:

    a. Tıklayın **alınabilecek**.

    b. Ayarlama **işleci** için **küçüktür**.

    c. Ayarlama **eşiği** için **20**.

    d. Ayarlama **işlemi** için **sayıyı şu kadar Azalt**.

   Ölçek ayarı artık sahipsiniz ölçek dışı/ölçekler, içinde CPU kullanımına göre temel.
   ![CPU üzerinde göre ölçeklendirin][8]
1. **Kaydet**’e tıklayın.

Tebrikler! Şimdi, ilk web uygulamanızı CPU kullanımına göre otomatik ölçeklendirme ölçek ayarı başarıyla oluşturdunuz.

> [!NOTE]
> Aynı adımlar bir sanal makine ölçek kümesi ile kullanmaya başlamak veya Bulut hizmeti rolü için geçerlidir.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar
### <a name="scale-based-on-a-schedule"></a>Bir zamanlamaya göre ölçeklendirin
CPU üzerinde temel ölçek ek olarak, Ölçek kümenizdeki farklı haftanın belirli gün için ayarlayabilirsiniz.

1. Tıklayın **ölçeklendirme koşulu Ekle**.
1. Ölçek modu ve kurallar ayarlayarak varsayılan koşul ile aynı olur.
1. Seçin **belirli günlerde Yinele** zamanlama için.
1. Gün ve ölçek koşulu ne zaman uygulanacağı başlangıç/bitiş saati seçin.

![Zamanlamaya göre ölçek koşulu][9]
### <a name="scale-differently-on-specific-dates"></a>Belirli tarihleri farklı ölçeklendirme
CPU üzerinde temel ölçek ek olarak, Ölçek kümenizdeki farklı belirli tarihleri için ayarlayabilirsiniz.

1. Tıklayın **ölçeklendirme koşulu Ekle**.
1. Ölçek modu ve kurallar ayarlayarak varsayılan koşul ile aynı olur.
1. Seçin **belirtin başlangıç/bitiş tarihlerini** zamanlama için.
1. Başlangıç/bitiş tarihlerini ve ölçek koşulu ne zaman uygulanacağı başlangıç/bitiş saati seçin.

![Tarihleri temel alarak ölçek koşulu][10]

### <a name="view-the-scale-history-of-your-resource"></a>Kaynağınızı ölçek geçmişini görüntüleme
Kaynağınızı yukarı veya aşağı ölçeklendirilebilir her etkinlik günlüğü'nde bir olay kaydedilir. Geçiş tarafından son 24 saat boyunca kaynağınızın ölçek geçmişini görüntüleyebilirsiniz **çalıştırma geçmişi** sekmesi.

![Çalıştırma geçmişi][11]

(En fazla 90 gün için) tam ölçek geçmişini görüntülemek isteyip istemediğinizi seçin **daha fazla ayrıntı görmek için buraya tıklayın**. Etkinlik günlüğü, kaynak ve kategori için önceden seçili otomatik ölçeklendirme ile açılır.

### <a name="view-the-scale-definition-of-your-resource"></a>Görünüm kaynak ölçek tanımı
Otomatik ölçeklendirme, bir Azure Resource Manager kaynağıdır. Geçerek ölçek tanımı JSON biçiminde görüntüleyebilirsiniz **JSON** sekmesi.

![Ölçek tanımı][12]

JSON'da doğrudan, gerekirse değişiklik yapabilirsiniz. Bunları kaydettiğinizde bu değişiklikler yansıtılır.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Otomatik ölçeklendirme devre dışı bırakın ve el ile örneklerinizi ölçeklendirin
Geçerli ölçek ayarı devre dışı bırakmanız ve kaynak el ile ölçeklendirme istediğinizde zamanlar olabilir.

Tıklayın **devre dışı bırakmak, otomatik ölçeklendirme** üstünde düğme.
![Otomatik ölçeklendirmeyi devre dışı][13]

> [!NOTE]
> Bu seçenek yapılandırmanızı devre dışı bırakır. Otomatik ölçeklendirme yeniden etkinleştirdikten sonra ancak kendisine dönebilirsiniz.

Artık, el ile ölçeklendirmek istediğiniz örnek sayısını ayarlayabilirsiniz.

![El ile ölçek kümesi][14]

Otomatik ölçeklendirme tıklayarak her zaman geri dönebilirsiniz **etkinleştirmek otomatik ölçeklendirme** ardından **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir etkinlik günlüğü aboneliğinizdeki tüm otomatik ölçeklendirme altyapısı işlemleri izlemek için uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Bir etkinlik günlüğü aboneliğinizdeki tüm başarısız otomatik ölçeklendirme ölçek/ölçeğini işlemleri izlemek için uyarı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/autoscale-get-started/azure-monitor-launch.png
[3]: ./media/autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-dotnet
[5]: ./media/autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/autoscale-get-started/scale-in-recommendation.png
[8]: ./media/autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/autoscale-get-started/scale-condition-schedule.png
[10]: ./media/autoscale-get-started/scale-condition-dates.png
[11]: ./media/autoscale-get-started/scale-history.png
[12]: ./media/autoscale-get-started/scale-definition-json.png
[13]: ./media/autoscale-get-started/disable-autoscale.png
[14]: ./media/autoscale-get-started/set-manualscale.png
