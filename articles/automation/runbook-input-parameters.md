---
title: Runbook giriş parametreleri
description: Runbook giriş parametreleri başlatıldığında bir runbook'a veri iletmek sağlayarak runbook'ları esnekliğini artırın. Bu makalede, giriş parametrelerinin runbook'ları kullanıldığı farklı senaryolar anlatılmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5f190d60a059108b9763f35e2ee8cf99ae77b694
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60500047"
---
# <a name="runbook-input-parameters"></a>Runbook giriş parametreleri

Runbook giriş parametreleri başlatıldığında verileri geçirin izin vererek runbook'ları esnekliğini artırın. Parametreler, belirli senaryolar ve ortamlar için hedeflenecek için runbook eylemleri olanak tanır. Bu makalede, giriş parametrelerinin runbook'ları kullanıldığı farklı senaryolarla size yol.

## <a name="configure-input-parameters"></a>Girdi parametrelerini yapılandırma

Giriş parametreleri, PowerShell, PowerShell iş akışı, Python ve grafik runbook'ları yapılandırılabilir. Bir runbook birden çok farklı veri türleri parametrelerle veya parametresiz hiç olabilir. Giriş parametreleri, zorunlu veya isteğe bağlı olabilir ve isteğe bağlı parametreler için varsayılan bir değer olabilir. Kullanılabilir yöntemlerin biri aracılığıyla başlattığınızda, bir runbook'un giriş parametreleri için değerler atayın. Azure portalı, bir web hizmeti veya PowerShell runbook başlatma bu yöntemleri içerir. Başka bir runbook'u satır içi olarak adlandırılan bir alt runbook olarak da başlatabilirsiniz.

## <a name="configure-input-parameters-in-powershell-runbooks"></a>PowerShell runbook'ları girdi parametrelerini yapılandırma

Azure Otomasyonu, PowerShell ve PowerShell iş akışı runbook'ları aşağıdaki öznitelikler tanımlanan giriş parametreleri destekler:  

| **Özellik** | **Açıklama** |
|:--- |:--- |
| `Type` |Gereklidir. Parametre değeri beklenen veri türü. Herhangi bir .NET türü geçerli değil. |
| `Name` |Gereklidir. Parametrenin adı. Bu gerekir runbook içinde benzersiz olmalı ve yalnızca harf, sayı içeren veya alt çizgi karakterlerini içermeli. Bu, bir harfle başlamalıdır. |
| `Mandatory` |İsteğe bağlı. Parametresi için bir değer sağlanmalıdır olup olmadığını belirtir. Bu ayar,  **\$true**, sonra da runbook başlatılırken bir değer belirtilmelidir. Bu ayar,  **\$false**, sonra da isteğe bağlı bir değerdir. |
| `Default value` |İsteğe bağlı. Runbook başlatılırken bir değer gönderilirse değil parametresi için kullanılan bir değer belirtir. Varsayılan değer otomatik olarak parametre zorunlu ayarından bağımsız olarak isteğe bağlı hale getirir ve herhangi bir parametre için ayarlanabilir. |

Windows PowerShell, girdi parametrelerinin olanlar burada doğrulama gibi diğer adlar, listelenmiş ve parametre ayarlar çok daha fazla özniteliklerini de destekler. Ancak, Azure Otomasyonu, şu anda yalnızca yukarıdaki giriş parametrelerini destekler.

PowerShell iş akışı runbook'ları bir parametre tanımında birden çok parametre virgüllerle ayrıldığı aşağıdaki genel biçimi vardır.

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
> Ne zaman tanımladığınız parametreleri belirtmezseniz **zorunlu** özniteliği sonra varsayılan olarak, parametrenin isteğe bağlı olarak kabul edilir. PowerShell iş akışı runbook'ları bir parametre için varsayılan bir değer ayarlarsanız, ayrıca, PowerShell tarafından isteğe bağlı bir parametre olarak bağımsız olarak işlem görür **zorunlu** öznitelik değeri.

Örneğin, sanal makineler, tek bir VM'nin ya da bir kaynak grubu içindeki tüm sanal makineler hakkında ayrıntıları veren bir PowerShell iş akışı runbook giriş parametrelerini yapılandıralım. Bu runbook aşağıdaki ekran görüntüsünde gösterildiği gibi yalnızca iki parametreye sahiptir: sanal makine ve kaynak grubunun adı.

![Otomasyon PowerShell iş akışı](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

Bu parametre tanımında parametre  **\$VMName** ve  **\$resourceGroupName** dize türündeki basit parametrelerdir. Bununla birlikte, PowerShell ve PowerShell iş akışı runbook'ları tüm basit türleri ve karmaşık türler gibi destekleyen **nesne** veya **PSCredential** giriş parametrelerine ilişkin.

Runbook'unuz bir nesne türü giriş parametresi varsa, ardından (ada, değere) içeren bir PowerShell hashtable kullanmak bir değer geçirmek için çiftleri. Örneğin, bir runbook'ta şu parametre varsa:

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

Ardından parametre şu değeri geçirebilirsiniz:

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```

> [!NOTE]
> Gönderdiğinizde herhangi bir değer için bir isteğe bağlı `[String]` tür parametresi olan bir _varsayılan değer_ , `\$null`, parametrenin değerini sonra bir _boş dize_, **değil** `\$null`.

## <a name="configure-input-parameters-in-graphical-runbooks"></a>Grafik runbook'larında girdi parametrelerini yapılandırma

İçin [grafik runbook yapılandırma](automation-first-runbook-graphical.md) giriş parametreleriyle ya da sanal makineleri ayrıntılarını veren bir grafik runbook tek bir VM veya tüm VM'ler içinde bir kaynak grubu oluşturalım. Bir runbook yapılandırma, aşağıda açıklandığı gibi iki önemli etkinliklerden oluşur.

[**Azure farklı çalıştır hesabıyla Runbook kimlik doğrulaması** ](automation-sec-configure-azure-runas-account.md) Azure ile kimlik doğrulaması.

[**Get-AzureRmVm** ](/powershell/module/azurerm.compute/get-azurermvm) bir sanal makinenin özelliklerini almak için.

Kullanabileceğiniz [ **Write-Output** ](/powershell/module/microsoft.powershell.utility/write-output) sanal makinelerin adlarını çıktısını almak için etkinlik. Etkinlik **Get-AzureRmVm** iki parametre kabul eden **sanal makine adı** ve **kaynak grubu adı**. Bu parametreler, runbook'u her başlattığınızda farklı değerler gerektirebilir olduğundan, giriş parametreleri runbook'unuza ekleyebilirsiniz. Giriş parametreleri eklemek için adımlar şunlardır:

1. Grafik runbook seçin **runbook'ları** dikey penceresinde ve ardından [ **Düzenle** ](automation-graphical-authoring-intro.md) bu.
2. Runbook Düzenleyicisi'nden tıklayın **giriş ve çıkış** açmak için **giriş ve çıkış** dikey penceresi.

   ![Otomasyonu grafiksel runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)

3. **Giriş ve çıkış** dikey penceresinde runbook için tanımlanan giriş parametreleri bir listesini görüntüler. Bu dikey pencerede, yeni bir giriş parametresi eklemek veya var olan bir giriş parametresi yapılandırmasını düzenleyin. Runbook için yeni bir parametre eklemek için tıklatın **Girişi Ekle** açmak için **Runbook girdi parametreniz** dikey penceresi. Burada, aşağıdaki parametreleri yapılandırabilirsiniz:

   | **Özellik** | **Açıklama** |
   |:--- |:--- |
   | `Name` |Gereklidir. Parametrenin adı. Bu gerekir runbook içinde benzersiz olmalı ve yalnızca harf, sayı içeren veya alt çizgi karakterlerini içermeli. Bu, bir harfle başlamalıdır. |
   | `Description` |İsteğe bağlı. Giriş parametresi amacı hakkında açıklama. |
   | `Type` |İsteğe bağlı. Parametre değeri beklenen veri türü. Desteklenen parametre türleri **dize**, **Int32**, **Int64**, **ondalık**, **Boole**,  **DateTime**, ve **nesne**. Bir veri türü seçili değilse, varsayılan **dize**. |
   | `Mandatory` |İsteğe bağlı. Parametresi için bir değer sağlanmalıdır olup olmadığını belirtir. Seçerseniz **Evet**, sonra da runbook başlatılırken bir değer belirtilmelidir. Seçerseniz **hiçbir**, bir değer runbook'u başlatan ve varsayılan değer ayarlama gerekli değildir. |
   | `Default Value` |İsteğe bağlı. Runbook başlatılırken bir değer gönderilirse değil parametresi için kullanılan bir değer belirtir. Zorunlu olmayan bir parametre için varsayılan bir değer ayarlanabilir. Varsayılan değer ayarlamak için **özel**. Runbook başlatılırken başka bir değer sağlanmadığı sürece bu değeri kullanılır. Seçin **hiçbiri** herhangi bir varsayılan değer sağlamak istemiyorsanız. |

    ![Yeni giriş Ekle](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Tarafından kullanılan aşağıdaki özelliklere sahip iki parametre oluşturma **Get-AzureRmVm** etkinlik:

   * **Parametre1:**
     * Adı - VMName
     * Türü - dize
     * Zorunlu - yok
   * **Parametre2:**
     * Adı - resourceGroupName
     * Türü - dize
     * Zorunlu - yok
     * Varsayılan değer - özel
     * Özel varsayılan değer - \<sanal makineleri içeren kaynak grubunun adı\>

5. Parametreleri ekledikten sonra tıklayın **Tamam**. Artık bunları görüntüleyebilirsiniz **giriş ve çıkış sayfası**. Tıklayın **Tamam** yeniden ve ardından **Kaydet** ve **Yayımla** runbook'unuzu.

## <a name="configure-input-parameters-in-python-runbooks"></a>Python runbook'ları girdi parametrelerini yapılandırma

Python runbook'ları, PowerShell, PowerShell iş akışı ve grafik runbook'ları farklı olarak, adlandırılmış parametreler almaz.
Tüm giriş parametrelerini bağımsız değişken değerlerini bir dizi olarak ayrıştırılır.
İçeri aktararak dizi erişim `sys` Python betiğini ve ardından kullanarak modüle `sys.argv` dizisi.
Dikkat etmeniz önemlidir dizinin ilk öğesi `sys.argv[0]`, ilk gerçek giriş parametresi, betiğin adı olduğundan `sys.argv[1]`.

Bir Python runbook giriş parametrelerini kullanma örneği için bkz: [Azure automation'da ilk Python runbook'um](automation-first-runbook-textual-python2.md).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Runbook'lardaki parametre girişi için değerler atayın

Runbook'ları aşağıdaki senaryolarda parametrelerinde giriş değerleri geçirebilirsiniz:

### <a name="start-a-runbook-and-assign-parameters"></a>Bir runbook başlatın ve parametreleri atayın

Bir runbook birçok yolu başlatılabilir: bir Web kancası ile PowerShell cmdlet'leriyle, REST API'si ile veya SDK'sı Azure Portalı aracılığıyla. Aşağıda bir runbook başlatma ve parametreleri atamak için farklı yöntemler ele alır.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Azure portalını kullanarak yayımlanan bir runbook başlatın ve parametreleri atayın

Olduğunda, [runbook'u başlatmak](start-runbooks.md#start-a-runbook-with-the-azure-portal), **Runbook'u Başlat** dikey penceresi açılır ve oluşturduğunuz parametreleri için değerler girebilirsiniz.

![Portalı kullanmaya başlayın](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Giriş kutusuna altındaki etiketinde parametresi için ayarladığınız öznitelikleri görebilirsiniz. Öznitelikler, zorunlu veya isteğe bağlı, türü ve varsayılan değeri içerir. Parametre adının yanındaki Yardım balonu giriş parametre değerleri ile ilgili kararlar için gereken tüm anahtar bilgileri görebilirsiniz. Bu bilgiler, bir parametre zorunlu veya isteğe bağlı olup olmadığını içerir. Ayrıca türü ve varsayılan değer (varsa) ve diğer yararlı olan notlar içerir.

> [!NOTE]
> Dize türü parametre desteği **boş** dize değerleri.  Girme **[oluşması]** giriş parametresinde kutusu boş bir dize parametresi geçirir. Ayrıca, tür parametreleri dize desteklemeyen **Null** geçirilen değer. Dize parametresi için herhangi bir değer geçirmezseniz sonra PowerShell, null olarak yorumlar.

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>PowerShell cmdlet'lerini kullanarak yayımlanan bir runbook başlatın ve parametreleri atayın

* **Azure Resource Manager cmdlet'leri:** Bir kaynak grubunda kullanılarak oluşturulmuş bir Otomasyon runbook'u başlatabilirsiniz [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook).
  
  **Örnek:**

  ```powershell
  $params = @{"VMName"="WSVMClassic";"resourceGroupeName"="WSVMClassicSG"}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" –ResourceGroupName $resourceGroupName -Parameters $params
  ```

* **Azure Klasik dağıtım modeli cmdlet'leri:** Varsayılan kaynak grubunda kullanılarak oluşturulmuş bir Otomasyon runbook'u başlatabilirsiniz [başlangıç AzureAutomationRunbook](/powershell/module/servicemanagement/azure/start-azureautomationrunbook).
  
  **Örnek:**

  ```powershell
  $params = @{"VMName"="WSVMClassic"; "ServiceName"="WSVMClassicSG"}
  
  Start-AzureAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" -Parameters $params
  ```

> [!NOTE]
> Varsayılan parametre, PowerShell cmdlet'lerini kullanarak bir runbook'u başlattığınızda **MicrosoftApplicationManagementStartedBy** değeri ile oluşturulan **PowerShell**. Bu parametrede görüntüleyebileceğiniz **iş ayrıntıları** sayfası.  

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Bir SDK'sını kullanarak bir runbook'u başlatmak ve parametreleri atayın

* **Azure Resource Manager yöntemi:** Bir programlama dili, SDK'sını kullanarak bir runbook başlatabilirsiniz. Otomasyon hesabınızda bir runbook başlatmak için bir C# kod parçacığı aşağıdadır. Tüm kodu görüntüleyebilirsiniz bizim [GitHub deposu](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  

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

* **Azure Klasik dağıtım modeli yöntemi:** Bir programlama dili, SDK'sını kullanarak bir runbook başlatabilirsiniz. Otomasyon hesabınızda bir runbook başlatmak için bir C# kod parçacığı aşağıdadır. Tüm kodu görüntüleyebilirsiniz bizim [GitHub deposu](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).

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

  Bu yöntem başlatmak için runbook parametreleri depolamak için Sözlük oluşturma **VMName** ve **resourceGroupName**ve bunların değerleri. Daha sonra runbook'u başlatın. Yukarıda tanımlanan yöntemini çağırmak için C# kod parçacığı aşağıda verilmiştir.

  ```csharp
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook("Get-AzureVMGraphical", RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>REST API kullanarak bir runbook'u başlatmak ve parametreleri atayın

Bir runbook işi oluşturulabilir ve Azure Otomasyonu REST API ile kullanmaya **PUT** aşağıdaki istek URI'si ile yöntemi: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Automation/automationAccounts/{automationAccountName}/jobs/{jobName}?api-version=2017-05-15-preview`


İstek URI'si aşağıdaki parametreleri değiştirin:

* **Subscriptionıd:** Azure abonelik kimliğinizi  
* **resourceGroupName:** Otomasyon hesabı için kaynak grubunun adı.
* **automationAccountName:** Belirtilen bulut Hizmeti'nde barındırılan Otomasyon hesabınızın adı.  
* **jobName:** İş için GUID. PowerShell'de GUID'ler kullanılarak oluşturulabilir **[GUID]::NewGuid(). ToString()** komutu.

Runbook işi için parametreleri geçirmek için istek gövdesi kullanın. JSON biçiminde sağlanan aşağıdaki iki özelliği sürer:

* **Runbook adı:** Gereklidir. İşin başlatılması için runbook'un adı.  
* **Runbook parametreleri:** İsteğe bağlı. Burada adı dize türünde olmalıdır ve değer geçerli bir JSON değeri olabilir (ada, değere) parametre listesinde sözlüğü biçimlendirin.

Başlamak isterseniz **Get-AzureVMTextual** ile daha önce oluşturulan runbook **VMName** ve **resourceGroupName** için şu JSON biçimini parametreleri kullanma istek gövdesi.

   ```json
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WindowsVM",
         "resourceGroupName":"ContosoSales"}
        }
    }
   ```

Proje başarıyla oluşturulursa, bir HTTP durum kodu 201 döndürülür. Yanıt üst bilgileri ve yanıt gövdesinin hakkında daha fazla bilgi için nasıl hakkındaki makaleye bakın [REST API kullanarak bir runbook işi oluşturma.](/rest/api/automation/job/create)

### <a name="test-a-runbook-and-assign-parameters"></a>Bir runbook'u test etme ve parametreleri atayın

Olduğunda, [, runbook'un taslak sürümünü test](automation-testing-runbook.md) test seçeneğini kullanarak **Test** sayfası açılır ve oluşturduğunuz parametreleri için değerleri yapılandırabilirsiniz.

![Test ve ata parametreleri](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Bir zamanlamayı runbook'a bağlamak ve parametreleri atayın

Yapabilecekleriniz [bir zamanlama Bağla](automation-schedules.md) runbook'unuzu için böylece belirli bir zamanda runbook başlatır. Zamanlama oluşturma ve zamanlamayı başlatıldığında runbook bu değerleri kullanır. giriş parametrelerini atayın. Tüm zorunlu parametre değerlerini sağlanana kadar zamanlaması kaydedilemiyor.

![Zamanlama ve parametreleri atama](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Bir runbook için bir Web kancası oluşturmanız ve parametreleri atayın

Oluşturabileceğiniz bir [Web kancası](automation-webhooks.md) runbook'unuzu için ve runbook girdi parametrelerini yapılandırma. Tüm zorunlu parametre değerlerini sağlanana kadar Web kancası kaydedemezsiniz.

![Web kancası oluşturma ve atama parametreleri](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Web kancası, önceden tanımlanmış giriş parametresini kullanarak bir runbook'u çalıştırdığınızda **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** tanımlamış giriş parametreleri birlikte gönderilir. Genişletmek için tıklatın **WebhookData** daha fazla ayrıntı için parametre.

![WebhookData parametresi](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="pass-a-json-object-to-a-runbook"></a>Bir runbook için bir JSON nesnesi geçirme

Bir runbook'ta bir JSON dosyası betiğine geçirmek istediğiniz verileri depolamak için yararlı olabilir.
Örneğin, tüm bir runbook'a geçirmek istediğiniz parametreler içeren bir JSON dosyası oluşturabilirsiniz. Bunu yapmak için JSON bir dizeye Dönüştür ve runbook'a geçirilmeden önce dizenin bir PowerShell nesnesine dönüştürmek sahip.

Bu örnekte, çağıran bir PowerShell komut dosyası sahip [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) JSON içeriği runbook'a geçirerek bir PowerShell runbook'u başlatın.
PowerShell runbook parametreleri geçirilen JSON VM için almaya bir Azure VM'yi yeniden başlatır.

### <a name="create-the-json-file"></a>JSON dosyası oluşturun

Bir metin dosyasına şu test yazın ve kaydedileceği `test.json` yerel bilgisayarınızda bir yerde.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

### <a name="create-the-runbook"></a>Runbook oluşturma

"Test-Json" Azure automation'da adlı yeni bir PowerShell runbook oluşturun.
Yeni bir PowerShell runbook'u oluşturma konusunda bilgi almak için bkz: [ilk PowerShell runbook'um](automation-first-runbook-textual-powershell.md).

JSON verilerini kabul etmek için runbook giriş parametresi olarak bir nesne gerçekleştirmeniz gerekir.

Runbook daha sonra JSON biçiminde tanımlanan özellikleri kullanabilirsiniz.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
```

Kaydedin ve bu runbook Otomasyon hesabınızda yayımlayın.

### <a name="call-the-runbook-from-powershell"></a>Powershell'den runbook'u çağırma

Artık Azure PowerShell kullanarak yerel makinenizde runbook çağırabilirsiniz.
Aşağıdaki PowerShell komutlarını çalıştırın:

1. Azure'da oturum açın:

   ```powershell
   Connect-AzureRmAccount
   ```

   Azure kimlik bilgilerinizi girmeniz istenir.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

1. JSON dosyasının içeriği almak ve bir dizeye dönüştürün:

   ```powershell
   $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
   ```

   `JsonPath` JSON dosyasını kaydettiğiniz yoludur.

1. Dize içeriklerini dönüştürme `$json` bir PowerShell nesnesi için:

   ```powershell
   $JsonParams = @{"json"=$json}
   ```

1. Parametre için bir karma tablo oluşturma `Start-AzureRmAutomationRunbook`:

   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```

   Değerini tutunun fark `Parameters` JSON dosyasından değerleri içeren bir PowerShell nesnesine.
1. Runbook’u başlatma

   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Bir runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz: [runbook başlatma](automation-starting-a-runbook.md).
* Metinsel bir runbook'u düzenlemek için başvurmak [metin runbook'ları düzenleme](automation-edit-textual-runbook.md).
* Grafik runbook'u düzenlemek için başvurmak [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md).
