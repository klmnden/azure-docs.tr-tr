---
title: Azure PowerShell ile Azure dosya paylaşımlarını yönetme hızlı başlangıcı
description: Azure PowerShell kullanarak Azure dosya paylaşımlarını yönetmeyi öğrenmek için bu hızlı başlangıcı kullanın.
services: storage
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 10/26/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 439a1c60942b1540328bf9972d74d7dd4d573a65
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64700640"
---
# <a name="quickstart-create-and-manage-an-azure-file-share-with-azure-powershell"></a>Hızlı Başlangıç: Oluşturma ve Azure PowerShell ile bir Azure dosya paylaşımı yönetme 
Bu kılavuzda, PowerShell kullanarak [Azure dosya paylaşımlarıyla](storage-files-introduction.md) çalışmanın temel bilgileri gösterilmektedir. Azure dosya paylaşımları diğer dosya paylaşımları gibidir, ancak bulutta depolanır ve Azure platformu tarafından desteklenir. Azure dosya paylaşımları endüstri standardı SMB protokolünü destekler ve birden çok makine, uygulama ve örnek arasında dosya paylaşmayı olanaklı kılar. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı istiyorsanız, bu kılavuzda, Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Azure PowerShell modülünün hangi sürümünü çalıştırdığınızı öğrenmek için `Get-Module -ListAvailable Az` komutunu yürütün. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure hesabınızda oturum açmak için `Login-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir Azure kaynak grubu zaten sahip değilseniz, ile yeni bir tane oluşturabilirsiniz [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet'i. 

Aşağıdaki örnek, Doğu ABD bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzResourceGroup `
    -Name myResourceGroup `
    -Location EastUS
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Depolama hesabı, Azure dosya paylaşımını veya bloblar ya da sorgular gibi diğer depolama kaynaklarını dağıtmak için kullanabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda paylaşım olabilir; paylaşım da, depolama hesabının kapasite sınırları içinde sınırsız sayıda dosya depolayabilir.

Bu örnek, bir depolama hesabı kullanarak oluşturur [yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) cmdlet'i. Depolama hesabı adı *mystorageaccount\<rastgele sayı >* ve bu depolama hesabı için bir başvuru değişkeninde depolanan **$storageAcct**. Depolama hesabı adları benzersiz olmalıdır; bu nedenle `Get-Random` kullanarak adın sonuna bir sayı ekleyip adı benzersiz hale getirin. 

```azurepowershell-interactive 
$storageAcct = New-AzStorageAccount `
                  -ResourceGroupName "myResourceGroup" `
                  -Name "mystorageacct$(Get-Random)" `
                  -Location eastus `
                  -SkuName Standard_LRS 
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Artık ilk Azure dosya paylaşımınızı oluşturabilirsiniz. Kullanarak bir dosya paylaşımı oluşturabilirsiniz [yeni AzStorageShare](/powershell/module/az.storage/New-AzStorageShare) cmdlet'i. Bu örnek, `myshare` adlı bir paylaşım oluşturur.

```azurepowershell-interactive
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

Paylaşım adları tümüyle küçük harf, sayı ve tek kısa çizgilerden oluşmalı ve kısa çizgiyle başlamamalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)

## <a name="use-your-azure-file-share"></a>Azure dosya paylaşımınızı kullanma
Azure Dosyaları, Azure dosya paylaşımınızdaki dosya ve klasörler ile çalışmak için iki yöntem sunar: sektör standardı [Sunucu İleti Bloğu (SMB) protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) ve [Dosya REST protokolü](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api). 

Bir dosya paylaşımını SMB ile bağlayabilmeniz için işletim sisteminize göre aşağıdaki belgeye bakın:
- [Windows](storage-how-to-use-files-windows.md)
- [Linux](storage-how-to-use-files-linux.md)
- [macOS](storage-how-to-use-files-mac.md)

### <a name="using-an-azure-file-share-with-the-file-rest-protocol"></a>Dosya REST protokolü ile bir Azure dosya paylaşımını kullanma 
Dosya REST protokolü ile doğrudan olası çalışma doğrudan olduğu (yani REST HTTP handcrafting çağrıları kendiniz), ancak dosya REST protokolü kullanmak için en yaygın yolu, Azure PowerShell modülü kullanmaktır [Azure CLI](storage-how-to-use-files-cli.md), veya bir Azure Depolama SDK'sı, her biri kendi tercih ettiğiniz betik programlama dilinde dosya REST Protokolü çevresinde güzel bir sarmalayıcı sağlar.  

Kullanabilmeyi umduğunuz uygulama ve araçlarını kullanmanıza izin vereceği için çoğu durumda Azure dosya paylaşımları ile SMP protokolü üzerinden çalışmanızı bekliyoruz, ancak SMB yerine Dosya REST API'si kullanmanın aşağıdaki gibi bazı avantajları bulunmaktadır:

- Dosya paylaşımınıza (SMB üzerinden dosya paylaşımı bağlayamayan) PowerShell Cloud Shell'den göz atıyorsanız.
- SMB paylaşımlarını bağlayamayan istemcilerden; örneğin 445 numaralı bağlantı noktası engeli kaldırılmamış şirket içi bir istemciden bir betik veya uygulama yürütmeniz gerekiyorsa.
- [Azure İşlevleri](../../azure-functions/functions-overview.md) gibi sunucusuz kaynaklardan yararlanıyorsanız. 

Aşağıdaki örnekler, Azure PowerShell modülünü Azure dosya paylaşımınıza dosya REST protokolü ile işlemek için nasıl kullanılacağını gösterir. 

#### <a name="create-directory"></a>Dizin oluşturma
Adlı yeni bir dizin oluşturmak için *myDirectory* Azure dosya paylaşımınızın kökünde kullanın [yeni AzStorageDirectory](/powershell/module/az.storage/New-AzStorageDirectory) cmdlet'i.

```azurepowershell-interactive
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Path "myDirectory"
```

#### <a name="upload-a-file"></a>Dosyayı karşıya yükleme
Kullanarak bir dosyayı karşıya yükleme işlemini göstermek için [kümesi AzStorageFileContent](/powershell/module/az.storage/Set-AzStorageFileContent) cmdlet'i, biz önce karşıya yüklemek üzere PowerShell Cloud Shell'inizin karalama sürücüsünün içinde bir dosya oluşturmanız gerekir. 

Bu örnek, karalama sürücünüzdeki yeni bir dosyaya geçerli tarih ve saati ekler, sonra dosyayı dosya paylaşımına yükler.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
Get-Date | Out-File -FilePath "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName "myshare" `
   -Source "C:\Users\ContainerAdministrator\CloudDrive\SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

PowerShell’i yerel olarak çalıştırıyorsanız, `C:\Users\ContainerAdministrator\CloudDrive\` bölümünün yerine makinenizde var olan bir yol kullanmalısınız.

Kullanabileceğiniz bir dosyayı karşıya yükledikten sonra [Get-AzStorageFile](/powershell/module/Az.Storage/Get-AzStorageFile) cmdlet'ini kullanarak dosyanın Azure dosya paylaşımınızı yüklenen emin olun. 

```azurepowershell-interactive
Get-AzStorageFile -Context $storageAcct.Context -ShareName "myshare" -Path "myDirectory" 
```

#### <a name="download-a-file"></a>Dosya indirme
Kullanabileceğiniz [Get-AzStorageFileContent](/powershell/module/az.storage/Get-AzStorageFilecontent) cmdlet'ini yüklediğiniz Cloud Shell'inizin karalama sürücüsüne için dosyanın bir kopyasını indirin.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item `
     -Path "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt" `
     -Force `
     -ErrorAction SilentlyContinue

Get-AzStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt" ` 
    -Destination "C:\Users\ContainerAdministrator\CloudDrive\SampleDownload.txt"
```

Dosyayı indirdikten sonra, `Get-ChildItem` komutunu kullanarak dosyanın PowerShell Cloud Shell’inizin karalama sürücüsüne indirildiğini görebilirsiniz.

```azurepowershell-interactive
Get-ChildItem -Path "C:\Users\ContainerAdministrator\CloudDrive"
``` 

#### <a name="copy-files"></a>Dosyaları kopyalama
Yaygın görevlerden biri dosyaları bir dosya paylaşımından diğerine veya Azure Blob depolama kapsayıcısına/kapsayıcısından kopyalamaktır. Bu işlevi göstermek için yeni bir paylaşım oluşturabilir ve yüklediğiniz yeni paylaşımı kullanarak bu dosyayı kopyalama [başlangıç AzStorageFileCopy](/powershell/module/az.storage/Start-AzStorageFileCopy) cmdlet'i. 

```azurepowershell-interactive
New-AzStorageShare `
    -Name "myshare2" `
    -Context $storageAcct.Context
  
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName "myshare2" `
   -Path "myDirectory2"

Start-AzStorageFileCopy `
    -Context $storageAcct.Context `
    -SrcShareName "myshare" `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestShareName "myshare2" `
    -DestFilePath "myDirectory2\SampleCopy.txt" `
    -DestContext $storageAcct.Context
```

Şimdi, yeni paylaşımdaki dosyaları listelerseniz kopyalanan dosyanızı görmeniz gerekir.

```azurepowershell-interactive
Get-AzStorageFile -Context $storageAcct.Context -ShareName "myshare2" -Path "myDirectory2" 
```

Sırada `Start-AzStorageFileCopy` cmdlet, Azure dosya paylaşımları ve Azure Blob Depolama kapsayıcıları arasında geçici dosyayı taşır için kullanışlıdır, daha büyük taşır (sayısı veya taşınan dosyaların boyutu açısından) için AzCopy kullanılmasını öneririz. [Windows için AzCopy](../common/storage-use-azcopy.md) ve [Linux için AzCopy](../common/storage-use-azcopy-linux.md) hakkında daha fazla bilgi edinin. AzCopy yerel olarak yüklenmelidir; Cloud Shell'de kullanılamaz. 

## <a name="create-and-manage-share-snapshots"></a>Paylaşım anlık görüntülerini oluşturma ve yönetme
Azure dosya paylaşımıyla yerine getirebileceğiniz kullanışlı bir diğer görev de paylaşım anlık görüntüleri oluşturmaktır. Anlık görüntü, Azure dosya paylaşımının zamanın bir noktasındaki durumunu saklar. Paylaşım anlık görüntüleri, aşağıdakiler gibi zaten tanıyor olabileceğiniz işletim sistemi teknolojilerine benzer:
- NTFS ve ReFS gibi Windows dosya sistemleri için [Birim Gölge Kopyası Hizmeti (VSS)](https://docs.microsoft.com/windows/desktop/VSS/volume-shadow-copy-service-portal)
- Linux sistemleri için [Mantıksal Birim Yöneticisi (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) anlık görüntüleri
- macOS için [Apple Dosya Sistemi (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) anlık görüntüleri. 
 Kullanarak bir paylaşım için paylaşım anlık görüntüsü oluşturabilirsiniz `Snapshot` alınan yöntemi PowerShell nesnesinde bir dosya paylaşımı için [Get-AzStorageShare](/powershell/module/az.storage/Get-AzStorageShare) cmdlet'i. 

```azurepowershell-interactive
$share = Get-AzStorageShare -Context $storageAcct.Context -Name "myshare"
$snapshot = $share.Snapshot()
```

### <a name="browse-share-snapshots"></a>Paylaşım anlık görüntülerine göz atma
`Get-AzStorageFile` cmdlet’inin `-Share` parametresine anlık görüntü başvurusunu (`$snapshot`) geçirerek paylaşım anlık görüntüsünün içeriklerine göz atabilirsiniz.

```azurepowershell-interactive
Get-AzStorageFile -Share $snapshot
```

### <a name="list-share-snapshots"></a>Paylaşım anlık görüntülerini listeleme
Paylaşımınız için aldığınız anlık görüntülerin listesini aşağıdaki komutla görebilirsiniz.

```azurepowershell-interactive
Get-AzStorageShare -Context $storageAcct.Context | Where-Object { $_.Name -eq "myshare" -and $_.IsSnapshot -eq $true }
```

### <a name="restore-from-a-share-snapshot"></a>Paylaşım anlık görüntüsünden geri yükleme
Daha önce kullandığımız `Start-AzStorageFileCopy` komutu kullanarak dosyayı geri yükleyebilirsiniz. Bu hızlı başlangıçta ilk olarak önceden karşıya yüklediğimiz `SampleUpload.txt` dosyamızı silerek anlık görüntüden bu dosyayı geri yükleyeceğiz.

```azurepowershell-interactive
# Delete SampleUpload.txt
Remove-AzStorageFile `
    -Context $storageAcct.Context `
    -ShareName "myshare" `
    -Path "myDirectory\SampleUpload.txt"
 # Restore SampleUpload.txt from the share snapshot
Start-AzStorageFileCopy `
    -SrcShare $snapshot `
    -SrcFilePath "myDirectory\SampleUpload.txt" `
    -DestContext $storageAcct.Context `
    -DestShareName "myshare" `
    -DestFilePath "myDirectory\SampleUpload.txt"
```

### <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme
Kullanarak bir paylaşım anlık görüntüsünü silebilirsiniz [Remove-AzStorageShare](/powershell/module/az.storage/Remove-AzStorageShare) içeren değişken ile cmdlet'ini `$snapshot` başvurusu `-Share` parametresi.

```azurepowershell-interactive
Remove-AzStorageShare -Share $snapshot
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
İşiniz bittiğinde, kullanabileceğiniz [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. 

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

Alternatif olarak kaynakları tek tek de kaldırabilirsiniz:

- Bu hızlı başlangıç için oluşturduğumuz Azure dosya paylaşımlarını kaldırmak için.

    ```azurepowershell-interactive
    Get-AzStorageShare -Context $storageAcct.Context | Where-Object { $_.IsSnapshot -eq $false } | ForEach-Object { 
        Remove-AzStorageShare -Context $storageAcct.Context -Name $_.Name
    }
    ```

- Depolama hesabının kendisini kaldırmak için (bu işlem örtülü olarak hem oluşturduğumuz Azure dosya paylaşımlarını hem de oluşturmuş olabileceğiniz Azure Blob depolama kapsayıcısı gibi diğer depolama kaynaklarını kaldırır).

    ```azurepowershell-interactive
    Remove-AzStorageAccount -ResourceGroupName $storageAcct.ResourceGroupName -Name $storageAcct.StorageAccountName
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Dosyaları nedir?](storage-files-introduction.md)
