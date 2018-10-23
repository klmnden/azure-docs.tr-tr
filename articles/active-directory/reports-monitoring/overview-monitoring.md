---
title: Azure Active Directory izleme nedir? (önizleme) | Microsoft Docs
description: Azure Active Directory izlemeye genel bir bakış sağlar.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 09/24/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 82cd29cf1a635d1cd613d289a5d8db6ef54ee661
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49398759"
---
# <a name="what-is-azure-active-directory-monitoring-preview"></a>Azure Active Directory izleme nedir? (önizleme)

Azure Active Directory (Azure AD) izleme özellikleriyle artık Azure AD etkinlik günlüklerinizi farklı uç noktalara yönlendirebilirsiniz. Ardından bu günlükleri uzun vadeli kullanım için saklayabilir veya ortamınızla ilgili içgörülere ulaşmak için üçüncü taraf Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarıyla tümleştirebilirsiniz.

Günlükleri şu an için şu hedeflere yönlendirebilirsiniz:

- Bir Azure depolama hesabı.
- Splunk ve Sumologic örneklerinizle tümleştirmek üzere bir Azure olay hub'ı.
- Verileri analiz edip belirli olaylar için pano ve uyarı oluşturabileceğiniz Azure Log Analytics çalışma alanı


## <a name="diagnostic-settings-configuration"></a>Tanılama ayarlarını yapılandırma

Azure AD etkinlik günlükleri izleme ayarlarını yapılandırmak için [Azure portalda](https://portal.azure.com) oturum açın ve **Azure Active Directory**'yi seçin. Buradan tanılama ayarları yapılandırma sayfasına erişmek için kullanabileceğiniz iki yöntem vardır:

* **İzleme** bölümünden **Tanılama ayarları**'nı seçin.

    ![Tanılama ayarları](./media/overview-monitoring/diagnostic-settings.png)
    
* **Denetim Günlükleri** veya **Oturum açma işlemleri**'ni ve ardından **Dışarı aktarma ayarları**'nı seçin. 

    ![Dışarı aktarma ayarları](./media/overview-monitoring/export-settings.png)


## <a name="route-logs-to-storage-account"></a>Günlükleri depolama hesabına yönlendirme

Günlükleri bir Azure depolama hesabına yönlendirerek [saklama ilkelerimizde](reference-reports-data-retention.md) belirtilen varsayılan saklama süresinden daha uzun bir süre boyunca saklayabilirsiniz. [Verileri depolama hesabınıza yönlendirmeyi](quickstart-azure-monitor-route-logs-to-storage-account.md) öğrenin.

## <a name="stream-logs-to-event-hub"></a>Günlükleri olay hub’ına aktarma

Günlükleri bir Azure olay hub'ına aktarmak, Sumologic ve Splunk gibi üçüncü taraf SIEM araçlarına tümleştirmenizi sağlar. Bu tümleştirme, Azure AD etkinlik günlüğü verilerinizi SIEM tarafından yönetilen diğer verilerle birleştirerek ortamınızla ilgili daha gelişmiş içgörüler elde etmenizi sağlar. [Olay hub'ına günlük akışı yapmayı](tutorial-azure-monitor-stream-logs-to-event-hub.md) öğrenin.

## <a name="send-logs-to-log-analytics"></a>Günlükleri Log Analytics’e gönderme

[Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), farklı kaynaklardan gelen izleme verilerini birleştirerek uygulamalarınızın ve kaynaklarınızın çalışmasıyla ilgili içgörüler sunan bir sorgu dili ve analiz altyapısı sağlar. Azure AD etkinlik günlüklerini Log Analytics'e göndererek toplanan verileri alma, izleme ve uyarı oluşturma süreçlerini hızlandırabilirsiniz. [Log Analytics'e veri göndermeyi](howto-integrate-activity-logs-with-log-analytics.md) öğrenin.

Oturum açma ve denetim etkinlikleri gibi yaygın senaryoları izlemek için önceden oluşturulmuş Azure AD etkinlik günlüklerini de görüntüleyebilirsiniz. [Azure AD etkinlik günlükleri için Log Analytics görünümlerini yüklemeyi ve kullanmayı](howto-install-use-log-analytics-views.md) öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici'deki etkinlik günlükleri](concept-activity-logs-azure-monitor.md)
* [Günlükleri olay hub’ına aktarma](tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Günlükleri Log Analytics’e gönderme](howto-integrate-activity-logs-with-log-analytics.md)