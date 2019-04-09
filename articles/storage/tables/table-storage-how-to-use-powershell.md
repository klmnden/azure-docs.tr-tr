---
title: PowerShell ile Azure tablo depolama işlemleri | Microsoft Docs
description: PowerShell ile Azure tablo depolama işlemleri gerçekleştirin.
services: cosmos-db
author: roygara
ms.service: cosmos-db
ms.topic: article
ms.date: 04/05/2019
ms.author: rogarana
ms.subservice: cosmosdb-table
ms.openlocfilehash: 840c2793928816c6346e2039a38678585f8e0bc7
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273133"
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure tablo depolama işlemleri 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Azure tablo depolama, depolama ve yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini sorgulamak için kullanabileceğiniz bir NoSQL veri deposu. Hizmet ana bileşenleri tabloları ve varlıkları Özellikleri ' dir. Bir varlık koleksiyonunu bir tablodur. Bir varlık özellikleri kümesidir. Her varlığın tüm ad-değer çiftleridir en çok 252 özellik olabilir. Bu makalede, zaten Azure tablo depolama hizmeti kavramlarına alışık olduğunuz varsayılır. Ayrıntılı bilgi için bkz. [tablo hizmeti veri modelini anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) ve [.NET kullanarak Azure tablo depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md).

Bu nasıl yapılır makalesi, yaygın Azure tablo depolama işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlık ekleme
> * Tablo sorgulama
> * Tablo varlıklarını silme
> * Bir tablo silme

Bu nasıl yapılır makalesi işiniz bittiğinde, kolayca, bu nedenle yeni bir kaynak grubunda yeni bir Azure depolama hesabı oluşturma işlemini gösterir. Bunun yerine mevcut bir depolama hesabını kullanmayı tercih ediyorsanız, bunun yerine bunu yapabilirsiniz.

Az PowerShell modülleri örneği gerektirir `Az.Storage (1.1.0 or greater)` ve `Az.Resources (1.2.0 or greater)`. Bir PowerShell penceresinde çalıştırın `Get-Module -ListAvailable Az*` sürümü bulmak için. Hiçbir şey görüntülenmez veya yükseltme için ihtiyacınız [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Powershell'den Azure bu özelliği kullanarak olması gerekir `Az` Modülü yüklü. Geçerli sürümü `AzTable` eski AzureRM modülü ile uyumlu değil.
> İzleyin [son yükleme Az modülünü yükleme yönergelerini](/powershell/azure/install-az-ps) gerekirse.

Azure PowerShell yüklenmiş veya güncelleştirildikten sonra modülünü yükleme **AzTable**, bunları yönetmek için komutlar vardır. Bu modülü yüklemek için PowerShell'i yönetici olarak çalıştırıp kullanım çalıştırmak **Install-Module** komutu.

> [!IMPORTANT]
> Biz yine de yayımlanıyor eski adla aynı bu modül için modülü adı uyumluluk nedenlerinden `AzureRmStorageTables` PowerShell galerisinde. Bu belge, yeni adı yalnızca başvurur.

```powershell
Install-Module AzTable
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Add-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Add-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Konumların listesini alma

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örneklerde **eastus**. Bu değer bir değişkende Store **konumu** gelecekte kullanım için.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adı, gelecekte kullanım için bir değişkende Store. Bu örnekte, adlı bir kaynak grubu *pshtablesrg* oluşturulur *eastus* bölge.

```powershell
$resourceGroup = "pshtablesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

Yerel olarak yedekli depolama (LRS) kullanarak bir genel amaçlı standart depolama hesabı oluşturma [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount). Benzersiz bir depolama hesabı adı belirttiğinizden emin olun. Ardından, depolama hesabını temsil eden bağlamını alın. Bir depolama hesabı üzerinde hareket ederken, kimlik bilgilerinizi tekrar tekrar sağlamak yerine başvurabilirsiniz.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

Bir tablo oluşturmak için kullanın [yeni AzStorageTable](/powershell/module/az.storage/New-AzStorageTable) cmdlet'i. Bu örnekte, tablo olarak adlandırılan `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Depolama hesabındaki tabloların listesini alma

Depolama hesabı kullanarak Tablo listesi alma [Get-AzStorageTable](/powershell/module/az.storage/Get-AzureStorageTable).

```powershell
Get-AzStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Belirli bir tabloya bir başvuru alın

Bir tablo üzerinde işlem gerçekleştirmek için belirli tabloyu başvuru gerekir. Kullanarak bir başvuru alma [Get-AzStorageTable](/powershell/module/az.storage/Get-AzureStorageTable).

```powershell
$storageTable = Get-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="reference-cloudtable-property-of-a-specific-table"></a>Belirli bir tablonun başvuru CloudTable özelliği

> [!IMPORTANT]
> İle çalışırken CloudTable kullanımını zorunlu **AzTable** PowerShell modülü. Çağrı **Get-AzTableTable** bu nesneye bir başvuru almak için komutu. Zaten yoksa, bu komut ayrıca tablo oluşturur.

Kullanarak bir tablo üzerinde işlemler gerçekleştirmeye **AzTable**, belirli bir tablonun CloudTable özelliğine başvuru gerekir.

```powershell
$cloudTable = (Get-AzStorageTable –Name $tableName –Context $ctx).CloudTable
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Bir tablo silme

Bir tablo silmek için kullanın [Remove-AzStorageTable](/powershell/module/az.storage/Remove-AzStorageTable). Bu cmdlet, tüm verilerini dahil olmak üzere bu tabloyu kaldırır.

```powershell
Remove-AzStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu nasıl yapılır başında yeni bir kaynak grubu ve depolama hesabı oluşturduysanız, tüm kaynak grubu kaldırarak, bu alıştırmada, oluşturduğunuz varlıkları kaldırabilirsiniz. Bu komut, kaynak grubunun yanı sıra grubun içinde yer alan tüm kaynakları siler.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede PowerShell ile yaygın Azure tablo depolama işlemleri hakkında bilgi öğrendiniz da dahil olmak üzere: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlık ekleme
> * Tablo sorgulama
> * Tablo varlıklarını silme
> * Bir tablo silme

Daha fazla bilgi için aşağıdaki makalelere bakın.

* [Depolama PowerShell cmdlet'leri](/powershell/module/az.storage#storage)

* [Azure PowerShell - AzureRmStorageTable/AzTable PS modülü v2.0 tablolardan ile çalışma](https://paulomarquesc.github.io/working-with-azure-storage-tables-from-powershell)

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
