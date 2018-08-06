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
ms.openlocfilehash: 0d88aef46761e8c7d8217f04befec88816587c03
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358301"
---
# <a name="azure-ad-activity-logs-in-azure-monitor-preview"></a>Azure İzleyici'deki Azure AD etkinlik günlükleri (önizleme)

Artık Azure İzleyici'yi kullanarak Azure Active Directory (Azure AD) etkinlik günlüklerini kendi depolama hesabınıza veya olay hub'ınıza yönlendirebilirsiniz. Azure İzleyici'deki Azure Active Directory günlükleri özelliğinin genel önizleme sürümü ile aşağıdakileri yapabilirsiniz:

* Azure depolama hesabınızın denetim günlüklerini arşivleyerek verileri uzun bir süre saklayabilirsiniz.
* Splunk ve QRadar gibi popüler Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarını kullanarak analiz etmek üzere denetim günlüklerinizin akışını bir Azure olay hub'ına yapabilirsiniz.
* Denetim günlüklerinizin akışını bir olay hub'ına yaparak kendi özel günlük çözümlerinizle tümleştirebilirsiniz.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Desteklenen raporlar

Bu özelliği kullanarak denetim etkinlik günlüklerinizi ve oturum açma işlemleri etkinlik günlüklerinizi Azure depolama hesabınıza, olay hub'ınıza veya özel çözümünüze yönlendirebilirsiniz. 

* **Denetim günlükleri**: [Denetim günlükleri etkinlik raporu](active-directory-reporting-activity-audit-logs.md), kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* **Oturum açma günlükleri**: [Oturum açma işlemleri etkinlik raporuyla](active-directory-reporting-activity-sign-ins.md), denetim günlüklerinde bildirilen görevleri kimlerin gerçekleştirdiğini saptayabilirsiniz.

> [!NOTE]
> B2C ile ilgili denetim ve oturum açma işlemleri etkinlik günlükleri şu an için desteklenmemektedir.
>

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure portaldan Azure AD günlüklerine erişmek için Azure AD Ücretsiz, Temel, Premium 1 veya Premium 2 [lisansı](https://azure.microsoft.com/pricing/details/active-directory/). 

Denetim günlüğü verilerinizi yönlendirmek istediğiniz yere bağlı olarak aşağıdakilerden birine ihtiyacınız olacaktır:

* *ListKeys* izinlerine sahip olduğunuz bir Azure depolama hesabı. Blob depolama hesabı değil genel bir depolama hesabı kullanmanızı öneririz. Depolamayla fiyatlandırma bilgileri için bkz. [Azure Depolama fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Üçüncü taraf çözümlerle tümleştirmek için Azure Event Hubs ad alanı.

> [!NOTE]
> Bir Azure AD kiracısındaki Azure AD etkinlik günlüklerine erişmek için *genel yönetici* veya *güvenlik yöneticisi* olmanız gerekir.
>

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


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu bölümde, Azure İzleyici'deki Azure AD günlükleriyle ilgili sık sorulan soruların yanıtları ve bilinen sorunlar yer almaktadır.

**S: Nereden başlamalıyım?** 

**Y**: Bu makalede bu özelliği dağıtmak için bilmeniz gerekenler açıklanmaktadır. Ön koşulları karşıladıktan sonra günlüklerinizi yapılandırmanıza ve bir olay hub'ına yönlendirmenize yardımcı olabilecek öğreticilere gidin.

---

**S: Bu özelliğe hangi günlükler dahildir?**

**Y**: Hem oturum açma etkinliği hem de denetim günlükleri bu özellik üzerinden yönlendirilebilir ancak B2C ile ilgili denetim olayları şu an için dahil değildir. Desteklenen günlük türlerini ve özellik tabanlı günlükleri öğrenmek için [Denetim günlüğü şemasını](reporting-azure-monitor-diagnostics-audit-log-schema.md) ve [Oturum açma günlüğü şemasını](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) inceleyin. 

---

**S: Eylem gerçekleştirildikten ne kadar süre sonra ilgili günlükler olay hub'ında gösterilir?**

**A**: Günlüklerin eylem gerçekleştirildikten sonra iki ila beş dakika içinde olay hub'ınızda gösterilmesi gerekir. Event Hubs hakkında daha fazla bilgi için bkz. [Azure Event Hubs nedir?](../event-hubs/event-hubs-about.md)

---

**S: Eylem gerçekleştirildikten ne kadar süre sonra ilgili günlükler depolama hesaplarında gösterilir?**

**Y**: Azure depolama hesapları için gecikme süresi eylemin gerçekleştirilmesinden itibaren 5 ile 15 dakika arasındadır.

---

**S: Verilerimin depolama maliyeti ne kadar olacaktır?**

**Y**: Depolama maliyeti, günlüklerinizin boyutuna ve seçtiğiniz saklama süresine göre değişir. Kiracılarınızda üretilen günlük hacmine dayalı maliyet tahmini için [Etkinlik günlükleri için depolama boyutu](https://review.docs.microsoft.com/en-us/azure/active-directory/reporting-azure-monitor-diagnostics-overview?branch=pr-en-us-47660#storage-size-for-activity-logs) bölümüne gidin.

---

**S: Verilerimin akışını olay hub'ına yapmanın maliyeti nedir?**

**Y**: Akış maliyeti, dakika başına aldığınız ileti sayısına göre değişir. Bu makalede maliyetlerin nasıl hesaplandığı gösterilmekte ve ileti sayısına dayalı maliyet tahminleri listelenmektedir. 

---

**S: Hangi SIEM araçları desteklenmektedir?** 

**Y**: Azure İzleyici şu anda Splunk, QRadar ve Sumo Logic tarafından desteklenmektedir. Ancak Splunk, Azure AD günlükleri için desteklenen tek SIEM aracıdır. Bağlayıcıların çalışma şekli hakkında daha fazla bilgi için bkz. [Azure izleme verilerini bir dış araç tarafından kullanılmak üzere bir olay hub'ına aktarma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

---

**S: Olay hub'ı verilerine harici bir SIEM aracı kullanmadan erişebilir miyim?** 

**Y**: Evet. Günlüklere özel uygulamanızdan erişmek için [Event Hubs API](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)’sini kullanabilirsiniz. 

---


## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüklerini depolama hesabında arşivleme](reporting-azure-monitor-diagnostics-azure-storage-account.md)
* [Etkinlik günlüklerini olay hub'ına yönlendirme](reporting-azure-monitor-diagnostics-azure-event-hub.md)
* [Azure Tanılama Günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
