---
title: Microsoft Azure'da ve Azure İzleyici Klasik uyarılar genel bakış
description: Uyarıları, Azure kaynak ölçümleri, olayları ve günlükleri izlemek ve belirttiğiniz koşulu karşılandığında size bildirilmesini sağlar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: a0abcbdaa7e998413efb717be6e0addc5607ec5c
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114020"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft azure'da Klasik uyarılar nedir?

> [!NOTE]
> Bu makalede, eski, Klasik ölçüm uyarısı oluşturmayı açıklar. Azure İzleyicisi'ni destekler [daha yeni ve neredeyse gerçek zamanlı ölçüm uyarılar ve yeni bir uyarı deneyimi](monitoring-overview-unified-alerts.md). 
>

Uyarıları kullanarak, veriler üzerinde koşulları yapılandırabilirsiniz ve son izleme verilerini koşullara uyan gönderdiğinde bildirim alın.


## <a name="alerts-on-azure-monitor-data"></a>Verileri Azure izleme ile ilgili uyarılar
Klasik uyarılar iki tür vardır: ölçüm uyarıları ve etkinlik günlüğü uyarıları.

* **Klasik ölçüm uyarıları**: Belirtilen Ölçüm değerini atadığınız eşiği aştığında bu uyarı tetikler. BT etkinleştirildi"(eşiği aşıldığında ve uyarı koşulu karşılandığında olduğunda)," uyarı bir bildirim üretir. "(Eşiği yeniden çapraz ve koşulu artık karşılanmıyor olduğunda) çözüldüğünde" da bir uyarı oluşturur. 

* **Klasik etkinlik günlüğü uyarıları**: bir etkinlik günlüğü olayında eşleşme atadığınız ölçütleri Filtrele oluşturulduğunda, bu akış günlüğü uyarısı tetikler. Uyarı altyapısı için yalnızca filtre ölçütlerini herhangi bir yeni olayın geçerli olduğundan bu uyarıların "etkinleştirildi" yalnızca bir durum vardır. Bu uyarılar, yeni bir hizmet durumu olay gerçekleştiğinde size veya bir kullanıcı veya uygulama bir işlem aboneliğinizde gibi gerçekleştirdiğinde "sanal makineyi silin."

Azure İzleyici kullanılabilir olan tanılama günlük verilerini kullanıma almak için verileri Log Analytics'e (eski adıyla Operations Management Suite) yönlendirmek ve Log Analytics sorgu uyarısını kullanın. Analizi şimdi kullandığı oturum [yöntemi yeni uyarı](monitoring-overview-unified-alerts.md). 

Aşağıdaki diyagram, Azure İzleyici'de veri kaynaklarını özetler ve bu verilerin nasıl uyarı önerir.

![Uyarılar açıklaması](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Azure İzleyici'nin Taksonomisi uyarılar (Klasik)
Azure Klasik uyarılar ve işlevlerini açıklamak için aşağıdaki terimler kullanılmaktadır:
* **Uyarı**: sağlandığında etkinleştirilmiş olur ölçütleri (bir veya daha fazla kural ya da koşulları) tanımı.
* **Etkin**: oluşan durumu klasik bir uyarı tarafından tanımlanan ölçütler karşılanıyorsa zaman.
* **Çözümlenen**: gerçekleşen durumu, klasik bir uyarı tarafından tanımlanan ölçütleri artık karşılanıyorsa daha önce karşılanıp karşılanmadığını sonra.
* **Bildirim**: Klasik bir uyarı etkin olduğunda göre gerçekleştirilecek eylemi.
* **Eylem**: belirli çağrıda alıcı (örneğin, bir e-posta veya bir Web kancası URL'si için bir post) bir bildirim gönderilir. Bildirimler, genellikle birden fazla eylem tetikleyebilirsiniz.

## <a name="how-do-i-receive-notifications-from-an-azure-monitor-classic-alert"></a>Klasik Azure İzleyici uyarısı bildirimleri nasıl alabilirim?
Tarihsel olarak, farklı hizmetler Azure uyarılarından kendi yerleşik bildirim yöntemleri kullanılır. 

Azure İzleyici, yeniden kullanılabilir bildirim gruplandırma adlı sunar artık *Eylem grupları*. Eylem grupları, bir dizi için bir bildirim alıcıları belirtin. Eylem grubu başvuran bir uyarı etkinleştirildiğinde, tüm alıcılar bu bildirim alırsınız. Bu işlev, alıcılar (örneğin, nöbet mühendisi listenize) gruplandırmasını yeniden arasında çok sayıda uyarı nesneleri sağlar. Eylem grupları, çeşitli yöntemlerle bildirim destekler. Bu yöntemler, Web kancası URL'si, e-posta, SMS iletileri ve diğer eylemler bir dizi göndermek için posta içerebilir. Daha fazla bilgi için [oluştur ve Eylem grupları Azure portalında yönetmek](monitoring-action-groups.md). 

Eski Klasik etkinlik günlüğü uyarıları, Eylem grupları kullanın.

Ancak, eski ölçüm uyarıları Eylem grupları kullanmayın. Bunun yerine, aşağıdaki eylemleri de yapılandırabilirsiniz: 
* Hizmet Yöneticisi veya ortak Yöneticiler veya belirttiğiniz ek e-posta adreslerine e-posta bildirimleri gönderin.
* Bu sayede ek Otomasyon eylemleri başlatmak bir Web kancası çağırın.

Web kancaları otomasyon ve düzeltme, örneğin, aşağıdaki hizmetleri kullanarak etkinleştirin:
- Azure Otomasyonu Runbook
- Azure İşlevleri
- Azure mantıksal uygulaması
- Bir üçüncü taraf hizmeti

## <a name="next-steps"></a>Sonraki adımlar
Uyarı kuralları ve aşağıdaki belgeleri kullanarak yapılandırma hakkında bilgi alın:

* Daha fazla bilgi edinin [ölçümleri](monitoring-overview-metrics.md)
* Yapılandırma [Azure portalını kullanarak Klasik ölçüm uyarıları](insights-alerts-portal.md)
* Yapılandırma [PowerShell kullanarak Klasik ölçüm uyarıları](insights-alerts-powershell.md)
* Yapılandırma [Azure CLI kullanarak Klasik ölçüm uyarıları](insights-alerts-command-line-interface.md)
* Yapılandırma [Azure İzleyici REST API'yi kullanarak Klasik ölçüm uyarıları](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Daha fazla bilgi edinin [etkinlik günlükleri](monitoring-overview-activity-logs.md)
* Yapılandırma [Azure portalını kullanarak Etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md)
* Yapılandırma [Kaynak Yöneticisi'ni kullanarak Etkinlik günlüğü uyarıları](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](monitoring-activity-log-alerts-webhook.md)
* Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md)
* Yapılandırma [yeni uyarılar](monitor-alerts-unified-usage.md)
