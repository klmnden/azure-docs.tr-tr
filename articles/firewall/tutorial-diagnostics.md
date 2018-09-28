---
title: Öğretici - Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme
description: Bu öğreticide Azure Güvenlik Duvarı günlükleri ile ölçümlerini etkinleştirmeyi ve yönetmeyi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 1940fb210481dc75fe48d110776185e90cb3e42f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991054"
---
# <a name="tutorial-monitor-azure-firewall-logs-and-metrics"></a>Öğretici: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme

Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz. Ölçümleri kullanarak portalda performans sayaçlarını görüntüleyebilirsiniz. 

Bu günlüklerden bazılarına portaldan erişebilirsiniz. Günlükler [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Depolama ve Event Hubs'a gönderilebilir, Log Analytics'te veya Excel ve Power BI gibi farklı araçlarda analiz edilebilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure portaldan günlüğe kaydetmeyi etkinleştirme
> * PowerShell ile günlüğe kaydetmeyi etkinleştirme
> * Etkinlik günlüğünü görüntüleme ve analiz etme
> * Ağ ve uygulama kuralı günlüklerini görüntüleme ve analiz etme
> * Ölçümleri görüntüle

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce, Azure Güvenlik Duvarında kullanılabilen tanılama günlüklerine ve ölçümlere genel bir bakış için [Azure Güvenlik Duvarı günlükleri ve ölçümleri](logs-and-metrics.md) yazısını okumanız gerekir.


## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Azure portaldan tanılama günlüğüne kaydetmeyi etkinleştirme

Tanılama günlüğüne kaydetme işlemi etkinleştirildikten sonra verilerin günlükte görünmesi birkaç dakika sürebilir. İlk seferde görünen veri olmazsa birkaç dakika sonra tekrar deneyin.

1. Azure portalda güvenlik duvarı kaynak grubunuzu açın ve güvenlik duvarına tıklayın.
2. **İzleme** bölümünde **Tanılama günlükleri**'ne tıklayın.

   Azure Güvenlik Duvarı için hizmete özgü iki günlük vardır:

   * AzureFirewallApplicationRule
   * AzureFirewallNetworkRule

3. Veri toplamaya başlamak için **Tanılamayı aç**'a tıklayın.
4. **Tanılama ayarları** sayfasında tanılama günlükleriyle ilgili ayarlar bulunur. 
5. Bu örnekte günlükler Log Analytics'te depolandığı için ad olarak **Firewall log analytics** yazın.
6. Çalışma alanınızı yapılandırmak için **Log Analytics'e gönder**'e tıklayın. Tanılama günlüklerini kaydetmek için Event Hubs'ı veya depolama hesabını da kullanabilirsiniz.
7. **Log Analytics** bölümünde **Yapılandır**'a tıklayın.
8. OMS Çalışma Alanları sayfasında **Yeni Çalışma Alanı Oluştur**'a tıklayın.
9. **Log Analytics çalışma alanı** sayfasında yeni **OMS Çalışma Alanı** adı olarak **firewall-oms** yazın.
10. Aboneliğinizi seçin, var olan güvenlik duvarı kaynak grubunu (**Test-FW-RG**) kullanın, konum olarak **Doğu ABD** seçin ve **Ücretsiz** fiyatlandırma katmanını belirleyin.
11. **Tamam** düğmesine tıklayın.
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

## <a name="view-metrics"></a>Ölçümleri görüntüle
Bir Azure Güvenlik Duvarına gidin, **İzleme** bölümünde **Ölçümler**’e tıklayın. Kullanılabilir değerleri görüntülemek için **ÖLÇÜM** açılan listesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik duvarınızı günlükleri toplayacak şekilde yapılandırdığınıza göre artık Log Analytics'e göz atarak verilerinizi görüntüleyebilirsiniz.

> [!div class="nextstepaction"]
> [Log Analytics'teki ağ izleme çözümleri](../log-analytics/log-analytics-azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png
