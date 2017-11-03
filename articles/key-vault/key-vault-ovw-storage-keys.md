---
ms.assetid: 
title: "Azure anahtar kasası depolama hesabı anahtarları"
description: "Depolama hesabı anahtarları Azure anahtar kasası ve anahtar tabanlı erişim arasında seemless tümleştirme Azure depolama hesabı sağlayın."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 1d92ffc03b60695c5ff7b6c3d2ac54808c527efd
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure anahtar kasası depolama hesabı anahtarları

Azure anahtar kasası depolama hesabı anahtarlarını önce geliştiricilerin kendi Azure depolama hesabı (ASA) anahtarları yönetebilir ve bunları el ile veya bir dış Otomasyon yoluyla döndürmek gerekiyordu. Şimdi, anahtar kasası depolama hesabı anahtarları olarak uygulanan [anahtar kasası gizli](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) bir Azure depolama hesabıyla kimlik doğrulaması için. 

Azure depolama hesabı (ASA) anahtar özelliği, gizli döndürme yönetir. Ayrıca, doğrudan iletişim gereksinimini ASA anahtarı ile paylaşılan erişim imzaları (SAS) bir yöntem olarak sunarak kaldırır. 

Azure Storage hesapları hakkında daha fazla genel bilgi için bkz: [Azure storage hesapları hakkında](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Arabirimleri destekleme

Tam listesi ve bizim programlama ve komut dosyası arabirimleri bağlantılar bulacaksınız [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Anahtar kasası ne yönetir

Yönetilen depolama hesabı anahtarlarını kullandığınızda anahtar kasası sizin adınıza birkaç iç yönetim işlevleri gerçekleştirir.

- Azure anahtar kasası anahtarları bir Azure depolama hesabı (ASA) yönetir.
    - Dahili olarak, Azure anahtar kasası (eşitleme) anahtarları bir Azure depolama hesabı ile listeleyebilirsiniz.  
    - Azure anahtar kasası yeniden oluşturur (döndüğü) anahtarları düzenli aralıklarla. 
    - Anahtar değerleri hiç yanıt arayan olarak döndürülür. 
    - Azure anahtar kasası depolama hesapları hem Klasik depolama hesaplarını anahtarları yönetir. 
- Azure anahtar kasası, SAS (hesabı veya hizmet SAS) tanımları oluşturmak için kasa/nesne sahibi sağlar.
    - SAS tanımı kullanılarak oluşturulan SAS değeri REST URI yolu gizlilik olarak döndürülür. Daha fazla bilgi için bkz: [Azure anahtar kasası depolama hesabı işlemleri](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

## <a name="naming-guidance"></a>Adlandırma

- Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.  
- Bir SAS tanımı adı yalnızca 0-9, a-z, A-Z uzunluğunda 1 102 karakter olmalıdır.

## <a name="developer-experience"></a>Geliştirici deneyimi

### <a name="before-azure-key-vault-storage-keys"></a>Önce depolama anahtarları Azure anahtar kasası 

Azure depolama erişmek için geliştiriciler bir depolama hesabı anahtarı ile aşağıdaki yöntemleri yapmanız için kullanılır. 
 
```powershell
//create an Azure Storage Account using a connection string containing an account name and a storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Azure anahtar kasası depolama anahtarları sonra 

```powershell
//Make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Get secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  

-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get a SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If your SAS token is about to expire, Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Geliştirici Kılavuzu

- Yalnızca anahtar kasası ASA tuşlarınızı yönetme izin verir. Kendiniz yönetmeye çalışmayın, anahtar Kasası'nın işlemlerle çalışmasını engeller. 
- Birden fazla anahtar kasası nesnesi tarafından yönetilecek ASA anahtarları izin vermez. 
- ASA anahtarlarınızı el ile yeniden oluşturmanız gerekiyorsa, bunları anahtar kasası yeniden öneririz. 

## <a name="getting-started"></a>Başlarken

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Rol tabanlı erişim denetimi (RBAC) izinler için Kurulum

Azure anahtar kasası uygulama kimliği için izin gerektirmesidir *listesi* ve *yeniden* için bir depolama hesabı anahtarları. Aşağıdaki adımları kullanarak bu izinlerini ayarlayabilirsiniz:

- Azure anahtar kasası kimlik objectID alın: 

    `Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093`

- Depolama anahtarı operatörü rolü Azure anahtar kasası kimliğine atayın: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Rol parametresi bir Klasik hesap türü için kümesine *"Klasik depolama hesabı anahtarı işleci Hizmeti rol."*

## <a name="working-example"></a>Çalışan örnek

Aşağıdaki örnek, bir anahtar kasası oluşturma Azure depolama hesabı ve ilişkili paylaşılan erişim imzası (SAS) tanımları yönetilen gösterir.

### <a name="assumptions"></a>Varsayımlar

Aşağıdaki deyimler givens bu çalışma örneğin şunlardır.

- Depolama kaynağı konumundadır: */subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1*

- Anahtar kasanızı adıdır: *yourtest1*

### <a name="get-a-service-principal"></a>Bir hizmet sorumlusu Al

```powershell
$yourKeyVaultServicePrincipalId = (Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Yukarıdaki komut çıktısı arayacağız, ServicePrincipal içerecektir *yourKeyVaultServicePrincipalId*. 

### <a name="set-permissions"></a>İzinleri ayarlama

Ayarlamak, depolama izinlere sahip olduğunuzdan emin olun *tüm*. YourKeyVaultServicePrincipalId almak ve aşağıdaki komutları kullanarak kasaya izinlerini ayarlayın.

```powershell
Get-AzureRmADUser -SearchString "your name"
```
Şimdi adınızı için arama ve izinleri kasaya ayarı kullanacağı ilgili objectID alın.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourtest1' -ObjectId $yourKeyVaultServicePrincipalId -PermissionsToStorage all
```

### <a name="allow-access"></a>Erişime izin ver

Yönetilen depolama hesabı ve SAS tanımları oluşturmadan önce depolama hesaplarına anahtar kasası hizmet erişim vermeniz gerekir.

```powershell
New-AzureRmRoleAssignment -ObjectId $yourKeyVaultServicePrincipalId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '/subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1'
```

### <a name="create-storage-account"></a>Depolama hesabı oluşturma

Şimdi bir yönetilen depolama hesabı ve iki SAS tanımları oluşturun. Hesap SAS farklı izinlerle blob hizmetine erişim sağlar.

```powershell
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="regeneration"></a>Yeniden oluşturma

Aşağıdaki komutları kullanarak yeniden üretme süresi ayarlama.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Kümesi SAS tanımları

SAS tanımları yönetilen depolama hesabınız için anahtar kasasına ayarlayın.

```powershell
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourtest1  -AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName yourtest1  -AccountName msak01 -Name blobsas2 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List,Write
```

### <a name="get-token"></a>Belirteci alma

Karşılık gelen SAS belirteci alın ve depolama birimine çağrıları yapma.

```powershell
$sasToken1 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas1).SecretValueText
$sasToken2 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas2).SecretValueText
```

### <a name="create-storage"></a>Depolama oluşturma

İle erişmeye dikkat *$sastoken1* biz ile erişebilir, ancak bu, başarısız *$sastoken2*. 

```powershell
$context1 = New-AzureStorageContext -SasToken $sasToken1 -StorageAccountName yourtest1
$context2 = New-AzureStorageContext -SasToken $sasToken2 -StorageAccountName yourtest1
Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt"  -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt"  -Context $context2
```

### <a name="example-summary"></a>Örnek özeti

Mümkün erişim yazma erişimi olduğundan SAS belirteci ile içerik depolama blob var.

### <a name="relevant-powershell-cmdlets"></a>İlgili Powershell cmdlet'leri

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Ekleme AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount?view=azurermps-4.3.1)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Güncelleştirme AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)

## <a name="storage-account-onboarding"></a>Depolama hesabı ekleme 

Örnek: bir depolama hesabı nesnesi Azure anahtar Kasası'na yerleşik bir depolama hesabı eklediğiniz bir anahtar kasası nesne sahibi.

Onboarding işlemi sırasında anahtar kasası hesabının kimliğini ve onboarding için izinlere sahip olduğunu doğrulayın gerekiyor *listesi* ve *yeniden* depolama anahtarları. Bu izinleri doğrulamak için anahtar kasası bir OBO (adına, üzerinde) kimlik doğrulama hizmeti, Azure kaynak yöneticisi için İzleyici belirteç alır ve yapar bir *listesi* anahtar Azure depolama hizmetine yapılan çağrı. Varsa *listesi* çağrısı başarısız olursa, nesne oluşturma bir HTTP durum kodu ile başarısız olur anahtar kasası *Yasak*. Bu şekilde listelenen anahtarları, anahtar kasası varlık depolamada önbelleğe alınır. 

Anahtar kasası, kimlik olduğunu doğrulamalısınız *yeniden* anahtarlarınızı yeniden sahipliğini almadan önce izinleri. Anahtar kasası birinci taraf kimlik yanı sıra OBO belirteci aracılığıyla kimliği bu izinlere sahip olduğunu doğrulamak için:

- Anahtar kasası depolama hesabı kaynakta RBAC izinleri listeler.
- Anahtar kasası Eylemler ve Eylemler olmayan normal ifadeyle eşleşen aracılığıyla yanıt doğrular. 

At destekleyen bazı örnekleri Bul [anahtar kasası - yönetilen depolama hesabı anahtarları örnekleri](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Kimlik yoksa *yeniden* izinler veya anahtar Kasası'nın ilk taraf kimlik yoksa *listesi* veya *yeniden* izni sonra ekleme İstek bir uygun hata kodu ve ileti döndürme başarısız olur. 

Kullandığınızda birinci taraf, PowerShell veya CLI yerel istemci uygulamalarının OBO belirteci yalnızca çalışır.

## <a name="other-applications"></a>Diğer uygulamalar

- SAS belirteci, anahtar kasası depolama hesabı anahtarları kullanılarak oluşturulan bir Azure depolama hesabı daha da denetimli erişim sağlar. Daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Ayrıca bkz.

- [Anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/rest/api/keyvault/)
- [Anahtar kasası ekip blogu](https://blogs.technet.microsoft.com/kv/)
