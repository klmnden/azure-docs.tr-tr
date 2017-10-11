---
ms.assetid: 
title: "Azure anahtar kasası - geçici silme CLI ile kullanma"
description: "Kullanım örneği soft-delete CLI kod parçalarını ile örnekleri"
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 3ee2c5dfb99d734cde25894174466b8e49823c67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>Anahtar kasası soft-delete CLI ile kullanma

Azure anahtar Kasası'nın geçici silme özelliği silinen kasalarını ve kasa nesneleri kurtarılmasını sağlar. Özellikle, soft-adresleri aşağıdaki senaryolarda Sil:

- Bir anahtar kasası kurtarılabilir silinmesini desteği
- Anahtar kasası nesnelerin kurtarılabilir silinmesi desteği; , gizli anahtarları ve sertifikaları

## <a name="prerequisites"></a>Ön koşullar

- Azure CLI, ortamınız için bu kurulumu yoksa 2.0 - bkz [yönetmek anahtar CLI 2.0 kullanan kasası](key-vault-manage-with-cli2.md).

CLI için anahtar kasası belirli başvuru bilgileri için bkz: [Azure CLI 2.0 anahtar kasası başvuru](https://docs.microsoft.com/cli/azure/keyvault).

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

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a>Yeni anahtar kasası

Yeni bir anahtar kasası için geçici silmeyi etkinleştirme oluşturma zamanında soft-delete etkinleştirme bayrağını ekleyerek yapılır, bir komut oluşturun.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Soft-delete etkinleştirme doğrulayın

Bir anahtar kasası soft-delete etkin olduğunu doğrulamak için çalıştırın *Göster* komut ve 'yumuşak silmek için etkin?' arayın özniteliği ve onun ayarı true veya false.

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Soft-delete tarafından korunan bir anahtar kasasını silme

Bir anahtar kasasını silme (veya kaldırma için) komutu aynı kalır, ancak davranışını olup soft-delete veya etkinleştirdiğiniz bağlı olarak değişir.

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
>Soft-delete etkin olmayan bir anahtar kasası için önceki komutu çalıştırırsanız, bu anahtar kasası ve tüm içeriğini kurtarma seçenekleri olmadan kalıcı olarak siler.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Soft-delete anahtar kasalarınıza nasıl korur

Soft-delete ile etkin:

- Bir anahtar kasası silindiğinde, kendi kaynak grubundan kaldırılmış olduğundan ve yalnızca ayrılmış bir ad alanında yerleştirildiğinden onu oluşturulduğu konumu ile ilişkili. 
- Silinen anahtar nesneleri gibi anahtarları, gizli ve sertifikalar erişilemeyen kasa ve bunların içeren anahtar kasası silinmiş durumda olsa da bunu kalır. 
- Silinmiş bir durumda bir anahtar kasası için DNS adını hala aynı ada sahip yeni bir anahtar kasası oluşturulamıyor için ayrılmıştır.  

Aşağıdaki komutu kullanarak, aboneliğinizle ilişkili silinen durumu anahtar kasalarını görüntüleyebilirsiniz:

```azurecli
az keyvault list-deleted
```

*Kaynak kimliği* çıktıda bu kasaya özgün kaynak Kimliğine başvuruyor. Bu anahtar kasasında şimdi silinmiş durumda olduğundan, bu kaynak kimliği ile hiçbir kaynak yok *Kimliği* alan, Kurtarma ya da temizleme kaynağı tanımlamak için kullanılabilir. *Temizleme tarih zamanlanmış* alan gösterir kasanın ne zaman kalıcı olarak silinecek (temizlendi) bu silinen kasa için bir eylem varsa. Hesaplamak için kullanılan varsayılan saklama dönemi *temizleme tarih zamanlanmış*, 90 gündür.

## <a name="recovering-a-key-vault"></a>Bir anahtar kasası kurtarma

Bir anahtar kasası kurtarmak için anahtar kasası adı, kaynak grubunu ve konumu belirtmeniz gerekir. Bu bir anahtar kasası kurtarma işlemi için gereken konum ve silinen anahtar kasasının kaynak grubu unutmayın.

```azurecli
az keyvault recover --location westus --name ContosoVault
```

Bir anahtar kasası kurtarıldığında, yeni bir kaynak anahtar kasasının özgün kaynak kimliğiyle oluşur Burada anahtar kasası varolan kaynak grubu kaldırdıysanız anahtar kasası kurtarılabilir önce aynı ada sahip yeni bir kaynak grubu oluşturulmalıdır.

## <a name="key-vault-objects-and-soft-delete"></a>Anahtar kasası nesneleri ve soft-Sil

İçin bir anahtar, 'ContosoFirstKey' yumuşak Sil ' ContosoVault etkin' adlı bir anahtar kasasının içinde burada nasıl, bu anahtarı silmeniz.

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

İçin geçici silme etkin anahtar kasanız ile silinen anahtar görünmeye devam eder dışında silinmiş gibi açıkça listelemek veya silinen anahtarlarını alma. Silinmiş durumda bir anahtar üzerindeki çoğu işlemi silinen anahtar listeleme, bu kurtarma veya onu temizleme dışında başarısız olur. 

Örneğin, bir anahtar kasası silinmiş listesi anahtarlarında istemek için aşağıdaki komutu kullanın:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Durum geçişi 

Bir anahtar kasasına bir anahtar soft-etkin delete ile sildiğinizde, geçişi tamamlamak için birkaç saniye sürebilir. Bu geçiş aşamasında, anahtarı etkin veya silinmiş durumda değil görünebilir. Bu komut tüm silinen anahtarlarında 'ContosoVault' adlı anahtar kasanızı listeler.

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Anahtar kasası nesneleriyle Soft-delete kullanma

Kurtarma ya da onu temizleyebilirsiniz sürece yalnızca anahtar kasalarını, silinen bir anahtar gibi gizli veya, sertifika silinmiş durumda 90 gün boyunca kalacaktır. 

#### <a name="keys"></a>Anahtarlar

Silinen bir anahtarı kurtarmak için:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

Bir anahtar kalıcı olarak silmek için:

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
>Bir anahtar temizleme kalıcı olarak, kurtarılabilir olmaz anlamı silinir.

**Kurtarmak** ve **Temizleme** Eylemler bir anahtar kasası erişim ilkesinde ilişkili kendi izinlere sahiptir. Bir kullanıcı veya hizmet sorumlusu yürütmek bir **kurtarmak** veya **Temizleme** gerekir iznine sahip oldukları ilgili (anahtar veya gizli) Bu nesne için anahtar kasası erişim ilkesinde eylem. Varsayılan olarak, **Temizleme** izni 'all' kısayol bir kullanıcı için tüm izinleri vermek için kullanıldığında, bir anahtar kasasının erişim ilkesine eklenmez. Açıkça vermelidir **Temizleme** izni. Örneğin, aşağıdaki verir komut user@contoso.com anahtarlarında çeşitli işlemleri gerçekleştirmek için izni *ContosoVault* dahil olmak üzere **Temizleme**.

#### <a name="set-a-key-vault-access-policy"></a>Bir anahtar kasası erişim ilkesi ayarlama

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Yalnızca etkin soft-delete oluşmuş olan bir anahtar kasası varsa, olmayabilir **kurtarmak** ve **Temizleme** izinleri.

#### <a name="secrets"></a>Gizli Diziler

Anahtarları gibi bir anahtar kasasına gizli anahtarları üzerinde kendi komutları ile çalıştırılan örnekleridir. Aşağıdaki, silme, listeleme, kurtarma ve gizli anahtarları Temizleme için komutlardır.

- SQLPassword adlı bir gizli anahtarı silin: 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- Tüm silinen gizli bir anahtar kasasına listesi: 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- Bir gizli anahtar silinmiş durumda kurtarma: 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- Bir gizli anahtar silinmiş durumda temizleme: 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
>Gizli temizleme kalıcı olarak, kurtarılabilir olmaz anlamı silinir.

## <a name="purging-and-key-vaults"></a>Temizleme ve anahtar kasalarını

### <a name="key-vault-objects"></a>Anahtar kasası nesneleri

Bir anahtar temizleme, gizli veya, sertifika kalıcı olarak, kurtarılabilir olmaz anlamına silinir. Anahtar Kasası'nda tüm diğer nesnelerin olarak Silinmiş nesne bulunan anahtar kasası ancak değişmeden kalır. 

### <a name="key-vaults-as-containers"></a>Anahtar kapsayıcıları kasaları
Bir anahtar kasası temizlenir, tüm içeriğini anahtarları, gizli ve sertifikalar dahil olmak üzere kalıcı olarak silinir. Bir anahtar kasası temizlenecek kullanmak `az keyvault purge` komutu. Konum aboneliğiniz silinen anahtar kasalarını komutunu kullanarak bulabilir `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
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

