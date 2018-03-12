---
title: Azure PowerShell'i Azure Storage ile kullanma | Microsoft Docs
description: "Azure Storage için Azure PowerShell cmdlet'lerini kullanmayı öğrenin."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: robinsh
ms.openlocfilehash: 7bd8d17d5a2c918f2bef770c224398e7332785f9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="using-azure-powershell-with-azure-storage"></a>Azure Storage ile Azure PowerShell’i kullanma

Azure PowerShell oluşturmak ve PowerShell komut satırından veya komut dosyalarında Azure kaynaklarınızı yönetmek için kullanılır. Azure depolama için bu cmdlet'ler iki kategoriye--denetim düzlemi ve veri düzlemi ayrılır. Denetim düzlemi cmdlet'leri, depolama hesabını yönetmek için--depolama hesapları oluşturmak, özelliklerini ayarlama, depolama hesapları silmek, erişim anahtarlarını döndürün ve benzeri için kullanılır. Veri düzlemi cmdlet'leri depolanan verileri yönetmek için kullanılan *içinde* depolama hesabı. Örneğin, dosya paylaşımları oluşturma ve bir kuyruk iletileri ekleme blobları yükleniyor.

Bu nasıl yapılır makalesi depolama hesaplarını yönetmek için yönetim düzlemi cmdlet'leri kullanarak ortak işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Depolama hesapları listeleme
> * Depolama hesabınız için bir başvuru al
> * Depolama hesabı oluşturma 
> * Depolama hesabı özellikleri ayarlama
> * Almak ve erişim anahtarlarını yeniden oluştur
> * Depolama hesabınıza erişim için koruma 
> * Storage Analytics etkinleştir

Etkinleştirme ve Storage Analytics erişim, veri düzlemi cmdlet'lerinin nasıl kullanılacağını ve nasıl Çin bulut, Almanca Bulut ve kamu gibi Azure bağımsız Bulutlar erişmek gibi depolama için bu makalede diğer çeşitli PowerShell makaleler için bağlantılar sağlar Bulut.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu alıştırmada, Azure PowerShell modülü 4,4 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps). 

Bu alıştırmada, normal bir PowerShell penceresine komutları yazabilir veya kullanabilirsiniz [Windows PowerShell Tümleşik komut dosyası ortamı (ISE)](/powershell/scripting/getting-started/fundamental/windows-powershell-integrated-scripting-environment--ise-) ve bir düzenleyicisine komutları yazın ve ardından bir anda bir veya daha fazla komut test örneklerle gidin. Yürütme ve bu komutları çalıştırma henüz için Seçileni Çalıştır'ı tıklatın istediğiniz satırları vurgulayın.

Depolama hesapları hakkında daha fazla bilgi için bkz: [depolama giriş](storage-introduction.md) ve [Azure storage hesapları hakkında](storage-create-storage-account.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="list-the-storage-accounts-in-the-subscription"></a>Abonelikteki depolama hesapları listesi

Çalıştırma [Get-AzureRMStorageAccount](/powershell/module/azurerm.resources/get-azurermstorageaccount) geçerli Abonelikteki depolama hesapları listesini almak için cmdlet. 

```powershell
Get-AzureRMStorageAccount | Select StorageAccountName, Location
```

## <a name="get-a-reference-to-a-storage-account"></a>Bir depolama hesabı için bir başvuru al

Ardından, bir depolama hesabı için bir başvuru gerekir. Yeni bir depolama hesabı oluşturun veya varolan bir depolama hesabı için bir başvuru. Aşağıdaki bölümde, her iki yöntem gösterilmektedir. 

### <a name="use-an-existing-storage-account"></a>Varolan depolama hesabını kullan 

Mevcut bir depolama hesabını almak için kaynak grubunun adını ve depolama hesabı adı gerekir. Bu iki alan değişkenleri ayarlayın, sonra kullanın [Get-AzureRmStorageAccount](/powershell/module/azurerm.storage/Get-AzureRmStorageAccount) cmdlet'i. 

```powershell
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"

$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
```

Artık mevcut bir depolama hesabını işaret $storageAccount vardır.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Aşağıdaki komut dosyası kullanarak bir genel amaçlı depolama hesabı oluşturmayı gösteren [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Hesabı oluşturduktan sonra sonraki komutlarda kullanılabilir onun içeriği almak yerine her çağrısı ile kimlik doğrulaması belirtme.

```powershell
# Get list of locations and select one.
Get-AzureRmLocation | select Location 
$location = "eastus"

# Create a new resource group.
$resourceGroup = "teststoragerg"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 

# Set the name of the storage account and the SKU name. 
$storageAccountName = "testpshstorage"
$skuName = "Standard_LRS"
    
# Create the storage account.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName $skuName

# Retrieve the context. 
$ctx = $storageAccount.Context
```

Komut dosyası için aşağıdaki PowerShell cmdlet'leri kullanır: 

*   [Get-AzureRmLocation](/powershell/module/azurerm.storage/Get-AzureRmLocation) --geçerli konumların bir listesini alır. Örnek kullanır `eastus` konumu.

*   [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup) --yeni bir kaynak grubu oluşturur. Bir kaynak grubu içine Azure kaynaklarınızı dağıtılan ve yönetilen mantıksal bir kapsayıcısıdır. Bizim adlı `teststoragerg`. 

*   [Yeni-AzureRmStorageAccount](/powershell/module/azurerm.resources/New-AzureRmStorageAcccount) --gerçek depolama hesabı oluşturur. Örnek kullanır `testpshstorage`.

SKU adı LRS (yerel olarak yedekli depolama) gibi depolama hesabı için çoğaltma türünü belirtir. Çoğaltma hakkında daha fazla bilgi için bkz: [Azure Storage çoğaltma](storage-redundancy.md).

> [!IMPORTANT]
> Depolama hesabınızın adının Azure içinde benzersiz olmalıdır ve küçük harf olması gerekir. Adlandırma kuralları ve sınırlamaları için bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
> 

Artık, yeni bir depolama hesabı ve kendisine bir başvuru vardır. 

## <a name="manage-the-storage-account"></a>Storage hesabı yönetme

Yeni bir depolama hesabı ya da mevcut bir depolama hesabını başvuru sahip olduğunuza göre aşağıdaki bölümde, depolama hesabınızı yönetmek için kullanabileceğiniz komutlar bazılarını gösterir.

### <a name="storage-account-properties"></a>Depolama hesabı özellikleri

Bir depolama hesabı ayarlarını değiştirmek için kullanmak [Set-AzureRmStorageAccount](/powershell/module/azurerm.resources/Set-AzureRmStorageAccount). Bir depolama hesabı ya da içinde bulunduğu kaynak grubu konumunu değiştiremezsiniz, ancak diğer özelliklerin çoğu değiştirebilirsiniz. PowerShell kullanarak değiştirebilirsiniz özelliklerden bazıları aşağıda listelenmiştir.

* **Özel etki alanı** depolama hesabına atanır.

* **Etiketleri** depolama hesabına atanır. Etiketler, genellikle faturalandırma amaçları için kaynakları sınıflandırmak için kullanılır.

* **SKU** gibi yerel olarak yedekli depolama LRS depolama hesabı için çoğaltma ayardır. Örneğin, standart değişebilir\_için standart LRS\_GRS veya standart\_RAGRS. Standart değiştiremezsiniz Not\_ZRS ya da Premium\_diğer SKU'ları için LRS veya bunları başka SKU'ları değiştirin.

* **Erişim katmanı** Blob storage hesapları için. Erişim katmanı için değer kümesine **etkin** veya **cool**, ve depolama hesabını kullanma ile hizalar erişim katmanı seçerek maliyetini en aza indirmenize olanak sağlar. Daha fazla bilgi için bkz: [, cool, sık erişimli ve depolama katmanları arşiv](../blobs/storage-blob-storage-tiers.md).

* Yalnızca HTTPS trafiğine izin verin. 

### <a name="manage-the-access-keys"></a>Erişim anahtarlarını Yönet

Bir Azure Storage hesabı iki hesabı anahtarları ile birlikte gelir. Anahtarları almak için kullanın [Get-AzureRmStorageAccountKey](/powershell/module/AzureRM.Storage/Get-AzureRmStorageAccountKey). Bu örnekte, ilk anahtarı alır. Başka bir almak kullanın `Value[1]` yerine `Value[0]`.

```powershell
$storageAccountKey = `
    (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroup `
    -Name $storageAccountName).Value[0]
```

Anahtarı yeniden oluşturmak için kullanın [yeni AzureRmStorageAccountKey](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccountKey). 

```powershell
New-AzureRmStorageAccountKey -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -KeyName key1 
```

Diğer anahtarı yeniden oluşturmak için kullanmak `key2` yerine anahtar adı olarak `key1`.

Anahtarlarınızı birini yeniden oluşturmak ve yeni değer yeniden görmek için alın.

> [!NOTE] 
> Bir üretim depolama hesabı anahtarı yeniden üretilirken önce dikkatli planlama gerçekleştirmeniz gerekir. Bir veya iki anahtarlarını yeniden oluşturma, yeniden üretildi anahtarını kullanarak herhangi bir uygulama için erişim geçersiz kılar. Daha fazla bilgi için lütfen bkz [depolama erişim tuşlarını yeniden](storage-create-storage-account.md#regenerate-storage-access-keys).


### <a name="delete-a-storage-account"></a>Bir depolama hesabını silme 

Bir depolama hesabını silmek için kullanın [Remove-AzureRmStorageAccount](/powershell/module/azurerm.storage/Remove-AzureRmStorageAccount).

```powershell
Remove-AzureRmStorageAccount -ResourceGroup $resourceGroup -AccountName $storageAccountName
```

> [!IMPORTANT]
> Bir depolama hesabını silmek, tüm hesapta depolanan ve varlıkları de silinir. Bir hesap yanlışlıkla silerseniz, destek hemen arayın ve depolama hesabını geri yüklemek için bilet. Verilerinizin kurtarma yıkıcıları ancak bazen çalışır. Destek bileti çözümlenene kadar eskisiyle aynı ada sahip yeni bir depolama hesabı oluşturmayın. 
>

### <a name="protect-your-storage-account-using-vnets-and-firewalls"></a>Sanal ağlar ve güvenlik duvarları kullanarak depolama hesabınızın korunmasına

Varsayılan olarak, tüm depolama hesapları internet erişimi olan herhangi bir ağ tarafından erişilebilir. Ancak, ağ kuralları yalnızca bir depolama hesabına erişmek belirli sanal ağlar uygulamalardan izin verecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Azure depolama güvenlik duvarlarını yapılandırın ve sanal ağlar](storage-network-security.md). 

Makaleyi bu ayarları aşağıdaki PowerShell cmdlet'lerini kullanarak yönetmek nasıl gösterir:
* [Add-AzureRmStorageAccountNetworkRule](/powershell/module/AzureRM.Storage/Add-AzureRmStorageAccountNetworkRule)
* [Update-AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset)
* [Remove-AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.storage/remove-azurermstorage-account-networkrule)

## <a name="use-storage-analytics"></a>Storage analytics kullanın  

[Azure Storage Analytics](storage-analytics.md) oluşan [Storage Analytics ölçümleri](/rest/api/storageservices/about-storage-analytics-metrics) ve [depolama Analytics günlüğü](/rest/api/storageservices/about-storage-analytics-logging). 

**Storage Analytics ölçümleri** bir depolama hesabı sağlığını izlemek için kullanabileceğiniz, Azure depolama hesapları için ölçümleri toplamak için kullanılır. Ölçümleri, BLOB'lar, dosyalar, tablolar ve sıralar için etkinleştirilebilir.

**Storage Analytics günlüğü** sunucu tarafı olur ve hem başarılı hem başarısız istekleri depolama hesabınız için kayıt ayrıntıları sağlar. Bu günlükler, okuma, yazma ve silme işlemleri tablolarınızı, kuyruklar ve BLOB'lar yanı sıra nedeniyle başarısız istekler için karşı ayrıntılarını görmek etkinleştirin. Azure dosyaları için günlük kaydı kullanılamaz.

Kullanarak izlemeyi yapılandırmadan [Azure portal](https://portal.azure.com), PowerShell veya program aracılığıyla depolama istemci kitaplığı kullanılarak. 

> [!NOTE]
> PowerShell kullanarak dakika analytics etkinleştirebilirsiniz. Bu özellik portalda kullanılabilir değildir.
>

* Etkinleştirmek ve PowerShell kullanarak depolama ölçüm verilerini görüntüleme hakkında bilgi almak için bkz: [etkinleştirme Azure Storage ölçümleri ve ölçüm verilerini görüntüleme](storage-enable-and-view-metrics.md#how-to-enable-metrics-using-powershell).

* Etkinleştirmek ve PowerShell kullanarak depolama günlüğü verilerini alma hakkında bilgi almak için bkz: [PowerShell kullanarak depolama günlüğü etkinleştirme](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data#how-to-enable-storage-logging-using-powershell) ve [depolama günlüğü günlük verilerinizi bulma](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data#finding-your-storage-logging-log-data).

* Depolama sorunları gidermek için depolama ölçümleri ve depolama oturum kullanma hakkında ayrıntılı bilgi için bkz: [izleme, Diagnosing ve sorun giderme Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="manage-the-data-in-the-storage-account"></a>Depolama hesabınızdaki verileri yönetme

PowerShell ile depolama hesabınızı yönetme anladığınıza göre veri nesneleri depolama hesabındaki erişim hakkında bilgi edinmek için aşağıdaki makalelere kullanabilirsiniz.

* [BLOB'lar PowerShell ile yönetme](../blobs/storage-how-to-use-blobs-powershell.md)
* [Dosyaları PowerShell ile yönetme](../files/storage-how-to-use-files-powershell.md)
* [Kuyruklar PowerShell ile yönetme](../queues/storage-powershell-how-to-use-queues.md)
* [PowerShell ile Azure Table depolama işlemleri](../../cosmos-db/table-storage-how-to-use-powershell.md)

Azure Cosmos DB tablo API anahtar teslimi genel dağıtım, düşük gecikme süresi okuma ve yazma, otomatik ikincil dizin oluşturma ve ayrılmış işleme gibi tablo depolama premium özellikleri sunar. 

* Daha fazla bilgi için bkz: [Azure Cosmos DB tablo API](../../cosmos-db/table-introduction.md). 
* Azure Cosmos DB tablo API işlemleri gerçekleştirmek için PowerShell'i kullanmayı öğrenmek için bkz: [PowerShell ile Azure Cosmos DB tablo API gerçekleştirmek işlemleri](../../cosmos-db/table-powershell.md).

## <a name="independent-cloud-deployments-of-azure"></a>Azure bağımsız bulut dağıtımları

Çoğu kişi Azure genel bulut, genel Azure dağıtım için kullanın. Ayrıca vardır Egemenlik, nedeniyle Microsoft Azure bağımsız bazı dağıtımları ve benzeri. Bu bağımsız dağıtımlar "ortamlar" adlandırılır Kullanılabilir ortamlar şunlardır:

* [Azure Bulutu](https://azure.microsoft.com/features/gov/)
* [Çin'de 21Vianet tarafından işletilen Azure Çin bulut](http://www.windowsazure.cn/)
* [Azure Almanca bulut](../../germany/germany-welcome.md)

Bu Bulut ve PowerShell ile kendi depolama nasıl erişileceği hakkında daha fazla bilgi için lütfen bkz [yönetme depolama PowerShell kullanarak Azure bağımsız bulutlarındaki](storage-powershell-independent-clouds.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yeni bir kaynak grubu ve bu alıştırma için bir depolama hesabı oluşturduysanız, yous tüm kaynak grubunu kaldırma tarafından oluşturulan ve varlıkları kaldırabilirsiniz. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturulan depolama hesabı ve kaynak grubu kaldırır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```
## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesi depolama hesaplarını yönetmek için yönetim düzlemi cmdlet'leri kullanarak ortak işlemleri kapsar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Depolama hesapları listeleme
> * Depolama hesabınız için bir başvuru al
> * Depolama hesabı oluşturma 
> * Depolama hesabı özellikleri ayarlama
> * Almak ve erişim anahtarlarını yeniden oluştur
> * Depolama hesabınıza erişim için koruma 
> * Storage Analytics etkinleştir

Bu makalede, veri nesnelerini yönetme, depolama çözümlemeleri etkinleştirme ve Çin bulut, Almanca Bulut ve Bulutu gibi Azure bağımsız Bulutlar erişmek nasıl gibi birkaç diğer makalelere, başvurular de sağlanır. Bazı daha ilgili makaleler ve başvuru kaynakları şunlardır:

* [Azure depolama denetim düzlemi PowerShell cmdlet'leri](/powershell/module/AzureRM.Storage/)
* [Azure Storage veri düzlemi PowerShell cmdlet'leri](/powershell/module/azure.storage/)
* [Windows PowerShell başvurusu](https://msdn.microsoft.com/library/ms714469.aspx)
