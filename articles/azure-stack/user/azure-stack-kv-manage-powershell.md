---
title: PowerShell kullanarak Azure yığınında anahtar kasası yönetme | Microsoft Docs
description: PowerShell kullanarak Azure yığınında anahtar kasası yönetmeyi öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.openlocfilehash: 5e9de401f64a835c286c226bfac88caf5168b96e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075774"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>Anahtar kasası Azure PowerShell kullanarak yığınında yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Anahtar kasası Azure PowerShell kullanarak yığınında yönetebilirsiniz. Anahtar kasası PowerShell cmdlet'lerinin nasıl kullanılacağını öğrenin:

* Bir anahtar kasası oluşturun.
* Depolayın ve şifreleme anahtarları ve gizli anahtarları yönetin.
* Kullanıcılar veya uygulamalar invoke kasasında işlemleri yetkilendirin.

>[!NOTE]
>Anahtar kasası PowerShell cmdlet'leri uyguladıktan bu makalede Azure PowerShell SDK'da sağlanır.

## <a name="prerequisites"></a>Önkoşullar

* Azure anahtar kasası hizmetindeki içeren bir teklif abone olmalısınız.
* [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).
* [Azure yığın kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Anahtar kasası işlemleri için Kiracı aboneliği etkinleştir

Bir anahtar kasası karşı herhangi bir işlem vermeden önce Kiracı aboneliğinize kasası işlemleri için etkinleştirildiğinden emin olmak gerekir. Kasa işlemlerinin etkin olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```PowerShell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**Çıktı**

Aboneliğinizi kasası işlemleri için etkinleştirilirse, çıkış "RegistrationState" "bir anahtar kasası tüm kaynak türleri için kayıtlı" gösterir.

![Anahtar kasası kayıt durumu](media/azure-stack-kv-manage-powershell/image1.png)

Kasası işlemleri etkinleştirilmezse, anahtar kasası hizmetindeki aboneliğinizde kaydetmek için aşağıdaki komutu çağırırsınız:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Çıktı**

Kayıt başarılı olursa, aşağıdaki çıkış verilir:

![Kayıt](media/azure-stack-kv-manage-powershell/image2.png) anahtar kasası komutları çağırdığınızda, "abonelik 'Microsoft.KeyVault' ad alanını kullanmak için kayıtlı değil."gibi bir hata alabilirsiniz Bir hata alırsanız, sahip olduğunuz onaylayın [anahtar kasası kaynak sağlayıcısı etkin](#enable-your-tenant-subscription-for-vault-operations) daha önce bahsedilen yönergeleri izleyerek.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Bir anahtar kasası oluşturmadan önce böylece tüm anahtar Kasası'na ilgili kaynaklar bir kaynak grubunda mevcut bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```PowerShell
New-AzureRmResourceGroup -Name “VaultRG” -Location local -verbose -Force

```

**Çıktı**

![Yeni kaynak grubu](media/azure-stack-kv-manage-powershell/image3.png)

Şimdi kullanın **New-AzureRMKeyVault** daha önce oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturmak için komutu. Bu komut üç zorunlu parametreye okur: kaynak grubu adı, anahtar kasası adı ve coğrafi konum.

Bir anahtar kasası oluşturmak için aşağıdaki komutu çalıştırın:

```PowerShell
New-AzureRmKeyVault -VaultName “Vault01” -ResourceGroupName “VaultRG” -Location local -verbose
```

**Çıktı**

![Yeni anahtar kasası](media/azure-stack-kv-manage-powershell/image4.png)

Bu komutun çıktısı, oluşturduğunuz anahtar kasasının özelliklerini gösterir. Bir uygulama bu kasaya eriştiğinde kullanmalısınız **kasa URI'sini** olan özelliği, "https://vault01.vault.local.azurestack.external" Bu örnekte.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Active Directory Federasyon Hizmetleri (AD FS) dağıtımı

Bu uyarı bir AD FS dağıtımında alabilirsiniz: "Erişim İlkesi ayarlanmadı. Hiçbir kullanıcı veya uygulama bu kasaya kullanmak için erişim izni var." Bu sorunu çözmek için kasa için bir erişim ilkesi kullanarak ayarlamak [kümesi AzureRmKeyVaultAccessPolicy](azure-stack-kv-manage-powershell.md#authorize-an-application-to-use-a-key-or-secret) komutu:

```PowerShell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Anahtarları ve gizli anahtarları Yönet

Bir kasası oluşturduktan sonra anahtarları ve gizli anahtarları kasadaki oluşturmak ve yönetmek için aşağıdaki adımları kullanın.

### <a name="create-a-key"></a>Bir anahtar oluşturma

Kullanım **Add-AzureKeyVaultKey** oluşturmak veya bir yazılım korumalı bir anahtar kasası anahtarında almak için komutu.

```PowerShell
Add-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01” -verbose -Destination Software
```

**Hedef** parametresi anahtarı yazılım korumalı olduğunu belirtmek için kullanılır. Anahtar başarıyla oluşturulduğunda komutu oluşturulan anahtarı ayrıntılarını çıkarır.

**Çıktı**

![Yeni anahtar](media/azure-stack-kv-manage-powershell/image5.png)

Artık oluşturulan anahtarı URI'sini kullanarak başvurabilirsiniz. Oluşturun veya varolan bir anahtarın aynı ada sahip bir anahtar alın, özgün anahtar yeni anahtarında belirtilen değerlerle güncelleştirilir. Önceki sürüm anahtarı sürüme özgü URI'sini kullanarak erişebilirsiniz. Örneğin:

* Kullan "https://vault10.vault.local.azurestack.external:443/keys/key01" her zaman geçerli sürümü almak için.
* Kullan "https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a" Bu belirli sürümü almak için.

### <a name="get-a-key"></a>Bir anahtarı edinme

Kullanım **Get-AzureKeyVaultKey** bir anahtar ve ayrıntılarını okumak için komutu.

```PowerShell
Get-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01”
```

### <a name="create-a-secret"></a>Gizli anahtar oluşturma

Kullanım **kümesi AzureKeyVaultSecret** oluşturmak veya bir kasadaki gizli güncelleştirmek için komutu. Bir gizli anahtarı zaten yoksa oluşturulur. Gizli yeni bir sürümü zaten mevcut değilse oluşturulur.

```PowerShell
$secretvalue = ConvertTo-SecureString “User@123” -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01” -SecretValue $secretvalue
```

**Çıktı**

![Gizli anahtar oluşturma](media/azure-stack-kv-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Bir gizli anahtar alma

Kullanım **Get-AzureKeyVaultSecret** gizli bir anahtar kasasına okumak için komutu. Bu komut tüm döndürebilir ya da belirli bir gizli anahtarın sürümlerini.

```PowerShell
Get-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01”
```

Anahtarları ve gizli anahtarları oluşturduktan sonra dış uygulamaları bunları kullanmaları için yetki verebilir.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Bir uygulamanın bir anahtar veya gizli kullanın

Kullanım **kümesi AzureRmKeyVaultAccessPolicy** bir anahtar veya gizli anahtar kasasında erişmek için bir uygulamanın komutu.
Aşağıdaki örnekte, kasa addır *ContosoKeyVault* ve yetkilendirmek istediğiniz uygulama istemci Kimliğini *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*. Uygulama yetkilendirmek için aşağıdaki komutu çalıştırın. İsteğe bağlı olarak, belirtebilirsiniz **PermissionsToKeys** parametresini kullanarak bir kullanıcı, uygulama veya bir güvenlik grubu için izinleri ayarlayın.

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Aynı uygulamayı kasanızdaki gizli anahtarları okumak için yetkilendirmek istiyorsanız, aşağıdaki cmdlet'i çalıştırın:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar kasasında depolanan bir parola ile bir VM'yi dağıtmak](azure-stack-kv-deploy-vm-with-secret.md)
* [Anahtar kasasında depolanan bir sertifika ile bir VM'yi dağıtmak](azure-stack-kv-push-secret-into-vm.md)
