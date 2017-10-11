---
title: "Microsoft Azure sanal makineler, bulut Hizmetleri ve Web uygulamaları otomatik ölçeklendirme genel bakış | Microsoft Docs"
description: "Microsoft Azure otomatik ölçeklendirme genel bakış. Sanal makineler, bulut Hizmetleri ve Web uygulamaları için geçerlidir."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 74bf03be-e658-4239-a214-c12424b53e4c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2016
ms.author: robb
ms.openlocfilehash: 413828d79d79c181c662bc7cfb4114345de57f90
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Microsoft Azure sanal makineler, bulut Hizmetleri ve Web uygulamaları otomatik ölçeklendirme genel bakış
Bu makalede, hangi Microsoft Azure otomatik ölçeklendirme, onun avantajlarını olduğu ve nasıl kullanmaya başlayacağınızı açıklar.  

Azure İzleyici otomatik ölçeklendirme uygular yalnızca [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/), ve [uygulama hizmeti - Web Apps](https://azure.microsoft.com/services/app-service/web/).

> [!NOTE]
> Azure iki otomatik ölçeklendirme yöntemlerine sahiptir. Otomatik ölçeklendirme daha eski bir sürümü (kullanılabilirlik kümeleri) sanal makineler için geçerlidir. Bu özellik sınırlı destek ve daha hızlı ve daha güvenilir otomatik ölçeklendirme desteği için sanal makine ölçek kümeleri geçirme öneririz. Bu makalede eski teknolojisini kullanmak nasıl bir bağlantı bulunur.  
>
>

## <a name="what-is-autoscale"></a>Otomatik ölçeklendirme nedir?
Otomatik ölçeklendirme, uygulamanızın üzerinde yükü işlemek üzere çalışan kaynakları doğru miktarda sahip olmanızı sağlar. Yük arttıkça işlemek ve ayrıca durduğunu kaynakları kaldırarak paradan tasarruf için kaynakları eklemek boşta sağlar. Örnekleri çalıştırmak ve eklemek veya otomatik olarak bir dizi kurala göre VM'ler kaldırmak için minimum ve maksimum sayısı belirtin. Bir minimum yapar emin olması, uygulamanızın her zaman bile herhangi bir yük altında çalışıyor. Maksimum sahip saatlik maliyet, toplam olası sınırlar. Oluşturduğunuz kurallarını kullanarak bu iki uç nokta arasında otomatik olarak ölçeklendirin.

 ![Otomatik ölçeklendirme açıklanmıştır. Sanal makineleri ekleyip](./media/monitoring-overview-autoscale/AutoscaleConcept.png)

Bir veya daha fazla otomatik ölçeklendirme eylemi Kural koşulu karşılandığında tetiklenir. Ekleme ve sanal makineleri kaldırın veya başka eylemler gerçekleştirebilir. Aşağıdaki alanlara yönelik kavramsal diyagram bu işlemi gösterilmektedir.  

 ![Otomatik ölçeklendirme Akış Diyagramı](./media/monitoring-overview-autoscale/Autoscale_Overview_v4.png)

Aşağıdaki açıklamaya önceki diyagramda parçaları için geçerlidir.   

## <a name="resource-metrics"></a>Kaynak ölçümleri
Kaynak ölçümleri yayma, bu ölçümleri daha sonra kuralları tarafından işlenir. Ölçümleri farklı yöntemler sunulur.
Web uygulamaları ve bulut Hizmetleri için telemetri doğrudan Azure altyapısından gelir ancak sanal makine ölçek kümeleri telemetri verilerini Azure Tanılama aracı kullanın. Bazı yaygın olarak kullanılan istatistik CPU kullanımı, bellek kullanımı, iş parçacığı sayıları, kuyruk uzunluğu ve disk kullanımını içerir. Kullanabileceğiniz hangi telemetri verilerini bir listesi için bkz: [otomatik ölçeklendirme ortak ölçümleri](insights-autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Özel ölçümleri
Uygulamaları yayma kendi özel ölçümleri da kullanabilirsiniz. Application Insights'a ölçümleri göndermek için uygulamaları yapılandırdıysanız olup olmadığına göre veya ölçeklendirmek kararlar almak için bu ölçümleri yararlanabilirsiniz. 

## <a name="time"></a>Zaman
Zamanlama tabanlı kuralları UTC üzerinde temel alır. Kuralları ayarladığınız ayarlarken, saat dilimini düzgün şekilde ayarlamalısınız.  

## <a name="rules"></a>Kurallar
Yalnızca bir otomatik ölçeklendirme kural diyagramda gösterilmektedir, ancak bunların kaç sahip olabilir. Durumunuz için gerektiği gibi karmaşık çakışan kurallar oluşturabilirsiniz.  Kural türlerini içerir  

* **Ölçüm tabanlı** -Örneğin, CPU kullanımı % 50 ' olduğunda bu eylemi gerçekleştirin.
* **Zamana bağlı** - Örneğin, tetikleyici bir Web kancası her 8: 00 belirli bir saat diliminde Cumartesi.

Ölçüm tabanlı kurallar uygulama yük ölçmek ve eklemek veya bu yüküne göre sanal makineleri kaldırın. Zamanlama tabanlı kuralları yükleme saat desenleri görmek ve olası yük artışı önce ölçeklendirmek istediğiniz veya düşüş ortaya çıktığında ölçeklendirmenizi izin verir.  

## <a name="actions-and-automation"></a>Eylemler ve Otomasyon
Kurallar, bir veya daha fazla tür eylemlerin tetikleyebilir.

* **Ölçek** -ölçek VM'ler veya uzaklaştırma
* **E-posta** -abonelik yöneticileri, ortak yöneticilere ve/veya belirttiğiniz ek e-posta adresini e-posta Gönder
* **Web kancası otomatikleştirmek** -birden çok karmaşık eylem içinde veya Azure dışında tetikleyebilir Web kancalarını çağırın. Azure içinde bir Azure Otomasyonu runbook, Azure işlevi veya Azure mantıksal uygulama başlatabilirsiniz. Azure dışında örnek üçüncü taraf URL kayma ve Twilio gibi hizmetleri içerir.

## <a name="autoscale-settings"></a>Otomatik ölçeklendirme ayarları
Otomatik ölçeklendirme aşağıdaki terimleri ve yapısını kullanın.

- Bir **otomatik ölçeklendirme ayarı** yukarı veya aşağı ölçeklendirmek karar vermek için otomatik ölçeklendirme altyapısı tarafından okunur. Bu bir veya daha fazla profiller, hedef kaynak ve bildirim ayarları hakkında bilgi içerir.

    - Bir **otomatik ölçeklendirme profili** a: birleşimidir

        - **Kapasite ayarı**, minimum, maksimum gösterir ve örnek sayısı için varsayılan değerler.
        - **Kural kümesi**, her biri bir tetikleyici (saat veya Metrik) ve bir ölçek eylemi (yukarı veya aşağı) içerir.
        - **Yineleme**, ne zaman otomatik ölçeklendirme bu profili yürürlüğe koymak gösterir.

        Farklı çakışan gereksinimlerini ilgilenebilmek olanak tanıyan birden çok profil olabilir. Örneğin, gün veya haftanın günlerini farklı saatler için farklı ölçeklendirme profili olabilir.

    - A **bildirim ayarı** otomatik ölçeklendirme olay otomatik ölçeklendirme ayarının profilleri birinin ölçütlerine göre oluştuğunda hangi bildirimleri gerçekleşmesi gerektiğini tanımlar. Otomatik ölçeklendirme, bir veya daha fazla e-posta adresleri bildir ya da bir veya daha fazla Web kancalarına çağrıları yapma.


![Azure otomatik ölçeklendirme ayarı, profil ve kural yapısı](./media/monitoring-overview-autoscale/AzureResourceManagerRuleStructure3.png)

Yapılandırılabilir alanları ve açıklamalarının tam listesi kullanılabilir [otomatik ölçeklendirme REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Kod örnekleri için bkz:

* [VM ölçek kümesi için Resource Manager şablonları kullanarak gelişmiş otomatik ölçeklendirme yapılandırma](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Otomatik ölçeklendirme REST API'si](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Yatay vs dikey ölçekleme
Otomatik ölçeklendirme yalnızca yatay artışı ("out") veya ("içinde") azaltmak VM örneği sayısını ölçeklendirir.  Yatay yükü işlemek üzere VM'ler binlerce potansiyel olarak çalıştırmanıza izin veren gibi bir bulut durumda daha esnektir.

Buna karşılık, dikey ölçekleme farklıdır. Sanal makineleri aynı sayıda tutar, ancak sanal makineleri ("yukarı") daha fazla veya az ("kapalı") güçlü yapar. Güç, bellek, CPU hızı, disk alanı vb. ölçülür.  Dikey ölçekleme daha fazla sınırlamaları vardır. Hızlı bir şekilde bir üst sınır İsabetli ve bölgeye göre farklılık gösterebilir büyük donanım kullanılabilirliğine bağlıdır. Dikey ölçeklendirme da genellikle durdurup yeniden başlatmak bir VM gerektirir.

Daha fazla bilgi için bkz: [Azure Automation ile Azure sanal makine dikey olarak ölçeklendirmek](../virtual-machines/linux/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="methods-of-access"></a>Erişim yöntemleri
Otomatik ölçeklendirme ayarlayabilirsiniz.

* [Azure portal](insights-how-to-scale.md)
* [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
* [Platformlar arası komut satırı arabirimi (CLI)](insights-cli-samples.md#autoscale)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Otomatik ölçeklendirme için desteklenen hizmetler
| Hizmet | Şema & belgeleri |
| --- | --- |
| Web Apps |[Web uygulamaları ölçeklendirme](insights-how-to-scale.md) |
| Cloud Services |[Otomatik ölçeklendirme bir bulut hizmeti](../cloud-services/cloud-services-how-to-scale.md) |
| Sanal makineler: Klasik |[Klasik sanal makine kullanılabilirlik kümeleri ölçeklendirme](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Sanal makineler: Windows ölçek kümeleri |[Sanal makine ölçek ölçeklendirme Windows ayarlar](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) |
| Sanal makineler: Linux ölçek ayarlar |[Sanal makine ölçek ölçeklendirme Linux ayarlar](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Sanal makineler: Windows örneği |[VM ölçek kümesi için Resource Manager şablonları kullanarak gelişmiş otomatik ölçeklendirme yapılandırma](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Sonraki adımlar
Otomatik ölçeklendirme hakkında daha fazla bilgi için kullanım otomatik ölçeklendirme izlenecek yollar daha önce listelenen veya aşağıdaki kaynaklara bakın:

* [Azure İzleyici otomatik ölçeklendirme ortak ölçümleri](insights-autoscale-common-metrics.md)
* [Azure İzleyici otomatik ölçeklendirme için en iyi yöntemler](insights-autoscale-best-practices.md)
* [E-posta ve Web kancası uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın](insights-autoscale-to-webhook-email.md)
* [Otomatik ölçeklendirme REST API'si](https://msdn.microsoft.com/library/dn931953.aspx)
* [Sorun giderme sanal makine ölçek kümeleri otomatik ölçeklendirme](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
