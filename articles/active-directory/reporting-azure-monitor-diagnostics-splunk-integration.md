---
title: Azure Active Directory günlükleri Azure İzleyicisi'ni (Önizleme) kullanarak Splunk ile tümleştirme | Microsoft Docs
description: Azure Active Directory günlükleri Azure İzleyicisi'ni (Önizleme) kullanarak Splunk ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d1dd62d06c7e3ed634795604ce9660694ea073ca
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239989"
---
# <a name="integrate-azure-active-directory-logs-with-splunk-using-azure-monitor-preview"></a>Azure İzleyicisi'ni (Önizleme) kullanarak Splunk ile Azure Active Directory günlükleri tümleştirme

Bu makalede, Azure Active Directory günlükleri Azure İzleyicisi'ni kullanarak Splunk ile tümleştirme öğreneceksiniz. İlk olarak, günlükleri, Azure olay hub'ına yönlendirmek ve ardından Splunk ile bunu tümleştirmeniz gerekir.

## <a name="prerequisites"></a>Önkoşullar

1. Azure olay Hub'nı içeren Azure AD etkinlik günlükleri. Bilgi edinmek için nasıl [, etkinlik günlükleri Olay hub'ına akış](reporting-azure-monitor-diagnostics-azure-event-hub.md). 
2. Aşağıdaki [yönergeleri](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md) Splunk için Azure İzleyici eklenti indirip Splunk örneğinizin yapılandırın.

## <a name="tutorial"></a>Öğretici 

1. Splunk örneğinizi açın ve tıklayın **veri özeti**.
    ![Veri Özet](./media/reporting-azure-monitor-diagnostics-splunk-integration/DataSummary.png "veri özeti")

2. Gidin **Sourcetypes** sekmenize **amal: aadal:audit** ![Sourcetypes sekmesini](./media/reporting-azure-monitor-diagnostics-splunk-integration/sourcetypeaadal.png "Sourcetypes sekmesi")

3. Gördüğünüz Azure AD etkinlik günlükleri aşağıdaki şekilde gösterildiği gibi.
    ![Etkinlik günlükleri](./media/reporting-azure-monitor-diagnostics-splunk-integration/activitylogs.png "etkinlik günlükleri")

> [!NOTE]
> (Örneğin, bir ara sunucu kullanarak ya da Splunk bulutta çalıştırılan) Splunk Örneğinizde bir eklenti yükleyemezse, size bu olayları Splunk HTTP olay Toplayıcıya bu kullanarak iletebilir [yeni iletileri tarafından tetiklenen işlevi Olay hub'ı](https://github.com/Microsoft/AzureFunctionforSplunkVS). " 
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici'de denetim günlükleri şema yorumlama](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Azure İzleyici'de oturum açma günlükleri şema yorumlama](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)