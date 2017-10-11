---
title: "Uygulama ağ geçidi için erişim günlükleri, performans günlükleri, arka uç sistem durumu ve ölçümleri izleme | Microsoft Docs"
description: "Etkinleştirme ve uygulama ağ geçidi için erişim günlüklerini ve performans günlüklerini yönetme hakkında bilgi edinin"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 12c252340b82aba5ee69b12db83353750782e7c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Arka uç sistem durumu, tanılama günlüklerini ve uygulama ağ geçidi ölçümleri

Azure uygulama ağ geçidi kullanarak, aşağıdaki yollarla kaynakları izleyebilirsiniz:

* [Arka uç sistem durumu](#back-end-health): uygulama Ağ Geçidi sunucularının arka uç havuzlarındaki PowerShell ve Azure Portalı aracılığıyla durum izleme yeteneği sağlar. Performans Tanılama günlükleri aracılığıyla arka uç havuzları durumunu da bulabilirsiniz.

* [Günlükleri](#diagnostic-logs): günlükleri izin performans, erişim ve diğer veri kaydedilmeyecek veya izleme amacıyla bir kaynaktan tüketilen.

* [Ölçümleri](#metrics): uygulama ağ geçidi şu anda bir ölçüm yok. Bu ölçüm, saniye başına bayt uygulama ağ geçidi verimini ölçer.

## <a name="back-end-health"></a>Arka uç sistem durumu

Uygulama ağ geçidi arka uç havuzları portal, PowerShell ve komut satırı arabirimi (CLI) aracılığıyla tek tek üyeleri sistem durumu izleme yeteneği sağlar. Birleşik bir sistem durumu da bulabilirsiniz performans tanılama günlükleri aracılığıyla arka uç havuzları özeti. 

Arka uç sistem durumu raporu arka uç örneklerine uygulama ağ geçidi durumu araştırması çıktısını yansıtır. Yoklama zaman başarısız olur ve arka uç trafik alabilir, sağlıklı kabul edilir. Aksi takdirde, kötü olarak değerlendirilir.

> [!IMPORTANT]
> Bir uygulama ağ geçidi alt ağı üzerinde bir ağ güvenlik grubu (NSG) ise, uygulama ağ geçidi alt ağı gelen trafik için bağlantı noktası aralıkları 65503 65534 açın. Bu bağlantı noktaları, arka uç sistem çalışmak için API için gereklidir.


### <a name="view-back-end-health-through-the-portal"></a>Portal üzerinden arka uç durumunu görüntüle

Portalda, arka uç sistem durumu otomatik olarak sağlanır. Mevcut bir uygulama ağ geçidi içinde seçin **izleme** > **arka uç sistem durumu**. 

(Bu bir NIC, IP veya FQDN olup olmadığı) arka uç havuzundaki her üyenin bu sayfada listelenir. Arka uç havuzu adını, bağlantı noktası, arka uç HTTP ayarları adı ve sistem durumu gösterilir. Sistem durumu için geçerli değerler **sağlıklı**, **sağlıksız**, ve **bilinmeyen**.

> [!NOTE]
> Arka uç sistem durumunu görüyorsanız, **bilinmeyen**, arka uç erişimi bir NSG kuralı, bir kullanıcı tanımlı yönlendirme (UDR) veya özel DNS sanal ağda tarafından engellenmediğinden emin olun.

![Arka uç sistem durumu][10]

### <a name="view-back-end-health-through-powershell"></a>PowerShell aracılığıyla arka uç durumunu görüntüle

Aşağıdaki PowerShell kod kullanarak arka uç durumunu görüntülemek nasıl gösterir `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a>Azure CLI 2.0 aracılığıyla arka uç durumunu görüntüle

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Sonuçlar

Aşağıdaki kod parçacığında yanıt örneği gösterilmektedir:

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Tanılama günlükleri

Azure'da günlükleri farklı türlerde yönetmek ve uygulama ağ geçitleri sorunlarını gidermek için kullanabilirsiniz. Bu günlükler bazıları portalı üzerinden erişebilirsiniz. Tüm günlükler Azure Blob depolama alanından ayıklanan ve farklı Araçlar'da gibi görüntülenebilir [günlük analizi](../log-analytics/log-analytics-azure-networking-analytics.md), Excel ve Power BI. Aşağıdaki listeden günlükleri farklı türleri hakkında daha fazla bilgi edinebilirsiniz:

* **Etkinlik günlüğü**: kullanabileceğiniz [Azure etkinlik günlükleri](../monitoring-and-diagnostics/insights-debugging-with-events.md) (önceki adıyla işletimsel ve Denetim günlükleri olarak bilinir), Azure aboneliğinizin ve durumlarını gönderilen tüm işlemleri görüntülemek için. Etkinlik günlüğü girişleri varsayılan olarak toplanır ve Azure Portalı'nda görüntüleyebilirsiniz.
* **Erişim günlüğüne**: uygulama ağ geçidi erişim düzenlerini görüntülemek ve arayanın IP, istenen URL, yanıt gecikme, dönüş kodu ve bayt ve kapatma gibi önemli bilgileri analiz etmek için bu günlük kullanabilirsiniz. Bir erişim günlüğü, her 300 saniyede toplanır. Bu günlük, uygulama ağ geçidi örneği başına bir kayıt içerir. Uygulama ağ geçidi örneğinin InstanceId'si özelliği tarafından belirlenebilir.
* **Performans günlüğü**: uygulama ağ geçidi örneklerinin nasıl performans gösterdiğini görüntülemek için bu günlük kullanabilirsiniz. Bu günlük sunulan, toplam istekleri dahil olmak üzere her bir örnek, üretilen iş bayt performans bilgilerini yakalar, toplam istek sayısı sunulan, başarısız istek sayısını ve sağlıklı ve sağlıksız arka uç örnek sayısı. Bir performans günlüğü, her 60 saniyede toplanır.
* **Güvenlik Duvarı günlük**: web uygulaması güvenlik duvarı ile yapılandırılmış bir uygulama Ağ Geçidi algılama veya önleme modu üzerinden oturum istekleri görüntülemek için bu günlük kullanabilirsiniz.

> [!NOTE]
> Günlükleri, yalnızca Azure Resource Manager dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynakların günlükleri kullanamazsınız. Daha iyi iki modellerinin anlamak için bkz: [anlama Resource Manager dağıtımını ve klasik dağıtımı](../azure-resource-manager/resource-manager-deployment-model.md) makalesi.

Günlüklerinizi depolamak için üç seçeneğiniz vardır:

* **Depolama hesabı**: günlükleri uzun bir süre depolandığında ve gerektiğinde gözden depolama hesapları günlükleri için en iyi kullanımı.
* **Olay hub'ları**: olay hub'ları olan diğer güvenlik bilgilerini ile tümleştirmek için harika bir seçenek ve Olay yönetimi (SEIM) araçları kaynaklarınız üzerinde uyarıları almak için.
* **Günlük analizi**: günlük analizi uygulamanızın veya genel gerçek zamanlı izleme veya eğilimleri bakarak için en iyi şekilde kullanılır.

### <a name="enable-logging-through-powershell"></a>PowerShell aracılığıyla günlük kaydını etkinleştir

Etkinlik günlüğü her Resource Manager kaynak için otomatik olarak etkinleştirilir. Erişim ve bu günlükleri kullanılabilir veri toplama başlatmak için oturum performans etkinleştirmeniz gerekir. Günlük kaydını etkinleştirmek için aşağıdaki adımları kullanın:

1. Depolama hesabınızın kaynak kimliği, günlük verilerinin depolandığı unutmayın. Bu değer biçimindedir: /subscriptions/\<Subscriptionıd\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Storage/storageAccounts/\<depolama hesabı adı\>. Aboneliğinizdeki herhangi bir depolama hesabı kullanabilirsiniz. Bu bilgileri bulmak için Azure portalını kullanabilirsiniz.

    ![Portal: depolama hesabı kaynak kimliği](./media/application-gateway-diagnostics/diagnostics1.png)

2. Günlük kaydı etkin uygulama ağ geçidinizin kaynak kimliği unutmayın. Bu değer biçimindedir: /subscriptions/\<Subscriptionıd\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Network/applicationGateways/\<uygulama ağ geçidi adı \>. Bu bilgileri bulmak için portalı kullanabilirsiniz.

    ![Portal: uygulama ağ geçidi kaynak kimliği](./media/application-gateway-diagnostics/diagnostics2.png)

3. Aşağıdaki PowerShell cmdlet'ini kullanarak tanılama günlük kaydını etkinleştir:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Etkinlik günlükleri ayrı bir depolama hesabı gerektirmez. Depolama kullanımı erişimi ve performans günlüğünü servis ücretleri doğurur.

### <a name="enable-logging-through-the-azure-portal"></a>Azure portalı üzerinden günlük kaydını etkinleştir

1. Azure portalında kaynağınızı bulun ve tıklatın **tanılama günlükleri**.

   Uygulama ağ geçidi için üç günlük vardır:

   * Erişim günlüğü
   * Performans günlüğü
   * Güvenlik Duvarı günlüğü

2. Veri toplama başlatmak için tıklatın **tanılamayı açın**.

   ![Tanılama açma][1]

3. **Tanılama ayarları** dikey tanılama günlükleri için ayarları sağlar. Bu örnekte, günlük analizi günlükleri depolar. Tıklatın **yapılandırma** altında **günlük analizi** çalışma alanınızı yapılandırmak için. Tanılama günlüklerini kaydetmek için olay hub'ları ve bir depolama hesabı kullanabilirsiniz.

   ![Yapılandırma işlemi başlatılıyor][2]

4. Varolan bir Operations Management Suite (OMS) çalışma alanını seçin veya yeni bir tane oluşturun. Bu örnek, mevcut bir kullanır.

   ![OMS çalışma alanları için seçenekleri][3]

5. Ayarları onaylayın ve tıklatın **kaydetmek**.

   ![Tanılama ayarları dikey seçimleri][4]

### <a name="activity-log"></a>Etkinlik günlüğü

Azure etkinlik günlüğü varsayılan olarak oluşturur. Günlükleri 90 gün boyunca Azure olay günlüklerini deposunda saklanır. Bu günlükler hakkında daha fazla bilgi okuyarak [olayları ve etkinlik günlüğü görüntüle](../monitoring-and-diagnostics/insights-debugging-with-events.md) makale.

### <a name="access-log"></a>Erişim günlüğü

Önceki adımlarda ayrıntılı olarak her bir uygulama ağ geçidi örnek üzerinde yalnızca etkinleştirdikten ise erişim günlüğü oluşturulur. Veri günlük kaydı etkinleştirildiğinde, belirttiğiniz depolama hesabında depolanır. Uygulama ağ geçidi'nin her erişim, aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|örnek kimliği     | İsteği sunan uygulama ağ geçidi örneği.        |
|ClientIP     | İstek için kaynak IP.        |
|clientPort     | İstek için kaynak bağlantı noktası.       |
|HttpMethod     | İstek tarafından kullanılan HTTP yöntemi.       |
|requestUri     | Alınan istek URI'si.        |
|RequestQuery     | **Sunucu yönlendirilen**: bir istek gönderildi arka uç havuzu örnek. </br> **X-AzureApplicationGateway-günlük-ID**: bağıntı istek için kullanılan kimliği. Arka uç sunucularına trafiği sorunlarını gidermek için kullanılabilir. </br>**Sunucu durumu**: uygulama ağ geçidi arka uçtan alınan HTTP yanıt kodu.       |
|UserAgent     | Kullanıcı Aracısı'ndan HTTP isteği üstbilgisi.        |
|httpStatus     | Uygulama ağ geçidi'nden istemciye döndürülen HTTP durum kodu.       |
|httpVersion     | İstek HTTP sürümü.        |
|ReceivedBytes     | Paket, alınan bayt cinsinden boyutu.        |
|SentBytes| Paket, gönderilen bayt cinsinden boyutu.|
|TimeTaken| Bir isteğin işlenmesi için ve gönderilecek yanıt için geçen süre (milisaniye cinsinden) uzunluğu. Bu, uygulama ağ geçidi bir HTTP isteğinin yanıtı gönderdiğinizde, işlem tamamlanmadan zaman için ilk baytını alır zaman aralığından olarak hesaplanır. Time-Taken alanı genellikle isteği ve yanıt paketlerin ağ üzerinden yolculuk zaman içerir dikkate almak önemlidir. |
|sslEnabled| Arka uç havuzları iletişimin SSL kullanılıp. Açma ve kapatma değerler geçerlidir.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a>Performans günlüğü

Önceki adımlarda ayrıntılı olarak her bir uygulama ağ geçidi örnek üzerinde yalnızca etkinleştirdiyseniz, performans günlüğü oluşturulur. Veri günlük kaydı etkinleştirildiğinde, belirttiğiniz depolama hesabında depolanır. Performans günlüğü verilerini 1 dakikalık aralıklarla oluşturulur. Aşağıdaki veriler günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|örnek kimliği     |  Uygulama ağ geçidi örneği performans verileri oluşturuluyor. Birden çok örnekli uygulama ağ geçidi için örneği başına bir satır yok.        |
|healthyHostCount     | Arka uç havuzundaki sağlıklı ana bilgisayar sayısı.        |
|unHealthyHostCount     | Arka uç havuzundaki sağlıksız ana bilgisayar sayısı.        |
|RequestCount     | Hizmet isteği sayısı.        |
|Gecikme süresi | İsteklere hizmet arka uç örneğinden isteklerinin gecikme süresi (milisaniye cinsinden). |
|failedRequestCount| Başarısız istek sayısı.|
|Üretilen iş| Saniyedeki bayt cinsinden son günlük itibaren ortalama performansıdır.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> Gecikme süresi, HTTP isteğin ilk baytını HTTP yanıtın son baytını gönderildiğinde zaman alındığında zamandan hesaplanır. Uygulama ağ geçidi işleme süresi ve arka uç ve arka uç, isteği işlemek için gereken süre ağ maliyetine toplamıdır.

### <a name="firewall-log"></a>Güvenlik Duvarı günlüğü

Önceki adımlarda ayrıntılı olarak her uygulama ağ geçidi için yalnızca etkinleştirdiyseniz, Güvenlik Duvarı günlük oluşturulur. Bu günlük Ayrıca web uygulaması Güvenlik Duvarı'nı bir uygulama ağ geçidi üzerinde yapılandırılmış olması gerekir. Veri günlük kaydı etkinleştirildiğinde, belirttiğiniz depolama hesabında depolanır. Aşağıdaki veriler günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|örnek kimliği     | Uygulama ağ geçidi örneği için hangi güvenlik duvarı veri oluşturuluyor. Birden çok örnekli uygulama ağ geçidi için örneği başına bir satır yok.         |
|clientIp     |   İstek için kaynak IP.      |
|clientPort     |  İstek için kaynak bağlantı noktası.       |
|requestUri     | Alınan istek URL'si.       |
|ruleSetType     | Kural türünü ayarlayın. Kullanılabilir OWASP değerdir.        |
|ruleSetVersion     | Kural kullanılan sürümünü ayarlayın. Değerleri 2.2.9 ve 3.0 kullanılabilir.     |
|RuleId     | Tetikleyici olay kimliği kuralı.        |
|İleti     | Tetikleyici olay kullanıcı dostu iletisi. Ayrıntılar bölümünde daha ayrıntılı bilgi sağlanır.        |
|Eylem     |  İstek üzerinde gerçekleştirilecek eylem. Engellenen ve izin verilen değerleri kullanılabilir.      |
|Site     | Günlük oluşturulduğu site. Şu anda, yalnızca genel kurallar genel olduğundan listelenir.|
|Ayrıntıları     | Olay Ayrıntıları.        |
|details.Message     | Kural açıklaması.        |
|details.Data     | Belirli veri kural eşleşen isteğinde bulundu.         |
|details.File     | Kural bulunan yapılandırma dosyası.        |
|details.Line     | Satır numarası yapılandırma dosyasında olay tetiklenir.       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-the-activity-log"></a>Görüntüleme ve etkinlik günlüğü çözümleme

Görüntüleme ve aşağıdaki yöntemlerden birini kullanarak Etkinlik günlüğü verilerini çözümleme:

* **Azure Araçları**: bilgi almanızı Azure PowerShell, Azure CLI, Azure REST API veya Azure Portalı aracılığıyla etkinlik günlüğü. Her yöntem için adım adım yönergeler ayrıntılı olarak [etkinlik işlemleri Resource Manager ile](../azure-resource-manager/resource-group-audit.md) makalesi.
* **Power BI**: henüz yoksa bir [Power BI](https://powerbi.microsoft.com/pricing) hesabı deneyebilirsiniz, ücretsiz. Kullanarak [Azure etkinlik günlükleri paketi Power BI için içerik](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), verilerinizi olduğu veya özelleştirin olarak kullanabileceğiniz, önceden yapılandırılmış panolarla çözümleyebilirsiniz.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Görüntüleme ve erişim, performans ve güvenlik duvarı günlüklerini çözümleme

Azure [günlük analizi](../log-analytics/log-analytics-azure-networking-analytics.md) Blob storage hesabınızdan sayacı ve olay günlük dosyaları toplayabilirsiniz. Görselleştirme ve günlükleriniz analiz etmek için güçlü arama özellikleri içerir.

Ayrıca, depolama hesabınıza bağlanın ve erişim ve performans günlüklerini JSON günlük girişlerini almak. JSON dosyaları indirdikten sonra bunları CSV'ye Dönüştür ve Excel, Power BI veya diğer herhangi bir veri görselleştirmesi aracı görüntüleyin.

> [!TIP]
> Visual Studio ve sabitleri ve değişkenleri C# değerlerini değiştirmenin temel kavramları biliyorsanız, kullanabileceğiniz [Dönüştürücü Araçları oturum](https://github.com/Azure-Samples/networking-dotnet-log-converter) github'dan kullanılabilir.
> 
> 

## <a name="metrics"></a>Ölçümler

Ölçümleri portalda performans sayaçları görüntüleyebileceğiniz belirli Azure kaynakları için bir özelliğidir. Uygulama ağ geçidi için bir ölçüm kullanıma sunulmuştur. Bu ölçümüdür üretilen iş ve Portalı'nda bkz. Bir uygulama ağ geçidi bulun ve tıklatın **ölçümleri**. Değerleri görüntülemek için seçin performansı **kullanılabilir ölçümler** bölümü. Aşağıdaki görüntüde, farklı zaman aralıkları verileri görüntülemek için kullanabileceğiniz filtreleri içeren bir örnek görebilirsiniz.

![Filtrelerle ölçüm görünümü][5]

Geçerli ölçümleri listesini görmek için bkz: [desteklenen Azure İzleyicisi ile ölçümleri](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

### <a name="alert-rules"></a>Uyarı kuralları

Bir kaynak için ölçümleri temel uyarı kuralları başlatabilirsiniz. Örneğin, bir uyarı bir Web kancası çağrısı veya uygulama ağ geçidi verimini belirli bir süre boyunca yukarıda, aşağıda veya bir eşik ise yönetici e-posta.

Aşağıdaki örnek eşiği bir işleme ihlallerini sonra yönetici e-posta gönderen bir uyarı kuralı oluşturmada size yol gösterir:

1. Tıklatın **ölçüm uyarı Ekle** açmak için **Ekle kuralı** dikey. Bu dikey ölçümleri dikey penceresinden de ulaşabilir.

   !["Ölçüm uyarı Ekle" düğmesi][6]

2. Üzerinde **Ekle kuralı** dikey penceresinde, adı, koşul, dolgu bölümleri bildir tıklatın ve **Tamam**.

   * İçinde **koşulu** Seçici, dört değerden birini seçin: **büyük**, **büyük veya ona eşit**, **değerinden**, veya **Küçük veya eşit**.

   * İçinde **süresi** Seçicisi, bir süre 5 dakika ile 6 saat seçin.

   * Seçerseniz **sahipleri, Katkıda Bulunanlar ve okuyucular e-posta**, bu kaynağa erişim izni olan kullanıcıların göre e-posta dinamik olabilir. Aksi takdirde kullanıcıların virgülle ayrılmış bir listesini sağlayabilirsiniz **ek yönetici email(s)** kutusu.

   ![Kural dikey ekleme][7]

Eşik ihlal varsa, aşağıdaki resimde gösterilene benzer bir e-posta ulaşır:

![E-posta ihlal edilen eşiği][8]

Ölçüm uyarı oluşturduktan sonra uyarıların bir listesi görüntülenir. Tüm uyarı kuralları genel bir bakış sağlar.

![Uyarılar ve kuralları listesi][9]

Uyarı bildirimleri hakkında daha fazla bilgi için bkz: [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Web kancası ve nasıl uyarılarla kullanabilmek için daha iyi anlamak için ziyaret [bir Web kancası Azure ölçüm uyarıyı yapılandırmak](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="next-steps"></a>Sonraki adımlar

* Sayaç ve olay günlüklerini kullanarak Görselleştir [günlük analizi](../log-analytics/log-analytics-azure-networking-analytics.md).
* [Azure etkinlik günlüğü Power BI ile görselleştirme](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog postası.
* [Görüntüleme ve Azure etkinlik günlükleri Power BI ve daha fazla çözümleme](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog postası.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
