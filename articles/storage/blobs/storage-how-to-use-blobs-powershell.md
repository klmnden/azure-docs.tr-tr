---
title: "PowerShell ile Azure Blob storage (nesne depolama) işlemleri | Microsoft Docs"
description: "Öğretici - PowerShell ile Azure Blob storage (nesne depolama) işlemleri"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: robinsh
ms.openlocfilehash: 170c3091efc90f640792682377ed10e2eab0cab3
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="perform-azure-blob-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure Blob Depolama işlemleri

Azure Blob Storage; HTTP veya HTTPS aracılığıyla dünyanın her yerinde erişilebilen metin veya ikili veriler gibi büyük miktarda yapılandırılmamış nesne verilerinin depolanması için bir hizmettir. Bu makalede, Azure Blob depolamada karşıya yükleme, indirme ve BLOB'ları silme gibi temel işlemleri yer almaktadır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir kapsayıcı oluşturma 
> * BLOB karşıya yükleme
> * Blob’ları bir kapsayıcıda listeleme 
> * Blob’ları indirme
> * BLOB kopyalama
> * Blob’ları silme
> * Görüntüleyin ve bir blob'un meta verileri ve özelliklerini ayarlama
> * Paylaşılan erişim imzaları kullanarak güvenliği yönetme

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bloblar her zaman bir kapsayıcıya yüklenir. Kapsayıcılar, bilgisayarınızda dosyalarınızı klasörlerde düzenleme gibi gruplar kapsayıcılarında BLOB düzenlemek sağlayarak, bilgisayarınızdaki dizinleri benzerdir. Bir depolama hesabı herhangi bir sayıda kapsayıcı olabilir; yalnızca depolama hesabında kapladığı alanı miktarıyla sınırlıdır (500 TB'ye kadar). 

Bir kapsayıcı oluşturduğunuzda, bu kapsayıcıdaki blobları erişebilecek kişileri tanımlamasına yardımcı olur erişim düzeyi ayarlayabilirsiniz. Örneğin, bunlar özel olabilir (erişim düzeyi = `Off`), hiç kimsenin anlamı erişebilmeniz paylaşılan erişim imzası veya depolama hesabı için erişim anahtarları olmadan. Kapsayıcı oluşturduğunuzda, erişim düzeyi belirtmezseniz, özel varsayar.

Genel olarak erişilebilir olmasını kapsayıcısında görüntüler isteyebilirsiniz. Web sitenizde görüntülenecek görüntüleri saklamak isterseniz, örneğin, herkese açık olmasını istediğiniz. Bu durumda, kapsayıcı erişim düzeyi için ayarladığınız `blob`, ve herkes bu kapsayıcıda blob işaret eden bir URL ile blob erişebilirsiniz.

Kapsayıcı oluşturmak için kapsayıcı adı ayarlayın, sonra blob' to' izinleri ayarlama kapsayıcı oluşturun. Kapsayıcı adları bir harf veya sayı ile başlamalı ve yalnızca harf, rakam ve kısa çizgi karakteri (-) içerebilir. BLOB'ları ve kapsayıcı adlandırma hakkında daha fazla kural için lütfen bkz. [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

İle yeni bir kapsayıcı oluşturmak [yeni AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer). Ortak erişim düzeyini ayarlayın. Bu örnek kapsayıcı adı *howtoblobs*.

```powershell
$containerName = "howtoblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-into-a-container"></a>BLOB'ları bir kapsayıcıya karşıya yükle

Azure Blob Storage blok blobları destekler, blobları ve sayfa bloblarını Ekle.  IaaS VM’lerini yedeklemek için kullanılan VHD dosyaları sayfa bloblarıdır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan çoğu dosya blok blobudur. 

Bir dosyayı bir blok blobuna yüklemek için, bir kapsayıcı başvurusu alın ve bu kapsayıcıdaki blok blobuna bir başvuru alın. Blob başvurusunu aldıktan sonra, [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) kullanarak verileri karşıya yükleyebilirsiniz. Bu işlem yok veya zaten mevcut değilse bu raporun üzerine blob oluşturur.

Bir kapsayıcıya bir blob karşıya nasıl yükleneceğini gösterir. İlk olarak, burada dosyaları bulunur ve karşıya yüklenecek dosyasının adı için bir değişken ayarlamak yerel makinede dizine işaret değişkenleri ayarlayın. Bu, aynı işlemi art arda gerçekleştirmek istediğinizde yararlıdır. Birden çok giriş BLOB kapsayıcısında listelerken görebilmek için birkaç dosyaları karşıya yükleyin.

Aşağıdaki örnekler Image001.jpg ve Image002.png D: yük\\oluşturduğunuz kapsayıcısı yerel diskteki _TestImages.

```powershell
$localFileDirectory = "D:\_TestImages\"

$blobName = "Image001.jpg"
$localFile = $localFileDirectory + $blobName
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx 
```

Başka bir dosyayı karşıya yükleyin. 

```powershell
$blobName = "Image002.png"
$localFile = $localFileDirectory + $blobName 
Set-AzureStorageBlobContent -File $localFile `
  -Container $containerName `
  -Blob $blobName `
  -Context $ctx
```

Devam etmeden önce istediğiniz sayıda dosyayı karşıya yükleyin.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

BLOB'ları kullanarak kapsayıcı listesini almak [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) ve görüntülenecek blob adı seçme.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Blob’ları indirme

Blobları yerel diskinize indirin. İlk olarak, blobları yüklemek istediğiniz yerel klasöre işaret eden bir değişken ayarlayın. Sonra her blob indirilmesi çağrısı ve adını ayarlayın [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) blob indirmek için.

Bu örnek BLOB D: kopyalar\\yerel diskteki _TestImages\Downloads. 

```powershell
# local directory to which to download the files
# example "D:\_TestImages\Downloads\"
$localTargetDirectory = "D:\_TestImages\Downloads\"

# download the first blob
$blobName = "Image001.jpg"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 

# download another blob
$blobName = "Image002.png"
Get-AzureStorageBlobContent -Blob $blobName `
  -Container $containerName `
  -Destination $localTargetDirectory `
  -Context $ctx 
```

## <a name="copy-blobs"></a>BLOB kopyalama

BLOB'ları kopyalarken dikkate alınması gereken birkaç nokta vardır. Yalnızca blob aynı konumda yeni bir ad içine kopyaladığınız. İçin ayrı bir depolama hesabı blob kopyalama. Farklı bir depolama hesabı için (örneğin, bir VHD dosyası) büyük bir blob kopyalama. Bunlar her farklı şekilde yapılır.

### <a name="simple-blob-copy"></a>Basit blob kopyalama

Yeni bir blob'a kopyalayarak bir kapsayıcıda BLOB bir kopyasını yapabilirsiniz. Bu örnek farklı bir ad ile aynı kapsayıcı içine kopyalayacak, ancak yalnızca kolayca olarak başka bir kapsayıcı oluşturun ve bunun yerine var kopyalayın. Bu örnek nasıl kullanılacağını gösterir [başlangıç AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) blob kopyalanamadı. Kaynak ve hedef blob adı hem kapsayıcı adı belirtmeniz gerekir.

```powershell
$blobName = "Image002.png"
$newBlobName = "CopyOf_" + $blobName 

Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName `
  -DestBlob $newBlobName `
  -Context $ctx 

# get list of blobs to verify the new one has been created
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

### <a name="copy-a-blob-to-a-different-storage-account"></a>Farklı bir depolama hesabı için bir blob kopyalama 

Bir blob ayrı depolama alanına kopyalanmaya isteyebilirsiniz. Bunu yapmak için bir örnek Vm'leriniz birini yedeklemeniz için farklı bir depolama hesabı için yedekleyen bir VHD dosyasının kopyalamaktır. 

İkinci bir depolama hesabı ayarlamanız, bağlamı alma, bir kapsayıcıda bu depolama hesabı ayarlama ve kopya gerçekleştirin. Komut dosyasının bu bölümü, ilk yerine ikinci depolama hesabıyla dışında betik neredeyse aynıdır.

```powershell
#create new storage account, get context 
$storageAccount2Name = "blobstutorialtestcopy"
$storageAccount2 = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccount2Name `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx2 = $storageAccount2.Context

#create a container in the second storage account 
$containerName2 = "tutorialblobscopied"
New-AzureStorageContainer -Name $containerName2 `
  -Context $ctx2 `
  -Permission blob

# copy one of the blobs from the first storage account to the second one 
# specify the source and destination container, blob name, and context
Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -SrcContext $ctx `
  -DestContext $ctx2 

# list the blobs in the container in the second storage account
# to verify that the newly copied blob is there
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

### <a name="copying-very-large-blobs-asynchronously"></a>Çok büyük BLOB'lar zaman uyumsuz olarak kopyalama

Birden çok GB'dir çok büyük bir blob kopyalıyorsanız zaman uyumsuz kopyasının, başlatma ve geri gelmelerini ve işlemi tamamlanana kadar durumunu denetleme yararlanabilirsiniz. Kopyalama işlemi gerçekleştirirken bu şekilde, bağlıdır değildir.

Bir depolama hesabında kopyalıyorsanız çok hızlı bir kopyasıdır. İki depolama hesabı aynı bölgede arasında kopyalıyorsanız oldukça hızlı bir kopyasıdır. Ancak, kaynak ve hedef ayrı bölgelerde bulunuyorsa, uzaklığı ve dosyanızın boyutunu bağlı olarak kopya tamamlanması birkaç saat sürebilir. 

Bu komut son kopyalama örnekte tanımlanan aynı değişkenleri kullanır. Bu bir kopya durumunu yakalar ve art arda işlemi tamamlanana kadar her 10 saniyede sorgular farktır. Çok büyük bir blob tamamlamak için birden fazla yineleme ele görmek için depolama hesabınıza yüklemeniz gerekebilir.

Kullanım [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/get-azurestorageblobcopystate) kopyalama durumu sorgulanamıyor. 

```powershell
# start the blob copy, and assign the result to $blobResult
$blobResult = Start-AzureStorageBlobCopy -SrcBlob $blobName `
  -SrcContainer $containerName `
  -DestContainer $containerName2 `
  -DestBlob $blobName `
  -Context $ctx `
  -DestContext $ctx2

# get the status of the blob copy
$status = $blobResult | Get-AzureStorageBlobCopyState 
# show the value of the status
$status 

# loop until it finishes copying, pausing for 10 seconds after each iteration
# it is finished when the status is no longer 'Pending'
while ($status.Status -eq "Pending") {
    $status = $blobResult | Get-AzureStorageBlobCopyState 
    $status
    Start-Sleep 10
}

# get the list of blobs to see the new file in the second storage account 
Get-AzureStorageBlob -Container $containerName2 -Context $ctx2 | select Name 
```

## <a name="delete-blobs"></a>Blob’ları silme

BLOB Depolama hesabından kaldırmak için kullanın [Kaldır AzureStorageBlob](/powershell/module/azure.storage/Remove-AzureStorageBlob). 

```powershell
$blobName = "Image001.jpg"
Remove-AzureStorageBlob -Blob $blobName -Container $containerName -Context $ctx

# get list of blobs to see the one you deleted is gone
Get-AzureStorageBlob -Container $containerName -Context $ctx | select Name 
```

## <a name="read-and-write-a-blobs-properties-and-metadata"></a>Bir blob'un özellikleri ve meta veri okuma ve yazma

Özellik ve yöntem kümesine erişmek için **Ilistblobıtem**, onu cast (veya gerekir dönüştürmeden) için bir **CloudBlockBlob**, **CloudPageBlob**, veya **CloudBlobDirectory** nesnesi. Tür bilinmiyorsa, hangisine yayınlayacağınızı belirlemek için bir tür denetimi kullanabilirsiniz. Aşağıdaki kodda almak ve daha önce kullanarak karşıya BLOB'ları birinin özelliklerini çıkış gösterilmiştir [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) ve CloudBlockBlob nesne sonucu atama.

```powershell
# blob properties
# get a reference to the blob you uploaded -- this is an **IListBlobItem** -- then
# convert it to a CloudBlockBlob, giving you access to the properties 
# and methods of the blob 
$blobName = "Image001.jpg"
$blob = Get-AzureStorageBlob -Context $ctx `
   -Container $containerName `
   -Blob $blobName 

#convert $blob to a CloudBlockBlob object
$cloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $blob.ICloudBlob

# you can view the properties by typing in $cloudBlockBlob 
#   and hitting the period key; this brings up IntelliSense,
#   which shows you all of the available properties
Write-Host "blob type = " $cloudBlockBlob.BlobType
Write-Host "blob name = " $cloudBlockBlob.Name
Write-Host "blob uri = " $cloudBLockBlob.Uri
```

Sistem özellikleri FetchAttributes çağırarak doldurmak sonra bu özelliklerini görüntüleyin. Bazı özelliklerin yazılabilir ve değerlerini değiştirebilirsiniz. Bu örnek olarak da bilinen MIME türü içerik türünün nasıl değiştirileceğini gösterir.

```powershell
# populate the system properties
$cloudBlockBlob.FetchAttributes()

# view some of the system properties
# contentType is commonly referred to as MIME type
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
Write-Host "size = " $cloudBlockBlob.Properties.Length 

# change the content type to image/png
$contentType = "image/jpg"
$cloudBlockBlob.Properties.ContentType = $contentType 
$cloudBlockBlob.SetProperties() 

# get the system properties again to show that the content type has changed
$cloudBlockBlob.FetchAttributes()
Write-Host "content type = " $cloudBlockBlob.Properties.ContentType
```

Her bir blob ayarlayabileceğiniz kendi meta verileri içeriyor. Örneğin, blob veya tarih/saat önce karşıya yüklenen kullanıcı adını depolayabilir. Meta verileri anahtar/değer çiftlerinden oluşur. Bu, PowerShell gibi kullanılarak değiştirilebilir.

```powershell
# "filename" is the key, "original file name" is the value
$cloudBlockBlob.Metadata["filename"] = "original file name"

# "owner" is the key, "RobinS" is the value
$cloudBlockBlob.Metadata["owner"] = "RobinS"

# save the metadata and then display it
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata

# clear the metadata, save it, and show it
$cloudBlockBlob.Metadata.Clear()
$cloudBlockBlob.SetMetadata()
$cloudBlockBlob.Metadata
```

## <a name="managing-security-for-blobs"></a>Blobların güvenliğini sağlama 

Varsayılan olarak, Azure Storage verilerinizin depolama hesabı erişim anahtarlarını elinde olan hesap sahibi erişimi kısıtlayarak güvende tutar. Depolama hesabınızda blob verileri paylaşmanız gerektiğinde bunu hesap erişim anahtarlarınızın güvenliğini tehlikeye atmadan yapmak önem taşır. Bunu yapmak için sorgu parametrelerini ve belirli bir süre için izni belirli bir düzeyde izin veren bir güvenlik belirteci içeren varlık için bir URL olduğu bir paylaşılan erişim imzası URL'SİNİN, kullanabilirsiniz. Örneğin, birisi görüntüleyebilmeniz için okuma erişimi için özel bir blob için 5 dakika verin. isteyebilirsiniz. 

### <a name="set-the-access-level-of-the-container-and-its-blobs-to-private"></a>Kapsayıcı ve bloblarını erişim düzeyini özel ayarlayın

İlk olarak, kapsayıcıya erişim düzeyini ayarlamak `Off`, paylaşılan erişim imzası veya hesap anahtarı olmadan erişim yok olduğunu belirler. Kullanım [kümesi AzureStorageContainerAcl](/powershell/module/azure.storage/Set-AzureStorageContainerAcl).

```powershell
Set-AzureStorageContainerAcl -Name $containerName -Context $ctx -Permission Off 
```

### <a name="test-private-access"></a>Test özel erişim

Bu kapsayıcıda blob yok erişimi olduğunu doğrulamak için BLOB paylaşılan erişim imzası olmadan bir URL oluşturur ve blob görüntülemeyi deneyin. URL HTTPS protokolünü kullanarak şu biçimde olacaktır:

    `https://storageaccountname.blob.core.windows.net/containername/blobname`

Bu blob URL'si oluşturulacağını gösterir.

```powershell
$blobUrl = "https://" `
  $storageAccountName ".blob.core.windows.net/" + `
  $containerName +  "/" $blobName

Write-Host "Blob URL = " $blobUrl 
```

Blob URL'yi kopyalayın ve bir özel tarayıcı penceresine yapıştırın--blob özel olduğundan ve bir paylaşılan erişim imzası dahil edilmeyen bir yetkilendirme hata gösterir. 

### <a name="create-the-sas-uri"></a>SAS URI'sini oluşturma

Bir SAS URI'sini oluşturma--SA'ları olun güvenlik belirteci ve sorgu parametreleri de dahil olmak üzere blob bağlantıdır. Bu URI özel bir tarayıcı penceresine yapıştırın--görüntüyü gösterir. 

İlk başlangıç tarihi/saati ve süre sonu tarihi/saati erişim oluşturun. Bu, 2 dakikalık kullanır. 

```powershell
# set the start time to the current date/time 
# set the end time to 2 minutes in the future
$StartTime = Get-Date
$EndTime = $StartTime.AddMinutes(2.0)
```

SAS kullanarak URI oluşturmak [yeni AzureStorageBlobSASToken](/powershell/module/azure.storage/new-azurestorageblobsastoken). Gereksinim duyduğunuz kapsayıcı adı, blob adı, depolama hesabı bağlamını, (Bu durumda, okuma, yazma ve silme) verilebilmesi için izinler ve erişim için başlangıç ve bitiş saati. 

```powershell
$SASURI = New-AzureStorageBlobSASToken -Container $containerName `
    -Blob $blobName `
    -Context $ctx `
    -Permission "rwd" `
    -StartTime $StartTime `
    -ExpiryTime $EndTime `
    -FullUri

Write-Host "URL with SAS = " $SASURI
```

SAS URI'sini kopyalayın ve bir özel tarayıcı penceresinde yerleştirin; görüntüyü gösterir.

(Bu örnekte 2 dakika) sona sonra URL başka bir özel tarayıcı penceresine yapıştırın URL yetecek kadar uzun süre bekleyin. Bu süre bir Yetkilendirme hatası alırsınız ve SAS URI'sini süresi dolduğundan resmi göstermeyecektir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tüm oluşturduğunuz ve varlıkları kaldırın. Bunu yapmak için Grup içinde bulunan tüm kaynaklar da siler kaynak grubunu kaldırabilirsiniz. Bu durumda, tüm depolama hesapları ve kaynak grubu kaldırır.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel Blob Depolama Yönetimi için nasıl gibi hakkında öğrenilen:

> [!div class="checklist"]
> * Bir kapsayıcı oluşturma 
> * BLOB karşıya yükleme
> * Blob’ları bir kapsayıcıda listeleme 
> * Blob’ları indirme
> * BLOB kopyalama
> * Blob’ları silme
> * Bir blob'un meta verileri ve özellikleri okuma ve yazma
> * Paylaşılan erişim imzaları kullanarak güvenliği yönetme

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell depolama cmdlet'leri
* [Depolama PowerShell cmdlet’leri](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
