---
title: Bir Web kancası ile bir Azure Otomasyonu runbook'u başlatma
description: Bir HTTP çağrısından Azure Otomasyon runbook'u başlatmak bir istemci izin veren bir Web kancası.  Bu makalede, bir Web kancası oluşturma ve bir runbook'u başlatmak için bir çağrı açıklanmaktadır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e047dffa86915b0cd6e8829ea27e0335e7f88cb2
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757165"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Bir Web kancası ile bir Azure Otomasyonu runbook'u başlatma

A *Web kancası* , belirli bir runbook, tek bir HTTP isteği aracılığıyla Azure Otomasyonu'nda başlatmanıza olanak verir. Bu, Visual Studio Team Services, GitHub, Azure Log Analytics veya Azure Otomasyon API'sini kullanarak tam bir çözüm uygulama olmadan runbook'ları başlatmak için özel uygulamalar gibi dış hizmetler sağlar.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Bir runbook'u başlatma diğer yöntemleri için Web kancası karşılaştırabilirsiniz [Azure Otomasyonu runbook başlatma](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Bir Web kancası ayrıntıları

Aşağıdaki tabloda, bir Web kancası için yapılandırmanız gerekir özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Ad |Bu istemciye gösterilmez bu yana bir Web kancası için istediğiniz herhangi bir ad sağlayabilirsiniz. Yalnızca sizin için Azure Automation runbook tanımlamak için kullanılır. <br> En iyi uygulama, Web kancası kullandığı istemcisiyle ilgili bir ad vermeniz gerekir. |
| URL'si |Web kancası URL'si için Web kancası bağlı runbook'u başlatmak için bir HTTP POST ile bir istemci çağıran benzersiz adrestir. Web kancası oluşturduğunuzda otomatik olarak oluşturulur. Özel bir URL belirtemezsiniz. <br> <br> URL runbook'un başka kimlik doğrulama ile bir üçüncü taraf sistemi tarafından çağrılan izin veren bir güvenlik belirteci içeriyor. Bu nedenle, bir parola gibi değerlendirilmelidir. Güvenlik nedeniyle, Web kancası oluşturulduğunda Azure portalında yalnızca URL görüntüleyebilirsiniz. Gelecekte kullanım için güvenli bir konumda URL unutmayın. |
| Son kullanma tarihi |Bir sertifika gibi her Web kancası aynı zamanda bu artık kullanılabilir bir sona erme tarihi vardır. Web kancası oluşturulduktan sonra bu süre sonu tarihi değiştirilebilir. |
| Etkin |Bir Web kancası oluşturulduğunda varsayılan olarak etkindir. Devre dışı olarak ayarlarsanız, hiçbir istemci kullanabilirsiniz. Ayarlayabileceğiniz **etkin** Web kancası veya dilediğiniz zaman bir kez oluşturduğunuzda özelliği oluşturulur. |

### <a name="parameters"></a>Parametreler

Bir Web kancası runbook o Web kancası tarafından çalıştırıldığında kullanılan runbook parametreleri için değerleri tanımlayabilirsiniz. Web kancası runbook'un zorunlu parametreler için değerler içermelidir ve isteğe bağlı parametreler için değerler içerebilir. Bir Web kancası için yapılandırılmış bir parametre değeri, Web kancası oluşturduktan sonra bile değiştirilebilir. Tek bir runbook bağlı birden çok Web kancalarını her farklı parametre değerlerini kullanabilirsiniz.

Bir istemciyi bir Web kancası kullanarak bir runbook'u başlattığında, Web kancası içinde tanımlanan parametre değerlerini geçersiz kılamaz. İstemciden veri almak için runbook adlı tek bir parametre kabul edebilir **$WebhookData** [object] türündeki POST isteğinde istemci içeren verileri içerir.

![Webhookdata özellikleri](media/automation-webhooks/webhook-data-properties.png)

**$WebhookData** nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| WebhookName |Web kancası adı. |
| RequestHeader |Gelen posta istek üstbilgileri içeren karma tablo. |
| RequestBody |Gelen posta istek gövdesi. Bu herhangi bir dize gibi JSON, XML, biçimlendirme korur veya kodlanmış form verileri. Runbook için beklenen veri biçimi ile çalışmak için yazılmış olmalıdır. |

Web kancası desteklemek için gerekli olan yapılandırma yok **$WebhookData** parametre ve runbook kabul etmek için gerekmez. Runbook parametresi tanımlamıyorsa, daha sonra istemci tarafından gönderilen istek, herhangi bir ayrıntıyı yoksayılır.

Web kancası oluşturduğunuzda, bir değer için $WebhookData belirtirseniz, Web kancası istemci POST isteğini verilerle runbook başlatıldığında, istemci istek gövdesinde herhangi bir veri içermez olsa bile değeri geçersiz kılınır olduğunu. Bir Web kancası dışında bir yöntem kullanılarak $WebhookData sahip bir runbook başlatırsanız, runbook tarafından tanınan $Webhookdata için bir değer sağlayabilir. Bu değer bir nesne ile aynı olmalıdır [özellikleri](#details-of-a-webhook) $Webhookdata olarak böylece Web kancası tarafından geçirilen gerçek WebhookData çalıştığı gibi runbook düzgün ile çalışabilirsiniz.

Aşağıdaki runbook Azure portalından başladılar ve test etmek için bazı örnek WebhookData WebhookData bir nesne olduğundan geçirmek istediğiniz, örneğin, bu arabirimdeki JSON olarak geçirilmesi gerekir.

![UI WebhookData parametresinden](media/automation-webhooks/WebhookData-parameter-from-UI.png)

WebhookData parametresi için aşağıdaki özellikler varsa aşağıdaki runbook için:

* WebhookName: *MyWebhook*
* RequestBody: *[{'ResourceGroup': 'myResourceGroup', 'Name': 'vm01'}, {'ResourceGroup': 'myResourceGroup', 'Name': 'vm02'}]*

Daha sonra kullanıcı arabiriminde WebhookData parametresi için aşağıdaki JSON değerini geçirin. Satır ile aşağıdaki örneğe verir ve yeni satır karakterlerini eşleşen bir Web kancası geçirilen biçimi.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![Kullanıcı Arabirimi başlangıç WebhookData parametresi](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Tüm giriş parametrelerinin değerlerini runbook işi ile günlüğe kaydedilir. Herhangi bir deyişle Web kancası isteği istemci tarafından sağlanan giriş günlüğe kaydedilen ve Otomasyon iş erişimi olan herkes için kullanılabilir olacaktır.  Bu nedenle, Web kancası çağrılarında hassas bilgiler dahil olmak üzere hakkında dikkatli olmanız gerekir.

## <a name="security"></a>Güvenlik

Bir Web kancası güvenlik çağrılmasına izin veren bir güvenlik belirteci içeriyor URL'sini gizlilik kullanır. Azure otomasyonu için doğru URL'yi yapılan sürece herhangi bir kimlik doğrulaması isteği gerçekleştirmez. Bu nedenle, Web kancalarını isteği doğrulanırken bir alternatif şekillerini kullanmadan son derece hassas işlevleri gerçekleştirmek runbook'lar için kullanılmamalıdır.

Bunu denetleyerek bir Web kancası tarafından çağrıldı belirlemek için runbook mantık içerebilir **WebhookName** $WebhookData parametresinin özelliği sağlar. Runbook daha fazla doğrulama belirli bilgileri arayarak gerçekleştirebilir **RequestHeader** veya **RequestBody** özellikleri.

Bir Web kancası isteği alındığında bir dış koşulun bazı doğrulama gerçekleştirmek runbook başka bir stratejisi olmalıdır. Örneğin, bir GitHub deposuna yeni bir yürütme olduğunda GitHub tarafından çağrılan bir runbook'u göz önünde bulundurun. Runbook devam etmeden önce yeni bir yürütme oluştu olduğunu doğrulamak için GitHub bağlanabilir.

## <a name="creating-a-webhook"></a>Bir Web kancası oluşturma

Azure Portalı'nda runbook'un bağlı yeni bir Web kancası oluşturmak için aşağıdaki yordamı kullanın.

1. Gelen **Runbook'lar sayfa** Azure portalında kendi ayrıntı sayfası görüntülemek için Web kancası başlayan ' a tıklayın.
1. Tıklatın **Web kancası** açmak için sayfanın üstündeki **ekleme Web kancası** sayfası.
1. Tıklatın **yeni Web kancası oluşturma** açmak için **Oluştur Web kancası sayfası**.
1. Belirtin bir **adı**, **sona erme tarihi** Web kancası ve olup etkinleştirilmelidir. Bkz: [bir Web kancası ayrıntılarını](#details-of-a-webhook) daha fazla bilgi için bu özellikleri.
1. Kopyala simgesine tıklayın ve Web kancası URL'yi kopyalamak için Ctrl + C tuşlarına basın. Ardından güvenli bir yerde kaydedin. **Web kancası oluşturulduktan sonra URL'yi yeniden alınamıyor.**

   ![Web kancası URL'si](media/automation-webhooks/copy-webhook-url.png)

1. Tıklatın **parametreleri** runbook parametreleri için değerler sağlamak için. Runbook zorunlu parametreler varsa, daha sonra değerleri belirtilmediği sürece Web kancası oluşturmak mümkün değildir.
1. Tıklatın **oluşturma** Web kancası oluşturulamadı.

## <a name="using-a-webhook"></a>Bir Web kancası kullanma

Bir Web kancası oluşturulduktan sonra kullanmak için istemci uygulamanız bir Web kancası URL'si ile HTTP POST kesmeniz gerekir. Web kancası söz dizimi aşağıdaki biçimdedir:

```http
http://<Webhook Server>/token?=<Token Value>
```

İstemci aşağıdaki dönüş kodları birini POST isteğini alır.

| Kod | Metin | Açıklama |
|:--- |:--- |:--- |
| 202 |Kabul Edildi |İstek kabul edildi ve runbook başarıyla sıraya alındı. |
| 400 |Hatalı İstek |İstek aşağıdaki nedenlerden birinden dolayı kabul edilmedi: <ul> <li>Web kancası süresi doldu.</li> <li>Web kancası devre dışı bırakılır.</li> <li>URL'deki belirteci geçersiz.</li>  </ul> |
| 404 |Bulunamadı |İstek aşağıdaki nedenlerden birinden dolayı kabul edilmedi: <ul> <li>Web kancası bulunamadı.</li> <li>Runbook bulunamadı.</li> <li>Hesap bulunamadı.</li>  </ul> |
| 500 |İç Sunucu Hatası |URL geçerli, ancak bir hata oluştu. Lütfen isteği yeniden gönderin. |

İstek başarılı olduğunu varsayarak, Web kancası yanıt gibi JSON biçiminde iş kimliğini içerir. Tek iş kimliği yer alır ancak JSON biçimi için gelecekteki olası geliştirmeleri sağlar.

```json
{"JobIds":["<JobId>"]}
```

İstemci, runbook işi tamamlandığında veya Web kancası gelen tamamlanma durumu belirlenemiyor. İş kimliği gibi başka bir yöntemle kullanarak bu bilgileri belirleyebilirsiniz [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) veya [Azure Otomasyon API](/rest/api/automation/job).

## <a name="sample-runbook"></a>Örnek runbook

Aşağıdaki örnek runbook'u kabul Web kancası verileri kabul eder ve istek gövdesinde belirtilen sanal makineleri başlatır. Bu runbook Otomasyon hesabınızda altında test etmek için **Runbook'lar**, tıklatın **+ runbook Ekle**. Bir runbook oluşturmak nasıl bilmiyorsanız bkz [runbook oluşturma](automation-quickstart-create-runbook.md).

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Retrieve VM's from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName" 

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal." 
            Add-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzureRMVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="test-the-example"></a>Örneği test

Aşağıdaki örnek, bir Web kancası ile bir runbook'u başlatmak için Windows PowerShell'i kullanır. Bir HTTP isteği yapabilen herhangi bir dil bir Web kancası kullanabilirsiniz; Windows PowerShell kullanılan burada bir örnek olarak.

Runbook istek gövdesinde JSON biçimli sanal makinelerin listesini bekliyor.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms

$response = Invoke-RestMethod -Method Post -Uri $uri -Body $body
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

Aşağıdaki örnek, bir runbook'ta kullanılabilir istek gövdesi gösterir **RequestBody** özelliği **WebhookData**. İstek gövdesinde eklenmiştir biçimi olduğu için bu JSON olarak biçimlendirilir.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

Aşağıdaki resimde, Windows PowerShell ve sonuçta elde edilen yanıt gönderilen istek gösterilir. İş kimliği yanıttan ayıklanmış ve bir dizeye dönüştürülür.

![Web kancası düğmesi](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Uyarıları'üzerinde eyleme geçmek için Azure Otomasyonu kullanmayı öğrenmek için bkz: [bir Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanmak](automation-create-alert-triggered-runbook.md).
