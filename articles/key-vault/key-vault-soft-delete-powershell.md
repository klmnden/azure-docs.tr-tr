---
ms.assetid: ''
title: Azure Key Vault - PowerShell ile geçici silmeyi kullanma
description: Kullanım büyük/küçük kod parçalarını PowerShell ile geçici silme örnekleri
services: key-vault
author: bryanla
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bryanla
ms.openlocfilehash: 174a5b41e6a48ea74cd813746b7c070463a8185b
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42059214"
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Key Vault geçici silmeyi PowerShell ile kullanma

Azure Key Vault'un geçici silme özelliği silinen kasa ve kasa nesneleri kurtarılmasını sağlar. Özellikle, geçici adresleri aşağıdaki senaryolarda silme:

- Bir anahtar kasası kurtarılabilir silinmesi için destek
- Anahtar kasası nesne kurtarılabilir silme desteği; , gizli dizileri, anahtarlar ve sertifikalar

## <a name="prerequisites"></a>Önkoşullar

- Azure PowerShell 4.0.0 veya yoksa zaten bu Kurulum, Azure PowerShell'i yükleme ve Azure aboneliğinizle ilişkilendirin, daha sonra - bakın [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Yoktur, anahtar kasası PowerShell çıktı biçimlendirmesi güncel olmayan sürümüne dosya **olabilir** doğru sürümü yerine ortamınıza yüklenen. Biz, PowerShell, çıktıyı biçimlendirmek için gereken düzeltme içerecek şekilde güncelleştirilmiş bir sürümünü kapasitesinden ve o anda bu konuyu güncelleştireceğiz. Geçerli çözüm biçimlendirme sorunun karşılaşmamalıdırlar olan:
> - Etkin özelliği bu konuda açıklanan değil gördüğünüz geçici silmeyi fark ederseniz aşağıdaki sorguyu kullanın: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


PowerShell için anahtar kasası özel referans bilgileri için bkz. [Azure anahtar kasası PowerShell başvurusu](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Gerekli izinler

Anahtar kasası işlemleri ayrı olarak şu şekilde rol tabanlı erişim denetimi (RBAC) izinleri yönetilir:

| İşlem | Açıklama | Kullanıcı izni |
|:--|:--|:--|
|Liste|Listeleri anahtar kasalarını silindi.|Microsoft.KeyVault/deletedVaults/read|
|Kurtar|Silinen bir anahtar kasasına yükler.|Microsoft.KeyVault/vaults/write|
|Temizle|Silinen bir anahtar kasasını ve tüm içerikleri kalıcı olarak kaldırır.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

İzinler ve erişim denetimi hakkında daha fazla bilgi için bkz. [anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Geçici silmeyi etkinleştirme

Silinen bir anahtar kasası veya bir anahtar Kasası'nda depolanan nesnelere kurtarmanız mümkün olması için önce bu key vault için geçici silme etkinleştirmeniz gerekir.

### <a name="existing-key-vault"></a>Var olan bir anahtar kasası

Varolan anahtar kasasında ContosoVault adlı bir geçici silme aşağıdaki gibi etkinleştirin. 

>[!NOTE]
>Şu anda doğrudan yazmak için Azure Resource Manager kaynak düzenlemesi kullanmanız gereken *enableSoftDelete* Key Vault kaynak özelliği.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Yeni anahtar kasası

Yeni key vault geçici silmeyi etkinleştirme oluşturma sırasında geçici silmeyi etkinleştir bayrak ekleyerek gerçekleştirilir, create komutu.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Geçici silmeyi etkinleştirme doğrulayın

Key vault geçici silmeyi etkin olduğunu doğrulamak için çalıştırın *alma* komut ve 'geçici Sil için etkin mi?' arayın özniteliği ve kendi ayarı true veya false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Geçici silme ile korumalı bir anahtar kasası silme

Bir anahtar kasasını silme (veya kaldırma için) komutu aynı kalır, ancak etkin olup olmadığı, geçici silme veya bağlı olarak davranışını değiştirir.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Geçici silme etkin olmayan bir anahtar kasası için önceki komutu çalıştırırsanız, bu anahtar kasası ve kurtarma için hiçbir seçenek olmadan tüm içeriği kalıcı olarak siler.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Geçici silme anahtar kasalarınıza nasıl korur

Geçici silme ile etkin:

- Bir anahtar kasası silindiğinde, kaynak grubundan kaldırılmış olduğundan ve yalnızca ayrılmış bir ad alanında yerleştirildiğinden oluşturulduğu yere konumu ile ilişkili. 
- Nesneler, silinen bir anahtar kasası gibi anahtarları, gizli diziler ve sertifikalar, erişilemez ve silinmiş durumda olsa da içeren, anahtar kasası bunu kalır. 
- Aynı ada sahip yeni bir anahtar kasası oluşturulamıyor, silinmiş durumda bir anahtar kasası için DNS adı ayrılmış olduğundan.  

Aşağıdaki komutu kullanarak, aboneliğinizle ilişkili silindi durumu anahtar kasalarını görüntüleyebilirsiniz:

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

*Kaynak kimliği* çıktısında bu kasa özgün kaynak Kimliğine başvuruyor. Bu anahtar kasası artık silinmiş bir durumda olduğundan, bu kaynak kimliği ile kaynak var. *Kimliği* alan kurtarma veya temizleme kaynağı tanımlamak için kullanılabilir. *Temizleme tarihine zamanlanmış* alanının ne zaman kasa kalıcı olarak silinecek (temizleneceği) silinen bu kasa için bir eylem varsa. Hesaplamak için kullanılan varsayılan saklama süresi *temizleme tarihine zamanlanmış*, 90 gündür.

## <a name="recovering-a-key-vault"></a>Bir anahtar kasası kurtarma

Bir anahtar kasası kurtarmak için anahtar kasası adı, kaynak grubunu ve konumu belirtmeniz gerekir. Bu anahtar kasası kurtarma işlemi için gereken konum ve kaynak grubu silindi anahtar kasasının unutmayın.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Bir anahtar kasası kurtarıldığında, anahtar kasasının özgün kaynak kimliği ile yeni bir kaynak sonucudur Burada anahtar kasası varolan kaynak grubu kaldırıldıysa, anahtar kasası kurtarılabilir önce aynı ada sahip yeni bir kaynak grubu oluşturulmalıdır.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault nesneleri ve geçici silme

'ContosoFirstKey' anahtar için geçici silme ile ' ContosoVault etkin' adlı bir anahtar kasasının içinde burada nasıl anahtara silmeniz.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Geçici silme için etkin anahtar kasanız ile silinen bir anahtar gösterilmeye devam eder dışında silindi gibi açıkça listesinde veya silinen anahtarlarını alma. Bir anahtarı silindi durumunda ilgili işlemlerin çoğu silinen bir anahtar listesi, onu kurtarma veya onu temizleme dışında başarısız olur. 

Örneğin, liste silindi anahtarları key vault'ta istemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Geçiş durumu 

Bir anahtarı bir anahtar Kasası'nda etkin ile geçici silme sildiğinizde, bu geçişin tamamlanması birkaç saniye sürebilir. Bu geçiş aşamasında, anahtar etkin durumunda veya silinmiş durumda değil görünebilir. Bu komut tüm silinen 'ContosoVault' adlı anahtar kasanıza anahtarlarını listeler.

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar kasası nesneleri ile geçici silmeyi kullanma

Kurtarma veya onu temizlemek sürece yalnızca anahtar kasası, silinen bir anahtar gibi parola veya sertifika silinmiş durumda 90 güne kadar kalır. 

#### <a name="keys"></a>Anahtarlar

Silinen bir anahtar kurtarmak için:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Bir anahtar kalıcı olarak silmek için:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Bir anahtar temizleme, yani kurtarılabilir olmayacaktır kalıcı olarak silinir.

**Kurtarmak** ve **Temizleme** eylemleri bir anahtar kasası erişim ilkesini ilişkili kendi izinlere sahiptir. Bir kullanıcı veya hizmet sorumlusu yürütmek bir **kurtarmak** veya **Temizleme** bunlar ilgili izniniz olmalıdır (anahtar veya gizli anahtarı) söz konusu nesnenin anahtar kasası erişim ilkesini eylem. Varsayılan olarak, **Temizleme** izni bir kullanıcı için tüm izinleri vermek için 'Tümü' kısayol kullanıldığında bir anahtar kasasının erişim ilkesini eklenmez. Açıkça vermelidir **Temizleme** izni. Örneğin, aşağıdaki verir komut user@contoso.com anahtarlarında üzerinde çeşitli işlemler gerçekleştirmek için izni *ContosoVault* dahil olmak üzere **Temizleme**.

#### <a name="set-a-key-vault-access-policy"></a>Bir anahtar kasası erişim ilkesini ayarlama

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Geçici silme etkinleştirilebilir yalnızca olan mevcut bir anahtar kasası varsa olmayabilir **kurtarmak** ve **Temizleme** izinleri.

#### <a name="secrets"></a>Gizli Diziler

Anahtarları gibi kendi komutları ile temel bir anahtar kasasındaki gizli dizileri işletilir. Ardından, silme, listeleme, kurtarma ve gizli dizileri temizleme komutlardır.

- SQLPassword adlı bir gizli anahtarı silin: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Bir anahtar kasasındaki tüm silinen gizli dizileri listeleme: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Gizli dizi silindi durumunda kurtarma: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Gizli dizi silinmiş durumda temizleme: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Gizli dizi temizleme, yani kurtarılabilir olmayacaktır kalıcı olarak silinir.

## <a name="purging-and-key-vaults"></a>Temizleme ve anahtar kasaları

### <a name="key-vault-objects"></a>Anahtar kasası nesneleri

Bir anahtar temizleme, parola veya sertifika, yani kurtarılabilir olmayacaktır kalıcı olarak silinir. Anahtar Kasası'nda tüm nesneleri olarak silinen nesnenin içerdiği anahtar kasasına ancak değişmeden kalır. 

### <a name="key-vaults-as-containers"></a>Kapsayıcıları olarak anahtar kasaları
Bir anahtar kasası temizlenir, tüm anahtarlar, parolalar ve sertifikalar dahil olmak üzere içeriğini kalıcı olarak silinir. Bir anahtar kasası temizlemek için kullanmak `Remove-AzureRmKeyVault` seçeneği ile komut `-InRemovedState` ve Silinen key vault ile konumunu belirterek `-Location location` bağımsız değişken. Komutunu kullanarak silinen bir kasa konumunu bulabilirsiniz `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Bir anahtar kasası temizleme, yani kurtarılabilir olmayacaktır kalıcı olarak silinir.

### <a name="purge-permissions-required"></a>Gerekli izinler Temizle
- Kasayı ve tüm içerikleri kalıcı olarak kaldırılır, silinen bir anahtar kasasını temizlemek için kullanıcının RBAC izni gerçekleştirmek için gereken bir *Microsoft.KeyVault/locations/deletedVaults/purge/action* işlemi. 
- Silinen bir anahtar listesi kasa kullanıcı gerçekleştirmek için RBAC izni gerekiyor, *Microsoft.KeyVault/deletedVaults/read* izni. 
- Varsayılan olarak yalnızca bir abonelik yöneticisinin bu izinlere sahiptir. 

### <a name="scheduled-purge"></a>Zamanlanmış bir temizleme

Silinen anahtar kasasını nesnelerinizi listeleme schedled Key Vault tarafından temizlenmesi olduklarında gösterir. *Temizleme tarihine zamanlanmış* alan sağlayacağını ne zaman bir anahtar kasası nesne kalıcı olarak silinecek, hiçbir işlem yapılmaz. Varsayılan olarak, silinen anahtar kasasını nesnesi için saklama süresi 90 gündür.

>[!NOTE]
>Tarafından tetiklenen bir Temizlenen kasasını kendi *temizleme tarihine zamanlanmış* alan, kalıcı olarak silinir. Kurtarılabilir değil.

## <a name="other-resources"></a>Diğer kaynaklar

- Key Vault geçici silme özelliği genel bakış için bkz. [Azure Key Vault geçici silmeyi genel bakış](key-vault-ovw-soft-delete.md).
- Azure Key Vault kullanımı genel bir bakış için bkz: [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md).

