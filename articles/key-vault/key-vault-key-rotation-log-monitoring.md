---
title: Azure anahtar kasası uçtan uca anahtar döndürme ve denetleme ile ayarlama | Microsoft Docs
description: Anahtar döndürme ayarlayın ve anahtar kasası günlükleri izlemenize yardımcı olması için bu nasıl yapılır kılavuzu kullanın.
services: key-vault
author: barclayn
manager: barbkess
tags: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: 785e60ddf54a3772ae7687b9d18477ef04707609
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713689"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Azure anahtar kasası anahtar döndürme ve denetleme ile ayarlama

## <a name="introduction"></a>Giriş

Bir anahtar kasası oluşturduktan sonra anahtarları ve parolaları saklamak için kullanmaya başlayabilirsiniz. Uygulamalarınız artık anahtarları veya gizli kalıcı hale getirmek gerekmez ancak bunları gerektiği şekilde kasadan talep edebilir. Bir anahtar kasasına bir anahtar ve gizli dizi Yönetimi kapsamını ayarlama açılır uygulamanızın davranışını etkilemeden, anahtarları ve gizli anahtarları güncelleştirme sağlar.

>[!IMPORTANT]
> Bu makaledeki örnekler yalnızca gösterim amacıyla sağlanır. Bunlar üretimde kullanıma yönelik tasarlanmıştır değil. 

Bu makalede açıklanmaktadır:

- Bir gizli dizi depolamak için Azure anahtar Kasası'nı kullanarak bir örnek. Bu makalede, depolanan gizli bir uygulama tarafından erişilen Azure depolama hesabı anahtarı ' dir. 
- Bu depolama hesabı anahtarı, zamanlanmış bir döndürme gerçekleştirme.
- İzleme anahtarı, Denetim günlükleri kasa ve beklenmeyen bir istek yapıldığında oluşturulacak.

> [!NOTE]
> Bu makalede, anahtar kasanıza ilk kurulumu ayrıntılı olarak açıklayan değil. Bu bilgi için bkz. [Azure anahtar kasası nedir?](key-vault-overview.md). Platformlar arası komut satırı arabirimi yönergeleri için bkz: [yönetmek için Azure CLI kullanarak Key Vault](key-vault-manage-with-cli2.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="set-up-key-vault"></a>Anahtar Kasası ayarlama

Key Vault'tan bir gizli dizi almak bir uygulamanın işlemesini etkinleştirmek için ilk gizli dizi oluşturun ve kasanıza karşıya yükleyin.

Bir Azure PowerShell oturumu başlatın ve aşağıdaki komutla Azure hesabınızda oturum açın:

```powershell
Connect-AzAccount
```

Açılır tarayıcı penceresinde Azure hesabınız için kullanıcı adı ve parola girin. PowerShell Bu hesapla ilişkili tüm abonelikleri alır. PowerShell, varsayılan olarak birinciyi kullanır.

Birden fazla aboneliğiniz varsa, anahtar kasanızı oluşturmak için kullanılan bir tane belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek için aşağıdakileri girin:

```powershell
Get-AzSubscription
```

Günlüğe kaydedilmesi anahtar kasasıyla ilişkili aboneliği belirtmek için şunu girin:

```powershell
Set-AzContext -SubscriptionId <subscriptionID>
```

Bu makalede bir gizli dizi bir depolama hesabı anahtarını depolamak gösterir çünkü bu depolama hesabı anahtarı almanız gerekir.

```powershell
Get-AzStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Gizli anahtarı (Bu durumda, depolama hesabı anahtarınızı) aldıktan sonra bu anahtarı güvenli bir dizeye dönüştürün ve ardından bir gizli dizi ile bu değeri anahtar kasanıza oluşturmanız gerekir.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

Ardından, oluşturduğunuz için gizli anahtar URI'sini alın. Anahtar kasası çağırın ve gizli anahtarı almak için sonraki bir adımda bu URI gerekir. Aşağıdaki PowerShell komutunu çalıştırın ve parolanın URI kimlik değerini not edin:

```powershell
Get-AzKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Uygulamasını ayarlama

Depolanan bir gizli dizi olduğuna göre almak ve birkaç adım gerçekleştirildikten sonra kullanmak için kodu kullanabilirsiniz.

İlk olarak, uygulamanızı Azure Active Directory'ye kaydetmeniz gerekir. Uygulamanızı gelen isteklere izin verebilir böylece Key Vault, uygulama bilgilerinizi söyleyin.

> [!NOTE]
> Uygulamanız aynı Azure Active Directory Kiracı anahtar kasanıza olarak oluşturulmalıdır.

1. Açık **Azure Active Directory**.
2. **Uygulama kayıtları**'nı seçin. 
3. Seçin **yeni uygulama kaydı** Azure Active Directory için bir uygulama eklemek için.

    ![Uygulamaları Azure Active Directory'de açın](./media/keyvault-keyrotation/azure-ad-application.png)

4. Altında **Oluştur**, uygulama türü olarak bırakın **Web uygulaması / API** ve uygulamanızı bir ad verin. Uygulamanızı vermek bir **oturum açma URL'si**. Bu URL, bu tanıtım için istediğiniz herhangi bir şey olabilir.

    ![Uygulama kaydı oluşturma](./media/keyvault-keyrotation/create-app.png)

5. Uygulamayı Azure Active Directory'ye eklendikten sonra uygulama sayfası açılır. Seçin **ayarları**ve ardından **özellikleri**. Kopyalama **uygulama kimliği** değeri. Sonraki adımlarda gerekir.

Ardından, Azure Active Directory ile etkileşim kurabilmesi uygulamanız için bir anahtar oluşturun. Bir anahtar oluşturmak için Seç **anahtarları** altında **ayarları**. Azure Active Directory uygulamanız için yeni oluşturulan anahtarı not edin. Daha sonraki bir adımda gerekecektir. Bu bölümde çıktığınızda anahtar kullanılabilir olmaz. 

![Azure Active Directory uygulama anahtarları](./media/keyvault-keyrotation/create-key.png)

Anahtar kasası uygulamanıza çağrıları kurmadan önce anahtar kasası uygulamanızı ve izinlerini hakkında söylemeniz gerekir. Kasa adı ve uygulama Kimliğini Azure Active Directory uygulamanızdan bir uygulama vermek için aşağıdaki komutu kullanır **alma** anahtar kasanıza erişim.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Artık uygulama çağrılarınızı oluşturmaya başlamak hazırsınız. Uygulamanızda Azure anahtar kasası ve Azure Active Directory ile etkileşim kurmak için gerekli NuGet paketlerini yüklemeniz gerekir. Visual Studio Paket Yöneticisi konsolundan aşağıdaki komutları girin. Bu makalenin yazma, Azure Active Directory paketinin geçerli sürüm 3.10.305231913., böylelikle en son sürümünü onaylamak ve gerektiği gibi güncelleştirin.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Uygulama kodunuzda, Azure Active Directory kimlik doğrulama yöntemi için bir sınıf oluşturun. Bu örnekte, bu sınıf olarak adlandırılır **Utils**. Aşağıdaki `using` deyimi:

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

Key Vault çağırın ve gizli bilgilerinizi değerini almak için gereken kodu ekleyin. İlk olarak, aşağıdaki ekleyin `using` deyimi:

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

> [!IMPORTANT]
> Azure Otomasyonu runbook'ları, yine de kullanılmasını gerektirir `AzureRM` modülü.

Değerlerin Key Vault gizli dizileri depolamak için bir dönüş stratejisi ayarlamak artık hazırsınız. Gizli dizileri çeşitli yollarla döndürülebilir:

- El ile işleminin bir parçası
- API çağrıları program aracılığıyla kullanarak
- Bir Azure Otomasyonu betiği

Bu makalenin amaçları için bir Azure depolama hesabının erişim anahtarı değiştirmek için Azure Otomasyonu ile birleştirilmiş PowerShell kullanacaksınız. Ardından, bu yeni anahtarla bir anahtar kasası gizli dizi güncelleştireceksiniz.

Azure Automation'ı, anahtar kasasında gizli dizi değerlerini ayarlamak izin vermek için adlı bağlantı için istemci kimliği almalısınız **AzureRunAsConnection**. Azure Otomasyonu örneğinizin kurulan olduğunda bu bağlantı oluşturuldu. Bu kodu bulmak için seçin **varlıklar** , Azure Otomasyonu örneğinden. Buradan seçin **bağlantıları**ve ardından **AzureRunAsConnection** hizmet sorumlusu. Not **ApplicationId** değeri.

![Azure Otomasyonu istemci kimliği](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

İçinde **varlıklar**seçin **modülleri**. Seçin **galeri**, bulun ve her biri aşağıdaki modüller güncelleştirilmiş sürümlerini içeri aktarın:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage

> [!NOTE]
> Bu makalede yazılmasını yalnızca daha önce not ettiğiniz modüller için aşağıdaki betiği güncelleştirilmesi gerekmiyor. Otomasyon iş başarısız olursa, tüm gerekli modülleri ve bunların bağımlılıklarını içe aktardığınız onaylayın.

Azure Otomasyonu bağlantınız için uygulama Kimliğini aldıktan sonra bu uygulamayı kasanızdaki gizli anahtarları güncelleştirme izni olduğunu anahtar kasanıza söylemeniz gerekir. Aşağıdaki PowerShell komutunu kullanın:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Ardından, **runbook'ları** Azure Otomasyonu örneği ve ardından altındaki **Runbook Ekle**. **Hızlı Oluştur**’u seçin. Runbook uygulamanızı adlandırın ve seçin **PowerShell** runbook türü. Bir açıklama ekleyebilirsiniz. Son olarak, seçin **Oluştur**.

![Runbook oluşturma](./media/keyvault-keyrotation/Create_Runbook.png)

Yeni runbook'unuz Düzenleyicisi bölmesinde aşağıdaki PowerShell betiğini yapıştırın:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection"
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

# Optionally you can set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureRmKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Düzenleyicisi bölmesinde seçin **Test bölmesi** kodunuzu test etmek için. Hatasız betik çalıştıktan sonra seçebileceğiniz **Yayımla**, ve ardından runbook yapılandırma bölmesinde runbook için bir zamanlama uygulayabilirsiniz.

## <a name="key-vault-auditing-pipeline"></a>Anahtar kasası denetim işlem hattı

Bir anahtar kasası ayarlama, anahtar Kasası'na erişim isteklerini üzerinde günlükleri toplamak için Denetim kapatabilirsiniz. Bu günlükler, belirtilen Azure depolama hesabında depolanır ve çıkışı, izlenen ve analiz çekilebilir. Aşağıdaki senaryoyu, web uygulamasının uygulama kimliği eşleşmeyen bir uygulama gizli dizileri kasadan aldığında bir e-posta gönderen bir işlem hattı oluşturmak için Azure işlevleri, Azure logic apps ve anahtar kasası denetim günlükleri'ni kullanır.

İlk olarak, anahtar kasanızı günlüğünü etkinleştirmeniz gerekir. Aşağıdaki PowerShell komutlarını kullanın. (Tam Ayrıntılar gördüğünüz [bu makalede anahtar kasası günlüğü hakkında](key-vault-logging.md).)

```powershell
$sa = New-AzStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzKeyVault -VaultName '<vaultName>'
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Günlük tutma etkinleştirildikten sonra Denetim günlükleri belirtilen depolama hesabında saklanmasını başlatın. Bu günlükler, anahtar kasalarınıza nasıl ve ne zaman erişildiğini ve kim tarafından olayları içerir.

> [!NOTE]
> Günlük bilgilerinize anahtar kasası işleminden sonra 10 dakika erişebilirsiniz. Genellikle daha çabuk kullanılabilir olacaktır.

Sonraki adım [bir Azure Service Bus kuyruğu oluşturma](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Anahtar kasası denetim günlükleri burada itilir bu kuyruğudur. Denetim günlüğü iletileri sıraya olduğunda, mantıksal uygulama tarafından toplanır ve bunlar üzerinde çalışır. Bir Service Bus örneği aşağıdaki adımlarla oluşturun:

1. (Zaten istediğiniz kullanmak için 2. adıma atlayın varsa) bir Service Bus ad alanı oluşturun.
2. Azure portalında Service Bus örneğine göz atın ve kuyrukta oluşturmak istediğiniz ad alanını seçin.
3. Seçin **kaynak Oluştur** > **Kurumsal tümleştirme** > **Service Bus**ve ardından gerekli ayrıntıları girin.
4. Ad alanı ve ardından seçerek Service Bus bağlantı bilgilerini bulmak **bağlantı bilgilerini**. Bu bilgi için sonraki bölüme ihtiyacınız olacak.

Ardından, [Azure işlevi oluşturma](../azure-functions/functions-create-first-azure-function.md) anahtar kasası günlükleri depolama hesabında yoklama ve yeni olayları seçin. Bu işlev, bir zamanlamaya göre tetiklenir.

Bir Azure işlev uygulaması oluşturmak için Seç **kaynak Oluştur**, markette Ara **işlev uygulaması**ve ardından **Oluştur**. Oluşturma sırasında var olan bir barındırma planı kullanın veya yeni bir tane oluşturun. Dinamik barındırma için de tercih edebilirsiniz. Azure işlevleri barındırma seçenekleri hakkında daha fazla bilgi için bkz. [Azure işlevlerini ölçeklendirme](../azure-functions/functions-scale.md).

Azure işlev uygulaması oluşturulduktan sonra gidin ve seçin **Zamanlayıcı** senaryo ve **C\#**  dil için. Ardından **bu işlevi Oluştur**.

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

            //Required to order by time as they might not be in the file
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
> Anahtar kasası günlüklerinin yazılacağı depolama hesabınıza, daha önce oluşturduğunuz bir Service Bus örneğine ve belirli depolama anahtar kasası günlükleri yoluna işaret edecek şekilde önceki kodda değişkenlerini değiştirin.

İşlevi, anahtar kasası günlüklerinin yazılacağı en son günlük dosyasının depolama hesabından seçer, bu dosyanın en son olayların Dallarınızla ve bunları bir Service Bus kuyruğuna gönderir. 

Tek bir dosyayı birden çok olayı olduğundan, işlev ayrıca toplanmış son etkinliğin zaman damgasını belirlemek için bakan bir sync.txt dosyası oluşturmanız gerekir. Bu dosyayı kullanan birden çok kez aynı olay gönderme yoksa sağlar. 

Sync.txt dosyanın son karşılaşılan olay için zaman damgası içerir. Günlükleri yüklendiğinde, bunların doğru sıralı emin olmak için zaman damgalarını göre sıralanması gerekir.

Bu işlev için size kullanıma hazır Azure işlevleri'nde kullanılamayan birkaç ek kitaplıklar başvuru. Bu kitaplıklar dahil etmek için Azure işlevleri'nin NuGet kullanarak çekme ihtiyacımız var. Altında **kod** kutusunda **dosyaları görüntüle**.

!["Dosyaları görüntüleme" seçeneği](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Aşağıdaki içeriğe sahip Project.JSON adlı bir dosya ekleyin:

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

Seçtikten sonra **Kaydet**, Azure işlevleri, gerekli ikili dosyaları indirir.

Geçiş **tümleştir** sekmesini ve Zamanlayıcı parametresi işlev içinde kullanmak için anlamlı bir ad verin. Önceki kodda; işlevi çağrılacak Zamanlayıcı bekliyor *myTimer*. Belirtin bir [CRON ifadesi](../app-service/webjobs-create.md#CreateScheduledCRON) aşağıdaki gibi bir zamanlayıcı için: `0 * * * * *`. Bu ifade, bir dakika çalışacak şekilde işlev neden olur.

Aynı **tümleştir** sekmesinde, türündeki bir girdi ekleyin **Azure Blob Depolama**. Bu giriş, işlev tarafından attınız son etkinliğin zaman damgasını içeren sync.txt dosyaya yönlendirir. Bu giriş, parametre adını kullanarak işlev içinde erişilir. Önceki kodda, Azure Blob Depolama giriş parametre adı olmasını bekliyor. *inputBlob*. Burada sync.txt dosyasının yerleştirileceği depolama hesabı seçin (Bu aynı veya farklı bir depolama hesabı olabilir). Yol alanında biçimde dosyanın yolunu belirtin `{container-name}/path/to/sync.txt`.

Çıkış türü Ekle **Azure Blob Depolama**. Bu çıkış, giriş tanımlanan sync.txt dosyaya yönlendirir. Bu çıkış, işlev tarafından attınız son etkinliğin zaman damgasını yazmak için kullanılır. Yukarıdaki kod çağrılması için bu parametreyi bekliyor *outputBlob*.

İşlevi artık hazırdır. Geri dönmek emin **geliştirme** sekmesini ve kodu kaydedin. Derleme hataları için çıktı penceresini denetleyin ve bunları gerektiği şekilde düzeltin. Kod derlenir, ardından kod artık dakikada anahtar kasası günlükleri denetleme ve tanımlanmış bir Service Bus kuyruğuna herhangi yeni bir olayı gönderme. Tetiklenen bir işlev her zaman için günlük penceresini yazılmasına yarar günlük bilgileri görmeniz gerekir.

### <a name="azure-logic-app"></a>Azure mantıksal uygulaması

Ardından, işlev Service Bus kuyruğuna göndermek, içeriği ayrıştırır ve eşleştirilen bir koşula göre bir e-posta gönderen olaylar seçer bir Azure mantıksal uygulaması oluşturmanız gerekir.

[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) seçerek **kaynak Oluştur** > **tümleştirme** > **mantıksal uygulama**.

Mantıksal uygulama oluşturulduktan sonra gidin ve seçin **Düzenle**. Mantıksal uygulama Düzenleyicisi'ndeki **hizmet veri yolu kuyruğu** ve Service Bus kuyruğuna bağlanmak için kimlik bilgilerinizi girin.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Seçin **koşul Ekle**. Koşul Gelişmiş düzenleyicisine geçin ve aşağıdaki kodu girin. Değiştirin *APP_ID* web uygulamanızın gerçek uygulama kimliği:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Bu ifade temelde döndürür **false** varsa *AppID* (aynı Service Bus iletisinin gövdesini)'ndan gelen olayı değil *AppID* uygulama.

Şimdi altında bir eylem oluşturun **Hayır ise, hiçbir şey yapma**.

![Azure Logic Apps eylem seçin](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Bir eylem seçin **Office 365 - e-postası gönderme**. Tanımlı koşul döndürdüğünde göndermek için e-posta oluşturmak için alanları doldurun **false**. Office 365 yoksa, aynı sonuçları elde etmek Alternatiflere bakın.

Artık, yeni anahtar kasası denetim günlükleri için bir dakika görünür bir uçtan uca işlem hattı sahipsiniz. Bulduğu yeni günlükleri bir Service Bus kuyruğuna gönderir. Mantıksal uygulama, yeni bir iletinin kuyrukta gölünüzdeki durumlarda tetiklenir. Varsa *AppID* içinde olay çağırma uygulamasının uygulama kimliği eşleşmiyor, e-posta gönderir.
