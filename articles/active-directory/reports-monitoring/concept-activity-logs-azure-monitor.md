---
title: Azure Active Directory etkinlik günlükleri Azure İzleyici | Microsoft Docs
description: Azure İzleyici'de giriş Azure Active Directory etkinlik günlükleri
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/22/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b924746c00a438ec4ac81dacc02905565adf30e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64682123"
---
# <a name="azure-ad-activity-logs-in-azure-monitor"></a>Azure İzleyici'de Azure AD etkinlik günlükleri

Azure Active Directory (Azure AD) etkinlik günlükleri, uzun vadeli bekletme ve veri öngörüleri için birkaç uç yönlendirebilirsiniz. Bu özellik sağlar:

* Arşiv Azure AD etkinlik günlüklerini verileri uzun süre korumak için Azure depolama hesabınız için.
* Azure olay hub'ına Splunk ve QRadar gibi popüler güvenlik bilgileri ve Olay yönetimi (SIEM) araçlarını kullanarak analiz için Stream Azure AD etkinlik günlükleri.
* Azure AD tümleştirme bunları bir olay hub'ına akış tarafından kendi özel günlük çözümleri ile etkinlik günlükleri.
* İzleme ve uyarı bağlı veriler üzerinde zengin görselleştirmeler etkinleştirmek için Azure İzleyici günlüklerine gönderme Azure AD etkinlik günlükleri.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="supported-reports"></a>Desteklenen raporlar

Azure yönlendirebilirsiniz AD denetim günlüklerini ve oturum açma günlüklerinin Azure depolama hesabı, olay hub'ı, Azure İzleyici günlüklerine veya özel bir çözüm için bu özelliği kullanarak. 

* **Denetim günlükleri**: [Denetim günlükleri Etkinlik Raporu](concept-audit-logs.md) kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* **Oturum açma günlükleri**: İle [oturum açma etkinliği raporunu](concept-sign-ins.md), Denetim günlüklerinde bildirilen görevleri gerçekleştiren belirleyebilirsiniz.

> [!NOTE]
> B2C ile ilgili denetim ve oturum açma işlemleri etkinlik günlükleri şu an için desteklenmemektedir.
>

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure portaldan Azure AD denetim günlüklerine erişmek için Azure AD Ücretsiz, Temel, Premium 1 veya Premium 2 [lisansı](https://azure.microsoft.com/pricing/details/active-directory/). 
* Azure AD kiracısı.
* Azure AD kiracısında **genel yönetici** veya **güvenlik yöneticisi** olan bir kullanıcı.
* Azure portaldan Azure AD oturum açma günlüklerine erişmek için Azure AD Premium 1 veya Premium 2 [lisansı](https://azure.microsoft.com/pricing/details/active-directory/). 

Denetim günlüğü verilerinizi yönlendirmek istediğiniz yere bağlı olarak aşağıdakilerden birine ihtiyacınız olacaktır:

* *ListKeys* izinlerine sahip olduğunuz bir Azure depolama hesabı. Blob depolama hesabı değil genel bir depolama hesabı kullanmanızı öneririz. Depolamayla fiyatlandırma bilgileri için bkz. [Azure Depolama fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Üçüncü taraf çözümlerle tümleştirmek için Azure Event Hubs ad alanı.
* Azure İzleyici günlüklerine günlükleri göndermek için bir Azure Log Analytics çalışma alanı.

## <a name="cost-considerations"></a>Maliyetle ilgili konular

Azure AD lisansınız varsa, depolama hesabı ve olay hub'ı kurulumu için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliği ücretsizdir, ancak arşivleme için kullandığınız depolama hesabı ve akış için kullandığınız olay hub'ı gibi Azure kaynaklarını kullandığınızda ücret ödemeniz gerekir. Veri miktarı ve dolayısıyla alınacak ücret, kiracı boyutuna göre değişiklik gösterebilir. 

### <a name="storage-size-for-activity-logs"></a>Etkinlik günlükleri için depolama boyutu

Her denetim günlüğü olayı yaklaşık 2 KB veri depolama alanı kullanır. 100.000 kullanıcıdan oluşan ve her gün yaklaşık 1,5 milyon olay gerçekleşecek bir kiracıda günlük yaklaşık 3 GB veri depolama alanına ihtiyaç duyulur. Yazma işlemleri yaklaşık beş dakikalık toplu işlemler halinde gerçekleştiğinden, ayda yaklaşık 9000 yazma işlemi olmasını bekleyebilirsiniz. 


Aşağıdaki tabloda, Batı ABD bölgesindeki bir genel amaçlı sürüm 2 depolama hesabında en az bir yıl saklama için kiracının boyutuna bağlı olarak yaklaşık bir maliyet hesabı verilmiştir. Uygulamanızın veri hacmine göre daha doğru bir yaklaşık değer elde etmek için [Azure depolama fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/storage/blobs/) kullanın.


| Günlük kategorisi | Kullanıcı sayısı | Günlük olay sayısı | Aylık veri hacmi (tahmini) | Aylık maliyet (tahmini) | Yıllık maliyet (tahmini) |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Denetim | 100.000 | 1,5&nbsp;milyon | 90 GB | $1,93 | $23,12 |
| Denetim | 1000 | 15.000 | 900 MB | $0,02 | $0,24 |
| Oturum açma işlemleri | 1000 | 34.800 | 4 GB | $0,13 | $1,56 |
| Oturum açma işlemleri | 100.000 | 15&nbsp;milyon | 1,7 TB | $35,41 | $424,92 |
 









### <a name="event-hub-messages-for-activity-logs"></a>Etkinlik günlükleri için olay hub'ı iletileri

Olaylar yaklaşık beş dakikalık aralıklarla toplanır ve bu zaman aralığındaki tüm olayları içeren tek bir ileti halinde gönderilir. Olay hub'ındaki bir ileti en fazla 256 KB boyutunda olur ve ilgili zaman aralığındaki tüm iletilerin toplam boyutunun bu düzeyi aşması halinde birden fazla ileti gönderilir. 

Örneğin 100.000’den fazla kullanıcısı olan büyük bir kiracıda saniyede yaklaşık 18 olay oluşur ve bu da beş dakikada bir 5400 olay yapar. Denetim günlükleri olay başına 2 KB boyutunda olduğundan toplam veri boyutu 10,8 MB olur. Bu nedenle bu beş dakikalık süre için 43 ileti gönderilir. 

Aşağıdaki tabloda Batı ABD bölgesinde yer alan temel bir olay hub'ı için olay verileri hacmine göre yaklaşık aylık maliyet hesabı gösterilmiştir. Uygulamanızın veri hacmine göre daha doğru bir yaklaşık değer hesaplamak için [Olay hub'ı fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/event-hubs/) kullanın.

| Günlük kategorisi | Kullanıcı sayısı | Saniye başına olay sayısı | Beş dakikalık aralık başına olay sayısı | Aralık başına boyut | Aralık başına ileti sayısı | Aylık ileti sayısı | Aylık maliyet (tahmini) |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Denetim | 100.000 | 18 | 5400 | 10,8 MB | 43 | 371.520 | $10,83 |
| Denetim | 1000 | 0.1 | 52 | 104 KB | 1 | 8640 | $10,80 |
| Oturum açma işlemleri | 1000 | 178 | 53.400 | 106,8&nbsp;MB | 418 | 3.611.520 | $11,06 |  

### <a name="azure-monitor-logs-cost-considerations"></a>Azure İzleyici maliyet konuları günlüğe kaydeder.

Azure İzleyici günlüklerine yönetmeyle ilgili maliyetleri gözden geçirmek için bkz: [veri hacmini ve saklamayı Azure İzleyici günlüklerine kontrol ederek maliyet yönetme](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-cost-storage).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu bölümde, Azure İzleyici'deki Azure AD günlükleriyle ilgili sık sorulan soruların yanıtları ve bilinen sorunlar yer almaktadır.

**S: Hangi günlüklerin dahildir?**

**A**: B2C ile ilgili denetim olaylarını şu anda dahil edilmez ancak her ikisi de bu özelliği aracılığıyla yönlendirmek için kullanılabilir olan denetim günlüklerini ve oturum açma etkinlik günlükleri. Desteklenen günlük türlerini ve özellik tabanlı günlükleri öğrenmek için [Denetim günlüğü şemasını](reference-azure-monitor-audit-log-schema.md) ve [Oturum açma günlüğü şemasını](reference-azure-monitor-sign-ins-log-schema.md) inceleyin. 

-----

**S: Ne kadar süre sonra eylemi sonra karşılık gelen günlükler my olay hub'ında görünür?**

**A**: Günlükleri Olay hub'ında eylem gerçekleştirildikten sonra iki ila beş dakika içinde gösterilmesi gerekir. Event Hubs hakkında daha fazla bilgi için bkz. [Azure Event Hubs nedir?](../../event-hubs/event-hubs-about.md)

-----

**S: Depolama Hesabımı nasıl yakında bir eylem sonra karşılık gelen günlükler gösterilir?**

**A**: Azure depolama hesapları için gecikme süresi, 5'ten herhangi bir eylem gerçekleştirildikten sonra 15 dakika ile olan.

-----

**S: Tanılama ayarını saklama süresi yöneticinin değişmesi durumunda ne olur?**

**A**: Yeni bir bekletme ilkesi değişiklikten sonra toplanan günlükleri uygulanır. İlke değişikliği etkilemeden önce toplanan günlükleri.

-----

**S: Ne kadar verilerimi depolama maliyeti?**

**A**: Depolama maliyetleri günlüklerinizi ve seçtiğiniz bekletme süresi her iki boyutuna bağlıdır. Kiracılarınızda üretilen günlük hacmine dayalı maliyet tahmini için bkz. [Etkinlik günlükleri için depolama boyutu](#storage-size-for-activity-logs).

-----

**S: Ne kadar verilerimi bir olay hub'ına akış maliyeti?**

**A**: Akış maliyetlerini dakika başına aldığınız ileti sayısını bağlıdır. Bu makalede maliyetlerin nasıl hesaplandığı gösterilmekte ve ileti sayısına dayalı maliyet tahminleri listelenmektedir. 

-----

**S: Nasıl Azure AD'ye tümleştirebilirim etkinlik günlüklerini SIEM sistemimle tümleştiremiyorum?**

**A**: Bunu iki şekilde yapabilirsiniz:

- Azure İzleyici ile Event Hubs'ı birlikte kullanarak günlüklerinizin akışını SIEM sisteminize yapabilirsiniz. İlk olarak [günlüklerin akışını olay hub'ına yapın](tutorial-azure-monitor-stream-logs-to-event-hub.md) ve ardından yapılandırılan olay hub'ını kullanarak [SIEM aracınızı ayarlayın](tutorial-azure-monitor-stream-logs-to-event-hub.md#access-data-from-your-event-hub). 

- [Raporlama Graph API’sini](concept-reporting-api.md) kullanarak verilere erişebilir ve kendi betiklerinizi kullanarak verileri SIEM sistemine gönderebilirsiniz.

-----

**S: Hangi SIEM araçları şu anda destekleniyor mu?** 

**A**: Azure İzleyici tarafından şu anda desteklenen [Splunk](tutorial-integrate-activity-logs-with-splunk.md), QRadar, ve [Sumo mantıksal](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory). Bağlayıcıların çalışma şekli hakkında daha fazla bilgi için bkz. [Azure izleme verilerini bir dış araç tarafından kullanılmak üzere bir olay hub'ına aktarma](../../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

-----

**S: Nasıl Azure AD'ye tümleştirebilirim Splunk Örneğim ile etkinlik günlükleri?**

**A**: İlk olarak, [rota Azure AD etkinlik günlüklerini Olay hub'ına](quickstart-azure-monitor-stream-logs-to-event-hub.md), ardından adımları [etkinlik günlükleri ile Splunk tümleştirme](tutorial-integrate-activity-logs-with-splunk.md).

-----

**S: Nasıl Azure AD'ye tümleştirebilirim Sumo mantığı ile etkinlik günlükleri?** 

**A**: İlk olarak, [rota Azure AD etkinlik günlüklerini Olay hub'ına](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory), ardından adımları [Azure AD uygulamasını yükleyin ve SumoLogic panoları görüntülemesine](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards).

-----

**S: Veriler bir olay hub'ından harici bir SIEM aracı kullanmadan erişebilirim?** 

**A**: Evet. Günlüklere özel uygulamanızdan erişmek için [Event Hubs API](../../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)’sini kullanabilirsiniz. 

-----


## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüklerini depolama hesabında arşivleme](quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Etkinlik günlüklerini olay hub'ına yönlendirme](quickstart-azure-monitor-stream-logs-to-event-hub.md)
* [Etkinlik günlükleri, Azure İzleyici ile tümleştirme](howto-integrate-activity-logs-with-log-analytics.md)