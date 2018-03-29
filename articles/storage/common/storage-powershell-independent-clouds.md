---
title: Bulutlar Azure PowerShell kullanarak Azure bağımsız depolama yönetme | Microsoft Docs
description: Depolama Çin bulut, Bulutu ve Almanca bulut Azure PowerShell kullanarak yönetme
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: rogarana
ms.openlocfilehash: 31b36e6fb6a1ebf09c559b2777ffa5f554c3cfa0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="managing-storage-in-the-azure-independent-clouds-using-powershell"></a>PowerShell kullanarak Azure bağımsız bulut depolamada yönetme

Çoğu kişi Azure genel bulut, genel Azure dağıtım için kullanın. Ayrıca vardır Egemenlik, nedeniyle Microsoft Azure bağımsız bazı dağıtımları ve benzeri. Bu bağımsız dağıtımlar "ortamlar" adlandırılır Aşağıdaki liste, şu anda kullanılabilir bağımsız bulut ayrıntıları verilmektedir.

* [Azure Bulutu](https://azure.microsoft.com/features/gov/)
* [Çin'de 21Vianet tarafından işletilen Azure Çin bulut](http://www.windowsazure.cn/)
* [Azure Almanca bulut](../../germany/germany-welcome.md)

## <a name="using-an-independent-cloud"></a>Bağımsız bir bulut kullanma 

Azure Storage bağımsız Bulutlar birini kullanmak için Azure ortak yerine bu buluta bağlanın. Bağımsız Bulutlar yerine Azure ortak birini kullanmak için:

* Belirttiğiniz *ortam* bağlantı kurmak için.
* Belirlemek ve kullanılabilir bölgelerin kullanın.
* Azure ortak farklı doğru uç nokta sonekini kullanın.

Azure PowerShell modülü sürümü 4.4.0 örnekler gerektiren veya sonraki bir sürümü. Bir PowerShell penceresinde çalıştırın `Get-Module -ListAvailable AzureRM` sürümü bulunamıyor. Hiçbir şey listelenen veya yükseltmek için bkz: gereken [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Çalıştırma [Get-AzureEnvironment](/powershell/module/azure/Get-AzureRmEnvironment) kullanılabilir Azure ortamları görmek için cmdlet:
   
```powershell
Get-AzureRmEnvironment
```

Hesabınıza bağlamak ve ortam kurun istediğiniz bulut erişimi oturum açın. Bu örnek, Azure Bulutu kullanan bir hesaba oturum gösterilmektedir.   

```powershell
Login-AzureRmAccount –Environment AzureUSGovernment
```

Çin bulut erişmek için bir ortam kullanmanız **AzureChinaCloud**. Almanca bulut erişmek için **AzureGermanCloud**.

Bu noktada, depolama hesabı veya başka bir kaynak oluşturmak için konumların listesini ihtiyacınız varsa, seçili bulut kullanılmasında kullanılabilir konumları sorgulayabilirsiniz [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation).

```powershell
Get-AzureRmLocation | select Location, DisplayName
```

Aşağıdaki tabloda Almanca bulut için döndürülen konumlarını da gösterir.

|Konum | Görünen adı |
|----|----|
| germanycentral | Almanya Orta|
| germanynortheast | Almanya Kuzeydoğu | 


## <a name="endpoint-suffix"></a>Uç nokta soneki

Uç nokta soneki her bu ortamlar için Azure ortak uç noktasından farklıdır. Örneğin, blob uç noktası için Azure ortak sonekidir **blob.core.windows.net**. Bulutu için blob uç noktası sonekidir **blob.core.usgovcloudapi.net**. 

### <a name="get-endpoint-using-get-azurermenvironment"></a>Get-AzureRMEnvironment kullanarak uç noktasını alın 

Uç nokta sonekini kullanarak almak [Get-AzureRMEnvironment](/powershell/module/azurerm.profile/get-azurermenvironment). Uç noktası *StorageEndpointSuffix* ortam özelliği. Aşağıdaki kod parçacıkları bunun nasıl yapılacağı gösterilmektedir. Bu komutların tümünü bir şey Döndür "core.cloudapp.net" veya "core.cloudapi.de", vb. gibi. Bu hizmete erişmek için depolama birimi hizmeti ekleyin. Örneğin, "queue.core.cloudapi.de" Almanca bulut kuyruk hizmetinde erişir.

Bu kod parçacığını tüm ortamları ve her biri için uç nokta sonekini alır.

```powershell
Get-AzureRmEnvironment | select Name, StorageEndpointSuffix 
```

Bu komut aşağıdaki sonuçları döndürür.

| Ad| StorageEndpointSuffix|
|----|----|
|AzureChinaCloud | core.chinacloudapi.cn|
| AzureCloud | core.windows.net |
| AzureGermanCloud | core.cloudapi.de|
| AzureUSGovernment | core.usgov.cloudapi.net |


Belirtilen ortamı için özelliklerin tümünü almak için arama **Get-AzureRmEnvironment** ve bulut adı belirtin. Bu kod parçacığını özelliklerin listesini döndürür; Ara **StorageEndpointSuffix** listesinde. Aşağıdaki örnek, Almanca bulut için aynıdır.

```powershell
Get-AzureRmEnvironment -Name AzureGermanCloud 
```

Sonuçlar aşağıdakine benzer:

|Özellik Adı|Değer|
|----|----|
| Ad | AzureGermanCloud |
| EnableAdfsAuthentication | False |
| ActiveDirectoryServiceEndpointResourceI | http://management.core.cloudapi.de/ |
| GalleryURL | https://gallery.cloudapi.de/ |
| ManagementPortalUrl | https://portal.microsoftazure.de/ | 
| ServiceManagementUrl | https://manage.core.cloudapi.de/ |
| PublishSettingsFileUrl| https://manage.microsoftazure.de/publishsettings/index |
| ResourceManagerUrl | http://management.microsoftazure.de/ |
| SqlDatabaseDnsSuffix | .database.cloudapi.de |
| **StorageEndpointSuffix** | core.cloudapi.de |
| ... | ... | 

Yalnızca depolama uç nokta soneki özelliğini almak için özel bulut alın ve yalnızca bu özellik için isteyin.

```powershell
$environment = Get-AzureRmEnvironment -Name AzureGermanCloud
Write-Host "Storage EndPoint Suffix = " $environment.StorageEndpointSuffix 
```

Bu, aşağıdaki bilgileri döndürür.

```
Storage Endpoint Suffix = core.cloudapi.de
```

### <a name="get-endpoint-from-a-storage-account"></a>Uç noktası bir depolama hesabından alın

Uç noktaları almak için bir depolama hesabının özelliklerini de inceleyebilirsiniz. Bu, PowerShell komut dosyanıza bir depolama hesabı kullanıyorsanız yararlı olabilir; gereksinim duyduğunuz uç nokta yalnızca alabilir. 

```powershell
# Get a reference to the storage account.
$resourceGroup = "myexistingresourcegroup"
$storageAccountName = "myexistingstorageaccount"
$storageAccount = Get-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName 
  # Output the endpoints.
Write-Host "blob endpoint = " $storageAccount.PrimaryEndPoints.Blob 
Write-Host "file endpoint = " $storageAccount.PrimaryEndPoints.File
Write-Host "queue endpoint = " $storageAccount.PrimaryEndPoints.Queue
Write-Host "table endpoint = " $storageAccount.PrimaryEndPoints.Table
```

Kamu bulutta bir depolama hesabı için bu aşağıda döndürür: 

```
blob endpoint = http://myexistingstorageaccount.blob.core.usgovcloudapi.net/
file endpoint = http://myexistingstorageaccount.file.core.usgovcloudapi.net/
queue endpoint = http://myexistingstorageaccount.queue.core.usgovcloudapi.net/
table endpoint = http://myexistingstorageaccount.table.core.usgovcloudapi.net/
```

## <a name="after-setting-the-environment"></a>Ortam ayarladıktan sonra

Burada, depolama hesaplarınızı makalesinde açıklandığı gibi veri düzlemi erişmek için kullanılan aynı PowerShell kullanabileceğiniz ileride gelen [Azure Storage ile Azure PowerShell'i kullanma](storage-powershell-guide-full.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yeni bir kaynak grubu ve bu alıştırma için bir depolama hesabı oluşturduysanız, tüm varlıkları kaynak grubu kaldırarak kaldırabilirsiniz. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturulan depolama hesabı ve kaynak grubu kaldırır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell oturumlar arasında kalıcı kullanıcı oturumu açma](/powershell/azure/context-persistence)
* [Azure kamu depolama](../../azure-government/documentation-government-services-storage.md)
* [Microsoft Azure kamu Geliştirici Kılavuzu](../../azure-government/documentation-government-developer-guide.md)
* [Azure Çin'de uygulamalar için Geliştirici Notları](https://msdn.microsoft.com/library/azure/dn578439.aspx)
* [Azure Almanya belgeleri](../../germany/germany-welcome.md)