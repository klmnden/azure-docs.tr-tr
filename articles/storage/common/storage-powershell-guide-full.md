---
title: Azure PowerShell'i Azure depolama ile kullanma | Microsoft Docs
description: Azure depolama için Azure PowerShell cmdlet'lerini kullanmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/16/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 082033cebc68fc97f7cff9ce80eb02acbbf5f4b0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145890"
---
# <a name="using-azure-powershell-with-azure-storage"></a>Azure Storage ile Azure PowerShell’i kullanma

Azure PowerShell, oluşturma ve PowerShell komut satırından veya betiklerdeki Azure kaynaklarını yönetmek için kullanılır. Azure depolama için bu cmdlet'ler iki kategoriye--denetim düzlemi ve veri düzlemine ayrılır. Depolama hesabını yönetmek için--depolama hesapları oluşturmak, özelliklerini ayarlayın, depolama hesaplarını, erişim anahtarlarını döndürme ve benzeri için Denetim düzlemi cmdlet'leri kullanılır. Veri düzlemi cmdlet'leri depolanan verileri yönetmek için kullanılan *içinde* depolama hesabı. Örneğin, blobları karşıya yükleme, dosya paylaşımları oluşturmak ve almak için kuyruk iletileri ekleme.

Bu nasıl yapılır makalesi, depolama hesaplarını yönetmek için yönetim düzlemi cmdlet'leri kullanarak ortak işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesapları listeleme
> * Mevcut bir depolama hesabı için bir başvuru alma
> * Depolama hesabı oluşturma
> * Depolama hesabı özelliklerini ayarlama
> * Alma ve erişim anahtarlarını yeniden oluştur
> * Depolama hesabınıza erişimi koruma
> * Depolama analizi etkinleştir

Etkinleştirme ve erişim depolama analizi, veri düzlemi cmdlet'lerinin nasıl kullanılacağını ve Çin Bulutu, Almanya Bulut ve kamu gibi Azure bağımsız bulutlarda erişim gibi depolama için bu makalede çeşitli PowerShell makalelere bağlantılar sağlar Bulut.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu alıştırmada, Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

Bu alıştırma için normal bir PowerShell penceresine komutları yazmanıza ya da kullanabileceğinizi [Windows PowerShell Tümleşik komut dosyası ortamı (ISE)](/powershell/scripting/getting-started/fundamental/windows-powershell-integrated-scripting-environment--ise-) ve komutlar bir düzenleyicisine yazın ve ardından bir anda bir veya daha fazla komut test örneklerle gittiğiniz. Bu komutları çalıştırma yalnızca Seçileni Çalıştır'a tıklayın ve yürütmek istediğiniz satırları vurgulayabilirsiniz.

Depolama hesapları hakkında daha fazla bilgi için bkz. [depolamaya giriş](storage-introduction.md) ve [Azure depolama hesapları hakkında](storage-create-storage-account.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Abonelikteki depolama hesaplarını listeleme

Çalıştırma [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount) geçerli Abonelikteki depolama hesaplarının listesini almak için cmdlet'i.

```powershell
Get-AzStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Bir depolama hesabı için bir başvuru alma

Ardından, bir depolama hesabı için bir başvuru gerekir. Yeni bir depolama hesabı oluşturun veya mevcut bir depolama hesabı için bir başvuru alın. Aşağıdaki bölümde, her iki yöntem de gösterir.

### <a name="use-an-existing-storage-account"></a>Mevcut bir depolama hesabı kullanın

Mevcut bir depolama hesabı almak için kaynak grubu adını ve depolama hesabı adı gerekir. Bu iki alan değişkenleri ayarlayın ve ardından kullanmak [Get-AzStorageAccount](/powershell/module/az.storage/Get-azStorageAccount) cmdlet'i.

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName
```

Artık mevcut bir depolama hesabına işaret eden $storageAccount var.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Aşağıdaki betiği kullanarak bir genel amaçlı depolama hesabı oluşturma işlemini gösterir [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Hesabı oluşturduktan sonra sonraki komutlarda kullanılabilecek kendi bağlamı almak yerine her çağrısı ile kimlik doğrulaması belirtme.

```powershell
# Get list of locations and select one.
Get-AzLocation | select Location
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzResourceGroup -Name $resourceGroup -Location $location

# Set the name of the storage account and the SKU name.
$storageAccountName = "testpshstorage"
$skuName = "Standard_LRS"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context.
$ctx = $storageAccount.Context
```

Betik aşağıdaki PowerShell cmdlet'lerini kullanır:

*   [Get-AzLocation](/powershell/module/az.resources/get-azlocation) --geçerli konumların bir listesini alır. Örnekte `eastus` konumu.

*   [Yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) --yeni bir kaynak grubu oluşturur. Bir kaynak grubu, Azure kaynaklarınızın dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Bizim çağrılır `teststoragerg`.

*   [Yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) --depolama hesabı oluşturur. Örnekte `testpshstorage`.

SKU adı LRS (yerel olarak yedekli depolama) gibi bir depolama hesabı için çoğaltma türünü belirtir. Çoğaltma hakkında daha fazla bilgi için bkz. [Azure depolama çoğaltma](storage-redundancy.md).

> [!IMPORTANT]
> Depolama hesabınızın adı Azure içinde benzersiz olmalıdır ve küçük harf olması gerekir. Adlandırma kuralları ve kısıtlamalar bkz [adlandırma ve başvuran kapsayıcıları, Blobları ve meta verileri](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
>

Artık, yeni bir depolama hesabı ve buna bir başvuru vardır.

## <a name="manage-the-storage-account"></a>Depolama hesabı yönetme

Yeni bir depolama hesabı veya mevcut bir depolama hesabını başvuru olduğuna göre aşağıdaki bölümde, depolama hesabınızı yönetmek için kullanabileceğiniz komutlar bazıları gösterilmektedir.

### <a name="storage-account-properties"></a>Depolama hesabı özellikleri

Bir depolama hesabı ayarlarını değiştirmek için kullanın [kümesi AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount). Bir depolama hesabı ya da içinde bulunduğu kaynak grubunun konumunu değiştiremezsiniz, ancak diğer özelliklerin birçoğu değiştirebilirsiniz. PowerShell kullanarak değiştirebileceğiniz özelliklerinden bazıları aşağıda listelenmektedir.

* **Özel etki alanı** depolama hesabına atanır.

* **Etiketleri** depolama hesabına atanır. Etiketler genellikle kaynakları faturalandırma için kategorilere ayırmak için kullanılır.

* **SKU** gibi yerel olarak yedekli depolama için LRS depolama hesabı için çoğaltma ayardır. Örneğin, standart değişebilir\_standart LRS\_GRS veya standart\_RAGRS. Standart değiştiremezsiniz Not\_ZRS veya Premium\_diğer SKU'lara LRS veya diğer SKU'lar bunları değiştirebilirsiniz.

* **Erişim katmanı** Blob Depolama hesapları için. Erişim katmanı için bir değer kümesine **sık erişimli** veya **seyrek erişimli**, ve depolama hesabını nasıl kullandığınız ile hizalar erişim katmanı seçerek maliyetini en aza indirmek sağlar. Daha fazla bilgi için [sık, seyrek ve Arşiv depolama katmanları](../blobs/storage-blob-storage-tiers.md).

* Yalnızca HTTPS trafiğine izin verin.

### <a name="manage-the-access-keys"></a>Erişim anahtarlarını yönetme

Bir Azure depolama hesabı, iki hesap anahtarları ile birlikte gelir. Anahtarlarını almak için kullanın [Get-AzStorageAccountKey](/powershell/module/az.Storage/Get-azStorageAccountKey). Bu örnek için ilk tuşu alır. Başka bir almak için kullanın `Value[1]` yerine `Value[0]`.

```powershell
$storageAccountKey = `
    (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Anahtarı yeniden oluşturmak için kullanmak [yeni AzStorageAccountKey](/powershell/module/az.Storage/New-azStorageAccountKey).

```powershell
New-AzStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1
```

Diğer anahtarı yeniden oluşturmak için kullanmak `key2` yerine anahtar adı olarak `key1`.

Anahtarlarınızdan birini yeniden oluşturun ve sonra yeniden yeni değeri görmek için alır.

> [!NOTE]
> Bir üretim depolama hesabı anahtarı yeniden önce dikkatli planlama yapmalısınız. Bir veya iki anahtarlarını yeniden oluşturma, yeniden oluşturuldu anahtarını kullanarak herhangi bir uygulama için erişim geçersiz kılar. Daha fazla bilgi için bkz. [Erişim anahtarları](storage-account-manage.md#access-keys)


### <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

Bir depolama hesabını silmek için kullanın [Remove-AzStorageAccount](/powershell/module/az.storage/Remove-azStorageAccount).

```powershell
Remove-AzStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

> [!IMPORTANT]
> Bir depolama hesabını silmek, tüm varlıkları hesapta depolanan de silinir. Bir hesap kaza ile silerseniz, destek hemen şu çağrıyı yapar ve depolama hesabını geri yüklemek için bir bilet açın. Verilerinizin kurtarılmasına garanti edilmez, ancak bazen çalışır. Destek bileti çözümlenene kadar eskisi ile aynı adda yeni bir depolama hesabı oluşturmayın.
>

### <a name="protect-your-storage-account-using-vnets-and-firewalls"></a>Sanal ağlar ve güvenlik duvarları kullanarak depolama hesabınızı koruyun

Varsayılan olarak, tüm depolama hesapları, internet erişimi olan herhangi bir ağ tarafından erişilebilir. Ancak, yalnızca belirli sanal ağlar uygulamalardan bir depolama hesabına erişmesine izin vermek için ağ kuralları yapılandırabilirsiniz. Daha fazla bilgi için [Azure depolama güvenlik duvarlarını yapılandırın ve sanal ağları](storage-network-security.md).

Bu makalede aşağıdaki PowerShell cmdlet'lerini kullanarak bu ayarlarının nasıl yönetileceğini gösterir:
* [Add-AzStorageAccountNetworkRule](/powershell/module/az.Storage/Add-azStorageAccountNetworkRule)
* [Update-AzStorageAccountNetworkRuleSet](/powershell/module/az.storage/update-azstorageaccountnetworkruleset)
* [Remove-AzStorageAccountNetworkRule](https://docs.microsoft.com/powershell/module/az.storage/remove-azstorageaccountnetworkrule)

## <a name="use-storage-analytics"></a>Depolama analizi kullanma  

[Azure depolama analizi](storage-analytics.md) oluşan [Storage Analytics ölçümleri](/rest/api/storageservices/about-storage-analytics-metrics) ve [depolama analizi günlük](/rest/api/storageservices/about-storage-analytics-logging).

**Storage Analytics ölçümleri** bir depolama hesabının sistem durumunu izlemek için kullanabileceğiniz Azure depolama hesaplarınız için ölçümleri toplamak için kullanılır. Ölçümler, bloblar, dosyalar, tablolar ve Kuyruklar için etkinleştirilebilir.

**Depolama analizi günlük** sunucu tarafı olur ve için depolama hesabınıza başarılı ve başarısız istekler için ayrıntıları sağlar. Bu günlükleri, okuma, yazma ve silme işlemleri tablolara, kuyrukları ve blobları yanı sıra başarısız isteklerin nedenlerini karşı ayrıntılarını görmek etkinleştirin. Azure dosyaları için kullanılabilir günlük yok.

Kullanarak izlemeyi yapılandırabilirsiniz [Azure portalında](https://portal.azure.com), PowerShell veya depolama istemci kitaplığı kullanarak program aracılığıyla.

> [!NOTE]
> PowerShell'i kullanarak dakika analytics etkinleştirebilirsiniz. Bu özellik portalda kullanılamaz.
>

* Etkinleştirmek ve PowerShell kullanarak depolama ölçümleri verileri görüntüleme hakkında bilgi edinmek için [Storage analytics ölçümleri](storage-analytics-metrics.md).

* Etkinleştirmek ve PowerShell kullanarak depolama günlük verileri almak nasıl öğrenmek için bkz. [PowerShell kullanarak depolama günlüğü etkinleştirme](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data) ve [depolama günlüğü günlük verilerinizi bulma](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data).

* Depolama sorunları gidermek için depolama ölçümleri ve depolama günlük kaydı kullanma hakkında ayrıntılı bilgi için bkz: [izleme Diagnosing ve Microsoft Azure depolama sorunlarını giderme](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="manage-the-data-in-the-storage-account"></a>Depolama hesabındaki verileri yönetme

PowerShell ile depolama hesabınızı yönetme anladığınıza göre veri nesnelerini depolama hesabındaki erişim hakkında bilgi edinmek için aşağıdaki makalelere kullanabilirsiniz.

* [PowerShell ile blobları yönetme](../blobs/storage-how-to-use-blobs-powershell.md)
* [Dosyaları PowerShell ile yönetme](../files/storage-how-to-use-files-powershell.md)
* [Kuyruklar PowerShell ile yönetme](../queues/storage-powershell-how-to-use-queues.md)
* [PowerShell ile Azure tablo depolama işlemleri](../../storage/tables/table-storage-how-to-use-powershell.md)

Azure Cosmos DB tablo API'si, tablo depolama gibi anahtar teslim küresel dağıtım, düşük gecikme süreli okuma ve yazma işlemleri, otomatik ikincil dizin oluşturma ve adanmış aktarım hızı için premium özellikleri sağlar.

* Daha fazla bilgi için [Azure Cosmos DB tablo API'si](../../cosmos-db/table-introduction.md).

## <a name="independent-cloud-deployments-of-azure"></a>Azure'nın bağımsız bulut dağıtımları

Çoğu kişi, Azure genel bulut, genel Azure dağıtım için kullanın. De vardır özerkliği, nedeniyle için Microsoft Azure'nın bağımsız bazı dağıtımları ve benzeri. Bu bağımsız dağıtımlar "ortam" adlandırılır Kullanılabilir bir ortam şunlardır:

* [Azure kamu Bulutu](https://azure.microsoft.com/features/gov/)
* [Çin'de 21Vianet tarafından işletilen azure Çin 21Vianet bulut](http://www.windowsazure.cn/)
* [Azure Almanya Bulutu](../../germany/germany-welcome.md)

Bu Bulutlar ve bu makinelerin depolama PowerShell ile erişme hakkında daha fazla bilgi için lütfen bkz [yönetme depolama PowerShell kullanarak Azure bağımsız bulutlarda](storage-powershell-independent-clouds.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yeni bir kaynak grubu ve bu alıştırma için depolama hesabı oluşturduysanız, tüm kaynak grubu kaldırarak oluşturduğunuz varlıkları yous kaldırabilirsiniz. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturduğunuz depolama hesabına ve kaynak grubunu kaldırır.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```
## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesi, depolama hesaplarını yönetmek için yönetim düzlemi cmdlet'leri kullanarak ortak işlemleri kapsar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Depolama hesapları listeleme
> * Mevcut bir depolama hesabı için bir başvuru alma
> * Depolama hesabı oluşturma
> * Depolama hesabı özelliklerini ayarlama
> * Alma ve erişim anahtarlarını yeniden oluştur
> * Depolama hesabınıza erişimi koruma
> * Depolama analizi etkinleştir

Bu makalede, veri nesnelerini yönetme, depolama analizi etkinleştirme ve Çin Bulutu, Almanya Bulut ve kamu Bulutu gibi Azure bağımsız bulutlarda erişme gibi birkaç başka makale, başvuruları de sağlanır. Daha fazla ilgili bazı makaleler ve başvuru kaynakları şunlardır:

* [Azure depolama denetim düzlemi PowerShell cmdlet'leri](/powershell/module/az.storage/)
* [Azure depolama veri düzlemi PowerShell cmdlet'leri](/powershell/module/azure.storage/)
* [Windows PowerShell başvurusu](https://msdn.microsoft.com/library/ms714469.aspx)
