---
title: Eski Log Analytics'ten API uyarılar Azure uyarıları API'de yeni geçiş
description: Eski savedSearch devre dışı bırakılması genel bakış, uyarı kuralları, ortak müşteri konuları ele alan ayrıntılarla yeni ScheduledQueryRules API'sine geçiş yapmak için Log Analytics uyarı API ve işlem tabanlı.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 1706fc050fecd2e4be3a40725ec3e63a9036b3a9
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486639"
---
# <a name="switch-api-preference-for-log-alerts"></a>Günlük uyarıları için API anahtarı tercihi

> [!NOTE]
> Belirtilen kullanıcılar için geçerli içerik yalnızca Azure genel bulutunda ve **değil** Azure kamu veya Azure Çin bulutu için.  

Yakın zamanda kadar Microsoft Operations Management Suite portalında uyarı kuralları yönetilen. Microsoft Azure Log Analytics de dahil olmak üzere çeşitli hizmetler ile entegre yeni uyarılar deneyimini ve biz istenir [uyarı kurallarınızı OMS portalından Azure'a genişletme](alerts-extend.md). Ancak müşteriler için çok az kesinti emin olmak için işlem - tüketim için programlama arabirimi değiştirmedi [Log Analytics uyarı API](api-alerts.md) SavedSearch üzerinde temel.

Ancak Log Analytics kullanıcıları uyarmak için bir true Azure programlı alternatif duyurmaktan artık [Azure İzleyici - ScheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), olduğu aynı zamanda, Yansıtıcı, [Azure faturalandırma - günlük uyarıları için](alerts-unified-log.md#pricing-and-billing-of-log-alerts). API'yi kullanarak günlük uyarıları yönetme hakkında daha fazla bilgi için bkz. [Azure kaynak şablonu kullanarak yönetme günlük uyarıları](alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [PowerShell, CLI veya API kullanarak yönetme günlük uyarıları](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api).

## <a name="benefits-of-switching-to-new-azure-api"></a>Yeni Azure API'sine geçiş avantajları

Oluşturma ve yönetme kullanarak uyarıları çeşitli avantajları vardır [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) üzerinden [eski Log Analytics uyarı API](api-alerts.md); bazı önemli olanları aşağıdaki listeledik:

- Olanağı [çalışma günlük araması çapraz](../log-query/cross-workspace-query.md) uyarı kuralları ve Log Analytics çalışma alanları veya hatta Application Insights uygulamaları gibi dış kaynakları yayılma
- Birden çok alanlar grubu için sorguda kullanılan kullandığınızda [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanıcı toplama-Azure Portalı'nda oturum hangi alanın belirtebilirsiniz
- Günlük uyarıları kullanılarak oluşturulan [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) süresi en fazla 48 saat ve verileri getirme daha uzun süre daha önce tanımladığınız
- Üç düzeyi ile kaynakları olarak oluşturmaya gerek olmadan tek bir kaynak olarak bir görüntüsünde uyarı kuralları oluşturma [eski Log Analytics uyarı API](api-alerts.md)
- Azure - yeni günlük sorgusu tabanlı uyarıları tüm çeşitlerini tek programlama arabirimi [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) Application Insights yanı sıra Log Analytics için kuralları yönetmek için kullanılabilir
- Tüm yeni günlük uyarı işlevselliği ve gelecekteki geliştirme yalnızca yeni kullanıma sunulacaktır [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Eski günlük uyarıları API'SİNDEN geçiş işlemi

Uyarı kurallarını taşıma işleminin [eski Log Analytics uyarı API](api-alerts.md) , uyarı tanımınızı, sorgunuzu veya yapılandırmanızı herhangi bir şekilde değiştirmeyi gerektirmez. Uyarı kurallarınızı ve Etkilenmeyen olan ve Uyarıları izleme değil durdurur veya durdurulması, sırasında veya sonrasında anahtarı.

Kullanıcılar ya da ücretsiz [eski Log Analytics uyarı API](api-alerts.md) veya yeni [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Uyarı kuralları olacaktır ya da API tarafından oluşturulan *yalnızca aynı API tarafından yönetilebilir* - gibi Azure portalından iyi. Varsayılan olarak, Azure İzleyici kullanmaya devam edecek [eski Log Analytics uyarı API](api-alerts.md) Azure portalından yeni bir uyarı kuralı oluşturmak için.

Tercihi scheduledQueryRules API anahtarı etkilerini aşağıda derlenir:

- Günlük uyarıları programlama arabirimleri aracılığıyla yönetmek artık yapılmalıdır için yapılan tüm etkileşimleri kullanarak [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) yerine. Daha fazla bilgi için bkz: [örnek kullanımı aracılığıyla Azure kaynak şablonu](alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [Azure CLI ve PowerShell aracılığıyla örnek kullanımı](alerts-log.md#managing-log-alerts-using-powershell-cli-or-api)
- Azure portalında oluşturulan tüm yeni günlük uyarı kuralı kullanılarak oluşturulacak [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) yalnızca ve kullanıcıların kullanmasına izin [yeni API ek işlevler](#benefits-of-switching-to-new-azure-api) Azure portal aracılığıyla
- Önem derecesi günlük uyarı kuralları için gelen kayacak: *Kritik, uyarı ve bilgilendirici*, *önem derecesi değerlerini 0, 1 ve 2*. Oluşturma/güncelleştirme seçeneği ile birlikte, kural de 4 önem derecesine sahip uyarı.

> [!CAUTION]
> Yeni tercih geçiş yapmak için bir kullanıcı bölgedeyse sonra [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), kuralları geri iyileştirilmiş veya eski kullanımı için geri [eski Log Analytics uyarı API](api-alerts.md).

Gönüllü olarak yeni geçmek isteyen müşteriler [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) engellemek kullanımdan [eski Log Analytics uyarı API](api-alerts.md); bir PUT çağrısı gerçekleştirerek yapabilirsiniz altındaki tüm uyarı geçiş yapmak için API belirli bir Log Analytics çalışma alanıyla ilişkili kurallar.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

İstek gövdesini içeren ile JSON aşağıda.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

API kullanarak bir PowerShell komut satırı da erişilebilir [ARMClient](https://github.com/projectkudu/ARMClient), Azure Resource Manager API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. Aşağıda gösterildiği gibi örnek PUT çağrısında uyarı kurallarının tümünü geçiş yapmak için ARMclient aracı kullanarak belirli bir Log Analytics çalışma alanıyla ilişkili.

```powershell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Varsa yeni kullanmak için Log Analytics çalışma alanında uyarı kurallarının tümünü'numaralandırıcısının switch [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) olan şu yanıtı başarılı sağlanacaktır.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Kullanıcılar ayrıca Log Analytics çalışma alanınızın geçerli durumunu denetleyin ve sahip veya kullanılacak kullanılamayacağı değil, görebilirsiniz [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) yalnızca. Denetlenecek, kullanıcıların bir GET çağrısı gerçekleştirdiklerinden API altında.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

PowerShell kullanarak komut satırı kullanarak yukarıdaki yürütmek için [ARMClient](https://github.com/projectkudu/ARMClient) aracı, aşağıdaki örneğe bakın.

```powershell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Belirtilen Log Analytics çalışma alanı kullanmak için kullanılamayacağı [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) yalnızca; ardından yanıt JSON aşağıda listelenen olacaktır.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Belirtilen günlük analizi çalışma alanı henüz kullanmaya geçiş yapıldı değil, başka [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) yalnızca; ardından yanıt JSON aşağıda listelenen olacaktır.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure İzleyici - günlük uyarıları](alerts-unified-log.md).
- Oluşturmayı [uyarılar Azure Uyarıları'nda oturum](alerts-log.md).
- Daha fazla bilgi edinin [Azure Uyarıları'deneyimi](../../azure-monitor/platform/alerts-overview.md).
