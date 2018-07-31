---
title: Azure İzleyici'deki Azure Active Directory etkinlik günlükleri (önizleme) | Microsoft Docs
description: Azure İzleyici'deki Azure Active Directory etkinlik günlüklerine genel bakış (önizleme)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9b594360bc760fa9eec8ab9900c3aadcb10e9db6
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240125"
---
# <a name="azure-active-directory-activity-logs-in-azure-monitor-preview"></a>Azure İzleyici'deki Azure Active Directory etkinlik günlükleri (önizleme)

Artık Azure İzleyici'yi kullanarak Azure AD etkinlik günlüklerini kendi depolama hesabınıza veya olay hub'ınıza yönlendirebilirsiniz. Azure İzleyici'deki Azure Active Directory günlükleri özelliğinin genel önizleme sürümü ile aşağıdakileri yapabilirsiniz:

* Azure depolama hesabınızın denetim günlüklerini arşivleyerek verileri uzun bir süre saklayabilirsiniz
* Denetim günlüklerinizi bir Azure olay hub'ına aktararak Splunk ve QRadar gibi popüler SIEM araçlarıyla analiz gerçekleştirebilirsiniz
* Denetim günlüklerinizin akışını bir olay hub'ına yaparak kendi özel günlük çözümlerinizle tümleştirebilirsiniz

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Desteklenen raporlar

Bu özelliği kullanarak denetim etkinlik günlüklerinizi ve oturum açma işlemleri etkinlik günlüklerinizi Azure depolama hesabınıza, olay hub'ınıza veya özel çözümünüze yönlendirebilirsiniz. 

* **Denetim günlükleri**: [Denetim günlükleri etkinlik raporu](active-directory-reporting-activity-audit-logs.md), kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* **Oturum açma işlemleri**: [Oturum açma işlemleri etkinlik raporuyla](active-directory-reporting-activity-sign-ins.md), denetim günlükleri raporunda bildirilen görevleri kimlerin gerçekleştirdiğini saptayabilirsiniz.

> [!NOTE]
> B2C ile ilgili denetim ve oturum açma işlemleri etkinlik günlükleri şu an için desteklenmemektedir.
>

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure portaldan Azure AD günlüklerine erişmek için Azure AD Ücretsiz, Temel, Premium 1 veya Premium 2 [lisansı](https://azure.microsoft.com/pricing/details/active-directory/). 

Denetim günlüğü verilerinizi yönlendirmek istediğiniz yere bağlı olarak aşağıdakilerden birine ihtiyacınız olacaktır:

* *ListKeys* izinlerine sahip olduğunuz bir Azure depolama hesabı. Blob depolama hesabı değil genel bir depolama hesabı kullanmanızı öneririz. Depolamayla ilgili fiyatlandırma bilgileri için bkz. [Azure Depolama fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Üçüncü taraf çözümlerle tümleştirmek için Azure Event Hubs ad alanı.

> [!NOTE]
> Bir Azure AD kiracısındaki Azure AD etkinlik günlüklerine erişmek için *Genel Yönetici* veya *Güvenlik Yöneticisi* olmanız gerekir.
>

## <a name="cost-considerations"></a>Maliyetle ilgili konular

Azure AD lisansınız varsa, depolama hesabı ve olay hub'ı kurulumu için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliği ücretsizdir, ancak arşivleme için kullandığınız depolama hesabı ve akış için kullandığınız olay hub'ı gibi Azure kaynaklarını kullandığınızda ücret ödemeniz gerekir. Veri miktarı ve dolayısıyla alınacak ücret, kiracı boyutuna göre değişiklik gösterecektir. 

### <a name="storage-size-for-activity-logs"></a>Etkinlik günlükleri için depolama boyutu

Her denetim günlüğü olayı yaklaşık 2 KB veri depolama alanı kullanır. Bu nedenle 100.000 kullanıcıdan oluşan ve her gün yaklaşık 1,5 milyon olay gerçekleşecek bir kiracıda günlük yaklaşık 3 GB veri depolama alanına ihtiyaç duyulur. Yazma işlemleri yaklaşık 5 dakikalık toplu işlemler halinde gerçekleştiğinden, ayda yaklaşık 9000 yazma işlemi olmasını bekleyebilirsiniz. Aşağıdaki tabloda, Batı ABD bölgesindeki bir genel amaçlı sürüm 2 depolama hesabında en az bir yıl saklama için kiracının boyutuna bağlı olarak yaklaşık bir maliyet hesabı yapılmıştır. [Azure depolama fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/storage/blobs/) kullanarak uygulamanızın veri hacmine göre daha doğru bir yaklaşık değer elde edebilirsiniz. Her denetim günlüğü olayı yaklaşık 2 KB veri depolama alanı kullanır. Bu nedenle 100.000 kullanıcıdan oluşan ve her gün yaklaşık 1,5 milyon olay gerçekleşecek bir kiracıda günlük yaklaşık 3 GB veri depolama alanına ihtiyaç duyulur. Yazma işlemleri yaklaşık 5 dakikalık toplu işlemler halinde gerçekleştiğinden, ayda yaklaşık 9000 yazma işlemi olmasını bekleyebilirsiniz. Aşağıdaki tabloda, Batı ABD bölgesindeki bir genel amaçlı sürüm 2 depolama hesabında en az bir yıl saklama için kiracının boyutuna bağlı olarak yaklaşık bir maliyet hesabı yapılmıştır. [Azure depolama fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/storage/blobs/) kullanarak uygulamanızın veri hacmine göre daha doğru bir yaklaşık değer elde edebilirsiniz. 

| Günlük kategorisi | Kullanıcı sayısı | Günlük olay sayısı | Yaklaşık aylık veri hacmi | Yaklaşık aylık maliyet | Yaklaşık yıllık maliyet |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Denetim | 100.000 | 1,5 milyon | 90 GB | $1,93 | $23,12 |
| Denetim | 1000 | 15000 | 900 MB | $0,02 | $0,24 |
| Oturum açma işlemleri | 1000 | 34800 | 4 GB | $0,13 | $1,56 |
| Oturum açma işlemleri | 100.000 | 15 milyon | 1,7 TB | $35,41 | $424,92 | 


### <a name="event-hub-messages-for-activity-logs"></a>Etkinlik günlükleri için olay hub'ı iletileri

Olaylar yaklaşık beş dakikalık aralıklarla toplanır ve bu zaman aralığındaki tüm olayları içeren tek bir ileti halinde gönderilir. Olay hub'ındaki bir ileti en fazla 256 kB boyutunda olur ve ilgili zaman aralığındaki tüm iletilerin boyutunun bu düzeyi aşması halinde birden fazla ileti gönderilir. 

Örneğin 100.000'in üzerinde kullanıcıya sahip olan büyük ölçekli bir kiracıda saniyede yaklaşık 18 olay olur ve bu da 5 dakikada bir 5400 olay yapar. Denetim günlüklerindeki her olay 2 KB boyutunda olduğundan 10,8 MB veri elde edilir ve bunun sonucunda olay hub'ına 5 dakikalık bölüm için 43 ileti gönderilir. Aşağıdaki tabloda Batı ABD bölgesinde yer alan temel bir olay hub'ı için olay verilerine göre yaklaşık maliyet hesabı gösterilmiştir. [Olay hub'ı fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/details/event-hubs/) kullanarak uygulamanızın veri hacmine göre daha doğru bir yaklaşık değer hesaplayabilirsiniz.

| Günlük kategorisi | Kullanıcı sayısı | Saniyedeki olay sayısı | 5 dakikalık zaman aralığındaki olay sayısı | Aralık başına boyut | Aralık başına ileti sayısı | Ay başına ileti sayısı | Yaklaşık aylık maliyet |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Denetim | 100.000 | 18 | 5400 | 10,8 MB | 43 | 371.520 | $10,83 |
| Denetim | 1000 | 0.1 | 52 | 104 KB | 1 | 8640 | $10,8 |
| Oturum açma işlemleri | 1000 | 178 | 53400 | 106,8 MB | 418 | 3.611.520 | $11,06 |  


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu bölümde, Azure İzleyici'deki Azure Active Directory günlükleriyle ilgili sık sorulan sorular ve bilinen sorunlar yer almaktadır.

**S: Nereden başlamalıyım?** 

**Y:** Bu özelliği dağıtmak için ihtiyacınız olanlarla ilgili bir fikir edinmek için [Genel bakış](reporting-azure-monitor-diagnostics-overview.md) bölümünden başlayın. Ön koşullar hakkında bilgi edindikten sonra günlüklerinizi yapılandırmanıza ve Event Hubs'a yönlendirmenize yardımcı olacak öğreticileri inceleyin.

---

**S: Bu özelliğe hangi günlükler dahildir?**

**Y:** Hem oturum açma hem de denetim günlükleri bu özellik üzerinden yönlendirilebilir ancak B2C ile ilgili denetim olayları şu an için dahil değildir. Desteklenen günlük türlerini ve özellik tabanlı günlükleri öğrenmek için [Denetim günlüğü şemasını](reporting-azure-monitor-diagnostics-audit-log-schema.md) ve [Oturum açma günlüğü şemasını](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) okuyun. 

---

**S: Eylem gerçekleştirildikten ne kadar süre sonra ilgili günlükler Event Hubs'da gösterilir?**

**Y:** Günlüklerin eylem gerçekleştirildikten sonra iki ila beş dakika içinde Event Hubs'da gösterilmesi gerekir. Event Hubs hakkında daha fazla bilgi için bkz. [Event Hubs nedir?](/azure/event-hubs/event-hubs-what-is-event-hubs.md)

---

**S: Eylem gerçekleştirildikten ne kadar süre sonra ilgili günlükler depolama hesaplarında gösterilir?**

**Y:** Azure depolama hesapları için gecikme süresi eylemin gerçekleştirilmesinden itibaren 5 ile 15 dakika arasındadır.

---

**S: Verilerimin depolama maliyeti ne kadar olacaktır?**

**Y:** Depolama maliyeti, günlüklerinizin boyutuna ve seçtiğiniz saklama süresine göre değişir. Üretilen günlük hacmine göre yaklaşık kiracı maliyetleri hakkında bilgi edinmek için [Genel bakış](reporting-azure-monitor-diagnostics-overview.md) bölümünü inceleyin.

---

**S: Verilerimin akışını Event Hubs'a yapmanın maliyeti nedir?**

**Y:** Akış maliyeti, dakika başına aldığınız ileti sayısına göre değişir. Maliyetin nasıl hesaplandığı hakkında bilgi almak ve ileti sayısına göre yaklaşık bir değer elde etmek için [Genel bakış](reporting-azure-monitor-diagnostics-overview.md) bölümünü inceleyin. 

---

**S: Hangi SIEM araçları desteklenmektedir?** 

**Y:** Azure İzleyici şu anda Splunk, QRadar ve Sumologic tarafından desteklenmektedir. Ancak Splunk, Azure Active Directory günlükleri için desteklenen tek SIEM aracıdır. Bağlayıcıların çalışma şekli hakkında daha fazla bilgi için bkz. [Azure izleme verilerini bir dış araç tarafından kullanılmak üzere bir olay hub'ına aktarma](/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs).

---

**S: Event Hubs verilerine harici bir SIEM aracı kullanmadan erişebilir miyim?** 

**Y:** Evet, [Event Hub API](/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph)'sini kullanarak günlüklere kendi özel uygulamanızdan erişebilirsiniz. 

---


## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüklerini depolama hesabında arşivleme](reporting-azure-monitor-diagnostics-azure-storage-account.md)
* [Etkinlik günlüklerini Event Hubs'a yönlendirme](reporting-azure-monitor-diagnostics-azure-event-hub.md)
* [Azure Tanılama Günlükleri](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)