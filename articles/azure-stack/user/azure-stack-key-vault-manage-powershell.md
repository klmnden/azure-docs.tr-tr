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
ms.date: 01/05/2019
ms.author: sethm
ms.openlocfilehash: db68d3ac626d80361e456a251b93d847a73afb8c
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192312"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Anahtar Kasası'nda Azure Stack PowerShell kullanarak yönetebilirsiniz. Bu makale için anahtar kasası PowerShell cmdlet'lerinin nasıl kullanılacağını açıklar:

* Bir anahtar kasası oluşturma.
* Store ve şifreleme anahtarlarını ve gizli yönetin.
* Kullanıcılar veya uygulamalar operations kasada çağırmak için yetkilendirin.

>[!NOTE]
>Bu makalede anahtar kasası PowerShell cmdlet'leri uyguladıktan Azure PowerShell SDK'sı sağlanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure Key Vault hizmetini içeren bir teklife abone olması gerekir.
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
* [Azure Stack kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Anahtar kasası işlemleri için Kiracı aboneliğinizi etkinleştirme

Herhangi bir anahtar kasası işlemleri gerçekleştirmek için önce Kiracı aboneliğinize kasa işlemleri için etkinleştirildiğinden emin emin olmanız gerekir. Kasa işlemleri etkinleştirildiğini doğrulamak için aşağıdaki komutu çalıştırın:

```PowerShell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**Çıktı**

Aboneliğiniz için kasa işlemleri etkinleştirilirse, çıktı gösterir **RegistrationState** olduğu **kayıtlı** bir anahtar kasası, tüm kaynak türleri için.

![Anahtar kasası kayıt durumu](media/azure-stack-key-vault-manage-powershell/image1.png)

Kasa işlemleri etkinleştirilmezse, Key Vault hizmeti, aboneliğinize kaydetmek için aşağıdaki komutu çalıştırın:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Çıktı**

Kayıt başarılı olursa, aşağıdaki çıktı döndürülür:

![Kaydolma](media/azure-stack-key-vault-manage-powershell/image2.png)

Key Vault komutları çağırdığınızda "abonelik 'Microsoft.KeyVault' ad alanını kullanacak şekilde kaydedilmemiş."gibi bir hata alabilirsiniz Bir hata alırsanız, sahip olduğunuz onaylayın [Key Vault kaynak sağlayıcısı etkin](#enable-your-tenant-subscription-for-vault-operations) daha önce verilen yönergeleri izleyerek.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Bir anahtar kasası oluşturmadan önce böylece tüm anahtar kasasıyla ilgili kaynakları bir kaynak grubunda mevcut bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```PowerShell
New-AzureRmResourceGroup -Name "VaultRG" -Location local -verbose -Force
```

**Çıktı**

![Yeni kaynak grubu](media/azure-stack-key-vault-manage-powershell/image3.png)

Artık, **New-AzureRMKeyVault** önceden oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturmak için cmdlet'i. Bu komut üç zorunlu parametreye okur: kaynak grubu adı, anahtar kasası adı ve coğrafi konum.

Bir anahtar kasası oluşturmak için aşağıdaki komutu çalıştırın:

```PowerShell
New-AzureRmKeyVault -VaultName "Vault01" -ResourceGroupName "VaultRG" -Location local -verbose
```

**Çıktı**

![Yeni anahtar kasası](media/azure-stack-key-vault-manage-powershell/image4.png)

Bu komutun çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. Bir uygulama bu kasa eriştiğinde kullanmalısınız `Vault URI` özelliğinin `https://vault01.vault.local.azurestack.external` Bu örnekte.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Active Directory Federasyon Hizmetleri (AD FS) dağıtımı

AD FS dağıtımında, bu uyarı alabilirsiniz: "Erişim İlkesi ayarlanmadı. Herhangi bir kullanıcı veya uygulama bu kasayı kullanmak için erişim izni var." Bu sorunu çözmek için kasa için bir erişim ilkesi kullanarak ayarlamak [Set-AzureRmKeyVaultAccessPolicy](azure-stack-key-vault-manage-powershell.md#authorize-an-application-to-use-a-key-or-secret) cmdlet:

```PowerShell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Anahtarları ve gizli anahtarları yönetme

Bir anahtar kasası oluşturduktan sonra anahtarları ve kasadaki gizli anahtarları oluşturmak ve yönetmek için aşağıdaki adımları kullanın.

### <a name="create-a-key"></a>Bir anahtar oluşturma

Kullanım **Add-AzureKeyVaultKey** cmdlet'ini oluşturabilir veya bir anahtar kasasına yazılım korumalı bir anahtar alabilirsiniz.

```PowerShell
Add-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01" -verbose -Destination Software
```

`Destination` Parametre anahtarı yazılım korumalı olduğunu belirtmek için kullanılır. Komut, anahtarı başarıyla oluşturulduğunda, yeni oluşturulan anahtarı ayrıntılarını çıkarır.

**Çıktı**

![Yeni anahtar](media/azure-stack-key-vault-manage-powershell/image5.png)

Artık, yeni oluşturulan anahtarı URI'sini kullanarak başvurabilirsiniz. Oluşturun veya var olan bir anahtar aynı ada sahip bir anahtarı içeri aktarma, özgün anahtar yeni anahtarında belirtilen değerleri ile güncelleştirilir. Önceki sürümü anahtarının sürüme özgü URI'sini kullanarak erişebilirsiniz. Örneğin:

* Kullanım `https://vault10.vault.local.azurestack.external:443/keys/key01` her zaman geçerli sürümü almak için.
* Kullanım `https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a` bu belirli sürümü almak için.

### <a name="get-a-key"></a>Bir anahtarı alma

Kullanım **Get-AzureKeyVaultKey** cmdlet'i bir anahtar ve ayrıntılarını okumak için.

```PowerShell
Get-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01"
```

### <a name="create-a-secret"></a>Gizli anahtar oluşturma

Kullanım **Set-AzureKeyVaultSecret** oluşturulacak veya güncelleştirilecek bir kasada bir gizli dizi cmdlet'i. Gizli dizi bir zaten mevcut değilse oluşturulur. Zaten varsa, yeni bir gizli dizi sürümü oluşturulur.

```PowerShell
$secretvalue = ConvertTo-SecureString "User@123" -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01" -SecretValue $secretvalue
```

**Çıktı**

![Gizli anahtar oluşturma](media/azure-stack-key-vault-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Gizli dizi alma

Kullanım **Get-AzureKeyVaultSecret** cmdlet'i bir anahtar kasasındaki gizli dizi okumak için. Bu komut tüm döndürebilir veya belirli bir gizli anahtarın sürümlerini.

```PowerShell
Get-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01"
```

Anahtarları ve gizli anahtarları oluşturduktan sonra dış uygulamalara bunları kullanabilmeleri için yetki verebilir.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Bir uygulamayı bir anahtar veya gizli anahtarı kullanması için yetkilendirin

Kullanım **Set-AzureRmKeyVaultAccessPolicy** cmdlet'i, bir uygulamanın bir anahtar veya gizli anahtar kasasındaki erişim yetkisi vermek için. Aşağıdaki örnekte, kasa adı olan `ContosoKeyVault`, ve yetkilendirmek istediğiniz uygulama istemci Kimliğini `8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed`. Uygulama yetkilendirmek için aşağıdaki komutu çalıştırın. İsteğe bağlı olarak belirleyebileceğiniz `PermissionsToKeys` parametresini kullanarak bir kullanıcı, uygulama veya bir güvenlik grubu için izinleri ayarlayın.

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız, aşağıdaki cmdlet'i çalıştırın:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Sonraki adımlar

* [Key Vault'ta depolanan bir parola ile VM dağıtma](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Key Vault'ta depolanan bir sertifika ile VM dağıtma](azure-stack-key-vault-push-secret-into-vm.md)
