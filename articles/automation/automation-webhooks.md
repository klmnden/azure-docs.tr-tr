---
title: Bir Web kancası ile bir Azure Otomasyonu runbook'u başlatma
description: HTTP çağrısı Azure Automation'da bir runbook başlatmak bir istemci sağlayan bir Web kancası.  Bu makalede bir Web kancası oluşturma ve bir runbook başlatmak için bir çağrı nasıl açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 153bb0304102906f7be64ae55dd0e0f6bb8d7146
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61305041"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Bir Web kancası ile bir Azure Otomasyonu runbook'u başlatma

A *Web kancası* , tek bir HTTP isteği ile Azure Otomasyonu'nda belirli bir runbook başlatmanızı sağlar. Bu, Azure DevOps Services, GitHub, Azure İzleyici günlüklerine veya Azure Otomasyonu API'sini kullanarak tam bir çözüm uygulamadan runbook'ları başlatmak için özel uygulamalar gibi dış hizmetlerle sağlar.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Bir runbook başlatma diğer yöntemleri için Web kancaları karşılaştırabilirsiniz [Azure Automation'da bir runbook başlatma](automation-starting-a-runbook.md)

> [!NOTE]
> Python runbook'u başlatmak için bir Web kancası kullanma desteklenmiyor.

## <a name="details-of-a-webhook"></a>Bir Web kancası ayrıntıları

Aşağıdaki tabloda, bir Web kancası için yapılandırmanız gereken özellikleri tanımlar.

| Özellik | Açıklama |
|:--- |:--- |
| Ad |Bu istemciye kullanıma bu yana bir Web kancası için istediğiniz herhangi bir ad sağlayabilirsiniz. Yalnızca sizin için Azure Otomasyonu'nda runbook tanımlamak için kullanılır. <br> En iyi uygulama, Web kancası kullanan istemcisiyle ilgili bir ad vermesi gerekir. |
| URL'si |Web kancası URL'si, Web kancası'na bağlı bir runbook başlatmak için bir HTTP POST ile bir istemci çağrıları benzersiz adresidir. Web kancasını oluşturduğunuzda otomatik olarak oluşturulur. Özel bir URL belirtemezsiniz. <br> <br> URL, başka bir kimlik doğrulaması ile üçüncü taraf sistemleri tarafından çağrılacak runbook izin veren bir güvenlik belirteci içeriyor. Bu nedenle, bir parola gibi düşünülmelidir. Güvenlik nedeniyle, Web kancası oluşturulduğunda Azure portalında yalnızca URL'yi görüntüleyebilirsiniz. Gelecekte kullanım için güvenli bir konumda URL'yi not alın. |
| Son kullanma tarihi |Bir sertifikanın gibi her Web kancası aynı zamanda artık kullanılabilir bir sona erme tarihi vardır. Bu süre sonu tarihi, Web kancasının süresi dolmuş değil sürece Web kancası oluşturulduktan sonra değiştirilebilir. |
| Enabled |Bir Web kancası, oluşturulduğunda varsayılan olarak etkindir. Devre dışı olarak ayarlarsanız, hiçbir istemci bunu kullanabilirsiniz. Ayarlayabileceğiniz **etkin** özelliği, Web kancası veya dilediğiniz zaman bir kez oluşturduğunuzda oluşturulur. |

### <a name="parameters"></a>Parametreler

Bir Web kancası, Web kancası tarafından runbook başlatılırken kullanılan runbook parametreleri için değerleri tanımlayabilirsiniz. Web kancası runbook'un zorunlu parametreler için değerler içermelidir ve isteğe bağlı parametreler için değerler içerebilir. Bir Web kancası için yapılandırılmış bir parametre değeri, Web kancası oluşturduktan sonra bile değiştirilebilir. Birden çok Web kancaları için tek bir runbook bağlı her farklı parametre değerlerini kullanabilirsiniz.

Bir istemciyi bir Web kancası kullanarak bir runbook'u başlattığında, Web kancasının tanımlanan parametre değerlerini geçersiz kılamazsınız. İstemciden veri almak, runbook adı verilen tek bir parametre kabul edebilir **$WebhookData**. Bu parametre istemci POST isteğinde içeren verileri içeren bir tür [object] içindir.

![Webhookdata özellikleri](media/automation-webhooks/webhook-data-properties.png)

**$WebhookData** nesne, aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| WebhookName |Web kancası adı. |
| RequestHeader |Gelen bir POST isteği üstbilgileri içeren karma tablo. |
| Includesearchresults: true |Gelen bir POST isteğinin gövdesi. Bu herhangi bir dize gibi JSON, XML, biçimlendirme korur veya form kodlanmış verileri. Runbook için beklenen veri biçimi ile çalışmak için yazılmış olmalıdır. |

Web kancasının desteklemek için gereken yapılandırma yok **$WebhookData** parametresi ve runbook değil kabul etmek için gereklidir. Runbook parametre tanımlamıyorsa istemci tarafından gönderilen isteğin herhangi bir ayrıntı yok sayıldı.

Web kancasını oluşturduğunuzda bir değer için $WebhookData belirtirseniz, Web kancası istemci POST isteği verilerle runbook başlatıldığında bile istemci istek gövdesinde herhangi bir veri içermez değeri geçersiz kılınmasını. Bir Web kancası dışında bir yöntem kullanılarak $WebhookData olan bir runbook başlatırsanız, runbook'u tarafından tanınan $Webhookdata için bir değer sağlayabilirsiniz. Bu değer bir nesne ile aynı olmalıdır [özellikleri](#details-of-a-webhook) $Webhookdata olarak alacağı bir Web kancası tarafından geçirilen gerçek WebhookData çalıştığı runbook düzgün ile çalışabilirsiniz.

Aşağıdaki runbook Azure portalından başlıyor ve WebhookData nesneyi olduğundan bazı örnek WebhookData test, geçirmek istediğiniz, örneğin, bu kullanıcı arabiriminde JSON olarak geçirilmelidir.

![WebhookData parametresinden kullanıcı Arabirimi](media/automation-webhooks/WebhookData-parameter-from-UI.png)

WebhookData parametresi için aşağıdaki özellikler varsa aşağıdaki runbook için:

* WebhookName: *MyWebhook*
* Includesearchresults: true: *[{'ResourceGroup': 'myResourceGroup', 'Name': 'vm01'}, {'ResourceGroup': 'myResourceGroup', 'Name': 'vm02'}]*

Ardından kullanıcı arabiriminde WebhookData parametresi için aşağıdaki JSON değeri geçirin. Aşağıdaki örnek bir satır başı ile verir ve yeni satır karakterleri Web kancasından geçirilen biçim eşleşir.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![UI WebhookData parametreden Başlat](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Tüm girdi parametrelerinin değerlerini, runbook işi ile günlüğe kaydedilir. Diğer herhangi bir deyişle Web kancası isteği istemci tarafından sağlanan giriş, oturum ve Otomasyon iş erişimi olan herkes tarafından kullanılabilir olacaktır.  Bu nedenle, Web kancası çağrıları hassas bilgiler dahil olmak üzere hakkında dikkatli olmanız gerekir.

## <a name="security"></a>Güvenlik

Bir Web kancası güvenliğini çağrılmasına izin veren bir güvenlik belirteci içeren URL'sini gizlilik kullanır. Azure otomasyonu için doğru URL yapılır sürece herhangi bir kimlik doğrulaması isteği gerçekleştirmez. Bu nedenle, Web kancalarını kullanarak istek doğrulama alternatif bir yol olmadan son derece hassas işlevleri gerçekleştiren runbook'lar için kullanılmamalıdır.

Bunu kontrol ederek bir Web kancası tarafından çağrıldı belirlemek için runbook mantık içerebilir **WebhookName** $WebhookData parametresinin özelliği sağlar. Runbook daha fazla doğrulama belirli bilgileri arayarak gerçekleştirebilir **RequestHeader** veya **ıncludesearchresults: true** özellikleri.

Başka bir strateji, bir Web kancası isteği alındığında dış bir koşulun bazı doğrulama gerçekleştirmek runbook olmasını sağlamaktır. Örneğin, bir GitHub deposu için yeni bir işleme yapıldığında GitHub tarafından çağrılan bir runbook düşünün. Runbook devam etmeden önce yeni bir işleme oluşmadı doğrulamak için GitHub bağlanabilir.

## <a name="creating-a-webhook"></a>Bir Web kancası oluşturuluyor

Azure Portalı'nda bir runbook bağlı yeni bir Web kancası oluşturmak için aşağıdaki yordamı kullanın.

1. Gelen **runbook'ları sayfa** Azure portalında, kendi ayrıntı sayfası görüntülemek için Web kancası başlatan runbook'a tıklayın. Runbook olun **durumu** olduğu **yayımlanan**.
2. Tıklayın **Web kancası** açmak için sayfanın üst kısmındaki **Web kancası Ekle** sayfası.
3. Tıklayın **yeni Web kancası oluşturma** açmak için **oluşturma Web kancası sayfası**.
4. Belirtin bir **adı**, **sona erme tarihi** Web kancası ve olup etkinleştirilmelidir. Bkz: [bir Web kancası ayrıntılarını](#details-of-a-webhook) daha fazla bilgi için bu özellikleri.
5. Kopyala simgesine tıklayın ve Web kancası URL'sini kopyalamak için Ctrl + C tuşlarına basın. Ardından güvenli bir yerde kaydı. **Web kancası oluşturulduktan sonra URL'yi yeniden alınamıyor.**

   ![Web kancası URL'si](media/automation-webhooks/copy-webhook-url.png)

1. Tıklayın **parametreleri** runbook parametreleri için değerler sağlamak için. Runbook zorunlu parametrelere sahipse, sonra değerleri belirtilmediği sürece, Web kancası oluşturmanız mümkün değildir.
1. Tıklayın **Oluştur** Web kancası oluşturmak için.

## <a name="using-a-webhook"></a>Bir Web kancası kullanma

İstemci uygulamanız bir Web kancası oluşturulduktan sonra kullanmak için Web kancası URL'si ile bir HTTP POST yayımlamanız gerekir. Web kancası söz dizimi aşağıdaki biçimdedir:

```http
http://<Webhook Server>/token?=<Token Value>
```

İstemci, aşağıdaki dönüş kodları POST isteği alır.

| Kod | Text | Açıklama |
|:--- |:--- |:--- |
| 202 |Kabul Edildi |İstek kabul edildi ve runbook başarıyla kuyruğa alındı. |
| 400 |Bozuk İstek |İstek aşağıdaki nedenlerden biri için kabul edilmedi: <ul> <li>Web kancasının süresi doldu.</li> <li>Web kancası devre dışı bırakıldı.</li> <li>URL'deki belirteci geçersiz.</li>  </ul> |
| 404 |Bulunamadı |İstek aşağıdaki nedenlerden biri için kabul edilmedi: <ul> <li>Web kancası bulunamadı.</li> <li>Runbook bulunamadı.</li> <li>Hesap bulunamadı.</li>  </ul> |
| 500 |İç Sunucu Hatası |URL geçerli, ancak bir hata oluştu. Lütfen isteği yeniden gönderin. |

İsteğin başarılı olup olmadığını varsayarsak, Web kancası yanıtı aşağıdaki gibi iş kimliği JSON biçiminde içerir. Bir tek iş kimliği içerecek ancak JSON biçimi için olası gelecekteki iyileştirmeler sağlar.

```json
{"JobIds":["<JobId>"]}
```

İstemci, runbook işi tamamlandığında veya Web kancası'nden tamamlanma durumunu belirleyemiyor. İş kimliği gibi başka bir yöntemle kullanarak bu bilgileri belirleyebilirsiniz [Windows PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjob) veya [Azure Automation API](/rest/api/automation/job).

## <a name="renew-webhook"></a>Bir Web kancasını yenileme

Bir Web kancası oluşturulurken bir yıllık bir geçerlilik süresi vardır. Bu yıl saatten sonra Web kancasını otomatik olarak sona erer. Bir Web kancası süresi dolduktan bunu etkinleştirilemez, onu kaldırıldı ve yeniden oluşturulması gerekir. Bir Web kancası, süre sonu ulaşılmamış, genişletilebilir.

Bir Web kancasını genişletmek için Web kancası içeren runbook'a gidin. Seçin **Web kancaları** altında **kaynakları**. Genişletmek istediğiniz Web kancası'ı tıklatın, bu eylem açar **Web kancası** sayfası.  Yeni sona erme tarihi ve saati seçin ve tıklayın **Kaydet**.

## <a name="sample-runbook"></a>Örnek runbook

Aşağıdaki örnek runbook'u webhook verilerini kabul eder ve istek gövdesinde belirtilen sanal makineleri başlatır. Bu runbook Otomasyon hesabınız kapsamında test **runbook'ları**, tıklayın **+ runbook Ekle**. Bir runbook oluşturmak nasıl bilmiyorsanız, bkz. [runbook oluşturma](automation-quickstart-create-runbook.md).

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)



# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Check header for message to validate request
    if ($WebhookData.RequestHeader.message -eq 'StartedbyContoso')
    {
        Write-Output "Header has required information"}
    else
    {
        Write-Output "Header missing required information";
        exit;
    }

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

## <a name="test-the-example"></a>Örnek test

Aşağıdaki örnek, bir Web kancası ile bir runbook başlatmak için Windows PowerShell kullanır. Bir HTTP isteği yapabilen herhangi bir dilde bir Web kancası kullanabilirsiniz; Windows PowerShell kullanılan bir örnek burada.

Runbook, isteğin gövdesindeki JSON biçimli sanal makinelerin bir listesini bekliyor. Runbook de üstbilgileri tanımlanan içerdiğini doğrular ileti Web kancası çağrıyı doğrulamak için geçerlidir.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms
$header = @{ message="StartedbyContoso"}
$response = Invoke-WebRequest -Method Post -Uri $uri -Body $body -Headers $header
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

Aşağıdaki örnek, bir runbook'ta kullanılabilir isteğin gövdesini gösterir **ıncludesearchresults: true** özelliği **WebhookData**. Bu, istek gövdesinde eklenmiştir biçimde olduğundan bu değer JSON olarak biçimlendirilir.

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

Aşağıdaki görüntüde, Windows PowerShell ve sonuçta elde edilen yanıttan gönderilen isteği gösterilmektedir. İş kimliği gelen yanıt ayıklanır ve bir dizeye dönüştürülür.

![Web kancaları düğmesi](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu Azure Uyarıları'üzerinde harekete kullanılacağını öğrenmek için bkz [Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın](automation-create-alert-triggered-runbook.md).

