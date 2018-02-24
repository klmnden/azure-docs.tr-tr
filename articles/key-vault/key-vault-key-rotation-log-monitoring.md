---
title: "Azure anahtar kasası uçtan uca anahtar döndürme ve denetim ayarlama | Microsoft Docs"
description: "Anahtar rotasyonu ve izleme anahtar kasası günlükleri ayarlanmasını yardımcı olması için bu nasıl yapılır konuları kullanın."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: 793f35bfd2e5e6b22e0804f01a69c0c20990d211
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Azure Anahtar Kasası’nı uçtan uca döndürme ve denetleme ile ayarlama
## <a name="introduction"></a>Giriş
Anahtar kasası oluşturduktan sonra anahtarları ve gizli anahtarları depolamak için bu kasası kullanmaya başlamak mümkün olur. Uygulamalarınızı artık anahtarları ve gizli anahtarları, ancak bunun yerine kalıcı hale getirmek için bunları anahtar Kasası'nı gerektiğinde ister. Bu, anahtarlar ve gizli anahtarı ve gizli yönetim olanakları derecesini açar uygulamanızın davranışını etkilemeden güncelleştirmenize olanak sağlar.

Bu makalede bir gizlilik bu durumda, bir uygulama tarafından erişilebilen bir Azure depolama hesabı anahtarı depolamak için Azure anahtar kasası kullanmaya ilişkin bir örnek anlatılmaktadır. Ayrıca, bu depolama hesabı anahtarı zamanlanmış dönüşünü uyarlamasını gösterir. Son olarak, anahtar kasası denetim günlüklerini izlemek ve beklenmeyen istekler yapıldığında uyarıları yükseltmek nasıl Tanıtımı anlatılmaktadır.

> [!NOTE]
> Bu öğretici anahtar kasanızı ilk kurulumu ayrıntılı olarak açıklamak için tasarlanmamıştır. Bu bilgi için bkz. [Azure Anahtar Kasası ile çalışmaya başlama](key-vault-get-started.md). Platformlar arası komut satırı arabirimi yönergeleri için bkz: [anahtar kasası CLI kullanarak yönetme](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Anahtar Kasası ayarlama
Bir gizli anahtar Kasası'nı almak bir uygulama etkinleştirmek için öncelikle gizli anahtar oluşturma ve, kasaya yükleyin. Bu, bir Azure PowerShell oturumu başlatarak ve aşağıdaki komut ile Azure hesabınızda oturum açma gerçekleştirilebilir:

```powershell
Login-AzureRmAccount
```

Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. PowerShell Bu hesapla ilişkili tüm abonelikleri alır. PowerShell varsayılan olarak birinciyi kullanır.

Birden çok aboneliğiniz varsa, anahtar kasanızı oluşturmak için kullanılan bir tane belirtmeniz gerekebilir. Hesabınız için abonelikleri görmek için aşağıdakileri girin:

```powershell
Get-AzureRmSubscription
```

Günlüğünü tutacağınız anahtar kasasıyla ilişkili aboneliği belirtmek için şunu girin:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Bu makalede, bir depolama hesabı anahtarı gizli depolama gösterilmektedir olduğundan, bu depolama hesabı anahtarı edinmeniz gerekir.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Gizli (Bu durumda, depolama hesabı anahtarınızı) aldıktan sonra güvenli bir dizeye dönüştürmek ve anahtar kasanızı'da bu değerle bir gizli anahtar oluşturmanız gerekir.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Ardından, oluşturduğunuz için gizli anahtar URI'sini Al. Gizli anahtarı almak için anahtar kasası çağırdığınızda bu bir sonraki adımda kullanılır. Aşağıdaki PowerShell komutunu çalıştırın ve gizli URI kimliği değeri not edin:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Uygulama ayarlama
Depolanan gizli sahip olduğunuza göre almak ve kullanmak için kodu kullanabilirsiniz. Bunu başarmak için gereken birkaç adım vardır. İlk ve en önemli adım Azure Active Directory ile uygulamanızı kaydetme ve böylece uygulamanız gelen istekleri izin verebilir anahtar kasası, uygulama bilgilerinizi belirtiyor.

> [!NOTE]
> Uygulamanız aynı Azure Active Directory Kiracı anahtar kasanızı olarak oluşturulmuş olması gerekir.
>
>

Azure Active Directory uygulamalar sekmesini açın.

![Azure Active Directory'de açık uygulamalar](./media/keyvault-keyrotation/AzureAD_Header.png)

Seçin **Ekle** Azure Active Directory'yi bir uygulama eklemek için.

![EKLE'yi seçin](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Uygulama türü olarak bırakın **WEB uygulaması ve/veya WEB API** ve uygulamanızı bir ad verin.

![Uygulama adı](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Uygulamanızı vermek bir **oturum açma URL** ve bir **uygulama kimliği URI'si**. Bunlar bu tanıtım için istediğiniz herhangi bir şey olabilir ve bunlar daha sonra gerekirse değiştirilebilir.

![Gerekli URI'ler sağlar](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Uygulama Azure Active Directory'ye eklendikten sonra uygulama sayfasına gidersiniz. Tıklatın **yapılandırma** sekmesini ve ardından bulmak ve kopyalama **istemci kimliği** değeri. Sonraki adımlar için istemci Kimliğini not edin.

Ardından, Azure Active Directory ile etkileşim kurabilmesi uygulamanız için bir anahtar oluşturun. Bu altında oluşturabilir **anahtarları** bölümüne **yapılandırma** sekmesi. Bir sonraki adımda kullanmak için Azure Active Directory uygulamanızdan yeni oluşturulan anahtarı not edin.

![Azure Active Directory uygulama anahtarları](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Anahtar kasası uygulamanıza gelen tüm çağrıları kurmadan önce anahtar kasası uygulamanız ve izinlerini hakkında bildirmeniz gerekir. Kasa adı ve istemci kimliği verir ve Azure Active Directory Uygulama aşağıdaki komutu alır **almak** anahtar kasanızı erişim uygulama.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Bu noktada, uygulama çağrılarınızı oluşturmaya başlamak hazırsınız. Uygulamanızda Azure anahtar kasası ve Azure Active Directory ile etkileşim kurmak için gerekli NuGet paketleri yüklemeniz gerekir. Visual Studio Paket Yöneticisi konsolundan aşağıdaki komutları yazın. Bu makalede yazıda geçerli paketinin sürümünü, Azure Active Directory 3.10.305231913, olduğundan en son sürümünü onaylamak ve buna uygun olarak güncelleştirmek isteyebilirsiniz.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Uygulama kodunuz, Azure Active Directory kimlik doğrulama yöntemi tutmak için bir sınıf oluşturun. Bu örnekte, o sınıfın adlı **yardımcı programları**. Şu deyimi kullanarak aşağıdakileri ekleyin:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Ardından, Azure Active Directory'den JWT belirtecini almak için aşağıdaki yöntemi ekleyin. Bakım için sabit kodlanmış dize değerleri, web veya uygulama yapılandırması taşımak isteyebilirsiniz.

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

Anahtar kasası çağırın ve gizli değer almak için gerekli kodu ekleyin. Aşağıdaki ilk eklemelisiniz deyimi kullanarak:

```csharp
using Microsoft.Azure.KeyVault;
```

Anahtar kasası çağırma ve gizli anahtarı almak için yöntem çağrıları ekleyin. Bu yöntemde, gizli, bir önceki adımda kaydettiğiniz URI sağlayın. Kullanımına dikkat edin **GetToken** yönteminden **yardımcı programları** daha önce oluşturduğunuz sınıfı.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Uygulamanızı çalıştırdığınızda, şimdi olmalısınız Azure Active Directory kimlik doğrulaması ve ardından Azure anahtar Kasası'gizli değer alma.

## <a name="key-rotation-using-azure-automation"></a>Azure otomasyonu kullanarak anahtar döndürme
Değerleri Azure anahtar kasası gizlilikleri olarak depolamak için bir dönüş stratejisi uygulamaya yönelik çeşitli seçenekleriniz vardır. El ile işleminin bir parçası gizli döndürülüp, bunlar program aracılığıyla API çağrılarını kullanarak döndürülüp ya da bir otomasyon komut dosyası aracılığıyla Döndürülmüş. Bir Azure depolama hesabı erişim anahtarı değiştirmek için bu makalede amaçları doğrultusunda, Azure Automation ile birlikte Azure PowerShell kullanacaksınız. Ardından bu yeni anahtarla bir anahtar kasası gizli anahtarı güncelleştirir.

Anahtar kasasına gizli değerlerini ayarlamak Azure Otomasyonu izin vermek için Azure Automation örneğinizi kurulan oluşturulduğu AzureRunAsConnection adlı bağlantı için istemci kimliği edinmeniz gerekir. Bu kimliği seçerek bulabileceğiniz **varlıklar** , Azure Automation örneğinden. Buradan, seçtiğiniz **bağlantıları** ve ardından **AzureRunAsConnection** hizmet ilkesi. Not edin **uygulama kimliği**.

![Azure otomasyon istemci kimliği](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

İçinde **varlıklar**, seçin **modülleri**. Gelen **modülleri**seçin **galeri**, arayın ve sonra ve **alma** güncelleştirilmiş sürümlerini her biri aşağıdaki modüller:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> Bu makalede yazma sırasında yalnızca daha önce not ettiğiniz modülleri için aşağıdaki komut güncelleştirilmesi gereken. Otomasyon işinizi başarısız olduğunu fark ederseniz, gerekli olan tüm modülleri ve bağımlılıklarını içeri aktardığınız onaylayın.
>
>

Azure Otomasyonu bağlantınız için uygulama kimliği aldıktan sonra bu uygulamayı kasanızdaki gizli anahtarları güncelleştirmek için erişimi olduğunu, anahtar kasanızı bildirmeniz gerekir. Bu aşağıdaki PowerShell komutuyla gerçekleştirilebilir:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Ardından, **Runbook'lar** Azure Automation örneği ve ardından altında **Runbook Ekle**. **Hızlı Oluştur**’u seçin. Runbook'unuzda adlandırın ve seçin **PowerShell** runbook türü. Bir açıklama ekleme seçeneğiniz vardır. Son olarak, tıklatın **oluşturma**.

![Runbook oluşturma](./media/keyvault-keyrotation/Create_Runbook.png)

Yeni runbook'unuz Düzenleyicisi bölmesinde aşağıdaki PowerShell betiğini yapıştırın:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
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

Düzenleyici bölmesinden seçin **Test bölmesi** kodunuzu test etmek için. Komut hatasız çalışmaya başladıktan sonra seçebileceğiniz **Yayımla**, ve daha sonra geri runbook yapılandırma bölmesinde runbook için bir zamanlama uygulayabilirsiniz.

## <a name="key-vault-auditing-pipeline"></a>Anahtar kasası denetim ardışık düzen
Bir anahtar kasasını ayarladığınızda, anahtar Kasası'na erişim isteklerini günlükleri toplamak için denetimi kapatabilirsiniz. Bu günlükler belirlenmiş bir Azure depolama hesabında depolanır ve çıkışı, izlenen ve analiz çekebilir. Aşağıdaki senaryoda, web uygulamasının uygulama kimliği eşleşen bir uygulama parolaları kasadan aldığında bir e-posta göndermek için bir işlem hattı oluşturmak için Azure işlevleri, Azure mantıksal uygulamaları ve anahtar kasası denetim günlüklerini kullanır.

İlk olarak, anahtar kasanızı günlüğü etkinleştirmeniz gerekir. Bu aşağıdaki PowerShell komutları yapılabilir (tam Ayrıntılar adresindeki görülme [anahtar kasası günlüğü](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Bu özellik etkinleştirildikten sonra belirlenen storage hesabınıza toplama başlangıç denetim günlüklerini. Bu günlükler olayları anahtar kasalarınıza erişilen nasıl ve ne zaman ve kim tarafından içerir.

> [!NOTE]
> Günlük bilgilerinize anahtar kasası işleminden sonra 10 dakika erişebilir. Genellikle bu değerden daha hızlı olacaktır.
>
>

Sonraki adım [bir Azure hizmet veri yolu kuyruğu oluşturma](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Anahtar kasası denetim günlüklerini burada gönderilen budur. Denetim günlüğü iletileri sırada, mantıksal uygulama bunları alır ve bunlar üzerinde çalışır. Hizmet veri yolu aşağıdaki adımlarla oluşturun:

1. (Bu, 2. adımı atla için kullanmak istediğiniz bir zaten varsa) bir hizmet veri yolu ad alanı oluşturun.
2. Azure Portalı'nda hizmet veri yoluna göz atın ve sırada oluşturmak istediğiniz ad alanını seçin.
3. Seçin **kaynak oluşturma**, **Kurumsal tümleştirme**, **Service Bus**ve ardından gerekli ayrıntıları girin.
4. Service Bus bağlantı bilgilerini ad alanını seçerek ve tıklatarak seçin **bağlantı bilgilerini**. Bu bilgiler için sonraki bölüme gerekir.

Ardından, [bir Azure işlevi oluşturma](../azure-functions/functions-create-first-azure-function.md) anahtar kasası günlükleri depolama hesabındaki yoklamak ve yeni olayları seçin. Bu bir zamanlamaya göre tetiklenen bir işlev olacaktır.

Bir Azure işlevi oluşturmak için seçtiğiniz **kaynak oluşturma**, Market arama _işlev uygulaması_, tıklatıp **oluşturma**. Oluşturma sırasında var olan bir barındırma planı kullanın veya yeni bir tane oluşturun. Ayrıca, dinamik barındırmak için tercih. İşlevi barındırma seçenekleri hakkında daha fazla ayrıntı bulunabilir [Azure işlevlerini ölçeklendirme](../azure-functions/functions-scale.md).

Azure işlevi oluşturulduğunda, kendisine gidin ve bir süreölçer işlevi C seçip\#. Ardından **bu işlev oluşturma**.

![Azure işlevleri Başlat dikey penceresi](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Üzerinde **geliştirme** sekmesinde, run.csx kodu aşağıdakilerle değiştirin:

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
> Anahtar kasası günlükleri yazılacağı depolama hesabınıza işaret etmek için önceki kod değişkenlerde, daha önce oluşturduğunuz hizmet veri yolu ve anahtar kasası depolama günlükleri belirli yolu değiştirdiğinizden emin olun.
>
>

İşlev anahtar kasası günlükleri yazılacağı en son günlük dosyasını depolama hesabından seçer, bu dosyanın en son olayların alan ve Service Bus kuyruğuna iter. Tek bir dosyada birden çok olay olabilir beri işlevi de çekilmiş yukarı son olay zaman damgası belirlemek için bakan bir sync.txt dosyası oluşturmanız gerekir. Bu, birden çok kez aynı olay gönderme yok sağlar. Bu sync.txt dosya son karşılaşılan olayı için bir zaman damgası içeriyor. Yüklendiğinde, günlükleri, doğru sıralanmıştır emin olmak için zaman damgasını tabanlı sıralanması gerekir.

Bu işlev için birkaç Azure işlevleri kutusuna dışında bulunmayan ek kitaplıklara başvuru. Azure işlevleri, bunları çıkarmak için ihtiyacımız bunlar için NuGet kullanma. Seçin **dosyaları görüntüle** seçeneği.

![Görünüm dosyaları seçeneği](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Ve aşağıdaki içeriğe sahip project.json adlı bir dosyaya ekleyin:

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
Bağlı **kaydetmek**, Azure işlevleri, gerekli ikili dosyaları indirir.

Geçiş **tümleştir** sekmesinde ve Zamanlayıcı parametre işlev içinde kullanmak için anlamlı bir ad verin. İsteğe bağlı olarak önceki kodda çağrılacak Zamanlayıcı bekliyor *myTimer*. Belirtin bir [CRON ifade](../app-service/web-sites-create-web-jobs.md#CreateScheduledCRON) gibi: 0 \* \* \* \* \* bir dakika çalıştırılacak işlev neden olacak Zamanlayıcı için.

Aynı **tümleştir** sekmesinde, türündeki bir girdi ekleme **Azure Blob Storage**. Bu, işlev tarafından aranan son olay zaman damgası içeren sync.txt dosyasını işaret edecek. Bu işlev parametre adı tarafından içinde kullanılabilir olacaktır. Önceki kodda, parametre adı olarak Azure Blob Storage girdi bekliyor *inputBlob*. Sync.txt dosya nerede bulunacağı depolama hesabını seçin (aynı veya farklı bir depolama hesabı olabilir). Yol alanı nerede dosya biçimi {container-name}/path/to/sync.txt. yaşıyor yolunu belirtin

Bir çıkış türü ekleme *Azure Blob Storage* çıktı. Bu sync.txt dosyası girdide tanımlı işaret edecek. Zaman damgası'Aranan en son olay yazmak için bu işlev tarafından kullanılır. Önceki kod çağrılacak Bu parametre bekliyor *outputBlob*.

Bu noktada, işlevi hazırdır. Dönmeyi emin olun **geliştirme** sekmesinde ve kod kaydedin. Derleme hataları için çıktı penceresini denetleyin ve uygun şekilde düzeltin. Kodu derler, ardından kod şimdi anahtar kasası günlükleri dakikada denetleniyor ve tanımlanmış hizmet veri yolu kuyruğu üzerine yeni olaylar Ftp'den. Günlük kaydı bilgileri Günlük penceresine işlevi her başlatılışında yazamadı görmeniz gerekir.

### <a name="azure-logic-app"></a>Azure mantıksal uygulama
Sonraki işlev Service Bus kuyruğuna Ftp'den, içeriği ayrıştırır ve eşleşen bir koşula göre bir e-posta gönderir olayları seçer bir Azure mantıksal uygulama oluşturmanız gerekir.

[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) giderek **yeni > mantıksal uygulama**.

Mantıksal uygulama oluşturulduktan sonra dosyayı bulun ve seçin **Düzenle**. Mantıksal uygulama düzenleyicisinde seçin **hizmet veri yolu kuyruğu** ve Service Bus kuyruğuna bağlanmak için kimlik bilgilerinizi girin.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Sonraki seçin **bir koşul eklemek**. Koşul Gelişmiş düzenleyicisine geçin ve APP_ID web uygulamanızı gerçek APP_ID ile değiştirerek aşağıdaki kodu girin:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Bu ifade temelde döndürür **false** varsa *AppID* (Service Bus ileti gövdesi olan)'ndan gelen olayı değil *AppID* uygulamanın.

Şimdi, altında bir eylem oluşturun **Öyle değilse, hiçbir şey yapma**.

![Azure mantıksal uygulama eylemi seçin](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Eylem için **Office 365 - e-postası gönderme**. Tanımlanan koşulu döndürdüğünde göndermek için e-posta oluşturmak için alanları doldurun **false**. Office 365 yoksa aynı sonucu elde etmek için alternatifleri görünebilir.

Bu noktada yeni anahtar kasası denetim günlükleri için bir dakika benzeyen bir uçtan uca ardışık vardır. Bu yeni günlükler bulduğu bir service bus kuyruğuna iter. Mantıksal uygulama yeni bir ileti sıraya adlandırıldığını geldiğinde tetiklenir. Varsa *AppID* içinde olay çağrı yapan uygulamanın uygulama Kimliğini eşleşmiyor, bir e-posta gönderir.
