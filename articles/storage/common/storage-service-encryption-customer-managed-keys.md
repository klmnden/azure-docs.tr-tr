---
title: Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak Azure depolama hizmeti şifrelemesi | Microsoft Docs
description: Müşteri tarafından yönetilen anahtarlar kullanılarak veri alınırken şifresini çözmek ve Azure Blob Depolama ve Azure dosyaları hizmet tarafında, verileri depolarken şifrelemek için Azure depolama hizmeti şifrelemesi özelliği kullanın.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 10/11/2018
ms.author: lakasa
ms.subservice: common
ms.openlocfilehash: 2718c5f06bb64ccd99844e402ac69237f30c310a
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497800"
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi

Microsoft Azure Kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi koruyarak yardımcı olmayı taahhüt etmektedir. Azure depolama platformu verilerinizi koruyan bir depolama hizmeti şifrelemesi (depolama alanına yazarken, verileri şifreler ve almadan olduğunda, verilerin şifresini çözer SSE aracılığıyla), yoludur. Şifreleme ve şifre çözme otomatik ve şeffaf ve 256 bit kullanır [AES şifreleme](https://wikipedia.org/wiki/Advanced_Encryption_Standard), aşağıdakilerden birini en güçlü blok şifreleme özelliklerinden kullanılabilir.

SSE ile Microsoft tarafından yönetilen bir şifreleme anahtarlarını kullanabilir veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Bu makale, kendi şifreleme anahtarlarınızı kullanmayı açıklar. Daha fazla SSE veya Microsoft tarafından yönetilen anahtarları kullanma hakkında genel bilgi için [bekleyen veriler için depolama hizmeti şifrelemesi](storage-service-encryption.md).

Böylece bir anahtar kasası, şifreleme anahtarlarınızı yönetmek için kullanabileceğiniz Azure Blob Depolama için SSE ve Azure dosyaları Azure anahtar kasası ile tümleştirilmiştir. Kendi şifreleme anahtarları oluşturmak ve bunları bir anahtar kasasında depolama veya Azure anahtar Kasası'nın API'leri, şifreleme anahtarları oluşturmak için kullanabilirsiniz. Azure Key Vault, yönetmek ve anahtarlarınızı denetleyebilir ve aynı zamanda, anahtar kullanımını denetleme.

> [!Note]  
> Depolama hizmeti müşteri tarafından yönetilen anahtarları kullanarak şifreleme kullanılabilir değil [Azure yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md). [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption-overview.md) endüstri standardı kullanan [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) Windows üzerinde ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) şifrelemesi çözümü sağlamak için Linux üzerinde tümleşik KeyVault ile.

Neden kendi anahtarlarınızı oluşturulsun mu? Oluşturma, döndürme, devre dışı bırakın ve erişim denetimleri tanımlamak için özel anahtarlar, daha fazla esneklik sağlar. Özel anahtarlar, verilerinizi korumak için kullanılan şifreleme anahtarlarını denetim sağlar.

## <a name="get-started-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar ile çalışmaya başlama

Müşteri tarafından yönetilen anahtarlar SSE ile kullanmak için anahtar veya bir var olan bir key vault ile anahtar kullanabilirsiniz ve ya da yeni bir anahtar kasası oluşturabilirsiniz. Depolama hesabı ve anahtar kasasının aynı bölgede olması gerekir, ancak bunlar farklı Aboneliklerde olabilir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="step-1-create-a-storage-account"></a>1. Adım: Depolama hesabı oluşturma

İlk olarak, zaten yoksa, bir depolama hesabı oluşturun. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>2. Adım: Blob ve dosya depolama için SSE etkinleştir

Müşteri tarafından yönetilen anahtarlar kullanılarak SSE etkinleştirmek için yazılım silin ve yapmak değil temizlemek, iki anahtar koruma özelliklerini Azure anahtar Kasası'nda da etkinleştirilmesi gerekir. Bu ayarlar, yanlışlıkla veya kasıtlı olarak anahtarlar silinemiyor emin olun. Anahtarların en uzun saklama süresi 90 gün, kullanıcıya kötü amaçlı aktörler ve fidye yazılımı saldırılarına karşı korumak için ayarlanır.

Program aracılığıyla müşteri tarafından yönetilen anahtarlar için SSE etkinleştirmek istiyorsanız, kullanabileceğiniz [Azure depolama kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/storagerp), [.NET için depolama kaynak sağlayıcısı istemci Kitaplığı](https://docs.microsoft.com/dotnet/api), [ Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), veya [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Müşteri tarafından yönetilen anahtarlar SSE ile kullanmak için depolama hesabı için bir depolama hesabı kimliği atamanız gerekir. Uygulamanın kimliğini aşağıdaki PowerShell veya Azure CLI komutunu yürüterek ayarlayabilir:

```powershell
Set-AzStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

```azurecli-interactive
az storage account \
    --account-name <account_name> \
    --resource-group <resource_group> \
    --assign-identity
```

Aşağıdaki PowerShell veya Azure CLI komutları yürüterek geçici silme ve temizleme değil yapmak etkinleştirebilirsiniz:

```powershell
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

```azurecli-interactive
az resource update \
    --id $(az keyvault show --name <vault_name> -o tsv | awk '{print $1}') \
    --set properties.enableSoftDelete=true

az resource update \
    --id $(az keyvault show --name <vault_name> -o tsv | awk '{print $1}') \
    --set properties.enablePurgeProtection=true
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>3. Adım: Müşteri tarafından yönetilen anahtarlarla şifrelemeyi etkinleştir

Varsayılan olarak, Microsoft tarafından yönetilen anahtarlar SSE kullanır. Müşteri tarafından yönetilen anahtarları kullanarak depolama hesabı için SSE etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com/). Üzerinde **ayarları** depolama hesabı dikey penceresine tıklayın **şifreleme**. Seçin **kendi anahtarınızı kullanın** seçeneği, aşağıdaki resimde gösterildiği gibi.

![Şifreleme seçeneği Portal gösteren ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>4. Adım: Anahtarınızı seçin

Anahtarınızı bir URI olarak veya bir anahtar kasasından anahtar seçerek belirtebilirsiniz.

#### <a name="specify-a-key-as-a-uri"></a>Bir anahtar olarak bir URI belirtin.

Anahtarınızı bir uri'den belirtmek için bu adımları izleyin:

1. Seçin **Enter tuşunu URI** seçeneği.
2. İçinde **anahtar URI'si** alan, URI belirtin.

   ![Anahtar URI'si seçeneği girin şifrelemeyle gösteren portalı ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

#### <a name="specify-a-key-from-a-key-vault"></a>Bir anahtar, anahtar kasası belirtin

Anahtar Kasası'ndaki anahtarınız belirtmek için bu adımları izleyin:

1. Seçin **Key Vault'tan seçin** seçeneği.
2. Kullanmak istediğiniz anahtarı içeren anahtar Kasası'nı seçin.
3. Anahtar, anahtar kasasından seçin.

   ![Portal gösteren ekran görüntüsü şifrelemeler kendi anahtar seçeneğini kullanın.](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Depolama hesabı anahtar kasasına erişim yoksa, erişim vermek için aşağıdaki görüntüde gösterildiği Azure PowerShell komutunu çalıştırabilirsiniz.

![Key vault için erişim gösteren portalı ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Ayrıca, Azure portalında Azure Key vault'a giderek ve depolama hesabı için erişim verme Azure portalından erişim izni verebilirsiniz.

Aşağıdaki PowerShell komutlarını kullanarak mevcut bir depolama hesabıyla yukarıdaki anahtar ilişkilendirebilirsiniz:

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```

### <a name="step-5-copy-data-to-storage-account"></a>5. Adım: Depolama hesabına veri kopyalama

Şifrelenir, böylece yeni depolama hesabınızı veri aktarmak için. Daha fazla bilgi için [depolama hizmeti şifrelemesi hakkında SSS](storage-service-encryption.md#faq-for-storage-service-encryption).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>6. Adım: Şifrelenmiş verilerin durumu

Şifrelenmiş veriler durumunu sorgulayın.

## <a name="faq-for-sse-with-customer-managed-keys"></a>SSE müşteri yönetilen anahtarları hakkında SSS

**Premium depolama kullanıyorum; Müşteri tarafından yönetilen anahtarlar SSE ile kullanabilir miyim?**  
Evet, Microsoft tarafından yönetilen ve müşteri tarafından yönetilen anahtarlarla SSE hem standart depolama hem de Premium depolama desteklenir.

**Azure PowerShell ve Azure CLI kullanılarak müşteri tarafından yönetilen anahtarlarla yeni depolama hesapları ile SSE oluşturabilirim?**  
Evet.

**Müşteri tarafından yönetilen anahtarlar SSE ile kullanırsam ne kadar Azure depolama maliyeti?**  
Azure Key Vault'u kullanarak için ilişkilendirilmiş bir maliyet yoktur. Daha fazla bilgi için ziyaret [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/). Tüm depolama hesapları için etkinleştirilmiş olan SSE için ek ücret yoktur.

**Depolama hizmeti şifrelemesi, Azure yönetilen diskler üzerinde kullanılabilir mi?**  
Depolama hizmeti şifrelemesi, Microsoft tarafından yönetilen anahtarlarla Azure yönetilen diskler için kullanılabilir, ancak değil müşteri tarafından yönetilen anahtarlar. Müşteri tarafından yönetilen anahtarlarla SSE destekleme diskleri yönetilen yerine öneririz [Azure Disk şifrelemesi](../../security/azure-security-disk-encryption-overview.md), endüstri standardı kullanan [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) Windows üzerinde ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)sağlamak için Linux üzerinde şifreleme KeyVault ile tümleşiktir.

**Depolama hizmeti şifrelemesi Azure Disk Şifrelemesi ' farklı mı?**  
Azure Disk şifrelemesi, BitLocker ve DM-Crypt gibi işletim sistemi tabanlı çözümler ve Azure anahtar kasası arasında tümleştirme sağlar. Depolama hizmeti şifrelemesi, yerel olarak katmanında Azure depolama platformu, sanal makine aşağıdaki şifreleme sağlar.

**Şifreleme anahtarlarına erişimi iptal etme?**
Evet, erişimi istediğiniz zaman iptal edebilirsiniz. Anahtarlarınızı erişimini iptal etmek için birkaç yolu vardır. Başvurmak [Azure anahtar kasası PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault/) ve [Azure anahtar kasası CLI](https://docs.microsoft.com/cli/azure/keyvault) daha fazla ayrıntı için. Erişimi iptal ediliyor hesap şifreleme anahtarı Azure Depolama tarafından erişilemez olduğundan etkili bir şekilde depolama hesabındaki tüm bloblar erişimini engeller.

**Farklı bir bölgede bir depolama hesabı ve anahtarı oluşturabilir?**  
Hayır, depolama hesabı ve Azure anahtar kasası ve anahtar da aynı bölgede olması gerekir.

**Müşteri tarafından yönetilen anahtarlar için SSE depolama hesabı oluşturulurken bir hata etkinleştirebilirim?**  
Hayır. İlk Depolama hesabı oluşturduğunuzda, yalnızca Microsoft tarafından yönetilen anahtarlar için SSE kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanmak için depolama hesabı özellikleri güncelleştirmeniz gerekir. Program aracılığıyla depolama hesabınızı güncelleştirmek için REST veya depolama istemci kitaplıklarından birini kullanın veya hesabı oluşturduktan sonra Azure portalı kullanarak depolama hesabı özelliklerini güncelleştirir.

**SSE ile müşteri tarafından yönetilen anahtarları kullanırken şifreleme devre dışı bırakabilirim?**  
Hayır, şifreleme devre dışı bırakılamıyor. Şifreleme, Azure Blob Depolama, Azure dosyaları, Azure kuyruk ve Azure tablo depolama için varsayılan olarak etkindir. Müşteri tarafından yönetilen anahtarlar kullanmaya Microsoft tarafından yönetilen anahtarlar kullanarak isteğe bağlı olarak geçiş yapabilir ve bunun tersi de geçerlidir.

**Yeni bir depolama hesabı oluşturduğumda SSE etkin mi?**  
SSE, Azure Blob Depolama, Azure dosyaları, Azure kuyruk depolama ve Azure tablo depolama ve tüm depolama hesapları için etkinleştirilir.

**Ben SSE storage hesabımdaki müşteri tarafından yönetilen anahtarlar kullanılarak etkinleştirilemiyor.**  
Bu, bir Azure Resource Manager depolama hesabı mi? Klasik depolama hesapları, müşteri tarafından yönetilen anahtarlarla desteklenmez. Müşteri tarafından yönetilen anahtarlarla SSE yalnızca Resource Manager depolama hesaplarında etkinleştirilebilir.

**Geçici silme ve temizleme değil yapmak nedir? Müşteri tarafından yönetilen anahtarlarla SSE kullanmak üzere bu ayarı etkinleştirmeniz gerekiyor mu?**  
Geçici silme ve temizleme yapmak değil müşteri tarafından yönetilen anahtarlarla SSE kullanmak için etkinleştirilmelidir. Bu ayarlar, anahtarınızı yanlışlıkla veya kasıtlı olarak silinmediğinden emin olun. Anahtarların en uzun saklama süresi 90 gün, kullanıcıların kötü amaçlı aktörler ve fidye yazılımı saldırılarına karşı korumak için ayarlanır. Bu ayar devre dışı bırakılamaz.

**Müşteri tarafından yönetilen anahtarlarla SSE, yalnızca belirli bölgelerde izin verilir?**  
Müşteri tarafından yönetilen anahtarlarla SSE, Azure Blob Depolama ve Azure dosyaları için tüm bölgelerde kullanılabilir.

**Herhangi bir sorununuz veya geri bildirim sağlamak isterseniz nasıl birisi başvurmam gerekir?**  
İlgili kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) için depolama hizmeti şifrelemesi ile ilgili sorunlar için.

## <a name="next-steps"></a>Sonraki adımlar

- Kapsamlı güvenlik hakkında daha fazla bilgi için geliştiriciler yardımcı özelliklerini güvenli uygulamalar oluşturun, bkz: [depolama Güvenlik Kılavuzu](storage-security-guide.md).
- Azure Key Vault hakkında genel bilgi için bkz. [Azure anahtar kasası nedir](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
- Azure Key Vault'a başlamak için bkz: [Azure anahtar kasası ile çalışmaya başlama](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
