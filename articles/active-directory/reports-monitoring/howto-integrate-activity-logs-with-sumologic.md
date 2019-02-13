---
title: Stream SumoLogic Azure İzleyicisi'ni (Önizleme) kullanarak Azure Active Directory günlükleri | Microsoft Docs
description: Azure Active Directory günlükleri Azure İzleyicisi'ni (Önizleme) kullanarak SumoLogic ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 085b23fe43db4936a0fd459872ec841f2b45e221
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194283"
---
# <a name="integrate-azure-active-directory-logs-with-sumologic-using-azure-monitor-preview"></a>Azure İzleyicisi'ni (Önizleme) kullanarak SumoLogic ile Azure Active Directory günlükleri tümleştirme

Bu makalede, Azure Active Directory (Azure AD) günlüklerini Azure İzleyicisi'ni kullanarak SumoLogic ile tümleştirme öğrenin. Ardından olay hub'ı SumoLogic ile tümleştirin ve günlükleri Azure olay hub'ına ilk yönlendirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:
* Azure AD etkinlik içeren bir Azure olay hub'ına kaydeder. Bilgi edinmek için nasıl [, etkinlik günlükleri Olay hub'ına akış](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Bir SumoLogic çoklu oturum açma abonelik etkin.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Azure AD günlükleri SumoLogic ile tümleştirmek için adımları 

1. İlk olarak, [Azure olay hub'ına Azure AD'ye günlüklerin akışını](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. SumoLogic Örneğinize yapılandırma [günlükleri toplamak için Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Azure AD SumoLogic uygulama yükleme](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) ortamınızın gerçek zamanlı analiz sağlayan önceden yapılandırılmış panoları kullanmak için.

 ![Pano](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure İzleyici oturum açma günlükleri şemasını yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
