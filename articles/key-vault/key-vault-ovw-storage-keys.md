---
ms.assetid: ''
title: Azure anahtar kasası depolama hesabı anahtarları
description: Depolama hesabı anahtarları Azure Key Vault ile anahtar tabanlı erişim arasında bir seemless tümleştirme için Azure depolama hesabı sağlayın.
ms.topic: article
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 08/21/2017
ms.openlocfilehash: 0112d48647c031845bc89ccebfcdd40954c59f14
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "42055878"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure anahtar kasası depolama hesabı anahtarları

Azure Key Vault depolama hesabı anahtarlarını önce geliştiriciler kendi Azure depolama hesabı (ASA) anahtarları yönetmek ve bunları el ile veya bir dış Otomasyon yoluyla döndürmek vardı. Artık, Key Vault depolama hesabı anahtarları olarak gerçekleştirilmektedir [Key Vault gizli dizileri](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) bir Azure depolama hesabıyla kimlik doğrulaması.

Azure depolama hesabı (ASA) temel bir özelliği, gizli dönüş yönetir. Ayrıca, doğrudan iletişim için gereken bir ASA anahtarı ile paylaşılan erişim imzaları (SAS) bir yöntem olarak sunarak kaldırır.

Azure depolama hesapları hakkında daha fazla genel bilgi için bkz. [Azure depolama hesapları hakkında](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Arabirimleri destekleme

Tam listesi ve bizim programlama ve komut dosyası arabirimleri bağlantılar bulabilirsiniz [anahtar kasası Geliştirici Kılavuzu](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Key Vault ne yönetir

Yönetilen depolama hesabı anahtarlarını kullanırken Key Vault çeşitli iç yönetim işlevlerini sizin yerinize gerçekleştirir.

- Azure Key Vault, anahtarları bir Azure depolama hesabı (ASA) yönetir.
    - Dahili olarak, Azure Key Vault (eşitleme) Azure depolama hesabı anahtarları listeleyebilirsiniz.
    - Azure Key Vault oluşturur (döndürür) anahtarlarını düzenli aralıklarla.
    - Anahtar değerler hiçbir zaman yanıt arayana döndürülür.
    - Azure Key Vault, depolama hesapları hem de klasik depolama hesapları, anahtarları yönetir.
- Azure Key Vault, SAS (paylaşılan erişim imzası, hesap veya hizmet SAS) tanımları oluşturmak için kasa/nesne sahibi sağlar.
    - SAS tanımı kullanılarak oluşturulan SAS değeri REST URI yolu bir gizli dizi döndürülür. SAS tanımı işlemlerinde daha fazla bilgi için bkz. [Azure anahtar kasası REST API Başvurusu](/rest/api/keyvault).

## <a name="naming-guidance"></a>Adlandırma yönergeleri

- Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
- Bir SAS tanımı adı uzunluğu içeren yalnızca 0-9, a-z, A-Z 102 1 karakter olmalıdır.

## <a name="developer-experience"></a>Bir geliştirici deneyimi

### <a name="before-azure-key-vault-storage-keys"></a>Azure anahtar kasası önce depolama anahtarları

Azure depolama erişim elde etmek için geliştiricilerin bir depolama hesabı anahtarını paylaşmak için aşağıdaki yöntemleri yapmanız için kullanılır.
1. Bağlantı dizesi veya SAS belirteci Azure AppService uygulama ayarları veya başka bir depolama alanında Store.
1. Uygulama başlatma sırasında bağlantı dizesi veya SAS belirtecini getirir.
1. Oluşturma [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) depolama ile etkileşimde bulunmak için.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Azure anahtar kasası depolama anahtarlarını sonra

Geliştiriciler oluşturma bir [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient) ve kendi depolama için SAS belirteci almak için yararlanabilir. Daha sonra oluşturdukları [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) o belirteç ile.

```cs
// Create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();

// Use the blobClientWithSas
...

// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Geliştirici Kılavuzu

- Yalnızca ASA anahtarlarınızı yönetmek Key Vault izin verir. Bunları kendiniz yönetmeyi denemeyin, Key Vault'un işlemlerini kesintiye neden.
- ASA anahtarları birden fazla anahtar kasası nesne tarafından yönetilecek izin vermez.
- ASA anahtarlarınızı el ile yeniden gerekiyorsa, bunları Key Vault aracılığıyla yeniden öneririz.

## <a name="getting-started"></a>Başlarken

### <a name="give-key-vault-access-to-your-storage-account"></a>Depolama hesabınıza Key Vault erişimi verin 

Birçok uygulama gibi diğer hizmetlere erişmek için OAuth kullanmak için Key Vault, Azure AD ile kaydedilir. Kayıt sırasında [bir hizmet sorumlusu](/azure/active-directory/develop/app-objects-and-service-principals) nesnesi oluşturulur, çalışma zamanında uygulamanın kimliğini temsil etmek için kullanılır. Hizmet sorumlusu, rol tabanlı erişim denetimi (RBAC) başka bir kaynağa erişmek için uygulamanın kimliğini yetkilendirmek için de kullanılır.

Azure anahtar kasası uygulama kimliği için izinler gerekiyor *listesi* ve *yeniden* anahtarları, depolama hesabınız için. Aşağıdaki adımları kullanarak bu izinleri ayarlama:

```powershell
# Get the resource ID of the Azure Storage Account you want to manage.
# Below, we are fetching a storage account using Azure Resource Manager
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get Application ID of Azure Key Vault's service principal
$servicePrincipal = Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ObjectId $servicePrincipal.Id -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

    >[!NOTE]
    > For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role."*

## <a name="working-example"></a>Çalışan bir örnek

Aşağıdaki örnek, yönetilen Azure depolama hesabı ve ilişkili SAS tanımlarını bir Key Vault oluşturma gösterilmektedir.

### <a name="prerequisite"></a>Önkoşul

Tamamladığınızdan emin olun [kurulumu için rol tabanlı erişim denetimi (RBAC) izinlerini](#setup-for-role-based-access-control-rbac-permissions).

### <a name="setup"></a>Kurulum

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Ardından, izinlerini ayarlayın **hesabınızı** için anahtar Kasası'nda tüm depolama izinleri yönetebileceğinizden emin olmanızı sağlar. Aşağıdaki örnekte, Azure hesabımız olduğu _developer@contoso.com_.

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Anahtar kasası oluşturma yönetilen depolama hesabı

Artık Azure anahtar Kasası'nda yönetilen depolama hesabı oluşturma ve SAS belirteçleri oluşturmak için depolama hesabınızdan bir erişim tuşunu kullanın.
- `-ActiveKeyName` SAS belirteçleri oluşturmak için 'anahtar2' kullanır.
- `-AccountName` Yönetilen depolama hesabınızı tanımlamak için kullanılır. Aşağıda basit tutmak için depolama hesabı adını kullanıyoruz ancak herhangi bir ad olabilir.
- `-DisableAutoRegenerateKey` belirtir depolama hesabı anahtarlarını yeniden değil.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Yeniden anahtar oluşturma

Key Vault, depolamanın erişim anahtarlarını düzenli aralıklarla yeniden oluşturmak istiyorsanız, bir oluşturma süresini ayarlayabilirsiniz. Aşağıda, bir oluşturma süresini 3 gün ayarlıyorsunuz. 3 gün sonra Key Vault yeniden 'anahtar1' ve 'anahtar1' active 'anahtar2' anahtarını değiştirme.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Kümesi SAS tanımları

Hesap SAS, farklı izinlerle blob hizmetine erişim sağlar.
SAS tanımları, yönetilen depolama hesabınız için anahtar Kasası'nda ayarlayın.
- `-AccountName` Anahtar Kasası'nda yönetilen depolama hesabının adıdır.
- `-Name` SAS belirtecini depolama alanınızda tanımlayıcısıdır.
- `-ValidityPeriod` oluşturulan SAS belirteci süre sonu tarihini ayarlar.

```powershell
$validityPeriod = [System.Timespan]::FromDays(1)
$readSasName = "readBlobSas"
$writeSasName = "writeBlobSas"

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName $keyVaultName -AccountName $accountName -Name $readSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName $keyVaultName -AccountName $accountName -Name $writeSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List,Write
```

### <a name="get-sas-tokens"></a>SAS belirteci alma

Karşılık gelen SAS belirteci alma ve depolama alanına çağrı. `-SecretName` girdiden kullanılarak oluşturulan `AccountName` ve `Name` parametreler, çalıştırıldığında [kümesi AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition).

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Depolama oluşturma

İle erişmeye dikkat *$readSasToken* biz ile erişebilir, ancak bu, başarısız *$writeSasToken*.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

# Ensure the txt file in command exists in local path mentioned
Set-AzureStorageBlobContent -Container containertest1 -File "./abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "./file.txt" -Context $context2
```

Mümkün erişimi bir depolama blobu yazma erişimi olan SAS belirteci ile içerik olursunuz.

### <a name="relevant-powershell-cmdlets"></a>İlgili Powershell cmdlet'leri

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Güncelleştirme AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="storage-account-onboarding"></a>Depolama hesabı ekleme

Örnek: bir depolama hesabı nesnesini Azure anahtar Kasası'na yerleşik bir depolama hesabı eklediğiniz bir anahtar kasası nesne sahibi.

Onboarding hesabının kimliğini izni olduğunu doğrulamak Key Vault ekleme sırasında gereken *listesi* ve *yeniden* depolama anahtarları. Bu izinleri doğrulamak için Key Vault bir OBO (adına birine) kimlik doğrulama hizmeti, Azure Resource Manager için ayarlanmış hedef kitle belirteci alır ve yapar bir *listesi* Azure depolama hizmetine çağrı anahtar. Varsa *listesi* çağrısı başarısız olursa, nesne oluşturma başarısız bir HTTP durum kodu ile Key Vault *Yasak*. Bu şekilde listelenen anahtarları ile anahtar kasası varlık depolama alanınızı önbelleğe alınır.

Key Vault kimlik olduğunu doğrulamalısınız *yeniden* anahtarlarınızı yeniden sahip olma olabilmesi izinleri. Key Vault birinci taraf kimlik yanı sıra OBO belirteci aracılığıyla kimliğinin bu izinlere sahip olduğunu doğrulamak için:

- Key Vault depolama hesabı kaynağı RBAC izinleri listeler.
- Key Vault aracılığıyla eylemler Eylemler ve normal ifadeyle eşleşen yanıt doğrular.

Destekleyen bazı örneklere Bul [Key Vault - yönetilen depolama hesabı anahtarları örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=).

Kimlik yoksa *yeniden* izinler veya Key Vault'un birinci taraf kimlik sahip değilse *listesi* veya *yeniden* izni, ardından ekleme İstek bir uygun hata kodu ve bir ileti döndürerek başarısız olur.

OBO belirteci, yalnızca kullandığınız zaman birinci taraf, PowerShell veya CLI yerel istemci uygulamalarıyla çalışır.

## <a name="other-applications"></a>Diğer uygulamalar

- Key Vault depolama hesabı anahtarları, kullanılarak oluşturulan SAS belirteçleri, Azure depolama hesabınız için daha da denetimli erişim sağlar. Daha fazla bilgi için [paylaşılan erişim imzaları kullanma](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Ayrıca bkz.

- [Anahtarlar, gizli diziler ve sertifikalar hakkında](https://docs.microsoft.com/rest/api/keyvault/)
- [Anahtar kasası ekibi blogu](https://blogs.technet.microsoft.com/kv/)
