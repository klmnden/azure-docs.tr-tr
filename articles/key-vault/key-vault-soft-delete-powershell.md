---
ms.assetid: 
title: "Azure anahtar kasası - soft-delete PowerShell ile kullanma"
description: "Kullanım örneği soft-delete PowerShell kod parçalarını ile örnekleri"
services: key-vault
author: lleonard-msft
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: alleonar
ms.openlocfilehash: 48569e31e6400e3ec8958e0bceda1fd3b72207ea
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Anahtar kasası soft-delete PowerShell ile kullanma

Azure anahtar Kasası'nın geçici silme özelliği silinen kasalarını ve kasa nesneleri kurtarılmasını sağlar. Özellikle, soft-adresleri aşağıdaki senaryolarda Sil:

- Bir anahtar kasası kurtarılabilir silinmesini desteği
- Anahtar kasası nesnelerin kurtarılabilir silinmesi desteği; , gizli anahtarları ve sertifikaları

## <a name="prerequisites"></a>Önkoşullar

- Azure PowerShell 4.0.0 veya yoksa zaten bu Kurulum, Azure PowerShell'i yükleme ve Azure aboneliğinizle ilişkilendirmek, daha sonra - bkz. [Azure PowerShell'i yükleme ve yapılandırma nasıl](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Yoktur, anahtar kasası PowerShell çıktı biçimlendirmesi eski bir sürümü dosya **olabilir** doğru sürümü yerine ortamınıza yüklü olmalıdır. Biz PowerShell çıktıyı biçimlendirmek için gereken düzeltme içerecek şekilde güncelleştirilmiş bir sürümünü bekleme ve bu konuda, o anda güncelleştirir. Geçerli geçici çözüm bu biçimlendirme sorunla karşılaştığınızda değil:
> - Aşağıdaki sorgu değil gördüğünüz soft-delete fark ederseniz bu konuda açıklanan özelliği etkin kullanın: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Anahtar kasası belirli referans için PowerShell bilgi [Azure anahtar kasası PowerShell başvurusu](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Gerekli izinler

Anahtar kasası işlemleri ayrı olarak aracılığıyla rol tabanlı erişim denetimi (RBAC) izinleri gibi yönetilen:

| İşlem | Açıklama | Kullanıcı izni |
|:--|:--|:--|
|Liste|Listeleri anahtar kasalarını silindi.|Microsoft.KeyVault/deletedVaults/read|
|Kurtar|Silinen bir anahtar kasası geri yükler.|Microsoft.KeyVault/vaults/write|
|Temizle|Kalıcı olarak silinmiş bir anahtar kasası ve tüm içeriğini kaldırır.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

İzinler ve erişim denetimi hakkında daha fazla bilgi için bkz: [anahtar kasanızı güvenli](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Etkinleştirme soft-Sil

Silinen bir anahtar kasası veya anahtar kasasında depolanan nesneler kurtarmanız mümkün olması için önce bu anahtar kasası için geçici silme etkinleştirmeniz gerekir.

### <a name="existing-key-vault"></a>Varolan anahtar kasası

ContosoVault adlı bir var olan anahtar kasasının için geçici silme gibi etkinleştirin. 

>[!NOTE]
>Doğrudan yazmak için Azure Resource Manager kaynak işleme kullanmanıza gerek şu anda *enableSoftDelete* anahtar kasası kaynak özelliğine.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Yeni anahtar kasası

Yeni bir anahtar kasası için geçici silmeyi etkinleştirme oluşturma zamanında soft-delete etkinleştirme bayrağını ekleyerek yapılır, bir komut oluşturun.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Soft-delete etkinleştirme doğrulayın

Bir anahtar kasası soft-delete etkin olduğunu doğrulamak için çalıştırın *almak* komut ve 'yumuşak silmek için etkin?' arayın özniteliği ve onun ayarı true veya false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Soft-delete tarafından korunan bir anahtar kasasını silme

Bir anahtar kasasını silme (veya kaldırma için) komutu aynı kalır, ancak davranışını olup soft-delete veya etkinleştirdiğiniz bağlı olarak değişir.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Soft-delete etkin olmayan bir anahtar kasası için önceki komutu çalıştırırsanız, bu anahtar kasası ve tüm içeriğini kurtarma seçenekleri olmadan kalıcı olarak siler.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Soft-delete anahtar kasalarınıza nasıl korur

Soft-delete ile etkin:

- Bir anahtar kasası silindiğinde, kendi kaynak grubundan kaldırılmış olduğundan ve yalnızca ayrılmış bir ad alanında yerleştirildiğinden onu oluşturulduğu konumu ile ilişkili. 
- Silinen anahtar nesneleri gibi anahtarları, gizli ve sertifikalar erişilemeyen kasa ve bunların içeren anahtar kasası silinmiş durumda olsa da bunu kalır. 
- Silinmiş bir durumda bir anahtar kasası için DNS adını hala aynı ada sahip yeni bir anahtar kasası oluşturulamıyor için ayrılmıştır.  

Aşağıdaki komutu kullanarak, aboneliğinizle ilişkili silinen durumu anahtar kasalarını görüntüleyebilirsiniz:

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

*Kaynak kimliği* çıktıda bu kasaya özgün kaynak Kimliğine başvuruyor. Bu anahtar kasasında şimdi silinmiş durumda olduğundan, bu kaynak kimliği ile hiçbir kaynak yok *Kimliği* alan, Kurtarma ya da temizleme kaynağı tanımlamak için kullanılabilir. *Temizleme tarih zamanlanmış* alan gösterir kasanın ne zaman kalıcı olarak silinecek (temizlendi) bu silinen kasa için bir eylem varsa. Hesaplamak için kullanılan varsayılan saklama dönemi *temizleme tarih zamanlanmış*, 90 gündür.

## <a name="recovering-a-key-vault"></a>Bir anahtar kasası kurtarma

Bir anahtar kasası kurtarmak için anahtar kasası adı, kaynak grubunu ve konumu belirtmeniz gerekir. Bu bir anahtar kasası kurtarma işlemi için gereken konum ve silinen anahtar kasasının kaynak grubu unutmayın.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Bir anahtar kasası kurtarıldığında, yeni bir kaynak anahtar kasasının özgün kaynak kimliğiyle oluşur Burada anahtar kasası varolan kaynak grubu kaldırdıysanız anahtar kasası kurtarılabilir önce aynı ada sahip yeni bir kaynak grubu oluşturulmalıdır.

## <a name="key-vault-objects-and-soft-delete"></a>Anahtar kasası nesneleri ve soft-Sil

İçin bir anahtar, 'ContosoFirstKey' yumuşak Sil ' ContosoVault etkin' adlı bir anahtar kasasının içinde burada nasıl, bu anahtarı silmeniz.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

İçin geçici silme etkin anahtar kasanız ile silinen anahtar görünmeye devam eder dışında silinmiş gibi açıkça listelemek veya silinen anahtarlarını alma. Silinmiş durumda bir anahtar üzerindeki çoğu işlemi silinen anahtar listeleme, bu kurtarma veya onu temizleme dışında başarısız olur. 

Örneğin, bir anahtar kasası silinmiş listesi anahtarlarında istemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Durum geçişi 

Bir anahtar kasasına bir anahtar soft-etkin delete ile sildiğinizde, geçişi tamamlamak için birkaç saniye sürebilir. Bu geçiş aşamasında, anahtarı etkin veya silinmiş durumda değil görünebilir. Bu komut tüm silinen anahtarlarında 'ContosoVault' adlı anahtar kasanızı listeler.

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

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar kasası nesneleriyle Soft-delete kullanma

Kurtarma ya da onu temizleyebilirsiniz sürece yalnızca anahtar kasalarını, silinen bir anahtar gibi gizli veya, sertifika silinmiş durumda 90 gün boyunca kalacaktır. 

#### <a name="keys"></a>Anahtarlar

Silinen bir anahtarı kurtarmak için:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Bir anahtar kalıcı olarak silmek için:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Bir anahtar temizleme kalıcı olarak, kurtarılabilir olmaz anlamı silinir.

**Kurtarmak** ve **Temizleme** Eylemler bir anahtar kasası erişim ilkesinde ilişkili kendi izinlere sahiptir. Bir kullanıcı veya hizmet sorumlusu yürütmek bir **kurtarmak** veya **Temizleme** gerekir iznine sahip oldukları ilgili (anahtar veya gizli) Bu nesne için anahtar kasası erişim ilkesinde eylem. Varsayılan olarak, **Temizleme** izni 'all' kısayol bir kullanıcı için tüm izinleri vermek için kullanıldığında, bir anahtar kasasının erişim ilkesine eklenmez. Açıkça vermelidir **Temizleme** izni. Örneğin, aşağıdaki verir komut user@contoso.com anahtarlarında çeşitli işlemleri gerçekleştirmek için izni *ContosoVault* dahil olmak üzere **Temizleme**.

#### <a name="set-a-key-vault-access-policy"></a>Bir anahtar kasası erişim ilkesi ayarlama

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Yalnızca etkin soft-delete oluşmuş olan bir anahtar kasası varsa, olmayabilir **kurtarmak** ve **Temizleme** izinleri.

#### <a name="secrets"></a>Gizli Diziler

Anahtarları gibi bir anahtar kasasına gizli anahtarları üzerinde kendi komutları ile çalıştırılan örnekleridir. Aşağıdaki, silme, listeleme, kurtarma ve gizli anahtarları Temizleme için komutlardır.

- SQLPassword adlı bir gizli anahtarı silin: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Tüm silinen gizli bir anahtar kasasına listesi: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Bir gizli anahtar silinmiş durumda kurtarma: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Bir gizli anahtar silinmiş durumda temizleme: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Gizli temizleme kalıcı olarak, kurtarılabilir olmaz anlamı silinir.

## <a name="purging-and-key-vaults"></a>Temizleme ve anahtar kasalarını

### <a name="key-vault-objects"></a>Anahtar kasası nesneleri

Bir anahtar temizleme, gizli veya, sertifika kalıcı olarak, kurtarılabilir olmaz anlamına silinir. Anahtar Kasası'nda tüm diğer nesnelerin olarak Silinmiş nesne bulunan anahtar kasası ancak değişmeden kalır. 

### <a name="key-vaults-as-containers"></a>Anahtar kapsayıcıları kasaları
Bir anahtar kasası temizlenir, tüm içeriğini anahtarları, gizli ve sertifikalar dahil olmak üzere kalıcı olarak silinir. Bir anahtar kasası temizlenecek kullanmak `Remove-AzureRmKeyVault` seçeneği ile komut `-InRemovedState` ve silinen anahtar kasası ile konumunu belirterek `-Location location` bağımsız değişkeni. Komutunu kullanarak silinmiş bir kasa konumunu bulabilirsiniz `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Bir anahtar kasası temizleme kalıcı olarak, kurtarılabilir olmaz anlamı silinir.

### <a name="purge-permissions-required"></a>Gerekli izinler Temizle
- Kasa ve tüm içeriğini kalıcı olarak kaldırılır, silinen bir anahtar kasası temizlemek için kullanıcının RBAC gerçekleştirme izni gerekiyor bir *Microsoft.KeyVault/locations/deletedVaults/purge/action* işlemi. 
- Kasa silinen anahtar listelemek için RBAC gerçekleştirme izni kullanıcının ihtiyacı *Microsoft.KeyVault/deletedVaults/read* izni. 
- Varsayılan olarak yalnızca bir abonelik yöneticisinin bu izinlere sahiptir. 

### <a name="scheduled-purge"></a>Zamanlanmış temizleme

Silinen anahtar kasası nesneleri listeleme schedled anahtar kasası tarafından temizlenecek olduklarında gösterir. *Temizleme tarih zamanlanmış* alan gösterir ne zaman bir anahtar kasası nesnesi kalıcı olarak silinecek, hiçbir işlem yapılmadı. Varsayılan olarak, silinen anahtar kasası nesne saklama süresi 90 gündür.

>[!NOTE]
>Tarafından tetiklenen silinen kasası nesne, kendi *temizleme tarih zamanlanmış* alan, kalıcı olarak silinir. Kurtarılabilir değil.

## <a name="other-resources"></a>Diğer kaynaklar

- Anahtar Kasası'nın soft-delete özelliğine genel bakış için bkz: [Azure anahtar kasası soft-delete genel bakış](key-vault-ovw-soft-delete.md).
- Azure anahtar kasası kullanımı genel bir bakış için bkz: [Azure anahtar kasası ile çalışmaya başlama](key-vault-get-started.md).

