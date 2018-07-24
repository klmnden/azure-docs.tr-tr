---
title: 'Öğretici: Azure Güvenlik Duvarı günlüklerini izleme'
description: Bu öğreticide Azure Güvenlik Duvarı günlüklerini etkinleştirmeyi ve yönetmeyi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: a4922fda80b957138a9929090f9d3c349348185d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991962"
---
# <a name="tutorial-monitor-azure-firewall-logs"></a>Öğretici: Azure Güvenlik Duvarı günlüklerini izleme

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

Azure Güvenlik Duvarı makalelerinde yer alan örneklerde Azure Güvenlik Duvarı'nın genel önizleme sürümünü etkinleştirdiğiniz kabul edilmektedir. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı genel önizleme sürümünü etkinleştirme](public-preview.md).

Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz.

Bu günlüklerden bazılarına portaldan erişebilirsiniz. Günlükler [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Depolama ve Event Hubs'a gönderilebilir, Log Analytics'te veya Excel ve Power BI gibi farklı araçlarda analiz edilebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portaldan günlüğe kaydetmeyi etkinleştirme
> * PowerShell ile günlüğe kaydetmeyi etkinleştirme
> * Etkinlik günlüğünü görüntüleme ve analiz etme
> * Ağ ve uygulama kuralı günlüklerini görüntüleme ve analiz etme

## <a name="diagnostic-logs"></a>Tanılama günlükleri

 Azure Güvenlik Duvarı'nda aşağıdaki tanılama günlükleri mevcuttur:

* **Uygulama kuralı günlüğü**

   Uygulama kuralı günlüğünü depolama hesabına kaydetmek, Event Hubs'a aktarmak ve/veya Log Analytics'e göndermek için her bir Azure Güvenlik Duvarı'nda etkinleştirmiş olmanız gerekir. Yapılandırdığınız uygulama kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Ağ kuralı günlüğü**

   Ağ kuralı günlüğünü depolama hesabına kaydetmek, Event Hubs'a aktarmak ve/veya Log Analytics'e göndermek için her bir Azure Güvenlik Duvarı'nda etkinleştirmiş olmanız gerekir. Yapılandırdığınız ağ kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Günlüklerinizi depolamak için kullanabileceğiniz üç seçenek vardır:

* **Depolama hesabı**: Depolama hesaplarının en iyi kullanım amacı, günlüklerin uzun süre depolanması ve ihtiyaç duyulduğunda gözden geçirilmesi durumlarıdır.
* **Event Hubs**: Event Hubs, kaynaklarınızla ilgili uyarılar almak için diğer güvenlik bilgisi ve olay yönetimi (SEIM) araçlarıyla tümleştirmek için idealdir.
* **Log Analytics**: Log Analytics'in en iyi kullanım amacı, uygulamanızın gerçek zamanlı olarak izlenmesi veya eğilimlerin incelenmesidir.

## <a name="activity-logs"></a>Etkinlik günlükleri

   Etkinlik günlüğü girişleri varsayılan olarak toplanır ve bunları Azure portalda görüntüleyebilirsiniz.

   [Azure etkinlik günlüklerini](../azure-resource-manager/resource-group-audit.md) (eski adıyla işlem günlükleri ve denetim günlükleri) kullanarak Azure aboneliğinize gönderilmiş olan tüm işlemleri görüntüleyebilirsiniz.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Azure portaldan tanılama günlüğüne kaydetmeyi etkinleştirme

Tanılama günlüğüne kaydetme işlemi etkinleştirildikten sonra verilerin günlükte görünmesi birkaç dakika sürebilir. İlk seferde görünen veri olmazsa birkaç dakika sonra tekrar deneyin.

1. Azure portalda güvenlik duvarı kaynak grubunuzu açın ve güvenlik duvarına tıklayın.
2. **İzleme** bölümünde **Tanılama günlükleri**'ne tıklayın.

   Azure Güvenlik Duvarı için hizmete özgü iki günlük vardır:

   * Uygulama kuralı günlüğü
   * Ağ kuralı günlüğü

3. Veri toplamaya başlamak için **Tanılamayı aç**'a tıklayın.
4. **Tanılama ayarları** sayfasında tanılama günlükleriyle ilgili ayarlar bulunur. 
5. Bu örnekte günlükler Log Analytics'te depolandığı için ad olarak **Firewall log analytics** yazın.
6. Çalışma alanınızı yapılandırmak için **Log Analytics'e gönder**'e tıklayın. Tanılama günlüklerini kaydetmek için Event Hubs'ı veya depolama hesabını da kullanabilirsiniz.
7. **Log Analytics** bölümünde **Yapılandır**'a tıklayın.
8. OMS Çalışma Alanları sayfasında **Yeni Çalışma Alanı Oluştur**'a tıklayın.
9. **Log Analytics çalışma alanı** sayfasında yeni **OMS Çalışma Alanı** adı olarak **firewall-oms** yazın.
10. Aboneliğinizi seçin, var olan güvenlik duvarı kaynak grubunu (**Test-FW-RG**) kullanın, konum olarak **Doğu ABD** seçin ve **Ücretsiz** fiyatlandırma katmanını belirleyin.
11. **Tamam**’a tıklayın.
   ![Yapılandırma işlemini başlatma][1]
12. **Günlük** bölümünde uygulama ve ağ kuralları için günlükleri toplamak için **AzureFirewallApplicationRule** ve **AzureFirewallNetworkRule** girişlerini seçin.
   ![Tanılama ayarlarını kaydetme][2]
13. **Kaydet**’e tıklayın.

## <a name="enable-logging-with-powershell"></a>PowerShell ile günlüğe kaydetmeyi etkinleştirme

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Bu günlükler aracılığıyla sunulan verileri toplamaya başlamak için tanılama günlüğüne kaydetme işlevinin etkinleştirilmesi gerekir.

Tanılama günlüğüne kaydetmeyi etkinleştirmek için aşağıdaki adımları izleyin:

1. Günlük verilerinin depolandığı depolama hesabınızın kaynak kimliğini not edin. Bu değer şu biçimdedir: */subscriptions/\<abonelik kimliği\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Storage/storageAccounts/\<depolama hesabı adı\>*.

   Aboneliğinizdeki herhangi bir depolama hesabını kullanabilirsiniz. Bu bilgileri Azure portalda bulabilirsiniz. Bu bilgiler kaynağın **Özellik** sayfasında yer alır.

2. Günlüğe kaydetmenin etkinleştirildiği güvenlik duvarının kaynak kimliğini not edin. Bu değer şu biçimdedir: */subscriptions/\<abonelik kimliği\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Network/azureFirewalls/\<Güvenlik duvarı adı\>*.

   Bu bilgileri portalda bulabilirsiniz.

3. Tanılama günlüğüne kaydetmeyi etkinleştirmek için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```
    
> [!TIP] 
>Tanılama günlükleri için ayrı bir depolama hesabına gerek yoktur. Depolama alanının erişim ve performans günlüğü kaydı için kullanılması durumunda hizmet ücreti tahsil edilir.

## <a name="view-and-analyze-the-activity-log"></a>Etkinlik günlüğünü görüntüleme ve analiz etme

Aşağıdaki yöntemlerden birini kullanarak etkinlik günlüğü verilerini görüntüleyebilir ve analiz edebilirsiniz:

* **Azure araçları**: Etkinlik günlüğü verilerini Azure PowerShell, Azure CLI, Azure REST API veya Azure portal üzerinden alabilirsiniz. Her yöntemle ilgili ayrıntılı adımlar [Kaynak Yöneticisi etkinlik işlemleri](../azure-resource-manager/resource-group-audit.md) makalesinde ayrıntılı bir şekilde anlatılmıştır.
* **Power BI**: [Power BI](https://powerbi.microsoft.com/pricing) hesabınız yoksa ücretsiz oluşturabilirsiniz. [Power BI için Azure Activity Logs içerik paketi](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) ile verilerinizi önceden yapılandırılmış panoları olduğu gibi veya değiştirerek kullanarak analiz edebilirsiniz.

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Ağ ve uygulama kuralı günlüklerini görüntüleme ve analiz etme

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), sayaç ve olay günlüğü dosyalarını toplar. Günlüklerinizi analiz etmek için görselleştirmelere ve güçlü arama özelliklerine sahiptir.

Dilerseniz depolama hesabınıza bağlanabilir ve JSON erişim günlüklerini ve performans günlüklerini alabilirsiniz. İndirdiğiniz JSON dosyalarını CSV biçimine dönüştürebilir ve Excel, Power BI veya diğer veri görselleştirme araçlarında görüntüleyebilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Güvenlik duvarınızı günlükleri toplayacak şekilde yapılandırdığınıza göre artık Log Anaytics'e göz atarak verilerinizi görüntüleyebilirsiniz.

> [!div class="nextstepaction"]
> [Log Analytics'teki ağ izleme çözümleri](../log-analytics/log-analytics-azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png
