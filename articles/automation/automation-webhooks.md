---
title: "Bir Azure Otomasyonu runbook'u ile bir Web kancası başlangıç | Microsoft Docs"
description: "Bir HTTP çağrısından Azure Otomasyon runbook'u başlatmak bir istemci izin veren bir Web kancası.  Bu makalede, bir Web kancası oluşturma ve bir runbook'u başlatmak için bir çağrı açıklanmaktadır."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: b1b9b804aa696419b52a03f127c59037c337be66
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Bir Web kancası ile bir Azure Otomasyonu runbook'u başlatma
A *Web kancası* , belirli bir runbook, tek bir HTTP isteği aracılığıyla Azure Otomasyonu'nda başlatmanıza olanak verir. Bu, Visual Studio Team Services, GitHub, Microsoft Operations Management Suite günlük analizi veya runbook'ları Azure Otomasyon API'sini kullanarak tam bir çözüm uygulama olmadan başlatmak için özel uygulamalar gibi dış hizmetler sağlar.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Bir runbook'u başlatma diğer yöntemleri için Web kancası karşılaştırabilirsiniz [Azure Otomasyonu runbook başlatma](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Bir Web kancası ayrıntıları
Aşağıdaki tabloda, bir Web kancası için yapılandırmanız gerekir özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Ad |Bu istemciye gösterilmez bu yana bir Web kancası için istediğiniz herhangi bir ad sağlayabilirsiniz.  Yalnızca sizin için Azure Automation runbook tanımlamak için kullanılır. <br>  En iyi uygulama, Web kancası kullanacağı istemcisiyle ilgili bir ad vermeniz gerekir. |
| URL |Web kancası URL'si için Web kancası bağlı runbook'u başlatmak için bir HTTP POST ile bir istemci çağıran benzersiz adrestir.  Web kancası oluşturduğunuzda otomatik olarak oluşturulur.  Özel bir URL belirtemezsiniz. <br> <br>  URL runbook'un başka kimlik doğrulama ile üçüncü taraf sistemleri tarafından çağrılan izin veren bir güvenlik belirteci içeriyor. Bu nedenle, bir parola gibi değerlendirilmelidir.  Güvenlik nedeniyle, Web kancası oluşturulduğunda Azure portalında yalnızca URL görüntüleyebilirsiniz. Gelecekte kullanım için güvenli bir konumda URL unutmamalısınız. |
| Bitiş tarihi |Bir sertifika gibi her Web kancası aynı zamanda bu artık kullanılabilir bir sona erme tarihi vardır.  Web kancası oluşturulduktan sonra bu süre sonu tarihi değiştirilebilir. |
| Etkin |Bir Web kancası oluşturulduğunda varsayılan olarak etkindir.  Devre dışı olarak ayarlarsanız, hiçbir istemci kullanmak mümkün olacaktır.  Ayarlayabileceğiniz **etkin** Web kancası veya dilediğiniz zaman bir kez oluşturduğunuzda özelliği oluşturulur. |

### <a name="parameters"></a>Parametreler
Bir Web kancası runbook o Web kancası tarafından çalıştırıldığında kullanılan runbook parametreleri için değerleri tanımlayabilirsiniz. Web kancası runbook'un zorunlu parametreler için değerler içermelidir ve isteğe bağlı parametreler için değerler içerebilir. Bir Web kancası için yapılandırılmış bir parametre değeri webhoook oluşturduktan sonra bile değiştirilebilir. Tek bir runbook bağlı birden çok Web kancalarını her farklı parametre değerlerini kullanabilirsiniz.

Bir istemciyi bir Web kancası kullanarak bir runbook'u başlattığında, Web kancası içinde tanımlanan parametre değerlerini geçersiz kılamaz.  İstemciden veri almak için runbook adlı tek bir parametre kabul edebilir **$WebhookData** [object] türündeki POST isteğinde istemci içeren veri içermez.

![Webhookdata özellikleri](media/automation-webhooks/webhook-data-properties.png)

**$WebhookData** nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| WebhookName |Web kancası adı. |
| RequestHeader |Gelen posta istek üstbilgileri içeren karma tablo. |
| RequestBody |Gelen posta istek gövdesi.  Bu herhangi bir dize gibi JSON, XML, biçimlendirme korur veya kodlanmış form verileri. Runbook için beklenen veri biçimi ile çalışmak için yazılmış olmalıdır. |

Web kancası desteklemek için gerekli olan yapılandırma yok **$WebhookData** parametre ve runbook kabul etmek için gerekmez.  Runbook parametresi tanımlamıyorsa, daha sonra istemci tarafından gönderilen istek, herhangi bir ayrıntıyı yoksayılır.

Web kancası oluşturduğunuzda, bir değer $WebhookData için Web kancası istemci POST isteğini verilerle runbook başlatıldığında, istemci istek gövdesinde herhangi bir veri içermez olsa bile değer kılınmadı olmasını belirtirseniz.  Bir Web kancası dışında bir yöntem kullanılarak $WebhookData sahip bir runbook başlatırsanız, runbook tarafından tanınan $Webhookdata için bir değer sağlayabilir.  Bu değer bir nesne ile aynı olmalıdır [özellikleri](#details-of-a-webhook) $Webhookdata olarak böylece Web kancası tarafından geçirilen gerçek WebhookData çalıştığı gibi runbook düzgün ile çalışabilirsiniz.

Aşağıdaki runbook Azure Portalı'ndan başladılar ve test etmek için bazı örnek WebhookData WebhookData bir nesne olduğundan geçirmek istediğiniz, örneğin, bu arabirimdeki JSON olarak geçirilmesi gerekir.

![UI WebhookData parametresinden](media/automation-webhooks/WebhookData-parameter-from-UI.png)

WebhookData parametresi için aşağıdaki özellikler varsa, yukarıdaki runbook için:

1. WebhookName: *MyWebhook*
2. RequestHeader: *= Test kullanıcıdan*
3. RequestBody: *["VM1", "VM2"]*

Daha sonra kullanıcı arabiriminde WebhookData parametresi için aşağıdaki JSON değeri geçirin:  

* {"WebhookName": "MyWebhook", "RequestHeader": {"Kimden": "Test kullanıcı"}, "RequestBody": "[\"VM1\",\"VM2\"]"}

![Kullanıcı Arabirimi başlangıç WebhookData parametresi](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Tüm giriş parametrelerinin değerlerini runbook işi ile günlüğe kaydedilir.  Herhangi bir deyişle Web kancası isteği istemci tarafından sağlanan giriş günlüğe kaydedilen ve Otomasyon iş erişimi olan herkes için kullanılabilir olacaktır.  Bu nedenle, Web kancası çağrılarında hassas bilgiler dahil olmak üzere hakkında dikkatli olmanız gerekir.
>

## <a name="security"></a>Güvenlik
Bir Web kancası güvenlik çağrılmasına izin veren bir güvenlik belirteci içeren URL'sini gizlilik kullanır. Azure otomasyonu için doğru URL'yi yapılan sürece herhangi bir kimlik doğrulaması isteği gerçekleştirmez. Bu nedenle, Web kancalarını isteği doğrulanırken bir alternatif şekillerini kullanmadan son derece hassas işlevleri gerçekleştirmek runbook'lar için kullanılmamalıdır.

Bunu denetleyerek bir Web kancası tarafından çağrıldı belirlemek için runbook mantık içerebilir **WebhookName** $WebhookData parametresinin özelliği sağlar. Runbook daha fazla doğrulama belirli bilgileri arayarak gerçekleştirebilir **RequestHeader** veya **RequestBody** özellikleri.

Bir Web kancası isteği alındığında bir dış koşulun bazı doğrulama gerçekleştirmek runbook başka bir stratejisi olmalıdır.  Örneğin, bir GitHub deposuna yeni bir yürütme olduğunda GitHub tarafından çağrılan bir runbook'u göz önünde bulundurun.  Runbook yeni bir yürütme gerçekte yalnızca devam etmeden önce oluştu olduğunu doğrulamak için GitHub bağlanabilir.

## <a name="creating-a-webhook"></a>Bir Web kancası oluşturma
Azure Portalı'nda runbook'un bağlı yeni bir Web kancası oluşturmak için aşağıdaki yordamı kullanın.

1. Gelen **Runbook'lar sayfa** Azure portalında kendi ayrıntı sayfası görüntülemek için Web kancası başlatacak ' a tıklayın.
2. Tıklatın **Web kancası** açmak için sayfanın üstündeki **ekleme Web kancası** sayfası. <br>
   ![Web kancası düğmesi](media/automation-webhooks/webhooks-button.png)
3. Tıklatın **yeni Web kancası oluşturma** açmak için **Oluştur Web kancası sayfası**.
4. Belirtin bir **adı**, **sona erme tarihi** Web kancası ve olup etkinleştirilmelidir. Bkz: [bir Web kancası ayrıntılarını](#details-of-a-webhook) daha fazla bilgi için bu özellikleri.
5. Kopyala simgesine tıklayın ve Web kancası URL'yi kopyalamak için Ctrl + C tuşlarına basın.  Ardından güvenli bir yerde kaydedin.  **Web kancası oluşturulduktan sonra URL'yi yeniden alınamıyor.** <br>
   ![Web kancası URL'si](media/automation-webhooks/copy-webhook-url.png)
6. Tıklatın **parametreleri** runbook parametreleri için değerler sağlamak için.  Runbook zorunlu parametreler varsa, daha sonra değerleri belirtilmediği sürece Web kancası oluşturmak mümkün olmaz.
7. Tıklatın **oluşturma** Web kancası oluşturulamadı.

## <a name="using-a-webhook"></a>Bir Web kancası kullanma
Bir Web kancası oluşturulduktan sonra kullanmak için istemci uygulamanız bir Web kancası URL'si ile HTTP POST kesmeniz gerekir.  Web kancası söz dizimi şu biçimde olacaktır.

    http://<Webhook Server>/token?=<Token Value>

İstemci aşağıdaki dönüş kodları birini POST isteğini alır.  

| Kod | Metin | Açıklama |
|:--- |:--- |:--- |
| 202 |Kabul Edildi |İstek kabul edildi ve runbook başarıyla sıraya alındı. |
| 400 |Hatalı İstek |İstek aşağıdaki nedenlerden birinden dolayı kabul edilmedi. <ul> <li>Web kancası süresi doldu.</li> <li>Web kancası devre dışı bırakılır.</li> <li>URL'deki belirteci geçersiz.</li>  </ul> |
| 404 |Bulunamadı |İstek aşağıdaki nedenlerden birinden dolayı kabul edilmedi. <ul> <li>Web kancası bulunamadı.</li> <li>Runbook bulunamadı.</li> <li>Hesap bulunamadı.</li>  </ul> |
| 500 |İç Sunucu Hatası |URL geçerli, ancak bir hata oluştu.  Lütfen isteği yeniden gönderin. |

İstek başarılı olduğunu varsayarak, Web kancası yanıt gibi JSON biçiminde iş kimliğini içerir. Tek iş kimliği yer alır ancak JSON biçimi için gelecekteki olası geliştirmeleri sağlar.

    {"JobIds":["<JobId>"]}  

İstemci, runbook işi tamamlandığında veya Web kancası gelen tamamlanma durumu belirlenemiyor.  İş kimliği gibi başka bir yöntemle kullanarak bu bilgileri belirleyebilirsiniz [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) veya [Azure Otomasyon API](https://msdn.microsoft.com/library/azure/mt163826.aspx).

### <a name="example"></a>Örnek
Aşağıdaki örnek, bir Web kancası ile bir runbook'u başlatmak için Windows PowerShell'i kullanır.  Bir HTTP isteği yapabilen herhangi bir dil bir Web kancası kullanabileceğinizi unutmayın; Windows PowerShell yalnızca kullanılan burada bir örnek olarak.

Runbook istek gövdesinde JSON biçimli sanal makinelerin listesini bekliyor. Biz de istek üstbilgisinde başlatıldığına kimin runbook ve tarih ve saat başlatma hakkında bilgi dahil.      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


Aşağıdaki resimde üst bilgileri gösterir (kullanarak bir [Fiddler](http://www.telerik.com/fiddler) izleme) bu istek. Bu, bir HTTP isteğinin özel tarih yanı sıra ve eklediğimiz başlıklarından standart üstbilgilerini içerir.  Bu değerlerin her birini runbook için kullanılabilir **RequestHeaders** özelliği **WebhookData**.

![Web kancası düğmesi](media/automation-webhooks/webhook-request-headers.png)

İstek gövdesini aşağıdaki görüntüde gösterir (kullanarak bir [Fiddler](http://www.telerik.com/fiddler) izleme) runbook'a kullanılabilir **RequestBody** özelliği **WebhookData**. İstek gövdesinde eklenmiştir biçimi olduğu için bu JSON olarak biçimlendirilir.     

![Web kancası düğmesi](media/automation-webhooks/webhook-request-body.png)

Aşağıdaki resimde, Windows PowerShell ve sonuçta elde edilen yanıt gönderilen istek gösterilir.  İş kimliği yanıttan ayıklanmış ve bir dizeye dönüştürülür.

![Web kancası düğmesi](media/automation-webhooks/webhook-request-response.png)

Aşağıdaki örnek runbook'u önceki örnek isteğini kabul eder ve istek gövdesinde belirtilen sanal makineleri başlatır.

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate to Azure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean to be started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-to-azure-alerts"></a>Azure uyarılara yanıt olarak runbook başlatılıyor
Web kancası etkin runbook'ları için tepki vermek için kullanılabilir [Azure uyarıları](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Azure kaynaklarında performans, kullanılabilirlik ve Azure uyarıları yardımıyla kullanım istatistikleri toplayarak izlenebilir. Ölçümleri izleme dayalı bir uyarı alabilirsiniz veya Azure kaynaklarınızı, şu anda Otomasyon hesapları için olaylar yalnızca ölçümleri destekler. Belirtilen bir ölçüm değerini atanan eşiğini aştığında veya bir bildirim uyarıyı çözmek için gönderilir Hizmet Yöneticisi veya ortak yöneticileri sonra yapılandırılmış olay tetikleniyorsa, ölçümleri ve olaylar hakkında daha fazla bilgi için lütfen [Azure uyarıları](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Azure Uyarıları Bildirim sistemi olarak kullanmanın yanı sıra, ayrıca uyarılara yanıt olarak runbook'ları devre dışı tetiklersiniz. Azure Otomasyonu Web kancası etkin runbook'ları ile Azure uyarılar yeteneği sağlar. Ölçüm aştığında, yapılandırılan eşik değeri sonra uyarı kuralı etkin hale gelir ve runbook sırayla yürütür Otomasyonu Web kancası tetikler.

![Web Kancaları](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a>Uyarı bağlamı
Bir sanal makine gibi bir Azure kaynağı düşünün, CPU kullanımı bu makinenin anahtar performans ölçüm biridir. CPU kullanımı % 100 veya belirli bir miktar fazla uzun süre, sorunu düzeltmek için sanal makineyi yeniden isteyebilirsiniz. Bu sanal makineye bir uyarı kuralı yapılandırarak çözülebilir ve bu kural CPU yüzdesi, ölçüm olarak alır. CPU yüzdesi burada yalnızca bir örnek olarak alınır ancak Azure kaynaklarınızı yapılandırabileceğiniz birçok ölçümleri vardır ve sanal makinenin yeniden başlatılması bu sorunu gidermek için gerçekleştirilecek bir eylem, diğer işlemler için runbook yapılandırabilirsiniz.

Bu uyarı kuralının etkin hale gelir ve tetikler Web kancası etkin runbook uyarı bağlamı runbook'a gönderir. [Uyarı bağlamı](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) dahil olmak üzere ayrıntılarını içeren **Subscriptionıd**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** ve **zaman damgası** üzerinde bu göz önünde bulundurarak eylem kaynağı tanımlamak runbook için gerekli olan. Bağlam içinde gövde bölümü katıştırılmış uyarı **WebhookData** runbook ve onu gönderilen nesnesi ile erişilebilir **Webhook.RequestBody** özelliği

### <a name="example"></a>Örnek
Bir Azure sanal makinesi, abonelik ve ilişkilendirme oluşturma bir [CPU yüzdesi ölçüm izlemek için uyarı](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Uyarı oluşturulurken Web kancası oluşturulurken oluşturulan Web kancası URL'si ile Web kancası alanını doldurmak emin olun.

Aşağıdaki örnek runbook'u uyarı kuralı etkin hale gelir ve runbook üzerinde bu eylemi gerçekleştirmeden kaynağı tanımlamak gerekli olan uyarı bağlam parametreleri toplar geldiğinde tetiklenir.

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on the webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain the WebhookBody containing the AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain the AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on the AlertContext data, in our case restarting the VM.
            # Authenticate to your Azure subscription using Organization ID to be able to restart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check the status property of the VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart the VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant to only be started from a webhook."  
        }  
    }



## <a name="next-steps"></a>Sonraki adımlar
* Bir runbook'u başlatmak için çeşitli yollar hakkında daha fazla bilgi için bkz [Runbook başlatma](automation-starting-a-runbook.md).
* Bir Runbook işinin durumunu görüntüleme hakkında daha fazla bilgi için bkz [Azure Otomasyonu Runbook yürütme](automation-runbook-execution.md).
* Azure Uyarıları'üzerinde eyleme geçmek için Azure Otomasyonu kullanmayı öğrenmek için bkz: [Otomasyon runbook'ları ile Azure VM uyarıları düzeltmek](automation-azure-vm-alert-integration.md).
