---
title: Azure anahtar kasası ayarlama ile uçtan uca anahtar döndürme ve denetleme - Azure Key Vault | Microsoft Docs
description: Anahtar döndürme ve izleme anahtar kasası günlükleri ile ayarlanmasını yardımcı olması için bu nasıl yapılır'ı kullanın.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: ''
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: barclayn
ms.openlocfilehash: 11506f63089564b5187ebd6177f7187f1faf6655
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999603"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Azure anahtar kasası anahtar döndürme ve denetleme ile ayarlama

## <a name="introduction"></a>Giriş

Bir anahtar kasası oluşturduktan sonra anahtarları ve parolaları saklamak için kullanmaya başlayabilirsiniz. Uygulamalarınız artık anahtarları veya gizli kalıcı hale getirmek gerekmez ancak bunları gerektiği şekilde kasadan talep edebilir. Bu anahtarları ve gizli anahtarı ve gizli dizi yönetimi olanakları kapsamını ayarlama açılır uygulamanızın davranışını etkilemeden güncelleştirmenizi sağlar.

>[!IMPORTANT]
> Bu makaledeki örnekler yalnızca gösterim amacıyla sağlanır. Üretim amacıyla kullanılması amaçlanmamıştır. 

Bu makalede açıklanmaktadır:

- Bir gizli dizi depolamak için Azure anahtar Kasası'nı kullanarak bir örnek. Bu öğreticide, depolanan gizli bir uygulama tarafından erişilen Azure depolama hesabı anahtardır. 
- Ayrıca, bu depolama hesabı anahtarı zamanlanmış bir dönüş uygulaması gösterir.
- Bu anahtar kasası denetim günlüklerini izleyin ve beklenmeyen bir istek yapıldığında oluşturulacak gösterilmektedir.

> [!NOTE]
> Bu öğreticide ilk anahtar kasanızı kurulumunun ayrıntılı olarak açıklayan yönelik değildir. Bu bilgi için bkz. [Azure Anahtar Kasası ile çalışmaya başlama](key-vault-get-started.md). Platformlar arası komut satırı arabirimi yönergeleri için bkz: [yönetme CLI ile anahtar kasası](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Anahtar Kasası ayarlama

Key Vault'tan bir gizli dizi almak bir uygulamanın işlemesini etkinleştirmek için ilk gizli dizi oluşturun ve kasanıza karşıya yükleyin. Bu, bir Azure PowerShell oturumu başlatarak ve aşağıdaki komutla Azure hesabınızda oturum açarken gerçekleştirilebilir:

```powershell
Connect-AzureRmAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. PowerShell Bu hesapla ilişkili tüm abonelikleri alır. PowerShell, varsayılan olarak birinciyi kullanır.

Birden fazla aboneliğiniz varsa, anahtar kasanızı oluşturmak için kullanılan bir tane belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek için aşağıdakileri girin:

```powershell
Get-AzureRmSubscription
```

Günlüğünü anahtar kasasıyla ilişkili aboneliği belirtmek için şunu girin:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Bu makalede bir gizli dizi bir depolama hesabı anahtarını depolamak gösterir çünkü bu depolama hesabı anahtarı almanız gerekir.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Gizli anahtarı (Bu durumda, depolama hesabı anahtarınızı) aldıktan sonra bir güvenli dizeye dönüştürün ve anahtar kasanıza'da bu değerle bir gizli dizi oluşturmak gerekir.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

Ardından, oluşturduğunuz için gizli anahtar URI'sini alın. Gizli anahtarı almak için anahtar kasası çağırdığınızda bu daha sonraki bir adımda kullanılır. Aşağıdaki PowerShell komutunu çalıştırın ve gizli URI kimlik değerini not edin:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Uygulamasını ayarlama

Depolanan bir gizli dizi olduğuna göre almak ve kullanmak için kodu kullanabilirsiniz. Bunu başarmak için gereken birkaç adım vardır. İlk ve en önemli adım, uygulamanızın Azure Active Directory'ye kaydetme ve uygulamanızı gelen isteklere izin verebilir böylece Key Vault, uygulama bilgilerini belirten.

> [!NOTE]
> Uygulamanız aynı Azure Active Directory Kiracı anahtar kasanıza olarak oluşturulmalıdır.
>
>

1. Azure Active Directory'ye gidin.
2. Seçin **uygulama kayıtları** 
3. Seçin **yeni uygulama kaydı** Azure Active Directory için bir uygulama eklemek için.

    ![Uygulamaları Azure Active Directory'de açın](./media/keyvault-keyrotation/azure-ad-application.png)

4. İçinde **Oluştur** bölüm uygulama türü olarak bırakın **WEB uygulaması ve/veya WEB API'si** ve uygulamanızı bir ad verin. Uygulamanızı vermek bir **oturum açma URL'si**. Bu Tanıtım için istediğiniz herhangi bir şey olabilir.

    ![Uygulama kaydı oluşturma](./media/keyvault-keyrotation/create-app.png)

5. Uygulamayı Azure Active Directory'ye eklendikten sonra uygulama sayfasına yönlendirilirsiniz. Seçin **ayarları** ve Özellikler'i seçin. Kopyalama **uygulama kimliği** değeri. Sonraki adımlarda gerekecektir.

Ardından, Azure Active Directory ile etkileşim kurabilmesi uygulamanız için bir anahtar oluşturun. Bir anahtarın altında giderek oluşturabileceğiniz **anahtarları** bölümüne **ayarları**. Yeni oluşturulan anahtarını Azure Active Directory uygulamanızdan kullanmak için daha sonraki bir adımda not edin. Bu bölümün dışına ilerledikten sonra anahtar kullanılabilir olmayacağını dikkat edin. 

![Azure Active Directory uygulama anahtarları](./media/keyvault-keyrotation/create-key.png)

Anahtar kasası uygulamanıza çağrıları oluşturmadan önce anahtar kasası uygulamanızı ve izinlerini bildirmeniz gerekir. Kasa adı ve uygulama kimliği verir ve Azure Active Directory uygulaması aşağıdaki komutu alır **alma** uygulaması için anahtar kasanıza erişim.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Bu noktada, uygulama çağrılarınızı oluşturmaya başlamak hazırsınız. Uygulamanızda Azure anahtar kasası ve Azure Active Directory ile etkileşim kurmak için gerekli NuGet paketlerini yüklemeniz gerekir. Visual Studio Paket Yöneticisi konsolundan aşağıdaki komutları girin. Bu makalede yazılmasını ve Azure Active Directory paketinin geçerli sürümü 3.10.305231913, olduğundan, en son sürümünü onaylamak ve buna uygun güncelleştirmek isteyebilirsiniz.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Uygulama kodunuzda, Azure Active Directory kimlik doğrulama yöntemi için bir sınıf oluşturun. Bu örnekte, bu sınıf olarak adlandırılır **Utils**. Şu deyimi kullanarak aşağıdakileri ekleyin:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Ardından, Azure Active Directory JWT belirteci almak için aşağıdaki yöntemi ekleyin. Bakım için sabit kodlanmış dize değerleri web ya da uygulama yapılandırma taşımak isteyebilirsiniz.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Key Vault çağırın ve gizli bilgilerinizi değerini almak için gereken kodu ekleyin. Aşağıdaki ilk eklemelisiniz using deyimi:

```csharp
using Microsoft.Azure.KeyVault;
```

Key Vault çağırmak ve gizli anahtarı almak için yöntem çağrıları ekleyin. Bu yöntemde, gizli bir önceki adımda kaydettiğiniz bir URI sağlayın. Kullanımına dikkat edin **GetToken** yönteminden **Utils** daha önce oluşturduğunuz sınıfı.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Uygulamanızı çalıştırdığınızda, artık olmalısınız Azure Active Directory kimlik doğrulaması ve ardından Azure Key Vault'tan gizli dizi, değer alınıyor.

## <a name="key-rotation-using-azure-automation"></a>Azure Otomasyonu ile anahtar döndürme

Değerleri Azure Key Vault gizli dizileri depolamak için bir dönüş stratejisi uygulamak için çeşitli seçenekler vardır. Gizli anahtarları el ile işleminin bir parçası döndürülebilir, bunlar programlı olarak API çağrıları kullanarak döndürülmesi veya bir Otomasyon betiği yoluyla Döndürülmüş. Bir Azure depolama hesabı erişim anahtarı değiştirmek için bu makalenin amaçları için Azure Otomasyonu ile birlikte Azure PowerShell kullanacaksınız. Ardından, bu yeni anahtarla bir anahtar kasası gizli dizi güncelleştirir.

Anahtar kasanızda gizli değerlerini ayarlamak Azure Otomasyonu izin vermek için Azure Otomasyonu örneğinizin kurulan oluşturulduğu AzureRunAsConnection adlı bağlantı için istemci Kimliğini almanız gerekir. Bu kimliği seçerek bulabilirsiniz **varlıklar** , Azure Otomasyonu örneğinden. Burada, seçtiğiniz **bağlantıları** seçip **AzureRunAsConnection** hizmet ilkesi. Not **uygulama kimliği**.

![Azure Otomasyonu istemci kimliği](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

İçinde **varlıklar**, seçin **modülleri**. Gelen **modülleri**seçin **galeri**, için arama yapın ve **alma** her aşağıdaki modül sürümlerini güncelleştirildi:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> Bu makalede yazılmasını yalnızca daha önce not ettiğiniz modüller için aşağıdaki betiği güncelleştirilmesi gerekmiyor. Otomasyon iş başarısız olduğunu fark ederseniz, gerekli olan tüm modülleri ve bunların bağımlılıklarını içeri aktardığınızı onaylayın.
>
>

Azure Otomasyonu bağlantınız için uygulama Kimliğini aldıktan sonra bu uygulamayı kasanızdaki gizli anahtarları güncelleştirme erişimi olduğunu, anahtar kasanıza söylemeniz gerekir. Bu, aşağıdaki PowerShell komutu ile gerçekleştirilebilir:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Ardından, **runbook'ları** Azure Otomasyonu örneği ve ardından altındaki **Runbook Ekle**. **Hızlı Oluştur**’u seçin. Seçin ve runbook'unuzu ad **PowerShell** runbook türü. Bir açıklama eklemek için seçeneğiniz vardır. Son olarak, tıklayın **Oluştur**.

![Runbook oluşturma](./media/keyvault-keyrotation/Create_Runbook.png)

Yeni runbook'unuz Düzenleyicisi bölmesinde aşağıdaki PowerShell betiğini yapıştırın:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Düzenleyici bölmesinden seçin **Test bölmesi** kodunuzu test etmek için. Komut hatasız çalışır duruma geçtikten sonra seçebileceğiniz **Yayımla**, ve ardından geri runbook yapılandırma bölmesinde runbook için bir zamanlama uygulayabilirsiniz.

## <a name="key-vault-auditing-pipeline"></a>Anahtar kasası denetim işlem hattı
Bir anahtar kasası ayarlama, anahtar Kasası'na erişim isteklerini üzerinde günlükleri toplamak için Denetim kapatabilirsiniz. Bu günlükler, belirlenen bir Azure depolama hesabında depolanır ve çıkışı, izlenen ve analiz çekilebilir. Aşağıdaki senaryoyu, web uygulamasının uygulama kimliği eşleşen uygulama gizli dizileri kasadan aldığında bir e-posta göndermek için bir işlem hattı oluşturmak için Azure işlevleri, Azure logic apps ve anahtar kasası denetim günlükleri'ni kullanır.

İlk olarak, anahtar kasanızı günlüğünü etkinleştirmeniz gerekir. Bu aşağıdaki PowerShell komutları yapılabilir (en ayrıntılı görülebilir [anahtar kasası günlüğü](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Bu etkinleştirildikten sonra belirtilen depolama hesabına toplama başlangıç denetim günlükleri. Bu günlükler, anahtar kasalarınıza nasıl ve ne zaman erişildiğini ve kim tarafından olayları içerir.

> [!NOTE]
> Günlük bilgilerinize anahtar kasası işleminden sonra 10 dakika erişebilirsiniz. Genellikle bu değerden daha hızlı olacaktır.
>
>

Sonraki adım [bir Azure Service Bus kuyruğu oluşturma](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Anahtar kasası denetim günlükleri burada itilir budur. Denetim günlüğü iletileri sıraya olduğunda, mantıksal uygulama tarafından toplanır ve bunlar üzerinde çalışır. Service bus, aşağıdaki adımlarla oluşturun:

1. (2. adım, atlama için kullanmak istediğiniz bir tane varsa) bir Service Bus ad alanı oluşturun.
2. Azure portalında hizmet veri yoluna göz atın ve kuyrukta oluşturmak istediğiniz ad alanını seçin.
3. Seçin **kaynak Oluştur**, **Kurumsal tümleştirme**, **Service Bus**ve ardından gerekli ayrıntıları girin.
4. Ad alanı seçme ve tıklayarak Service Bus bağlantı bilgilerini seçme **bağlantı bilgilerini**. Bu bilgi için sonraki bölüme ihtiyacınız olacak.

Ardından, [Azure işlevi oluşturma](../azure-functions/functions-create-first-azure-function.md) anahtar kasası günlükleri depolama hesabında yoklama ve yeni olayları seçin. Bu, bir zamanlamaya göre tetiklenen bir işlev olacaktır.

Bir Azure işlevi oluşturmak için seçin **kaynak Oluştur**, markette Ara _işlev uygulaması_, tıklatıp **Oluştur**. Oluşturma sırasında var olan bir barındırma planı kullanın veya yeni bir tane oluşturun. Dinamik barındırma için iyileştirilmiş da. İşlev barındırma seçenekleri hakkında daha fazla ayrıntı şu adreste bulunabilir: [Azure işlevlerini ölçeklendirme](../azure-functions/functions-scale.md).

Azure işlevini oluşturulduğunda, buna gitmek ve Zamanlayıcı işlevi C seçip\#. Ardından **bu işlevi Oluştur**.

![Azure işlevleri'ni Başlat dikey penceresi](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Üzerinde **geliştirme** sekmesinde, run.csx kodu aşağıdakiyle değiştirin:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> Değişkenleri anahtar kasası günlüklerinin yazılacağı depolama hesabınıza işaret edecek şekilde önceki kodda, daha önce oluşturduğunuz hizmet veri yolu ve özel anahtar kasası depolama günlükleri yolu değiştirdiğinizden emin olun.
>
>

İşlevi, anahtar kasası günlüklerinin yazılacağı en son günlük dosyasının depolama hesabından seçer, bu dosyanın en son olayların Dallarınızla ve bunları bir Service Bus kuyruğuna gönderir. Tek bir dosyayı birden çok olayı olabileceğinden, işlev ayrıca toplanmış son etkinliğin zaman damgasını belirlemek için bakan bir sync.txt dosyası oluşturmanız gerekir. Bu, birden çok kez aynı olay gönderme yoksa sağlar. Bu sync.txt dosya karşılaşılan son olay zaman damgası içerir. Yüklendiğinde, günlükleri, doğru sıralanır emin olmak için zaman damgasını göre sıralanması gerekir.

Bu işlev için sizi birkaç Azure işlevleri'nde hazır bulunmayan ek kitaplıklar başvuru. Bunlar için Azure işlevleri'nin bunları çekmesi gerekir NuGet kullanma. Seçin **dosyaları görüntüle** seçeneği.

![Dosyaları seçeneğini görüntüleyin](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Ve aşağıdaki içeriğe sahip project.json adlı bir dosya ekleyin:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```

Bağlı **Kaydet**, Azure işlevleri, gerekli ikili dosyaları indirir.

Geçiş **tümleştir** sekmesini ve Zamanlayıcı parametresi işlev içinde kullanmak için anlamlı bir ad verin. İsteğe bağlı olarak önceki kodda, çağrılacak Zamanlayıcı bekliyor *myTimer*. Belirtin bir [CRON ifadesi](../app-service/webjobs-create.md#CreateScheduledCRON) gibi: 0 \* \* \* \* \* bir dakika çalışacak şekilde işlev neden olacak Zamanlayıcı için.

Aynı **tümleştir** sekmesinde, türündeki bir girdi ekleyin **Azure Blob Depolama**. Bu işlev tarafından attınız son olayın zaman damgası içeren sync.txt dosyasını işaret edecek. Bu işlevin parametre adı içindeki kullanılabilir olacaktır. Önceki kodda, Azure Blob depolamaya giriş parametre adı olmasını bekliyor. *inputBlob*. Sync.txt dosyanın bulunacağı depolama hesabı seçin (Bu aynı veya farklı bir depolama hesabı olabilir). Yol alanı nerede dosya biçimi {container-name}/path/to/sync.txt. yaşıyor yolunu belirtin

Çıkış türü Ekle *Azure Blob Depolama* çıktı. Bu, giriş tanımlanan sync.txt dosyaya yönlendirir. Bu işlev tarafından attınız son olayın zaman damgası yazmak için kullanılır. Yukarıdaki kod çağrılması için bu parametreyi bekliyor *outputBlob*.

Bu noktada, işlev hazırdır. Geri dönmek emin **geliştirme** sekmesini ve kodu kaydedin. Derleme hataları için çıktı penceresini denetleyin ve uygun şekilde düzeltin. Kod derlenir, ardından kod artık dakikada anahtar kasası günlükleri denetleme ve tanımlanmış bir Service Bus kuyruğu üzerine yeni meydana gelen olayları gönderme. Tetiklenen bir işlev her zaman için günlük penceresini yazılmasına yarar günlük bilgileri görmeniz gerekir.

### <a name="azure-logic-app"></a>Azure mantıksal uygulaması

Sonraki işlev Service Bus kuyruğuna göndermek, içeriği ayrıştırır ve eşleştirilen bir koşula göre bir e-posta gönderen olaylar seçer bir Azure mantıksal uygulaması oluşturmanız gerekir.

[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) giderek **yeni > mantıksal uygulama**.

Mantıksal uygulama oluşturulduktan sonra dosyayı bulun ve seçin **Düzenle**. Mantıksal uygulama Düzenleyici içinde seçin **hizmet veri yolu kuyruğu** ve Service Bus kuyruğuna bağlanmak için kimlik bilgilerinizi girin.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Sonraki seçin **koşul Ekle**. Koşul Gelişmiş düzenleyicisine geçin ve web uygulamanızın gerçek APP_ID ile APP_ID değiştirerek aşağıdaki kodu girin:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Bu ifade temelde döndürür **false** varsa *AppID* (aynı Service Bus iletisinin gövdesini)'ndan gelen olayı değil *AppID* uygulama.

Şimdi altında bir eylem oluşturun **yok ise, hiçbir şey yapma**.

![Azure mantıksal uygulama eylemi seçin](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Eylem için **Office 365 - e-postası gönderme**. Tanımlı koşul döndürdüğünde göndermek için e-posta oluşturmak için alanları doldurun **false**. Office 365 yoksa aynı sonuçları elde etmek için alternatifleri görünebilir.

Bu noktada, yeni anahtar kasası denetim günlükleri için bir dakika görünen bir uçtan uca işlem hattı sahip. Bulduğu yeni günlükleri bir service bus kuyruğuna gönderir. Mantıksal uygulama, yeni bir iletinin kuyrukta gölünüzdeki durumlarda tetiklenir. Varsa *AppID* içinde olay çağırma uygulamasının uygulama kimliği eşleşmiyor, e-posta gönderir.
