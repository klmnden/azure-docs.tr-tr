---
title: Azure PowerShell ile Azure dosya paylaşımlarını yönetme
description: Azure PowerShell kullanarak Azure dosya paylaşımlarını yönetmeyi öğrenin.
services: storage
documentationcenter: ''
author: wmgries
manager: jeconnoc
editor: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: c30bcc293cfe57452844dd74378593c234146802
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-azure-powershell"></a>Azure PowerShell ile Azure dosya paylaşımlarını yönetme 
[Azure Dosyaları](storage-files-introduction.md), Microsoft’un kullanımı kolay bulut dosya sistemidir. Azure dosya paylaşımları, Windows, Linux ve macOS platformlarına bağlanabilir. Bu kılavuzda, PowerShell kullanarak Azure dosya paylaşımlarıyla çalışmanın temel bilgileri gösterilmektedir. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak grubu ve depolama hesabı oluşturma
> * Azure dosya paylaşımı oluşturma 
> * Dizin oluşturma
> * Dosyayı karşıya yükleme 
> * Dosya indirme
> * Paylaşım anlık görüntüsü oluşturma ve kullanma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell’i yerel olarak yükleyip kullanmak istiyorsanız bu kılavuz, Azure PowerShell modülü 5.1.1 veya sonraki bir sürümünü gerektirir. Azure PowerShell modülünün hangi sürümünü çalıştırdığınızı öğrenmek için `Get-Module -ListAvailable AzureRM` komutunu yürütün. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Henüz bir Azure kaynak grubunuz yoksa, [`New-AzureRmResourceGroup`](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet’ini kullanarak yeni bir grup oluşturabilirsiniz. 

Aşağıdaki örnek, Doğu ABD bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Depolama hesabı, Azure dosya paylaşımını veya bloblar ya da sorgular gibi diğer depolama kaynaklarını dağıtmak için kullanabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda paylaşım olabilir; paylaşım da, depolama hesabının kapasite sınırları içinde sınırsız sayıda dosya depolayabilir.

Bu örnek, `mystorageacct<random number>` adlı bir depolama hesabı oluşturur ve **$storageAcct** değişkenine, o depolama hesabına yönelik bir başvuru ekler. Depolama hesabı adları benzersiz olmalıdır; bu nedenle `Get-Random` kullanarak adın sonuna bir sayı ekleyip adı benzersiz hale getirin. 

```azurepowershell-interactive 
$storageAcct = New-AzureRmStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Artık ilk Azure dosya paylaşımınızı oluşturabilirsiniz. [New-AzureStorageShare](/powershell/module/azurerm.storage/new-azurestorageshare) cmdlet’ini kullanarak bir dosya paylaşımı oluşturabilirsiniz. Bu örnek, `myshare` adlı bir paylaşım oluşturur.

```azurepowershell-interactive
New-AzureStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

> [!Important]  
> Paylaşım adları tümüyle küçük harf, sayı ve tek kısa çizgilerden oluşmalı ve kısa çizgiyle başlamamalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Azure dosya paylaşımının içindekileri işleme
Artık Azure dosya paylaşımını oluşturduğunuza göre, [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) veya [macOS](storage-how-to-use-files-mac.md)'ta SMB ile dosya paylaşımını bağlayabilirsiniz. Alternatif olarak, Azure dosya paylaşımınızı PowerShell modülüyle de işleyebilirsiniz. Bu yöntem dosya paylaşımını SMB ile bağlamaktan daha avantajlıdır çünkü PowerShell ile yapılan tüm istekler Dosya REST API ile yapıldığından, dosya paylaşımınızdaki dosyaları ve dizinleri şuralardan oluşturabilir, değiştirebilir ve silebilirsiniz:

- PowerShell Cloud Shell (SMB üzerinden dosya paylaşımlarını bağlayamaz).
- Engeli kaldırılmış 445 numaralı bağlantı noktası olmayan şirket içi istemcileri gibi, SMB paylaşımlarını bağlayamayan istemciler.
- [Azure İşlevleri](../../azure-functions/functions-overview.md) gibi sunucusuz senaryolar. 

### <a name="create-directory"></a>Dizin oluşturma
Azure dosya paylaşımınızın kökünde *myDirectory* adlı yeni bir dizin oluşturmak için [`New-AzureStorageDirectory`](/powershell/module/azurerm.storage/new-azurestoragedirectory) cmdlet’ini kullanın.

```azurepowershell-interactive
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme
[`Set-AzureStorageFileContent`](/powershell/module/azure.storage/set-azurestoragefilecontent) cmdlet’ini kullanarak dosyanın nasıl karşıya yükleneceğini göstermek için, önce karşıya yüklemek üzere PowerShell Cloud Shell'inizin karalama sürücüsünün içinde bir dosya oluşturmalısınız. 

Bu örnek, karalama sürücünüzdeki yeni bir dosyaya geçerli tarih ve saati ekler, sonra dosyayı dosya paylaşımına yükler.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzureStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

PowerShell’i yerel olarak çalıştırıyorsanız, `C:\Users\ContainerAdministrator\CloudDrive\` bölümünün yerine makinenizde var olan bir yol kullanmalısınız.

Dosyayı karşıya yükledikten sonra, [`Get-AzureStorageFile`](/powershell/module/Azure.Storage/Get-AzureStorageFile) cmdlet’ini kullanarak dosyanın Azure dosya paylaşımınıza yüklendiğinden emin olabilirsiniz. 

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

### <a name="download-a-file"></a>Dosya indirme
Cloud Shell'inizin karalama sürücüsüne yüklediğiniz dosyanın bir kopyasını indirmek için [`Get-AzureStorageFileContent`](/powershell/module/azure.storage/get-azurestoragefilecontent) cmdlet’ini kullanabilirsiniz.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item 
    `-Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzureStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Dosyayı indirdikten sonra, `Get-ChildItem` komutunu kullanarak dosyanın PowerShell Cloud Shell’inizin karalama sürücüsüne indirildiğini görebilirsiniz.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

### <a name="copy-files"></a>Dosyaları kopyalama
Yaygın görevlerden biri dosyaları bir dosya paylaşımından diğerine veya Azure Blob depolama kapsayıcısına/kapsayıcısından kopyalamaktır. Bu işlevi göstermek için, yeni bir paylaşım oluşturabilir ve [`Start-AzureStorageFileCopy`](/powershell/module/azure.storage/start-azurestoragefilecopy) cmdlet’ini kullanarak bu yeni paylaşıma yüklediğiniz dosyayı kopyalayabilirsiniz. 

```azurepowershell-interactive
New-AzureStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzureStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzureStorageFileCopy 
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Şimdi, yeni paylaşımdaki dosyaları listelerseniz kopyalanan dosyanızı görmeniz gerekir.

```azurepowershell-interactive
Get-AzureStorageFile -Context $storageAcct.Context -Share "myshare2" -Path "myDirectory2" 
```

`Start-AzureStorageFileCopy` cmdlet’i Azure dosya paylaşımları ve Azure Blob depolama kapsayıcıları arasında plansız dosya taşıma işlemlerinde kullanışlı olsa da, daha büyük taşıma işlemleri (taşınan dosyaların sayısı ve boyutu açısından) için AzCopy kullanılmasını öneririz. [Windows için AzCopy](../common/storage-use-azcopy.md) ve [Linux için AzCopy](../common/storage-use-azcopy-linux.md) hakkında daha fazla bilgi edinin. AzCopy yerel olarak yüklenmelidir; Cloud Shell'de kullanılamaz 

## <a name="create-and-modify-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve değiştirme
Azure dosya paylaşımıyla yerine getirebileceğiniz kullanışlı bir diğer görev de paylaşım anlık görüntüleri oluşturmaktır. Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki durumunu saklar. Paylaşım anlık görüntüleri, aşağıdakiler gibi zaten tanıyor olabileceğiniz işletim sistemi teknolojilerine benzer:
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636)
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri. 

[`Get-AzureStorageShare`](/powershell/module/azure.storage/get-azurestorageshare) cmdlet’iyle alınan bir dosya paylaşımı için PowerShell nesnesinde `Snapshot` yöntemini kullanarak bir paylaşım için paylaşım anlık görüntüsü oluşturabilirsiniz. 

```azurepowershell-interactive
$share = Get-AzureStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Paylaşım anlık görüntülerine göz atma
`Get-AzureStorageFile` cmdlet’inin `-Share` parametresine anlık görüntü başvurusunu (`$snapshot`) geçirerek paylaşım anlık görüntüsünün içeriklerine göz atabilirsiniz.

```azurepowershell-interactive
Get-AzureStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme
Paylaşımınız için aldığınız anlık görüntülerin listesini aşağıdaki komutla görebilirsiniz.

```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Daha önce kullandığımız `Start-AzureStorageFileCopy` komutu kullanarak dosyayı geri yükleyebilirsiniz. Bu hızlı başlangıçta ilk olarak önceden karşıya yüklediğimiz `SampleUpload.txt` dosyamızı silerek anlık görüntüden bu dosyayı geri yükleyeceğiz.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzureStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"

# Restore SampleUpload.txt from the share snapshot
Start-AzureStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
`-Share` parametresine yönelik `$snapshot` başvurusunu içeren değişken ile [`Remove-AzureStorageShare`](/powershell/module/azure.storage/remove-azurestorageshare) cmdlet’ini kullanarak bir paylaşım anlık görüntüsünü silebilirsiniz.

```azurepowershell-interactive
Remove-AzureStorageShare -Share $snapshot
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
İşlem bittiğinde, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. 

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Alternatif olarak kaynakları tek tek de kaldırabilirsiniz:

- Bu hızlı başlangıç için oluşturduğumuz Azure dosya paylaşımlarını kaldırmak için.
```azurepowershell-interactive
Get-AzureStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
    Remove-AzureStorageShare -Context $storageAcct.Context -Name $_.Name
}
```

- Depolama hesabının kendisini kaldırmak için (bu işlem örtülü olarak hem oluşturduğumuz Azure dosya paylaşımlarını hem de oluşturmuş olabileceğiniz Azure Blob depolama kapsayıcısı gibi diğer depolama kaynaklarını kaldırır).
```azurepowershell-interactive
Remove-AzureRmStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
```

## <a name="next-steps"></a>Sonraki adımlar
- [Dosya paylaşımlarını Azure portalıyla yönetme](storage-how-to-use-files-portal.md)
- [Dosya paylaşımlarını Azure CLI ile yönetme](storage-how-to-use-files-cli.md)
- [Depolama Gezgini ile dosya paylaşımlarını yönetme](storage-how-to-use-files-storage-explorer.md)
- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)