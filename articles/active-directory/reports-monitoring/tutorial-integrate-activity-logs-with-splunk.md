---
title: Stream Splunk Azure İzleyicisi'ni (Önizleme) kullanarak Azure Active Directory günlükleri | Microsoft Docs
description: Azure İzleyicisi'ni (Önizleme) kullanarak Azure Active Directory günlükleri Splunk ile tümleştirmeyi öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94039655a6cbe7878fa1f61ce759485392dc7e98
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438861"
---
# <a name="integrate-azure-ad-logs-with-splunk-using-azure-monitor-preview"></a>Azure İzleyicisi'ni (Önizleme) kullanarak Splunk ile Azure AD günlükleri tümleştirme

Bu makalede, Azure İzleyicisi'ni kullanarak Azure Active Directory (Azure AD) günlükleri Splunk ile tümleştirmeyi öğrenin. Splunk ile olay hub'ı tümleştirdikten sonra ve bir Azure olay hub'ına ilk günlükleri yönlendirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:
* Azure AD etkinlik içeren bir Azure olay hub'ına kaydeder. Bilgi edinmek için nasıl [, etkinlik günlükleri Olay hub'ına akış](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Azure İzleyici eklenti Splunk için. [İndirme ve yapılandırma Splunk örneğinizin](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="tutorial"></a>Öğretici 

1. Splunk örneğinizi açın ve seçin **veri özeti**.

    !["Veri özeti" düğmesi](./media/tutorial-integrate-activity-logs-with-splunk/DataSummary.png)

2. Seçin **Sourcetypes** sekmesine tıklayın ve ardından **amal: aadal:audit**

    ![Veri Özet Sourcetypes sekmesi](./media/tutorial-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    Azure AD etkinlik günlükleri, aşağıdaki şekilde gösterilmiştir:

    ![Etkinlik günlükleri](./media/tutorial-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> (Örneğin, bir ara sunucu kullanarak ya da Splunk bulutta çalıştırılan) Splunk Örneğinizde bir eklenti yükleyemezse, bu olayları Splunk HTTP olay Toplayıcıya iletir. Bunu yapmak için bunu kullanın [Azure işlevi](https://github.com/Microsoft/AzureFunctionforSplunkVS), olay hub'ındaki yeni iletileri tarafından tetiklenen. 
>

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure İzleyici oturum açma günlükleri şemasını yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
* [Sık sorulan sorular ve bilinen sorunlar](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
