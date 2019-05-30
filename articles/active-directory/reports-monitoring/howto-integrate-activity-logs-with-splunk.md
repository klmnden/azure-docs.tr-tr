---
title: Azure Active Directory günlükleri Azure İzleyicisi'ni kullanarak Splunk ile tümleştirme | Microsoft Docs
description: Azure Active Directory günlükleri Azure İzleyicisi'ni kullanarak SumoLogic ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f30e19a351f7b25f995a85cfd566bcba091ac27
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65597829"
---
# <a name="how-to-integrate-azure-active-directory-logs-with-splunk-using-azure-monitor"></a>Nasıl yapılır: Azure İzleyicisi'ni kullanarak Splunk ile Azure Active Directory günlükleri tümleştirme

Bu makalede, Azure İzleyicisi'ni kullanarak Azure Active Directory (Azure AD) günlükleri Splunk ile tümleştirmeyi öğrenin. Splunk ile olay hub'ı tümleştirdikten sonra ve bir Azure olay hub'ına ilk günlükleri yönlendirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:
* Azure AD etkinlik içeren bir Azure olay hub'ına kaydeder. Bilgi edinmek için nasıl [, etkinlik günlükleri Olay hub'ına akış](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Azure İzleyici eklenti Splunk için. [İndirme ve yapılandırma Splunk örneğinizin](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="integrate-azure-active-directory-logs"></a>Azure Active Directory günlükleri tümleştirme 

1. Splunk örneğinizi açın ve seçin **veri özeti**.

    !["Veri özeti" düğmesi](./media/howto-integrate-activity-logs-with-splunk/DataSummary.png)

2. Seçin **Sourcetypes** sekmesine tıklayın ve ardından **amal: aadal:audit**

    ![Veri Özet Sourcetypes sekmesi](./media/howto-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    Azure AD etkinlik günlükleri, aşağıdaki şekilde gösterilmiştir:

    ![Etkinlik günlükleri](./media/howto-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> (Örneğin, bir ara sunucu kullanarak ya da Splunk bulutta çalıştırılan) Splunk Örneğinizde bir eklenti yükleyemezse, bu olayları Splunk HTTP olay Toplayıcıya iletir. Bunu yapmak için bunu kullanın [Azure işlevi](https://github.com/Microsoft/AzureFunctionforSplunkVS), olay hub'ındaki yeni iletileri tarafından tetiklenen. 
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure İzleyici oturum açma günlükleri şemasını yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](concept-activity-logs-azure-monitor.md#frequently-asked-questions)