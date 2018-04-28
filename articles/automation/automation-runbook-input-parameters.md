---
title: Runbook giriş parametreleri
description: Runbook giriş parametreleri başlatıldığında bir runbook'a veri iletmek sağlayarak runbook'lar esnekliğini artırır. Bu makalede giriş parametreleri runbook'ları kullanıldığı farklı senaryolar anlatılmaktadır.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 19b0e17807adc0e7a4522fd13cd85779cdbcafd6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="runbook-input-parameters"></a>Runbook giriş parametreleri

Runbook giriş parametreleri başlatıldığında veri ona geçirmek izin vererek runbook'lar esnekliğini artırır. Parametreleri runbook eylemlerin belirli senaryolar ve ortamlar için hedeflenen sağlar. Bu makalede, giriş parametreleri runbook'ları kullanıldığı farklı senaryolar üzerinden yol.

## <a name="configure-input-parameters"></a>Giriş Parametrelerini Yapılandır

Giriş parametreleri PowerShell, PowerShell iş akışı, Python ve grafik runbook'lar yapılandırılabilir. Bir runbook farklı veri türleriyle birden çok parametre ya da hiç parametre hiç olabilir. Giriş parametreleri zorunlu veya isteğe bağlı olabilir ve isteğe bağlı parametreler için varsayılan bir değer atayabilirsiniz. Kullanılabilir yöntemlerin biri aracılığıyla başlattığınızda, bir runbook'un giriş parametreleri için değerler atayabilirsiniz. Bu yöntemler, portalı veya web hizmetinden bir runbook'u başlatma içerir. Başka bir runbook'u satır içi olarak adlandırılan bir alt runbook'u olarak da başlatabilirsiniz.

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a>Giriş parametreleri PowerShell ve PowerShell iş akışı runbook'ları yapılandırma

PowerShell ve [PowerShell iş akışı runbook'ları](automation-first-runbook-textual.md) Azure Otomasyonu'nda aşağıdaki öznitelikler tanımlanan giriş parametreleri destekler:  

| **Özellik** | **Açıklama** |
|:--- |:--- |
| Tür |Gereklidir. Parametre değeri beklenen veri türü. Herhangi bir .NET türü geçerli değil. |
| Ad |Gereklidir. Parametrenin adı. Bu gerekir runbook içinde benzersiz olmalıdır ve yalnızca harf, sayı içeren veya alt çizgi karakterleri. Bir harf ile başlamalıdır. |
| Zorunlu |İsteğe bağlı. Bir değer parametresi için sağlanan olup olmadığını belirtir. Bu ayar, **$true**, sonra da runbook başlatılırken bir değer sağlanmalıdır. Bu ayar, **$false**, sonra da bir değer isteğe bağlıdır. |
| Varsayılan değer |İsteğe bağlı. Runbook başlatılırken bir değer değil geçtiyse parametresi için kullanılan bir değeri belirtir. Varsayılan değer otomatik olarak parametresi zorunlu ayarından bağımsız olarak isteğe bağlı hale getirir ve herhangi bir parametre için ayarlayabilirsiniz. |

Windows PowerShell giriş parametreleri burada doğrulama gibi diğer adlar, listelenenler ve parametre kümeleri çok daha fazla özniteliklerini destekler. Ancak, Azure Automation şu anda yalnızca yukarıdaki giriş parametreleri destekler.

PowerShell iş akışı runbook'ları parametre tanımında burada birden çok parametre virgülle ayrılır aşağıdaki genel biçime sahiptir.

```powershell
Param
(
  [Parameter (Mandatory= $true/$false)]
  [Type] $Name1 = <Default value>,

  [Parameter (Mandatory= $true/$false)]
  [Type] $Name2 = <Default value>
)
```

> [!NOTE]
> Ne zaman tanımladığınız parametreler belirtmediyseniz **zorunlu** özniteliği sonra varsayılan olarak, parametre isteğe bağlı olarak kabul edilir. PowerShell iş akışı runbook'ları bir parametre için varsayılan bir değer ayarlarsanız, ayrıca, PowerShell tarafından isteğe bağlı bir parametre öğesinden bağımsız olarak işlem görür **zorunlu** öznitelik değeri.
> 
> 

Örnek olarak, sanal makineler, tek bir VM veya bir kaynak grubundaki tüm sanal makineleri ayrıntılarını çıkarır bir PowerShell iş akışı runbook giriş parametreleri yapılandıralım. Aşağıdaki ekran görüntüsünde gösterildiği gibi bu runbook iki parametreye sahiptir: adını sanal makine ve kaynak grubunun adı.

![Automation PowerShell iş akışı](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

Bu parametre tanımında parametreleri **$VMName** ve **$resourceGroupName** dize türünde basit parametreleridir. Bununla birlikte, PowerShell ve PowerShell iş akışı runbook'ları tüm basit türleri ve karmaşık türler gibi destek **nesne** veya **PSCredential** giriş parametreleri için.

Runbook'unuz bir nesne türü giriş parametresi varsa, (adı, değer) içeren bir PowerShell hashtable kullanmak bir değer geçirmek için çiftleri. Örneğin, bir runbook'ta aşağıdaki parametre varsa:

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

Ardından parametresi şu değer geçirebilirsiniz:

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```

## <a name="configure-input-parameters-in-graphical-runbooks"></a>Giriş parametreleri grafik runbook'ları yapılandırma

İçin [grafik runbook yapılandırma](automation-first-runbook-graphical.md) giriş parametreleri ile sanal makineleri ayrıntılarını ya da çıkarır bir grafik runbook tek bir VM veya bir kaynak grubundaki tüm sanal makineleri oluşturalım. Bir runbook yapılandırma, aşağıda açıklandığı gibi iki ana etkinliklerini oluşur.

[**Runbook'ları Azure farklı çalıştır hesabıyla kimlik doğrulaması** ](automation-sec-configure-azure-runas-account.md) Azure kimlik doğrulaması için.

[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) bir sanal makinenin özellikleri alınamıyor.

Kullanabileceğiniz [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) sanal makinelerin adlarını çıktısını almak için etkinlik. Etkinlik **Get-AzureRmVm** iki parametre kabul eden **sanal makine adı** ve **kaynak grubu adı**. Bu parametreler, runbook'u her başlattığınızda farklı değerler gerektirebilir olduğundan, giriş parametreleri runbook'a ekleyebilirsiniz. Giriş parametreleri ekleme adımları şunlardır:

1. Grafik runbook'tan seçin **Runbook'lar** dikey ve ardından [ **Düzenle** ](automation-graphical-authoring-intro.md) onu.
2. Runbook Düzenleyicisi'nden tıklatın **giriş ve çıkış** açmak için **giriş ve çıkış** dikey.
   
    ![Otomasyon grafik runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. **Giriş ve çıkış** dikey runbook için tanımlanan giriş parametreleri listesini görüntüler. Bu dikey penceresinde, yeni bir giriş parametresi eklemek veya var olan bir giriş parametresi yapılandırmasını düzenleyin. Runbook için yeni bir parametre eklemek için tıklatın **giriş Ekle** açmak için **Runbook giriş parametresi** dikey. Burada, aşağıdaki parametreleri yapılandırabilirsiniz:
   
   | **Özellik** | **Açıklama** |
   |:--- |:--- |
   | Ad |Gereklidir. Parametrenin adı. Bu gerekir runbook içinde benzersiz olmalıdır ve yalnızca harf, sayı içeren veya alt çizgi karakterleri. Bir harf ile başlamalıdır. |
   | Açıklama |İsteğe bağlı. Giriş parametresi amacı hakkında açıklama. |
   | Tür |İsteğe bağlı. Parametre değeri beklenen veri türü. Desteklenen parametre türleri **dize**, **Int32**, **Int64**, **ondalık**, **Boolean**, **DateTime**, ve **nesne**. Bir veri türü seçili değilse, varsayılan olarak **dize**. |
   | Zorunlu |İsteğe bağlı. Bir değer parametresi için sağlanan olup olmadığını belirtir. Seçerseniz **Evet**, sonra da runbook başlatılırken bir değer sağlanmalıdır. Seçerseniz **hiçbir**, bir değer runbook başlatıldığında ve varsayılan bir değer ayarlanabilir gerekli değildir. |
   | Varsayılan Değer |İsteğe bağlı. Runbook başlatılırken bir değer değil geçtiyse parametresi için kullanılan bir değeri belirtir. Varsayılan değer, zorunlu olmayan bir parametre için ayarlanabilir. Varsayılan bir değer ayarlamak için seçin **özel**. Runbook başlatılırken başka bir değer sağlanmadığı sürece bu değer kullanılır. Seçin **hiçbiri** herhangi bir varsayılan değer sağlamak istemiyorsanız. |
   
    ![Yeni giriş Ekle](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Tarafından kullanılan aşağıdaki özelliklere sahip iki parametre oluşturma **Get-AzureRmVm** etkinlik:
   
   * **Parametre1:**
     
     * Ad - VMName
     * -Dize türünde
     * Zorunlu - yok
   * **Parametre2:**
     
     * Ad - resourceGroupName
     * -Dize türünde
     * Zorunlu - yok
     * Varsayılan değer - özel
     * Özel varsayılan değer - \<sanal makineleri içeren kaynak grubu adı >
5. Parametreleri ekledikten sonra tıklatın **Tamam**. Bunları artık görüntüleyebilirsiniz **giriş ve çıkış dikey**. Tıklatın **Tamam** yeniden ve ardından **kaydetmek** ve **Yayımla** runbook'unuz.

## <a name="configure-input-parameters-in-python-runbooks"></a>Giriş parametreleri Python runbook'ları yapılandırma

PowerShell, PowerShell iş akışı ve grafik runbook'lar, Python runbook'ları adlandırılmış parametreleri kazanmaz.
Tüm giriş parametreleri bir bağımsız değişken değerleri dizisi olarak ayrıştırılır.
İçeri aktararak dizi erişim `sys` , Python betiği ve ardından kullanarak modüle `sys.argv` dizi.
Bu dikkate almak önemlidir dizisinin ilk öğesi `sys.argv[0]`, ilk gerçek girdi parametresi komut adı olduğundan `sys.argv[1]`.

Python runbook'ta giriş parametrelerini kullanma örneği için bkz: [Azure automation'da ilk Python runbook'um](automation-first-runbook-textual-python2.md).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Giriş runbook parametreleri için değerleri atayın

Giriş aşağıdaki senaryolarda runbook parametreleri için değerleri geçirebilirsiniz:

### <a name="start-a-runbook-and-assign-parameters"></a>Bir runbook başlatın ve parametreleri atayın

Bir runbook birçok yolu başlatılabilir: bir Web kancası ile PowerShell cmdlet'leri ile REST API ile veya SDK'sı Azure Portalı aracılığıyla. Aşağıda bir runbook'u başlatma ve parametreleri atamak için farklı yöntemler açıklanmaktadır.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Azure portalını kullanarak yayımlanan bir runbook başlatın ve parametreleri atayın

Olduğunda, [runbook'u başlatmak](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), **Runbook'u Başlat** dikey penceresi açılır ve oluşturduğunuz parametreleri için değerler girebilirsiniz.

![Portalı kullanmaya başlama](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Giriş kutusuna altındaki etiketinde parametresi için belirlenen öznitelikleri görebilirsiniz. Öznitelikler, zorunlu veya isteğe bağlı, türü ve varsayılan değeri içerir. Parametre adının yanındaki Yardım balonu parametre giriş değerleri hakkında kararlar gereken tüm anahtar bilgileri görebilirsiniz. Bir parametre zorunlu veya isteğe bağlı olup, bu bilgiler içerir. Ayrıca, türü ve varsayılan değer (varsa) ve diğer yararlı notlar içerir.

> [!NOTE]
> Dize türü parametreleri desteği **boş** dize değerleri.  Girme **[oluşması]** giriş parametresinde kutusu boş bir dize parametresi geçirir. Ayrıca, dize türü parametreleri desteklemeyen **Null** geçirilen değerleri. Herhangi bir değer dizesi parametresi geçirmezseniz sonra PowerShell bunu boş olarak yorumlar.
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Yayımlanan bir runbook'ta PowerShell cmdlet'lerini kullanarak başlatmak ve parametreleri atayın

* **Azure Resource Manager cmdlet'lerini:** bir kaynak grubunda kullanılarak oluşturulmuş bir Otomasyon runbook'u başlatabilirsiniz [başlangıç AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).
  
  **Örnek:**
  
  ```powershell
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* **Azure Klasik dağıtım modeli cmdlet'leri:** bir varsayılan kaynak grubunda kullanılarak oluşturulmuş bir Otomasyon runbook'u başlatabilirsiniz [başlangıç AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).
  
  **Örnek:**
  
  ```powershell
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> PowerShell cmdlet'lerini, varsayılan parametre kullanarak bir runbook'u başlattığınızda **MicrosoftApplicationManagementStartedBy** değeri ile oluşturulan **PowerShell**. Bu parametrede görüntüleyebilirsiniz **iş ayrıntıları** dikey.  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Bir SDK kullanarak bir runbook'u başlatmak ve parametreleri atayın

* **Azure Resource Manager yöntemi:** bir programlama dili SDK'yi kullanarak bir runbook'u başlatabilirsiniz. Otomasyon hesabınızda bir runbook'u başlatmak için C# kod parçacığı aşağıdadır. Tüm koda görüntüleyebilirsiniz bizim [GitHub deposunu](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  
  
  ```csharp
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* **Azure Klasik dağıtım modeli yöntemi:** bir programlama dili SDK'yi kullanarak bir runbook'u başlatabilirsiniz. Otomasyon hesabınızda bir runbook'u başlatmak için C# kod parçacığı aşağıdadır. Tüm koda görüntüleyebilirsiniz bizim [GitHub deposunu](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).
  
  ```csharp
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  Bu yöntem başlatmak için runbook parametreleri depolamak için Sözlük oluşturma **VMName** ve **resourceGroupName**ve değerleri. Daha sonra runbook'u başlatın. Yukarıda tanımlanan yöntemi çağırmak için C# kod parçacığı aşağıda verilmiştir.
  
  ```csharp
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>REST API kullanarak bir runbook'u başlatmak ve parametreleri atayın
Bir runbook işi oluşturulur ve kullanarak Azure Otomasyon REST API'si ile çalışmaya **PUT** yöntemi aşağıdaki istek URI'si ile:

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

İstek URI'si aşağıdaki parametreleri değiştirin:

* **Abonelik kimliği:** Azure abonelik kimliğinizi  
* **Bulut hizmet adı:** bulut adını hizmet isteği hangi gönderilmelidir için.  
* **Otomasyon hesabı adı:** içinde belirtilen bulut hizmeti barındırılan Otomasyon hesabınızın adını.  
* **İş Kimliği:** iş için GUID. PowerShell'de GUID'ler kullanarak oluşturulabilir **[GUID]::NewGuid(). ToString()** komutu.

Runbook işi parametreleri geçirmek için istek gövdesini kullanın. JSON biçiminde sağlanan aşağıdaki iki özelliklerini alır:

* **Runbook adı:** gerekli. Runbook başlatma işinin adıdır.  
* **Runbook parametreleri:** isteğe bağlı. (Adı, değer) parametre listesinde sözlüğü biçiminde burada adı dize türünde olmalıdır ve değer geçerli bir JSON değeri olabilir.

Başlatmak istiyorsanız **Get-AzureVMTextual** daha önce oluşturulmuş runbook **VMName** ve **resourceGroupName** parametre olarak istek gövdesi için şu JSON biçimini kullanın.

   ```json
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

İş başarıyla oluşturulduysa 201 HTTP durum kodu döndürülür. Yanıt Üstbilgileri ve yanıt gövdesi hakkında daha fazla bilgi için ilgili makaleye bakın [REST API kullanarak bir runbook işi oluşturun.](https://msdn.microsoft.com/library/azure/mt163849.aspx)

### <a name="test-a-runbook-and-assign-parameters"></a>Bir runbook'u test ve parametreleri atayın
Olduğunda, [, runbook'un taslak sürümünü test](automation-testing-runbook.md) test seçeneğini kullanarak **Test** sayfası açılır ve oluşturduğunuz parametre değerlerini yapılandırabilirsiniz.

![Test ve ata parametreleri](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Bir zamanlamayı runbook'a bağlamak ve parametreleri atayın
Yapabilecekleriniz [bir zamanlama Bağla](automation-schedules.md) runbook'unuzu için böylece runbook belirli bir zamanda başlatır. Giriş parametreleri, zamanlamayı oluşturduğunuzda ve zamanlama tarafından başlatıldığında bu değerleri runbook kullanır atayın. Tüm zorunlu parametre değerleri sağlanana kadar zamanlaması kaydedilemiyor.

![Zamanlama ve parametreleri atama](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Bir runbook için bir Web kancası oluşturun ve parametreleri atayın
Oluşturabileceğiniz bir [Web kancası](automation-webhooks.md) runbook'unuzu için ve runbook giriş parametreleri yapılandırın. Tüm zorunlu parametre değerleri sağlanana kadar Web kancası kaydedilemiyor.

![Web kancası oluşturun ve parametreleri atayın](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Bir Web kancası, önceden tanımlanmış giriş parametresi kullanılarak bir runbook yürütülürken **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** tanımlanan giriş parametreleri birlikte gönderilir. Genişletmek için tıklatın **WebhookData** daha fazla ayrıntı için parametre.

![WebhookData parametresi](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a>Sonraki adımlar
* Runbook giriş ve çıkış hakkında daha fazla bilgi için bkz: [Azure Automation: runbook giriş, çıkış ve iç içe runbook](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).
* Bir runbook'u başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz: [runbook başlatma](automation-starting-a-runbook.md).
* Bir metinsel runbook'u düzenlemek için başvurmak [metinsel runbook'ları düzenleme](automation-edit-textual-runbook.md).
* Bir grafik runbook'u düzenlemek için başvurmak [Azure Automation'da grafik yazma](automation-graphical-authoring-intro.md).

