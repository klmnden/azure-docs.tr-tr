---
title: "Müşteri tarafından yönetilen anahtarları Azure anahtar kasası kullanarak Azure depolama hizmeti şifrelemesi | Microsoft Docs"
description: "Müşteri tarafından yönetilen anahtarlar kullanılarak veri alınırken şifresini çözmek ve Azure depolama hizmeti şifrelemesi özelliği hizmet tarafında Azure Blob Depolama veri depolarken şifrelemek için kullanın."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/07/2018
ms.author: lakasa
ms.openlocfilehash: 1360d8bb0911c424747209c69b830fc1ee461798
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Müşteri tarafından yönetilen anahtarları Azure anahtar kasası kullanarak depolama hizmeti şifrelemesi

Microsoft Azure korumak ve Kuruluş güvenliği ve uyumluluk taahhüt karşılamak için verilerinizi korumaya yardımcı olmayı taahhüt eder. Azure depolama, verilerinizi koruyan bir depolama hizmeti şifreleme (depolama birimine yazarken, verileri şifreler ve onu alırken, verilerin şifresini çözer SSE aracılığıyla), yoludur. Şifreleme ve şifre çözme otomatik, saydam ve 256 bit kullanır [AES şifreleme](https://wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.

Microsoft tarafından yönetilen şifreleme anahtarları ile SSE kullanabilir veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Bu makale, kendi şifreleme anahtarlarınızı kullanmayı açıklar. Daha fazla SSE veya Microsoft tarafından yönetilen anahtarlar kullanılarak hakkında genel bilgi için [bekleyen veri için depolama hizmeti şifrelemesi](storage-service-encryption.md).

Böylece, şifreleme anahtarlarını yönetmek için bir anahtar kasası kullanabilirsiniz SSE Blob ve dosya depolama için Azure anahtar kasası ile tümleşiktir. Kendi şifreleme anahtarlarınızı oluşturabilir ve bunları bir anahtar kasası depolamak ya da şifreleme anahtarları oluşturmak için Azure anahtar Kasası'nın API'ları kullanabilirsiniz. Azure anahtar kasası ile yönetmek ve anahtarlarınızı denetlemek ve ayrıca, anahtar kullanımını denetleme.

Neden kendi anahtarları oluşturulsun mu? Böylece oluşturmak, döndürme, devre dışı bırakın ve erişim denetimleri tanımlamak özel anahtarları, daha fazla esneklik sağlar. Özel anahtarları verilerinizi korumak için kullanılan şifreleme anahtarlarını denetim sağlar.

## <a name="get-started-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla kullanmaya başlama

Müşteri tarafından yönetilen anahtarları SSE ile kullanmak için yeni bir anahtar kasası oluşturabilir ya da ve anahtar veya bir var olan anahtar kasasını ve anahtar kullanabilirsiniz. Depolama hesabı ve anahtar kasasıyla aynı bölgede olması gerekir, ancak bunlar farklı Aboneliklerde olabilir. 

### <a name="step-1-create-a-storage-account"></a>1. adım: depolama hesabı oluşturma

İlk olarak, zaten yoksa, bir depolama hesabı oluşturun. Daha fazla bilgi için bkz: [yeni depolama hesabı oluşturma](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>2. adım: SSE Blob ve dosya depolama için etkinleştirme

Müşteri tarafından yönetilen anahtarlar kullanılarak SSE etkinleştirmek için yazılım silin ve yapmak değil temizleme, iki anahtar koruması özellik de etkinleştirilmesi gerekir. Bu ayarlar, anahtarları yanlışlıkla veya bilerek silinmiş olamaz emin olun. Kullanıcıların kötü amaçlı aktörler veya yazılımı saldırılara karşı koruma 90 gün için anahtarların maksimum saklama dönemi ayarlayın.

Program aracılığıyla müşteri tarafından yönetilen anahtarlar için SSE etkinleştirmek istiyorsanız, kullanabileceğiniz [Azure depolama kaynak sağlayıcısı REST API](https://docs.microsoft.com/rest/api/storagerp), [.NET için depolama kaynak sağlayıcısı istemci Kitaplığı](https://docs.microsoft.com/dotnet/api), [ Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), veya [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Müşteri tarafından yönetilen anahtarları SSE ile kullanmak için depolama hesabı için bir depolama hesabı kimlik atamanız gerekir. Aşağıdaki PowerShell komutunu yürüterek kimliğini ayarlayabilirsiniz:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

Aşağıdaki PowerShell komutlarını çalıştırarak yumuşak silin ve temizleme değil etkinleştirebilirsiniz:

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>3. adım: müşteri tarafından yönetilen anahtarlarla şifrelemeyi etkinleştir

Varsayılan olarak, Microsoft tarafından yönetilen anahtarlar SSE kullanır. Depolama hesabı kullanarak müşteri tarafından yönetilen tuşları ile SSE etkinleştirebilirsiniz [Azure portal](https://portal.azure.com/). Üzerinde **ayarları** depolama hesabı için dikey tıklayın **şifreleme**. Seçin **kendi anahtarınızı kullanmaya** , aşağıdaki çizimde gösterildiği gibi seçeneği.

![Portal gösteren ekran görüntüsü şifreleme seçeneği](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>4. adım: anahtarınızı seçin

Anahtarınızı bir URI olarak ya da anahtarı anahtar Kasası'nı seçerek belirtebilirsiniz.

#### <a name="specify-a-key-as-a-uri"></a>Bir anahtar bir URI belirtin

Anahtarınızı bir uri'den belirtmek için aşağıdaki adımları izleyin:

1. Seçin **Enter tuşuna URI** seçeneği.  
2. İçinde **anahtar URI** alan, URI belirtin.

    ![Şifreleme ile anahtar URI seçenek girin gösteren portal ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)


#### <a name="specify-a-key-from-a-key-vault"></a>Bir anahtar kasası anahtarından belirtin 

Anahtarınızı bir anahtar Kasası'ndan belirtmek için aşağıdaki adımları izleyin:

1. Seçin **anahtar Kasası'nı seçin** seçeneği.  
2. Kullanmak istediğiniz anahtarı içeren anahtar Kasası'ı seçin.
3. Bu anahtar, anahtar Kasası'nı seçin.

    ![Portal gösteren ekran görüntüsü şifrelemeleri anahtar seçeneğinizi kullanın](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Depolama hesabı anahtar kasasına erişim yoksa, erişim vermek için aşağıdaki görüntüde gösterilen Azure PowerShell komutunu çalıştırabilirsiniz.

![Erişim için anahtar kasasını reddedildi gösteren portal ekran görüntüsü](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Ayrıca, Azure portalında Azure anahtar kasası gittikten depolama hesabına erişim izni verme Azure portalı üzerinden erişim izni verebilir.


Aşağıdaki PowerShell komutlarını kullanarak var olan bir depolama hesabıyla yukarıdaki anahtar ilişkilendirebilirsiniz:
```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzureRmKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -EnableEncryptionService "Blob" -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```


### <a name="step-5-copy-data-to-storage-account"></a>5. adım: veri depolama alanına kopyalanmaya

Böylece şifreli verileri yeni depolama hesabınızda aktarmak için adım 3'e başvurun [Başlarken bölümünde kalan verileri için depolama hizmeti şifrelemesi](storage-service-encryption.md#step-3-copy-data-to-storage-account).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>6. adım: şifrelenmiş verileri durumunu sorgulama

Şifrelenmiş veriler durumunu sorgulamak için adım 4'e başvurun [Başlarken bölümünde kalan verileri için depolama hizmeti şifrelemesi](storage-service-encryption.md#step-4-query-the-status-of-the-encrypted-data).

## <a name="faq-for-sse-with-customer-managed-keys"></a>Müşteri yönetilen anahtarlara sahip SSE hakkında SSS

**S: Premium depolama kullanıyorum; Müşteri tarafından yönetilen anahtarları ile SSE kullanabilir miyim?**

A: Evet Microsoft tarafından yönetilen ve müşteri tarafından yönetilen anahtarlarla SSE hem standart depolama hem de Premium depolama desteklenir.

**S: yeni depolama hesapları ile SSE Azure PowerShell ve Azure CLI kullanarak etkin müşteri tarafından yönetilen anahtarlarla oluşturabiliyorum?**

Y: Evet.

**S: müşteri tarafından yönetilen anahtarları ile SSE kullanırsanız daha fazlasını nasıl Azure depolama maliyeti?**

Y: var. Azure anahtar kasası kullanmak için ilişkili bir maliyet Daha fazla ayrıntı için ziyaret [anahtar kasası fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/). Tüm depolama hesapları için etkinleştirildi SSE için ek bir maliyet yoktur.

**S: şifreleme anahtarlarının erişimi iptal?**

A: Evet erişimi dilediğiniz zaman iptal edebilirsiniz. Anahtarlarınızı erişimi iptal etmek için birkaç yolu vardır. Başvurmak [Azure anahtar kasası PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) ve [Azure anahtar kasası CLI](https://docs.microsoft.com/cli/azure/keyvault) daha fazla ayrıntı için. Hesap şifreleme anahtarı Azure Storage tarafından erişilemez olduğundan erişimi iptal ediliyor verimli depolama hesabındaki tüm BLOB'lar erişmesini engeller.

**S: bir depolama hesabı ve anahtarı farklı bölgede oluşturabilirim?**

A: Hayır depolama hesabını ve Azure anahtar kasası ve anahtar aynı bölgede olması gerekir.

**S: müşteri tarafından yönetilen anahtarları SSE için depolama hesabı oluşturulurken etkinleştirebilirim?**

A: No. İlk Depolama hesabı oluşturduğunuzda, yalnızca Microsoft tarafından yönetilen anahtarları SSE için kullanılabilir. Müşteri tarafından yönetilen anahtarları kullanmak için depolama hesabı özellikleri güncelleştirmeniz gerekir. Program aracılığıyla depolama hesabınızı güncelleştirmek için REST veya depolama istemci kitaplıklarından birini kullanın veya hesabı oluşturduktan sonra Azure portalını kullanarak depolama hesabı özellikleri güncelleştirin.

**S: müşteri tarafından yönetilen anahtarları ile SSE kullanırken şifreleme devre dışı bırakabilirim?**

A: Hayır şifreleme devre dışı bırakılamıyor. Şifreleme, tüm hizmetleri – Blob, dosya, tablo ve kuyruk depolama için varsayılan olarak etkinleştirilir. Müşteri tarafından yönetilen anahtarlar kullanılarak Microsoft tarafından yönetilen anahtarları kullanarak isteğe bağlı olarak geçiş yapabilir ve tersi.

**S: yeni bir depolama hesabı oluşturduğunuzda, varsayılan olarak etkin SSE mi?**

Y: SSE tüm hizmetleri – Blob, dosya, tablo ve kuyruk depolama ve tüm depolama hesapları için varsayılan olarak etkindir.

**S: müşteri tarafından yönetilen depolama Hesabımı tuşlarıyla SSE etkinleştiremezsiniz.**

A: bir Azure Resource Manager depolama hesabı mı? Klasik depolama hesapları, müşteri tarafından yönetilen anahtarlarla desteklenmez. Müşteri tarafından yönetilen anahtarlarla SSE yalnızca Resource Manager depolama hesaplarında etkinleştirilebilir.

**S: yumuşak silin ve temizleme değil nedir? Müşteri tarafından yönetilen anahtarlarla SSE kullanmak için bu ayarı etkinleştirmeniz gerekiyor mu?**

A: yumuşak silin ve temizleme değil SSE müşteri tarafından yönetilen anahtar ile kullanmak için etkinleştirilmesi gerekir. Bu ayarlar, anahtarınızı değil yanlışlıkla veya bilerek silinmiş olduğundan emin olun. Kullanıcıların kötü amaçlı aktörler ve yazılımı saldırılarına karşı koruma 90 gün için anahtarların maksimum saklama dönemi ayarlayın. Bu ayar devre dışı bırakılamaz.

**S: SSE yalnızca belirli bölgelerine izin müşteri tarafından yönetilen anahtarlarla mi?**

A: müşteri tarafından yönetilen anahtarlarla SSE Blob ve dosya depolama için tüm bölgelerde kullanılabilir.

**S: herhangi bir sorununuz veya geri bildirim sağlamak istiyorsanız nasıl ı biriyle iletişim?**

Y: kişi [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) depolama hizmeti şifrelemesi ile ilgili sorunlar için.

## <a name="next-steps"></a>Sonraki adımlar

-   Güvenlik kapsamlı kümesi hakkında daha fazla bilgi için geliştiricilerin yetenekleri güvenli uygulamaları oluşturmak, bkz: [depolama Güvenlik Kılavuzu](storage-security-guide.md).

-   Azure anahtar kasası hakkında genel bilgi için bkz: [Azure anahtar kasası nedir](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?

-   Azure anahtar kasası kullanmaya başlama için bkz: [Azure anahtar kasası ile çalışmaya başlama](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
