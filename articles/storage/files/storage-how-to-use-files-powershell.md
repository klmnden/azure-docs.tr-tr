---
title: "PowerShell kullanarak Azure Dosyaları'nı yönetme | Microsoft Docs"
description: "PowerShell kullanarak Azure Dosyaları'nı yönetmeyi öğrenin."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: f919e1880f709b416867a29de14f1dcc63a165fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-powershell-to-manage-azure-files"></a>PowerShell kullanarak Azure Dosyaları'nı yönetme
Dosya paylaşımları oluşturmak ve yönetmek için Azure PowerShell’i kullanabilirsiniz.

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Azure Storage için PowerShell cmdlet’leri yükleme
PowerShell’i kullanmaya hazırlamak için Azure PowerShell cmdlet’lerini indirin ve yükleyin. Yükleme noktası ve yükleme yönergeleri için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> En güncel Azure PowerShell modülünü indirmeniz ve yüklemeniz veya yükseltmeniz önerilir.
> 
> 

**Başlat**’a tıklayıp **Windows PowerShell** yazarak Azure PowerShell penceresini açın. PowerShell penceresi sizin için Azure Powershell modülünü yükler.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Depolama hesabınız ve anahtarı için bir bağlam oluşturma
Depolama hesabı bağlamını oluşturun. Bağlam, depolama hesabı adını ve hesap anahtarını kapsar. Hesap anahtarını [Azure portalından](https://portal.azure.com) kopyalama yönergeleri için bkz. [Depolama erişim anahtarlarını görüntüleme ve kopyalama](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

`storage-account-name` ve `storage-account-key` değerlerini aşağıdaki örnekte olduğu gibi depolama hesabı adı ve anahtarınız ile değiştirin.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Yeni dosya paylaşımı oluşturma
`logs` adında yeni bir paylaşım oluşturun.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Artık, File Storage’da bir dosya paylaşımınız bulunur. Şimdi, bir dizin ve dosya ekleyeceğiz.

> [!IMPORTANT]
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://msdn.microsoft.com/library/azure/dn167011.aspx)
> 
> 

## <a name="create-a-directory-in-the-file-share"></a>Dosya paylaşımında bir dizin oluşturma
Paylaşımda bir dizin oluşturun. Aşağıdaki örnekte, dizin `CustomLogs` olarak adlandırılmıştır.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a>Dizine yerel bir dosya yükleme
Şimdi, dizine yerel bir doya yükleyin. Aşağıdaki örnekte `C:\temp\Log1.txt` konumundan bir dosya yüklenir. Dosya yolunu yerel makinenizdeki geçerli bir dosyaya işaret edecek şekilde düzenleyin.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a>Dizindeki dosyaları listeleme
Dizindeki dosyaları görmek üzere tüm dizin dosyalarını listeleyebilirsiniz. Bu komut, CustomLogs dizinindeki dosyaları ve alt dizinleri (varsa) döndürür.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile, hangi dizin nesnesi geçiriliyorsa, onun için dosyaların ve dizinlerin bir listesini döndürür. "Get-AzureStorageFile -Share $s" kök dizindeki dosyaların ve dizinlerin bir listesini döndürür. Alt dizindeki dosyaların bir listesini almak için alt dizini Get-AzureStorageFile dizinine geçirmeniz gerekir. Böylece şu işlemleri gerçekleştirmiş olursunuz; komutun kanala kadar olan ilk parçası CustomLogs alt dizininin dizin örneğini döndürür. Daha sonra, CustomLogs alt dizinindeki dosyaları ve dizinleri döndüren Get-AzureStorageFile dizinine geçirilir.

## <a name="copy-files"></a>Dosyaları kopyalama
Azure PowerShell’in 0.9.7 sürümünden başlayarak, bir dosyayı başka bir dosyaya, bir dosyayı başka bir bloba veya bir blobu bir dosyaya kopyalayabilirsiniz. Bu kopyalama işlemlerinin PowerShell cmdlet'leri kullanılarak nasıl yapılacağı aşağıda gösterilmiştir.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

* [SSS](../storage-files-faq.md)
* [Windows’da sorun giderme](storage-troubleshoot-windows-file-connection-problems.md)      
* [Linux’ta sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)    