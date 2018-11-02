---
title: Azure İzleyici'de izleme diğer hizmetlerden Uyarıları yönetme
description: Azure İzleyici'de Nagios ve Zabbix SCOM Uyarıları yönetme
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: bc597d42fe89c0e03c4af1db3a935031b9043a98
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748281"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Diğer izleme hizmetlerinden gelen uyarıları yönetme

Artık, System Center Operations Manager'da Nagios ve Zabbix uyarıları görüntüleyebilirsiniz [uyarı deneyimi birleşik](https://aka.ms/azure-alerts-overview). Bu uyarılar tümleştirmeler Nagios/Zabbix sunucuları veya System Center Operations Manager ile Log Analytics'e gelir. 

## <a name="prerequisites"></a>Önkoşullar
Bu kayıtları toplamak için gerekli yapılandırmayı gerçekleştirmelidir, böylece herhangi bir kayıt Log Analytics deposunda bir tür uyarılarla birleşik uyarı deneyimi içe.
1. İçin **Nagios** ve **Zabbix** uyarıları [bu sunucuları yapılandırmak](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) uyarıları Log Analytics'e göndermek için.
1. İçin **System Center Operations Manager** uyarıları [Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlanmak](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). System Center Operations Manager'da oluşturulan tüm uyarılar, Log Analytics'e aktarılır.

## <a name="view-your-alert-instances"></a>Uyarı örneklerinizi görüntüleyebilir
Log Analytics'na aktarma yapılandırdıktan sonra bu hizmetleri izleme uyarı örneklerden görüntüleme başlayabilirsiniz [uyarı deneyimi birleşik](https://aka.ms/azure-alerts-overview). Birleştirilmiş uyarılar deneyimi varsa sonra [uyarı örneklerinizi yönetin](https://aka.ms/managing-alert-instances), [Bu uyarılar üzerinde oluşturulan akıllı grupları yönetme](https://aka.ms/managing-smart-groups) ve [uyarılarınızı durumunu değiştirebilir ve akıllı grupları](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Birleştirilmiş uyarılar deneyiminin Nagios uyarıları olmayan durum bilgisi olan – örneğin [izleme koşulu](https://aka.ms/azure-alerts-overview) uyarının "Fired" "Çözülmüş" geçer değil. Bunun yerine "Fired" ve "Çözülmüş" ayrı uyarı örneklerinin görüntülenir. 
