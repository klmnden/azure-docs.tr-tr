---
title: Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetme | Microsoft Docs
description: PowerShell kullanarak anahtar Kasası'nda Azure Stack yönetmeyi öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: sethm
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: d2324f9538ce8079be5e660a1613c1c093ecc85a
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484605"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetebilirsiniz. Anahtar kasası PowerShell cmdlet'leri için kullanmayı öğrenin:

* Bir anahtar kasası oluşturma.
* Store ve şifreleme anahtarlarını ve gizli yönetin.
* Kullanıcılar veya uygulamalar operations kasada çağırmak için yetkilendirin.

>[!NOTE]
>Bu makalede açıklanan anahtar kasası PowerShell cmdlet'leri, Azure PowerShell SDK'sı sağlanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure Key Vault hizmetini içeren bir teklife abone olması gerekir.
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
* [Azure Stack kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Anahtar kasası işlemleri için Kiracı aboneliğinizi etkinleştirme

Herhangi bir anahtar kasası işlemleri vermeden önce Kiracı aboneliğinize kasa işlemleri için etkinleştirildiğinden emin olmak gerekir. Kasa işlemleri etkinleştirildiğini doğrulamak için aşağıdaki komutu çalıştırın:

```powershell  
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**Çıktı**

Aboneliğiniz için kasa işlemleri etkinleştirilirse, çıktı "RegistrationState" kayıtlı"bir anahtar kasası tüm kaynak türleri için" gösterir.

![Anahtar kasası kayıt durumu](media/azure-stack-key-vault-manage-powershell/image1.png)

Kasa işlemleri etkinleştirilmezse, Key Vault hizmeti, aboneliğinize kaydetmek için aşağıdaki komutu çağırır:

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Çıktı**

Kayıt başarılı olursa, aşağıdaki çıktı döndürülür:

![Kayıt](media/azure-stack-key-vault-manage-powershell/image2.png) çağırdığınızda, anahtar kasası komutları "abonelik 'Microsoft.KeyVault' ad alanını kullanacak şekilde kaydedilmemiş."gibi bir hata alabilirsiniz Bir hata alırsanız, daha önce bahsedilen yönergeleri izleyerek Key Vault kaynak sağlayıcısı etkinleştirdiyseniz onaylayın.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Bir anahtar kasası oluşturmadan önce böylece tüm anahtar kasasıyla ilgili kaynakları bir kaynak grubunda mevcut bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroup -Name "VaultRG" -Location local -verbose -Force

```

**Çıktı**

![Yeni kaynak grubu](media/azure-stack-key-vault-manage-powershell/image3.png)

Artık, **New-AzureRMKeyVault** daha önce oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturmak için komutu. Bu komut üç zorunlu parametreye okur: kaynak grubu adı, anahtar kasası adı ve coğrafi konum.

Bir anahtar kasası oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-AzureRmKeyVault -VaultName "Vault01" -ResourceGroupName "VaultRG" -Location local -verbose
```

**Çıktı**

![Yeni anahtar kasası](media/azure-stack-key-vault-manage-powershell/image4.png)

Bu komutun çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. Bir uygulama bu kasa eriştiğinde kullanmalısınız **Vault URI'si** özelliğinin "https:\//vault01.vault.local.azurestack.external" Bu örnekte.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Active Directory Federasyon Hizmetleri (AD FS) dağıtımı

AD FS dağıtımında, bu uyarı alabilirsiniz: "Erişim İlkesi ayarlanmadı. Herhangi bir kullanıcı veya uygulama bu kasayı kullanmak için erişim izni var." Bu sorunu çözmek için kasa için bir erişim ilkesi kullanarak ayarlamak [Set-AzureRmKeyVaultAccessPolicy](#authorize-an-application-to-use-a-key-or-secret) komutu:

```powershell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Anahtarları ve gizli anahtarları yönetme

Bir kasayı oluşturduktan sonra anahtarları ve kasadaki gizli anahtarları oluşturmak ve yönetmek için aşağıdaki adımları kullanın.

### <a name="create-a-key"></a>Bir anahtar oluşturma

Kullanım **Add-AzureKeyVaultKey** oluşturmak veya bir anahtar Kasası'nda yazılım korumalı bir anahtarı içeri aktarmak için komutu.

```powershell
Add-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01" -verbose -Destination Software
```

**Hedef** parametre anahtarı yazılım korumalı olduğunu belirtmek için kullanılır. Komut, anahtarı başarıyla oluşturulduğunda oluşturulan anahtarı ayrıntılarını çıkarır.

**Çıktı**

![Yeni anahtar](media/azure-stack-key-vault-manage-powershell/image5.png)

Artık, oluşturulan anahtar URI'sini kullanarak başvurabilirsiniz. Oluşturun veya var olan bir anahtar aynı ada sahip bir anahtarı içeri aktarma, özgün anahtar yeni anahtarında belirtilen değerleri ile güncelleştirilir. Önceki sürümü anahtarının sürüme özgü URI'sini kullanarak erişebilirsiniz. Örneğin:

* Kullanım "https:\//vault10.vault.local.azurestack.external:443/keys/key01" her zaman geçerli sürümü almak için.
* Kullanım "https:\//vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a" Bu belirli sürümü almak için.

### <a name="get-a-key"></a>Bir anahtarı alma

Kullanım **Get-AzureKeyVaultKey** bir anahtar ve ayrıntılarını okumak için komutu.

```powershell
Get-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01"
```

### <a name="create-a-secret"></a>Gizli anahtar oluşturma

Kullanım **Set-AzureKeyVaultSecret** oluşturulacak veya güncelleştirilecek bir kasada bir gizli dizi komutu. Gizli dizi önceden yoksa oluşturulur. Zaten varsa, yeni bir gizli dizi sürümü oluşturulur.

```powershell
$secretvalue = ConvertTo-SecureString "User@123" -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01" -SecretValue $secretvalue
```

**Çıktı**

![Gizli anahtar oluşturma](media/azure-stack-key-vault-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Gizli dizi alma

Kullanım **Get-AzureKeyVaultSecret** bir anahtar kasasındaki gizli dizi okumak için komutu. Bu komut tüm döndürebilir veya belirli bir gizli anahtarın sürümlerini.

```powershell
Get-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01"
```

Anahtarları ve gizli anahtarları oluşturduktan sonra dış uygulamalara bunları kullanabilmeleri için yetki verebilir.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Bir uygulamayı bir anahtar veya gizli anahtarı kullanması için yetkilendirin

Kullanım **Set-AzureRmKeyVaultAccessPolicy** bir anahtar veya gizli anahtar Kasası'nda erişmek için bir uygulamanın komutu.
Aşağıdaki örnekte, kasa adı olan *ContosoKeyVault* ve yetkilendirmek istediğiniz uygulama istemci Kimliğini *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*. Uygulama yetkilendirmek için aşağıdaki komutu çalıştırın. İsteğe bağlı olarak belirleyebileceğiniz **PermissionsToKeys** parametresini kullanarak bir kullanıcı, uygulama veya bir güvenlik grubu için izinleri ayarlayın.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız, aşağıdaki cmdlet'i çalıştırın:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault'ta depolanan bir parola ile VM dağıtma](azure-stack-kv-deploy-vm-with-secret.md)
* [Key Vault'ta depolanan bir sertifika ile VM dağıtma](azure-stack-kv-push-secret-into-vm.md)
