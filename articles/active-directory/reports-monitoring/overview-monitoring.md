---
title: Azure Active Directory izleme nedir? (önizleme) | Microsoft Docs
description: Azure Active Directory izlemeye genel bir bakış sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: e2b3d8ce-708a-46e4-b474-123792f35526
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: de416d18505d0258da446318b3dc6a9853ff13e7
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58434866"
---
# <a name="what-is-azure-active-directory-monitoring-preview"></a>Azure Active Directory izleme nedir? (önizleme)

Azure Active Directory (Azure AD) izleme özellikleriyle artık Azure AD etkinlik günlüklerinizi farklı uç noktalara yönlendirebilirsiniz. Ardından bu günlükleri uzun vadeli kullanım için saklayabilir veya ortamınızla ilgili içgörülere ulaşmak için üçüncü taraf Güvenlik Bilgileri ve Olay Yönetimi (SIEM) araçlarıyla tümleştirebilirsiniz.

Günlükleri şu an için şu hedeflere yönlendirebilirsiniz:

- Bir Azure depolama hesabı.
- Splunk ve Sumologic örneklerinizle tümleştirmek üzere bir Azure olay hub'ı.
- Verileri analiz edip belirli olaylar için pano ve uyarı oluşturabileceğiniz Azure Log Analytics çalışma alanı

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="diagnostic-settings-configuration"></a>Tanılama ayarlarını yapılandırma

Azure AD etkinlik günlükleri izleme ayarlarını yapılandırmak için [Azure portalda](https://portal.azure.com) oturum açın ve **Azure Active Directory**'yi seçin. Buradan tanılama ayarları yapılandırma sayfasına erişmek için kullanabileceğiniz iki yöntem vardır:

* **İzleme** bölümünden **Tanılama ayarları**'nı seçin.

    ![Tanılama ayarları](./media/overview-monitoring/diagnostic-settings.png)
    
* **Denetim Günlükleri** veya **Oturum açma işlemleri**'ni ve ardından **Dışarı aktarma ayarları**'nı seçin. 

    ![Dışarı aktarma ayarları](./media/overview-monitoring/export-settings.png)


## <a name="route-logs-to-storage-account"></a>Günlükleri depolama hesabına yönlendirme

Günlükleri bir Azure depolama hesabına yönlendirerek [saklama ilkelerimizde](reference-reports-data-retention.md) belirtilen varsayılan saklama süresinden daha uzun bir süre boyunca saklayabilirsiniz. [Verileri depolama hesabınıza yönlendirmeyi](quickstart-azure-monitor-route-logs-to-storage-account.md) öğrenin.

## <a name="stream-logs-to-event-hub"></a>Günlükleri olay hub’ına aktarma

Günlükleri bir Azure olay hub'ına aktarmak, Sumologic ve Splunk gibi üçüncü taraf SIEM araçlarına tümleştirmenizi sağlar. Bu tümleştirme, Azure AD etkinlik günlüğü verileri ortamınızı daha zengin Öngörüler sağlamak için SIEM tarafından yönetilen diğer verilerle birleştirmek sağlar. [Olay hub'ına günlük akışı yapmayı](tutorial-azure-monitor-stream-logs-to-event-hub.md) öğrenin.

## <a name="send-logs-to-azure-monitor-logs"></a>Azure İzleyici günlüklerine günlükleri gönderme

[Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) izleme farklı kaynaklardan verileri bir araya getirir ve uygulamalarınızın ve kaynaklarınızın çalışmasını öngörülerin bir sorgu dili ve analiz altyapısı sağlayan bir çözümdür. Göndererek Azure AD etkinlik günlüklerini Azure İzleyici günlüklerine hızlı bir şekilde alabilirsiniz, izleme ve uyarılar toplanan verileri. Bilgi edinmek için nasıl [veri göndermek için Azure İzleyici günlüklerine](howto-integrate-activity-logs-with-log-analytics.md).

Oturum açma ve denetim etkinlikleri gibi yaygın senaryoları izlemek için önceden oluşturulmuş Azure AD etkinlik günlüklerini de görüntüleyebilirsiniz. Bilgi nasıl [yüklemek ve log analytics görünümleri kullanmak için Azure AD etkinlik günlüklerini](howto-install-use-log-analytics-views.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici'deki etkinlik günlükleri](concept-activity-logs-azure-monitor.md)
* [Günlükleri olay hub’ına aktarma](tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Azure İzleyici günlüklerine günlükleri gönderme](howto-integrate-activity-logs-with-log-analytics.md)
