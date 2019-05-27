---
title: Azure Application Gateway için erişim günlükleri, performans günlükleri, arka uç sistem durumu ve ölçümleri izleme
description: Etkinleştirme ve erişim günlükleri ve performans günlüklerini Azure Application Gateway için yönetme hakkında bilgi edinin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 3/28/2019
ms.author: amitsriva
ms.openlocfilehash: 367da8a1948b9feb42bc82d85762ae314fe165a0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66135643"
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway

Azure Application Gateway kullanarak kaynakları aşağıdaki şekillerde izleyebilirsiniz:

* [Arka uç sistem durumu](#back-end-health): Uygulama Ağ Geçidi sunucularının arka uç havuzlarındaki ve PowerShell aracılığıyla Azure portalında izleme yeteneği sağlar. Performans Tanılama günlükleri aracılığıyla arka uç havuzları durumunu da bulabilirsiniz.

* [Günlükleri](#diagnostic-logging): Günlükleri, performans, erişim ve kaydedilen ya da bir kaynaktan izleme amacıyla kullanılan diğer verileri için izin verin.

* [Ölçümleri](#metrics): Application Gateway şu anda performans sayaçlarını görüntülemek için yedi ölçümleri sahiptir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="back-end-health"></a>Arka uç sistem durumu

Application Gateway, tek bir portal, PowerShell ve komut satırı arabirimi (CLI) aracılığıyla arka uç havuzu üyeleri durumunu izleme yeteneği sağlar. Birleşik bir sistem durumu da bulabilirsiniz performans tanılama günlükleri aracılığıyla arka uç havuzları özeti. 

Arka uç sistem durumu raporu, arka uç örneklerine uygulama ağ geçidi sistem durumu araştırması çıktısını yansıtır. Yoklama zaman başarılı olur ve arka uç, trafiğin alabilir, sağlıklı kabul edilir. Aksi takdirde, kötü olarak değerlendirilir.

> [!IMPORTANT]
> Bir uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grubu (NSG) varsa, bağlantı noktası aralıkları 65503 65534 gelen trafik için uygulama ağ geçidi alt ağında açın. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bunlar Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikaları olmadan, bu ağ geçitlerinin müşterileri dahil dış varlıklar, bu uç noktalarında herhangi bir değişiklik başlatmak mümkün olmayacaktır.


### <a name="view-back-end-health-through-the-portal"></a>Portal aracılığıyla arka uç durumunu görüntüle

Portalda, arka uç sistem durumu otomatik olarak sağlanır. Mevcut bir application Gateway'i seçin **izleme** > **arka uç sistem durumu**. 

(Bu NIC, IP veya FQDN olup olmadığı) arka uç havuz içindeki her üyenin bu sayfada listelenir. Arka uç havuzu adı, bağlantı noktasını, arka uç HTTP ayarları adı ve sistem durumu gösterilir. Sistem durumu için geçerli değerler **sağlıklı**, **sağlıksız**, ve **bilinmeyen**.

> [!NOTE]
> Arka uç sistem durumu görürseniz **bilinmeyen**, arka uç erişimi bir NSG kuralı, bir kullanıcı tanımlı yol (UDR) veya sanal ağda özel DNS tarafından engellenmediğinden emin olun.

![Arka uç sistem durumu][10]

### <a name="view-back-end-health-through-powershell"></a>PowerShell aracılığıyla arka uç durumunu görüntüle

Aşağıdaki PowerShell kod kullanarak arka uç sistem durumu görüntülemek nasıl gösterir `Get-AzApplicationGatewayBackendHealth` cmdlet:

```powershell
Get-AzApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli"></a>Azure CLI aracılığıyla arka uç durumunu görüntüle

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Sonuçlar

Aşağıdaki kod parçacığı bir yanıt örneği gösterilmektedir:

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

Azure'da günlükleri farklı türde, yönetme ve sorun giderme application gateway'ler için kullanabilirsiniz. Bu günlüklerden bazılarına portaldan erişebilirsiniz. Tüm günlükler Azure Blob depolama alanından ayıklanır ve gibi farklı araçlarında görüntülenen [Azure İzleyici günlükleri](../azure-monitor/insights/azure-networking-analytics.md), Excel ve Power BI. Günlükleri aşağıdaki listeden farklı türleri hakkında daha fazla bilgi edinebilirsiniz:

* **Etkinlik günlüğü**: Kullanabileceğiniz [Azure etkinlik günlüklerini](../monitoring-and-diagnostics/insights-debugging-with-events.md) (eski adıyla işletimsel ve Denetim günlükleri), Azure aboneliğinizin ve durumlarını gönderilen tüm işlemleri görüntülemek için. Etkinlik günlüğü girişleri varsayılan olarak toplanır ve bunları Azure portalda görüntüleyebilirsiniz.
* **Erişim günlüğü**: Bu günlük, uygulama ağ geçidi erişim desenlerini görüntülemek ve önemli bilgileri analiz etmek için kullanabilirsiniz. Bu çağrı sahibinin IP, istenen URL, yanıt gecikme süresi, dönüş kodu ve bayt giriş ve çıkış içerir. Bir erişim günlüğü, her 300 saniyede toplanır. Bu günlük, uygulama ağ geçidi örneği başına tek bir kayıt içerir. Uygulama ağ geçidi örneğinin InstanceId özelliği tarafından tanımlanır.
* **Performans günlük**: Bu günlük, Application Gateway örneğinden nasıl performans gösterdiğini görüntülemek için kullanabilirsiniz. Bu günlük hizmet, toplam istekleri dahil olmak üzere her bir örnek için aktarım hızı bayt performans bilgileri yakalar, toplam istek sunulan, başarısız istek sayısı ve sağlıklı ve sağlıksız arka uç örnek sayısı. 60 saniyede bir performans günlük toplanır.
* **Güvenlik Duvarı günlük**: Bu günlük, web uygulaması güvenlik duvarı ile yapılandırılmış bir uygulama Ağ Geçidi algılama veya önleme modu aracılığıyla oturum isteklerini görmek için kullanabilirsiniz.

> [!NOTE]
> Günlükleri Azure Resource Manager dağıtım modelinde dağıtılan kaynaklar için kullanılabilir. Klasik dağıtım modelinde kaynakların günlükleri'ni kullanamazsınız. Daha iyi iki modelleri anlamak için bkz: [anlama Resource Manager dağıtımını ve klasik dağıtımı](../azure-resource-manager/resource-manager-deployment-model.md) makalesi.

Günlüklerinizi depolamak için kullanabileceğiniz üç seçenek vardır:

* **Depolama hesabı**: Günlükleri uzun bir süre için depolanır ve gerektiğinde gözden depolama hesapları en iyi günlükler için kullanılır.
* **Olay hub'ları**: Olay hub'ları, kaynaklarınız üzerinde uyarıları almak için diğer güvenlik bilgileri ve Olay yönetimi (SEIM) araçları ile tümleştirmeye yönelik mükemmel bir seçenektir ' dir.
* **Azure İzleyici günlüklerine**: Azure İzleyici günlüklerine en iyi şekilde kullanılır uygulamanızın genel gerçek zamanlı izleme veya eğilimlere bakmaya.

### <a name="enable-logging-through-powershell"></a>PowerShell üzerinden günlük kaydını etkinleştirme

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Erişim ve bu günlükleri kullanılabilir veri toplamaya başlamak için oturum performans etkinleştirmeniz gerekir. Günlüğe kaydetmeyi etkinleştirmek için aşağıdaki adımları kullanın:

1. Günlük verilerinin depolandığı depolama hesabınızın kaynak kimliğini not edin. Bu değer biçimindedir: /subscriptions/\<Subscriptionıd\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Storage/storageAccounts/\<depolama hesabı adı\>. Aboneliğinizdeki herhangi bir depolama hesabını kullanabilirsiniz. Bu bilgileri Azure portalda bulabilirsiniz.

    ![Portalı: depolama hesabı kaynak kimliği](./media/application-gateway-diagnostics/diagnostics1.png)

2. Uygulama ağ geçidinizin kaynak kimliği için hangi günlük kaydı etkin olduğunu unutmayın. Bu değer biçimindedir: /subscriptions/\<Subscriptionıd\>/resourceGroups/\<kaynak grubu adı\>/providers/Microsoft.Network/applicationGateways/\<uygulama ağ geçidi adı \>. Bu bilgileri portalda bulabilirsiniz.

    ![Portalı: application gateway kaynak kimliği](./media/application-gateway-diagnostics/diagnostics2.png)

3. Tanılama günlüğüne kaydetmeyi etkinleştirmek için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Etkinlik günlükleri ayrı bir depolama hesabı gerektirmez. Depolama alanının erişim ve performans günlüğü kaydı için kullanılması durumunda hizmet ücreti tahsil edilir.

### <a name="enable-logging-through-the-azure-portal"></a>Azure portaldan günlüğe kaydetmeyi etkinleştirme

1. Azure portalında, kaynağınızı bulun ve seçin **tanılama ayarları**.

   Application Gateway için üç günlükleri kullanılabilir:

   * Erişim günlüğü
   * Performans günlüğü
   * Güvenlik Duvarı günlüğü

2. Veri toplamaya başlamak için seçin **tanılamayı Aç**.

   ![Tanılamayı açma][1]

3. **Tanılama ayarları** sayfasında tanılama günlükleriyle ilgili ayarlar bulunur. Bu örnekte, Log Analytics, günlükleri depolar. Tanılama günlüklerini kaydetmek için Event Hubs'ı veya depolama hesabını da kullanabilirsiniz.

   ![Yapılandırma işlemi başlatılıyor][2]

5. Ayarları için bir ad yazın, ayarları onaylayın ve seçin **Kaydet**.

### <a name="activity-log"></a>Etkinlik günlüğü

Azure etkinlik günlüğü varsayılan olarak oluşturur. Günlükleri, olay günlüklerini Azure Mağazası'nda 90 gün boyunca korunur. Bu günlükler hakkında daha fazla bilgi edinmek [görüntülemek, olayları ve etkinlik günlüğüne](../monitoring-and-diagnostics/insights-debugging-with-events.md) makalesi.

### <a name="access-log"></a>Erişim günlüğü

Yalnızca, önceki adımlarda açıklandığı her uygulama ağ geçidi örneğinde etkinleştirdiyseniz erişim günlüğü oluşturulur. Veri günlük kaydı etkinleştirildiğinde, belirtilen depolama hesabında depolanır. Application Gateway her erişim, aşağıdaki örnekte gösterildiği gibi JSON biçiminde kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|instanceId     | Hizmet isteği uygulama ağ geçidi örneği.        |
|Clientıp     | İsteğin kaynak IP.        |
|clientPort     | İstek için kaynak bağlantı noktası.       |
|HttpMethod     | İstek tarafından kullanılan HTTP yöntemi.       |
|requestUri     | Alınan istek URI'si.        |
|RequestQuery     | **Sunucu yönlendirilen**: İsteğin gönderildiği arka uç havuzu örneği.</br>**X-AzureApplicationGateway-günlük-ID**: İstek için kullanılan bağıntı kimliği. Arka uç sunucularda trafiği sorunları gidermek için kullanılabilir. </br>**SUNUCU DURUMU**: Application Gateway, arka uçtan alınan HTTP yanıt kodu.       |
|UserAgent     | HTTP isteği üst bilgisinden kullanıcı aracısı.        |
|Httpstatus'a     | Uygulama ağ geçidinden istemciye döndürülen HTTP durum kodu.       |
|httpVersion     | İstek HTTP sürümü.        |
|ReceivedBytes     | Paket, alınan bayt cinsinden boyutu.        |
|SentBytes| Paket, gönderilen bayt cinsinden boyutu.|
|timeTaken| Uzunluğu, bir isteğin işlenmesi için ve yanıtının gönderilmesi için geçen süre (milisaniye cinsinden). Bu, uygulama ağ geçidi bir HTTP isteği işlemi tamamlanmadan gönderdiğinizde yanıt süresi'nin ilk baytı aldığında zaman aralığından olarak hesaplanır. Time-Taken alanı genellikle istek ve yanıt paketlerin ağ üzerinden gezilerinizde zaman içerdiğine dikkat edin önemlidir. |
|Express'e| Arka uç havuzları ile iletişim SSL kullanılıp. Geçerli değerler, açma ve kapatma olmalı.|
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

Önceki adımlarda açıklandığı her uygulama ağ geçidi örneğinde yalnızca etkinleştirdiyseniz, performans günlüğü oluşturulur. Veri günlük kaydı etkinleştirildiğinde, belirtilen depolama hesabında depolanır. Performans günlüğü verilerini 1 dakikalık aralıklar olarak oluşturulur. Aşağıdaki veriler günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|instanceId     |  Uygulama ağ geçidi örneği performans verileri oluşturulur. Çok örnekli application gateway için örnek başına bir satır var.        |
|HealthyHostCount     | Arka uç havuzundaki sağlıklı konakların sayısı.        |
|unHealthyHostCount     | Arka uç havuzunda iyi durumda olmayan konak sayısı.        |
|RequestCount     | Hizmet isteklerinin sayısı.        |
|gecikme | Hizmet istekleri arka uç örneğinden gelen isteklerin ortalama gecikme süresi (milisaniye cinsinden). |
|failedRequestCount| Başarısız istek sayısı.|
|throughput| Ortalama aktarım hızını saniye başına bayt cinsinden son günlüğü itibaren.|

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
> Gecikme süresi, ilk baytı HTTP isteğinin HTTP yanıtın son baytını gönderildiğinde zaman alındığında saatinden hesaplanır. Uygulama ağ geçidi işleme süresi ve ağ maliyeti arka uç yanı sıra, arka uç, isteği işlemek için geçen süreyi toplamıdır.

### <a name="firewall-log"></a>Güvenlik Duvarı günlüğü

Önceki adımlarda açıklandığı her uygulama ağ geçidi için yalnızca etkinleştirdiyseniz, güvenlik duvarı günlüğü oluşturulur. Bu günlük, ayrıca bir application gateway üzerinde web uygulaması güvenlik duvarı yapılandırıldığını gerektirir. Veri günlük kaydı etkinleştirildiğinde, belirtilen depolama hesabında depolanır. Aşağıdaki veriler günlüğe kaydedilir:


|Değer  |Açıklama  |
|---------|---------|
|instanceId     | Uygulama ağ geçidi örneği için hangi güvenlik duvarı veri oluşturuluyor. Çok örnekli application gateway için örnek başına bir satır var.         |
|Clientıp     |   İsteğin kaynak IP.      |
|clientPort     |  İstek için kaynak bağlantı noktası.       |
|requestUri     | Alınan istek URL'si.       |
|ruleSetType     | Kural türü ayarlayın. Kullanılabilir OWASP değerdir.        |
|ruleSetVersion     | Kural kullanılan sürümünü ayarlama. Değerleri 2.2.9 ve 3. 0'ı kullanılabilir.     |
|RuleId     | Tetikleyici olayın kural kimliği.        |
|message     | Tetikleyici olay kullanıcı dostu iletisi. Ayrıntılar bölümünde daha ayrıntılı bilgi sağlanır.        |
|eylem     |  İstekte gerçekleştirilen eylem. Engellenen ve izin verilen değerleri kullanılabilir.      |
|site     | Günlük oluşturulduğu site. Genel kurallar olduğundan şu anda yalnızca genel listelenir.|
|ayrıntılar     | Olay Ayrıntıları.        |
|details.Message     | Kural açıklaması.        |
|details.data     | Belirli veri kural eşleşen isteğinde bulundu.         |
|details.File     | Kural bulunan yapılandırma dosyası.        |
|details.Line     | Olayı tetikleyen yapılandırma dosyasındaki satır numarası.       |

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

### <a name="view-and-analyze-the-activity-log"></a>Etkinlik günlüğünü görüntüleme ve analiz etme

Aşağıdaki yöntemlerden birini kullanarak etkinlik günlüğü verilerini görüntüleyebilir ve analiz edebilirsiniz:

* **Azure araçlarını**: Azure PowerShell, Azure CLI, Azure REST API'si veya Azure Portalı aracılığıyla etkinlik günlüğünden bilgi alın. Her yöntemle ilgili ayrıntılı adımlar [Kaynak Yöneticisi etkinlik işlemleri](../azure-resource-manager/resource-group-audit.md) makalesinde ayrıntılı bir şekilde anlatılmıştır.
* **Power BI**: Henüz yoksa bir [Power BI](https://powerbi.microsoft.com/pricing) hesabı deneyebilirsiniz, ücretsiz. [Power BI için Azure Activity Logs içerik paketi](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) ile verilerinizi önceden yapılandırılmış panoları olduğu gibi veya değiştirerek kullanarak analiz edebilirsiniz.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Erişim, performans ve güvenlik duvarı günlükleri görüntüleme ve çözümleme

[Azure İzleyici günlüklerine](../azure-monitor/insights/azure-networking-analytics.md) sayacını ve olay günlüğü dosyaları Blob Depolama hesabınızı sık toplayabilirsiniz. Günlüklerinizi analiz etmek için görselleştirmelere ve güçlü arama özelliklerine sahiptir.

Dilerseniz depolama hesabınıza bağlanabilir ve JSON erişim günlüklerini ve performans günlüklerini alabilirsiniz. İndirdiğiniz JSON dosyalarını CSV biçimine dönüştürebilir ve Excel, Power BI veya diğer veri görselleştirme araçlarında görüntüleyebilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.
> 
> 

#### <a name="analyzing-access-logs-through-goaccess"></a>GoAccess aracılığıyla erişim günlüklerini analiz etme

Yüklenen ve popüler çalıştırılan bir Resource Manager şablonu yayımladık [GoAccess](https://goaccess.io/) Çözümleyicisi uygulama ağ geçidi günlüklerine erişim için oturum açın. GoAccess benzersiz ziyaretçiler, istenen dosyaları, konaklar, işletim sistemleri, tarayıcılar, HTTP durum kodları ve daha fazlası gibi değerli HTTP trafiğini istatistikler sağlar. Daha fazla ayrıntı için lütfen bkz [GitHub Resource Manager şablonu bir klasörde Benioku dosyası](https://aka.ms/appgwgoaccessreadme).

## <a name="metrics"></a>Ölçümler

Ölçümler, performans sayaçları portalda görüntüleyebileceğiniz belirli Azure kaynakları için bir özelliğidir. Application Gateway için şu ölçümler kullanılabilir:

- **Geçerli bağlantılar**
- **Başarısız istekler**
- **Konak sayısı**

   Filtreleyebilirsiniz bir arka uç havuzu olarak belirli arka uç havuzunda iyi durumda ve uygun olmayan konakları göstermek için.


- **Yanıt durumu**

   Yanıt durum kodu dağıtımı yanıtları 2xx, 3xx, 4xx ve 5xx kategorileri göstermek için daha fazla kategorilere ayrılabilir.

- **Aktarım hızı**
- **Toplam istek**
- **İyi durumda olmayan konak sayısı**

   Filtreleyebilirsiniz bir arka uç havuzu olarak belirli arka uç havuzunda iyi durumda ve uygun olmayan konakları göstermek için.

Bir uygulama ağ geçidi için altında Gözat **izleme** seçin **ölçümleri**. Kullanılabilir değerleri görüntülemek için **ÖLÇÜM** açılan listesini seçin.

Aşağıdaki görüntüde üç ölçümlerle son 30 dakika boyunca görüntülenen bir örneğe bakın:

[![](media/application-gateway-diagnostics/figure5.png "Ölçüm görüntüle")](media/application-gateway-diagnostics/figure5-lb.png#lightbox)

Ölçümleri geçerli listesini görmek için bkz: [Azure İzleyici ile desteklenen ölçümler](../azure-monitor/platform/metrics-supported.md).

### <a name="alert-rules"></a>Uyarı kuralları

Uyarı kuralları bir kaynağına ait ölçümleri temel başlayabilirsiniz. Örneğin, bir uyarı bir Web kancası çağırma veya bir yönetici, uygulama ağ geçidinin aktarım hızı belirli bir süre için yukarıda, aşağıda veya bir eşiği ise, e-posta.

Aşağıdaki örnek, bir eşik ihlallerini aktarım hızı sonra yönetici e-posta gönderen bir uyarı kuralı oluşturma işleminde size yol gösterir:

1. seçin **ölçüm uyarısı Ekle** açmak için **Kuralı Ekle** sayfası. Bu sayfayı ölçümleri sayfasından da ulaşabilirsiniz.

   !["Ölçüm uyarısı Ekle" düğmesi][6]

2. Üzerinde **Kuralı Ekle** sayfasında adı, koşul, doldurun ve bölüm bildirim ve seçin **Tamam**.

   * İçinde **koşul** Seçici, dört değerden birini seçin: **Büyüktür**, **büyüktür veya eşittir**, **küçüktür**, veya **ya da eşit**.

   * İçinde **süresi** Seçici, bir dönem için altı saat beş dakika seçin.

   * Seçerseniz **e-posta sahipleri, Katkıda Bulunanlar ve okuyucular**, bu kaynağa erişimi olan kullanıcılar göre e-posta dinamik olabilir. Aksi takdirde, kullanıcıların virgülle ayrılmış bir listesini sağlayabilirsiniz **ek yönetici email(s)** kutusu.

   ![Kural Sayfası Ekle][7]

Eşiği ihlal edilirse, aşağıdaki görüntüde gösterilene benzer bir e-posta geldiğinde:

![İhlal edilen eşiği için e-posta][8]

Ölçüm uyarısı oluşturduktan sonra uyarıların bir listesi görüntülenir. Bu, uyarı kurallarının tümünü genel bir bakış sağlar.

![Uyarılar ve kuralları listesi][9]

Uyarı bildirimleri hakkında daha fazla bilgi için bkz: [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Web kancaları ve nasıl uyarılarla kullanabilmek için daha iyi anlamak için ziyaret [Azure bir ölçüm uyarısında Web kancası yapılandırma](../azure-monitor/platform/alerts-webhooks.md).

## <a name="next-steps"></a>Sonraki adımlar

* Sayaç ve olay günlüklerini kullanarak görselleştirme [Azure İzleyicisi](../azure-monitor/insights/azure-networking-analytics.md).
* [Azure etkinlik günlüğü Power BI ile görselleştirin](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog gönderisi.
* [Görüntüleme ve Power BI ve diğer Azure etkinlik günlüklerini analiz](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog gönderisi.

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
