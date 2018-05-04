---
ms.assetid: ''
title: Azure anahtar kasası depolama hesabı anahtarları
description: Depolama hesabı anahtarları Azure anahtar kasası ve anahtar tabanlı erişim arasında seemless tümleştirme Azure depolama hesabı sağlayın.
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 4f42a47a6d934bf0538efccbcf7f057fd28e2c03
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
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
1. Azure uygulama hizmeti uygulama ayarları veya başka bir depolama deposu bağlantı dizesi veya SAS belirteci.
1. Uygulama başlatma sırasında bağlantı dizesi veya SAS belirteci getirme.
1. Oluşturma [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) depolama ile etkileşim kurmak için.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Azure anahtar kasası depolama anahtarları sonra

Geliştiriciler oluşturma bir [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient) ve bunların depolama alanı için SAS belirteci almak için yararlanın. Daha sonra oluşturdukları [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) Bu belirteci ile.

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

- Yalnızca anahtar kasası ASA tuşlarınızı yönetme izin verir. Kendiniz yönetmeye çalışmayın, anahtar Kasası'nın işlemlerle çalışmasını engeller.
- Birden fazla anahtar kasası nesnesi tarafından yönetilecek ASA anahtarları izin vermez.
- ASA anahtarlarınızı el ile yeniden oluşturmanız gerekiyorsa, bunları anahtar kasası yeniden öneririz.

## <a name="getting-started"></a>Başlarken

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Rol tabanlı erişim denetimi (RBAC) izinler için Kurulum

Azure anahtar kasası uygulama kimliği için izin gerektirmesidir *listesi* ve *yeniden* için bir depolama hesabı anahtarları. Aşağıdaki adımları kullanarak bu izinlerini ayarlayabilirsiniz:

```powershell
# Get the resource ID of the Azure Storage Account you want to manage.
# Below, we are fetching a storage account using Azure Resource Manager
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure Key Vault Identity
$servicePrincipal = Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ObjectId $servicePrincipal.Id -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

    >[!NOTE]
    > For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role."*

## <a name="working-example"></a>Çalışan örnek

Aşağıdaki örnek, bir anahtar kasası oluşturma Azure depolama hesabı ve ilişkili paylaşılan erişim imzası (SAS) tanımları yönetilen gösterir.

### <a name="prerequisite"></a>Önkoşul

Tamamladığınızdan emin olun [Kurulum rol tabanlı erişim denetimi (RBAC) izinlerini](#setup-for-role-based-access-control-rbac-permissions).

### <a name="setup"></a>Kurulum

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Ardından, izinlerini ayarlamak **hesabınızı** anahtar Kasası'nda tüm depolama izinlerini yönetebilir emin olmak için. Aşağıdaki örnekte, Azure bizim hesabıdır _developer@contoso.com_.

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Bir anahtar kasası oluşturma yönetilen depolama hesabı

Artık Azure anahtar kasası yönetilen depolama hesabı oluşturma ve SAS belirteci oluşturmak için depolama hesabınızdan bir erişim tuşunu kullanın.
- `-ActiveKeyName` SAS belirteçleri oluşturmak için 'key2' kullanır.
- `-AccountName` Yönetilen depolama hesabınızı tanımlamak için kullanılır. Aşağıda, depolama hesabı adı basit tutmak için kullanıyoruz ancak herhangi bir ad olabilir.
- `-DisableAutoRegenerateKey` belirtir depolama hesabı anahtarlarını yeniden değil.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Anahtarını yeniden üretme

Anahtar kasası depolama kişinin erişim tuşlarını düzenli aralıklarla yeniden oluşturmak istiyorsanız, yeniden üretme süresi ayarlayabilirsiniz. Aşağıda, yeniden üretme süresi 3 gün ayarlıyorsunuz. 3 gün sonra anahtar Kasası 'key1' yeniden ve 'key1' active 'key2' anahtarını değiştirme.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Kümesi SAS tanımları

Hesap SAS farklı izinlerle blob hizmetine erişim sağlar.
SAS tanımları yönetilen depolama hesabınız için anahtar kasasına ayarlayın.
- `-AccountName` Anahtar kasası yönetilen depolama hesabında adıdır.
- `-Name` Depolama alanınızın içinde SAS belirteci tanımlayıcısıdır.
- `-ValidityPeriod` oluşturulan SAS belirteci sona erme tarihini ayarlar.

```powershell
$validityPeriod = [System.Timespan]::FromDays(1)
$readSasName = "readBlobSas"
$writeSasName = "writeBlobSas"

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName $keyVaultName -AccountName $accountName -Name $readSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName $keyVaultName -AccountName $accountName -Name $writeSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List,Write
```

### <a name="get-sas-tokens"></a>SAS belirteci alma

Karşılık gelen SAS belirteci alın ve depolama birimine çağrıları yapma. `-SecretName` girdisinden kullanılarak oluşturulan `AccountName` ve `Name` , çalıştırıldığında parametreleri [kümesi AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition).

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Depolama oluşturma

İle erişmeye dikkat *$readSasToken* biz ile erişebilir, ancak bu, başarısız *$writeSasToken*.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt" -Context $context2
```

Mümkün erişim yazma erişimi olduğundan SAS belirteci ile içerik depolama blob var.

### <a name="relevant-powershell-cmdlets"></a>İlgili Powershell cmdlet'leri

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Güncelleştirme AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

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
