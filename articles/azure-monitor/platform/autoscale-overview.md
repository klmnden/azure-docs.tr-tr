---
title: Sanal makineler, bulut Hizmetleri ve Web uygulamalarını otomatik ölçeklendirmeye genel bakış
description: Microsoft azure'da otomatik ölçeklendirme. Sanal makine ölçek kümeleri, bulut Hizmetleri ve Web uygulamaları gibi sanal makineler için geçerlidir.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: autoscale
ms.openlocfilehash: 05f20aec536ebdb702caea37051a65af9bbc659f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787602"
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Microsoft Azure sanal makineleri, bulut Hizmetleri ve Web uygulamalarını otomatik ölçeklendirmeye genel bakış
Bu makalede, hangi Microsoft Azure otomatik ölçeklendirme, aboneliğin avantajları olduğu ve nasıl kullanmaya başlayacağınızı açıklar.  

Azure İzleyici otomatik ölçeklendirme için yalnızca geçerlidir [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

> [!NOTE]
> Azure otomatik ölçeklendirme iki yöntem vardır. Otomatik ölçeklendirme daha eski bir sürümünü (kullanılabilirlik kümeleri) sanal makineler için geçerlidir. Bu özellik desteği sınırlıdır ve sanal makine ölçek kümeleri için daha hızlı ve daha güvenilir otomatik ölçeklendirme desteği geçiş önerilir. Bu makalede eski teknoloji kullanma hakkında bir bağlantı bulunur.  
>
>

## <a name="what-is-autoscale"></a>Otomatik ölçeklendirme nedir?
Otomatik ölçeklendirme, uygulamanızın üzerindeki yükü işlemek için çalışan kaynakları doğru miktarda sahip olmanızı sağlar. Bu, yük artışlarını işlemek ve da oturan kaynakları kaldırarak tasarruf etmek için kaynakları eklemek boşta sağlar. Çalıştırın ve eklemek veya Vm'leri otomatik olarak bir kurallar kümesine göre kaldırmak için örneklerin minimum ve maksimum bir sayı belirtin. Bir en düşük yapar emin olması, uygulamanızın her zaman bile hiçbir yük altında çalışıyor. En fazla olması, saatlik maliyet toplam olası sınırlar. Sizin oluşturduğunuz kurallarını kullanarak bu iki uç arasında otomatik olarak ölçeklendirin.

 ![Otomatik ölçeklendirme açıklanmıştır. VM’leri ekleme ve kaldırma](./media/autoscale-overview/AutoscaleConcept.png)

Bir veya daha fazla otomatik ölçeklendirme eylemlerinin kural koşulları karşılandığında tetiklenir. Ekleyebilir ve sanal makineleri kaldırın veya başka işlemler gerçekleştirme. Bu işlem aşağıdaki kavramsal diyagramı görülmektedir.  

 ![Otomatik ölçeklendirme Akış Diyagramı](./media/autoscale-overview/Autoscale_Overview_v4.png)

Aşağıdaki açıklama, önceki diyagramda parçaları için geçerlidir.   

## <a name="resource-metrics"></a>Kaynak Ölçümleri
Kaynak ölçümleri yayma, bu ölçümleri daha sonra kuralları tarafından işlenir. Ölçümleri farklı yöntemleri sunulur.
Doğrudan Azure altyapısını telemetri Web uygulamaları ve bulut Hizmetleri sunulur ancak sanal makine ölçek kümeleri Azure tanılama aracılardan telemetri verilerini kullanın. Yaygın olarak kullanılan bazı istatistikler, CPU kullanımı, bellek kullanımı, iş parçacığı sayıları, kuyruk uzunluğu ve disk kullanımını içerir. Hangi telemetri verilerini kullanabileceğiniz bir listesi için bkz. [otomatik ölçeklendirme ortak ölçümleri](../../azure-monitor/platform/autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Özel Ölçümler
Ayrıca, uygulamaları yayma kendi özel ölçümler de yararlanabilirsiniz. Application Insights ölçüm göndermek için uygulamaları yapılandırdıysanız veya ölçeklendirme yapılıp hakkında kararlar almak için bu ölçümleri yararlanabilirsiniz.

## <a name="time"></a>Zaman
Zamanlama tabanlı kurallar UTC temel alır. Saat diliminizi yaparken, kurallarını ayarlama düzgün şekilde ayarlamalısınız.  

## <a name="rules"></a>Kurallar
Yalnızca bir Otomatik ölçek kuralını diyagramda gösterilmektedir, ancak çoğu sahip olabilir. Durumunuz için gerektiği şekilde, karmaşık çakışan kurallar oluşturabilirsiniz.  Kural türlerini içerir  

* **Ölçüm tabanlı** -CPU kullanımı % 50 olduğunda, örneğin, bu eylemi gerçekleştirin.
* **Zamana bağlı** - Örneğin, belirli bir saat dilimindeki Cumartesi günleri sabah her 8 Web kancası tetikleyici.

Ölçüm tabanlı kurallar uygulama yükü ölçmek ve ekleyin veya bu yüküne göre sanal makineleri kaldırın. Zamanlama tabanlı kurallar saat düzenleri yükleme bakın ve bir olası yük artışı önce ölçeklendirmek istiyorsunuz veya düşüş ortaya çıktığında ölçeklendirme sağlar.  

## <a name="actions-and-automation"></a>Eylemler ve Otomasyon
Kurallar, bir veya daha fazla tür eylem tetikleyebilirsiniz.

* **Ölçek** -ölçek Vm'leri daraltma veya genişletme
* **E-posta** -abonelik yöneticileri, ortak Yöneticiler ve/veya belirttiğiniz ek e-posta adresine e-posta Gönder
* **Web kancaları otomatikleştirmek** -içinde veya dışında Azure birden çok karmaşık eylemleri tetiklemek Web kancaları çağırın. Azure içinde bir Azure Otomasyonu runbook, Azure işlevi veya Azure Logic App başlayabilirsiniz. Örnek Azure dışındaki üçüncü taraf URL'sini Slack ve Twilio gibi hizmetler içerir.

## <a name="autoscale-settings"></a>Otomatik Ölçeklendirme Ayarları
Otomatik ölçeklendirme, aşağıdaki terimler ve yapısını kullanın.

- Bir **otomatik ölçeklendirme ayarı** ölçeği artırın veya azaltın belirlemek için otomatik ölçeklendirme altyapısı tarafından okunur. Bir veya daha fazla profiller, hedef kaynak ve bildirim ayarları hakkında bilgi içerir.

  - Bir **otomatik ölçeklendirme profilini** y: birleşimidir

    - **Kapasite ayarı**, minimum, maksimum gösterir ve örnek sayısı için varsayılan değerler.
    - **Kural kümesi**, her biri bir tetikleyici (zaman veya ölçüm) ve bir ölçeklendirme eylemi (yukarı veya aşağı) içerir.
    - **Yinelenme**, otomatik ölçeklendirme yürürlüğe bu profili zaman koymalısınız gösterir.

      Farklı çakışan gereksinimleri ölçeklendirilmesini sağlayan birden çok profil olabilir. Örneğin, gün veya Haftanın günlerinin ingilizceleridir farklı saatler için farklı otomatik ölçeklendirme profilleri olabilir.

  - A **bildirim ayarı** otomatik ölçeklendirme ayarının profilleri birinin ölçütlerine göre otomatik ölçeklendirme olay gerçekleştiğinde hangi bildirimleri gerçekleşmesi gerektiğini tanımlar. Otomatik ölçeklendirme, bir veya daha fazla e-posta adreslerine bildirim ya da bir veya daha fazla Web kancaları çağrı yapmak.


![Azure otomatik ölçeklendirme ayarı, profil ve kural yapısı](./media/autoscale-overview/AzureResourceManagerRuleStructure3.png)

Yapılandırılabilir alanları ve açıklamaları tam listesi kullanılabilir [otomatik ölçeklendirmeyi REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Kod örnekleri için bkz.

* [VM ölçek kümeleri için Resource Manager şablonlarını kullanarak gelişmiş otomatik ölçeklendirme yapılandırması](../../azure-monitor/platform/autoscale-virtual-machine-scale-sets.md)  
* [Otomatik ölçeklendirme REST API](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Yatay ve dikey olarak ölçeklendirme
Otomatik ölçeklendirme yalnızca yatay bir artış ("dışarı" olmak üzere) veya ("içinde") azaltmak sanal makine örneği sayısını ölçeklendirir.  Yatay potansiyel olarak binlerce sanal makinelerin yükünü işlemek için çalıştırmanıza izin verdiğinden, bir bulut durumda daha esnektir.

Buna karşılık, dikey ölçeklendirme farklıdır. VM'lerin aynı sayıda tutar, ancak daha fazla ("yukarı") veya ("kapalı") güçlü Vm'leri yapar. Güç, bellek, CPU hızı, disk alanı, vb. ölçülür.  Dikey ölçeklendirme, daha fazla sınırlamaları vardır. Hızla bir üst sınır denk gelir ve bölgeye göre değişiklik büyük donanım kullanılabilirliğine bağlıdır. Dikey ölçeklendirme da genellikle sanal Makineyi durdurun ve yeniden başlatma gerektirir.

Daha fazla bilgi için [Azure Otomasyonu ile Azure sanal makine dikey olarak ölçeklendirme](../../virtual-machines/linux/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="methods-of-access"></a>Erişim yöntemi
Otomatik ölçeklendirme ayarlayabilirsiniz.

* [Azure portal](../../azure-monitor/platform/autoscale-get-started.md)
* [PowerShell](../../azure-monitor/platform/powershell-quickstart-samples.md#create-and-manage-autoscale-settings)
* [Platformlar arası Komut Satırı Arabirimi (CLI)](../../azure-monitor/platform/cli-samples.md#autoscale)
* [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Desteklenen hizmetler için otomatik ölçeklendirme

| Hizmet | Şema ve belgeler |
| --- | --- |
| Web Apps |[Scaling Web Apps](../../azure-monitor/platform/autoscale-get-started.md) |
| Cloud Services |[Otomatik ölçeklendirme bir bulut hizmeti](../../cloud-services/cloud-services-how-to-scale-portal.md) |
| Sanal Makineler: Klasik |[Klasik sanal makine kullanılabilirlik kümeleri ölçeklendirme](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Sanal Makineler: Windows ölçek kümeleri |[Sanal makine ölçeğini artırma veya azaltma Windows ayarlar](../../virtual-machine-scale-sets/tutorial-autoscale-powershell.md) |
| Sanal Makineler: Linux ölçek kümeleri |[Linux sanal makine ölçeğini artırma veya azaltma ayarlar](../../virtual-machine-scale-sets/tutorial-autoscale-cli.md) |
| Sanal Makineler: Windows örneği |[VM ölçek kümeleri için Resource Manager şablonlarını kullanarak gelişmiş otomatik ölçeklendirme yapılandırması](../../azure-monitor/platform/autoscale-virtual-machine-scale-sets.md) |
| API Management hizmeti|[Bir Azure API Management örneğini otomatik olarak ölçeklendirme](https://docs.microsoft.com/azure/api-management/api-management-howto-autoscale)

## <a name="next-steps"></a>Sonraki adımlar
Otomatik ölçeklendirme hakkında daha fazla bilgi için otomatik ölçeklendirme izlenecek yollar kullanmak daha önce listelenen veya aşağıdaki kaynaklara bakın:

* [Azure İzleyici otomatik ölçeklendirme ortak ölçümleri](../../azure-monitor/platform/autoscale-common-metrics.md)
* [Azure İzleyici otomatik ölçeklendirme için en iyi yöntemler](../../azure-monitor/platform/autoscale-best-practices.md)
* [E-posta ve Web kancası uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın](../../azure-monitor/platform/autoscale-webhook-email.md)
* [Otomatik ölçeklendirme REST API](https://msdn.microsoft.com/library/dn931953.aspx)
* [Sorun giderme sanal makine ölçek kümelerini otomatik ölçeklendirme](../../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
