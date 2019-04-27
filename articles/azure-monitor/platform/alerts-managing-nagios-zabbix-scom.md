---
title: SCOM, Zabbix ve Nagios Azure İzleyici'de Uyarıları yönetme
description: SCOM, Zabbix ve Nagios Azure İzleyici'de Uyarıları yönetme
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 48fb9d8eaf2003834a420b48d649c830c608fd6e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712998"
---
# <a name="manage-alerts-from-scom-zabbix-and-nagios-in-azure-monitor"></a>SCOM, Zabbix ve Nagios Azure İzleyici'de Uyarıları yönetme

Artık, System Center Operations Manager'da Nagios ve Zabbix uyarıları görüntüleyebilirsiniz [Azure İzleyici](https://aka.ms/azure-alerts-overview). Bu uyarılar tümleştirmeler Nagios/Zabbix sunucuları veya System Center Operations Manager ile Log Analytics'e gelir. 

## <a name="prerequisites"></a>Önkoşullar
Bu kayıtları toplamak için gerekli yapılandırmayı gerçekleştirmelidir, böylece herhangi bir kayıt Log Analytics deposunda bir tür uyarılarla Azure İzleyici ile içe.
1. İçin **Nagios** ve **Zabbix** uyarıları [bu sunucuları yapılandırmak](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) için [uyarıları göndermek](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-alerts-nagios-zabbix?toc=%2Fazure%2Fazure-monitor%2Ftoc.json) Log analytics'e.
1. İçin **System Center Operations Manager** uyarıları [Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlanmak](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Aşağıdaki dağıtma [uyarı Yönetimi](https://docs.microsoft.com/azure/azure-monitor/platform/alert-management-solution) çözümleri Azure Market çözüm. Bunu yaptıktan sonra System Center Operations Manager'da oluşturulan herhangi bir uyarı Log Analytics'e aktarılır.

## <a name="view-your-alert-instances"></a>Uyarı örneklerinizi görüntüleyebilir
Log Analytics'na aktarma yapılandırdıktan sonra bu hizmetleri izleme uyarı örneklerden görüntüleme başlayabilirsiniz [Azure İzleyici](https://aka.ms/azure-alerts-overview). Azure İzleyicisi'nde mevcut olduklarından sonra [uyarı örneklerinizi yönetin](https://aka.ms/managing-alert-instances), [Bu uyarılar üzerinde oluşturulan akıllı grupları yönetme](https://aka.ms/managing-smart-groups) ve [uyarıları ve akıllı gruplarıdurumunudeğiştirme](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  1. Bu çözüm yalnızca Azure İzleyicisi'nde SCOM/Zabbix/Nagios tetiklenen uyarı örneklerinin görüntülemenize izin verir. Uyarı kuralı yapılandırması yalnızca ilgili izleme araçları içinde görüntülenen/düzenlenmiş olabilir. 
>  1. Tetiklenme tüm uyarı örnekleri hem de Azure İzleyici ve Azure Log Analytics kullanılabilir. Şu anda arasında iki seçin veya yalnızca belirli uyarıları harekete alma için hiçbir yolu yoktur.
>  1. Temel alınan telemetri türü kullanılamadığından SCOM, Zabbix ve Nagios tüm uyarıların sinyal türü "Bilinmeyen" vardır.
>  1. Nagios uyarılar olmayan durum bilgisi olan – örneğin [izleme koşulu](https://aka.ms/azure-alerts-overview) uyarının "Fired" "Çözülmüş" geçer değil. Bunun yerine "Fired" ve "Çözülmüş" ayrı uyarı örneklerinin görüntülenir. 

