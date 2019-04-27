---
title: Azure Key Vault - CLI ile geçici silmeyi kullanma
description: Geçici silmeyi CLI kod parçalarını ile büyük/küçük harf örnekleri kullan
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: mbaldwin
ms.openlocfilehash: aa9b89b9afec069e97236b7652e0f1d37644f5cf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640489"
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>Key Vault geçici silmeyi CLI ile kullanma

Azure Key Vault'un geçici silme özelliği silinen kasa ve kasa nesneleri kurtarılmasını sağlar. Özellikle, geçici adresleri aşağıdaki senaryolarda silme:

- Bir anahtar kasası kurtarılabilir silinmesi için destek
- Anahtar kasası nesne kurtarılabilir silme desteği; , gizli dizileri, anahtarlar ve sertifikalar

## <a name="prerequisites"></a>Önkoşullar

- Bu kurulum, ortamınız için bkz: yoksa, azure CLI - [yönetme Azure CLI ile anahtar kasası](key-vault-manage-with-cli2.md).

CLI için Key Vault özel referans bilgileri için bkz. [Azure CLI Key Vault başvurusunu](https://docs.microsoft.com/cli/azure/keyvault).

## <a name="required-permissions"></a>Gerekli izinler

Anahtar kasası işlemleri ayrı olarak şu şekilde rol tabanlı erişim denetimi (RBAC) izinleri yönetilir:

| İşlem | Açıklama | Kullanıcı izni |
|:--|:--|:--|
|Liste|Listeleri anahtar kasalarını silindi.|Microsoft.KeyVault/deletedVaults/read|
|Kurtar|Silinen bir anahtar kasasına yükler.|Microsoft.KeyVault/vaults/write|
|Temizle|Silinen bir anahtar kasasını ve tüm içerikleri kalıcı olarak kaldırır.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

İzinler ve erişim denetimi hakkında daha fazla bilgi için bkz. [anahtar kasanızın güvenliğini sağlama](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Geçici silmeyi etkinleştirme

"Geçici silinen anahtar kasası veya bir anahtar Kasası'nda depolanan nesnelere kurtarılabilmesi silme" sağlar.

> [!IMPORTANT]
> Bir anahtar kasasında 'geçici Sil' etkinleştirme işlemi geri alınamaz bir eylemdir. Geçici silme özelliği ayarlandıktan sonra "true", bu değiştirilemez veya kaldırıldı.  

### <a name="existing-key-vault"></a>Var olan bir anahtar kasası

Varolan anahtar kasasında ContosoVault adlı bir geçici silme aşağıdaki gibi etkinleştirin. 

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a>Yeni anahtar kasası

Yeni key vault geçici silmeyi etkinleştirme oluşturma sırasında geçici silmeyi etkinleştir bayrak ekleyerek gerçekleştirilir, create komutu.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Geçici silmeyi etkinleştirme doğrulayın

Key vault geçici silmeyi etkin olduğunu doğrulamak için çalıştırın *Göster* komut ve 'geçici Sil için etkin mi?' arayın Öznitelik:

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Anahtar kasası korumalı geçici silme siliniyor

Geçici silme etkinleştirilip etkinleştirilmediği bağlı olarak davranış, bir anahtar kasası değişiklikleri Sil komutu.

> [!IMPORTANT]
>Geçici silme etkin olmayan bir anahtar kasası için aşağıdaki komutu çalıştırırsanız, bu anahtar kasası ve kurtarma için hiçbir seçenek olmadan tüm içeriği kalıcı olarak silinecek!

```azurecli
az keyvault delete --name ContosoVault
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Geçici silme anahtar kasalarınıza nasıl korur

Geçici silme ile etkin:

- Silinen bir anahtar kasası kaynak grubundan kaldırıldı ve oluşturulduğu yere konumla ilişkili ayrılmış bir ad alanı yerleştirilir. 
- Silinen nesneleri gibi kendi içeren anahtar kasası silinmiş durumda olduğu sürece anahtarları, gizli diziler ve sertifikalar, erişilemez. 
- Silinen bir anahtar kasası için DNS adı ayrılmış, aynı ada sahip yeni bir anahtar kasası oluşturulmasını engelliyor.  

Aşağıdaki komutu kullanarak, aboneliğinizle ilişkili silindi durumu anahtar kasalarını görüntüleyebilirsiniz:

```azurecli
az keyvault list-deleted
```
- *Kimliği* kurtarma veya temizleme kaynağı tanımlamak için kullanılabilir. 
- *Kaynak Kimliği* özgün bu kasası kaynak kimliğidir. Bu anahtar kasası artık silinmiş bir durumda olduğundan, bu kaynak kimliği ile kaynak var. 
- *Zamanlanmış temizleme tarihi* kasa kalıcı olarak silinecek, hiçbir işlem yapılmadı ise. Hesaplamak için kullanılan varsayılan saklama süresi *temizleme tarihine zamanlanmış*, 90 gündür.

## <a name="recovering-a-key-vault"></a>Bir anahtar kasası kurtarma

Bir anahtar kasası kurtarmak için anahtar kasası adı, kaynak grubunu ve konumu belirtin. Kurtarma işlemi için gereken konum ve kaynak grubu silindi anahtar kasasının unutmayın.

```azurecli
az keyvault recover --location westus --resource-group ContosoRG --name ContosoVault
```

Bir anahtar kasası kurtarıldığında, anahtar kasasının özgün kaynak kimliği ile yeni bir kaynak oluşturulur Özgün kaynak grubunu kaldırılırsa, aynı ada sahip kurtarma denemeden önce oluşturulmalıdır.

## <a name="deleting-and-purging-key-vault-objects"></a>Anahtar kasası nesne temizleme ve silme

Aşağıdaki komut 'ContosoFirstKey' anahtarında geçici silme etkin olan'ContosoVault ' adlı bir anahtar kasası siler:

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Geçici silme için etkin anahtar kasanız ile silinen bir anahtar gösterilmeye devam eder dışında silindi gibi açıkça listesinde veya silinen anahtarlarını alma. Bir anahtarı silindi durumunda ilgili işlemlerin çoğu silinen bir anahtar listesi, onu kurtarma veya onu temizleme dışında başarısız olur. 

Örneğin, liste silindi anahtarları key vault'ta istemek için aşağıdaki komutu kullanın:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Geçiş durumu 

Bir anahtarı bir anahtar Kasası'nda etkin ile geçici silme sildiğinizde, bu geçişin tamamlanması birkaç saniye sürebilir. Bu geçiş sırasında anahtar etkin durumunda veya silinmiş durumda olmadığını görünebilir. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar kasası nesneleri ile geçici silmeyi kullanma

Kurtarma veya onu temizlemek sürece yalnızca anahtar kasalarını gibi silinmiş durumda silinen anahtar, parola veya sertifika, en fazla 90 gün boyunca kalır.

#### <a name="keys"></a>Anahtarlar

Geçici silinen anahtar kurtarmak için:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

(Temizleme olarak da bilinir) kalıcı olarak silmek için bir geçici silinen anahtar:

> [!IMPORTANT]
> Bir anahtar temizleme kalıcı olarak silinir ve kurtarılabilir olmayacak! 

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

**Kurtarmak** ve **Temizleme** eylemleri bir anahtar kasası erişim ilkesini ilişkili kendi izinlere sahiptir. Bir kullanıcı veya hizmet sorumlusu yürütmek bir **kurtarmak** veya **Temizleme** eylemi, bu anahtarı veya gizli anahtarı için ilgili izninin olması gerekir. Varsayılan olarak, **Temizleme** bir anahtar kasasının erişim ilkesi için tüm izinleri vermek için 'Tümü' kısayol kullanıldığında eklenmez. Özellikle vermelisiniz **Temizleme** izni. 

#### <a name="set-a-key-vault-access-policy"></a>Bir anahtar kasası erişim ilkesini ayarlama

Aşağıdaki komut verir user@contoso.com anahtarlarında çeşitli işlemleri kullanma izni *ContosoVault* dahil olmak üzere **Temizleme**:

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Geçici silme etkinleştirilebilir yalnızca olan mevcut bir anahtar kasası varsa olmayabilir **kurtarmak** ve **Temizleme** izinleri.

#### <a name="secrets"></a>Gizli Diziler

Anahtarları gibi gizli dizileri kendi komutları ile yönetilir:

- SQLPassword adlı bir gizli anahtarı silin: 
  ```azurecli
  az keyvault secret delete --vault-name ContosoVault -name SQLPassword
  ```

- Bir anahtar kasasındaki tüm silinen gizli dizileri listeleme: 
  ```azurecli
  az keyvault secret list-deleted --vault-name ContosoVault
  ```

- Gizli dizi silindi durumunda kurtarma: 
  ```azurecli
  az keyvault secret recover --name SQLPassword --vault-name ContosoVault
  ```

- Gizli dizi silinmiş durumda temizleme: 

  > [!IMPORTANT]
  > Gizli dizi temizleme kalıcı olarak silinir ve kurtarılabilir olmayacak! 

  ```azurecli
  az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Anahtar kasası korumalı geçici silme temizleme

> [!IMPORTANT]
> Bir anahtar kasası veya bağımlı nesnelerinin birinin temizleme, yani kurtarılabilir olmayacaktır kalıcı olarak silinir!

Temizleme işlevi, bir anahtar kasası nesne ya da daha önce geçici olarak silinen tüm bir key vault, kalıcı olarak silmek için kullanılır. Önceki bölümde gösterildiği gibi geçici silme özelliği etkin, anahtar kasasında depolanan nesnelere birden çok durumlarında gidebilir:

- **Etkin**: silmeden önce.
- **Geçici olarak silinen**: hesabınız silindikten sonra listelenir ve kurtarılan etkin duruma geri mümkün.
- **Kalıcı olarak silinmiş**: sonra temizleme kurtarılması mümkün değildir.

Aynı anahtar kasası için geçerlidir. Geçici silinen anahtar kasasını ve içeriğini kalıcı olarak silmek için anahtar kasası temizlemelidir.

### <a name="purging-a-key-vault"></a>Bir anahtar kasası temizleme

Bir anahtar kasası temizlenir, tüm içeriği kalıcı olarak, anahtarlara, parolalara ve sertifikalara dahil olmak üzere silinir. Geçici silinen anahtar kasasını Temizle kullanın `az keyvault purge` komutu. Aboneliğinizin silinen anahtar kasalarının komutunu kullanarak konumu bulabilirsiniz `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
```

### <a name="purge-permissions-required"></a>Gerekli izinler Temizle
- Silinen bir anahtar kasasını temizlemek için kullanıcının RBAC izni gereken *Microsoft.KeyVault/locations/deletedVaults/purge/action* işlemi. 
- Silinen bir anahtar kasasını listelemek için RBAC izni kullanıcının gereken *Microsoft.KeyVault/deletedVaults/read* işlemi. 
- Varsayılan olarak yalnızca bir abonelik yöneticisinin bu izinlere sahiptir. 

### <a name="scheduled-purge"></a>Zamanlanmış bir temizleme

Silinen anahtar kasasını nesnelerin listesini, ayrıca, bunlar Key Vault tarafından temizlenmesi zamanlanmış gösterir. *Zamanlanmış temizleme tarihi* ne zaman bir anahtar kasası nesne kalıcı olarak silinecek, hiçbir işlem yapılmadı, belirtir. Varsayılan olarak, silinen anahtar kasasını nesnesi için saklama süresi 90 gündür.

>[!IMPORTANT]
>Tarafından tetiklenen bir Temizlenen kasasını kendi *temizleme tarihine zamanlanmış* alan, kalıcı olarak silinir. Kurtarılabilir değil!

## <a name="enabling-purge-protection"></a>Temizleme korumasını etkinleştirme

Koruma yapılandırması bir kasa veya bir nesne silinmiş açık olduğunda 90 gün saklama süresi bitene kadar durumu temizlenemiyor. Böyle bir kasa veya nesne hala kurtarılabilir. Bu özellik bir kasa veya bir nesne hiçbir zaman kalıcı olarak olabilecek ek güvence verir bekletme süresi geçene kadar silindi.

Yalnızca geçici silmeyi de etkinse koruma yapılandırması etkinleştirebilirsiniz. 

Her iki geçici silme üzerinde açın ve kasa oluştururken koruma temizlemek için kullanın [az keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutu:

```
az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
```

(Bu, geçici silme etkinleştirilebilir zaten) var olan bir kasa için temizleme korumasını eklemek için kullanmak [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) komutu:

```
az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
```

## <a name="other-resources"></a>Diğer kaynaklar

- Key Vault geçici silme özelliği genel bakış için bkz. [Azure Key Vault geçici silmeyi genel bakış](key-vault-ovw-soft-delete.md).
- Azure Key Vault kullanımı genel bir bakış için bkz: [Azure anahtar kasası nedir?](key-vault-overview.md).

