---
title: PowerShell ile Azure Cosmos DB tablo API işlemleri | Microsoft Docs
description: PowerShell ile Azure Cosmos DB tablo API işlemleri gerçekleştirme
services: storage
author: SnehaGunda
manager: kfile
editor: tysonn
ms.service: storage
ms.component: cosmosdb-table
ms.devlang: na
ms.topic: how-to
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 9365fd70036c8b489efaea42bda9c670182c496c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082283"
---
# <a name="perform-azure-cosmos-db-table-api-operations-with-azure-powershell"></a>Azure PowerShell ile Azure Cosmos DB tablo API işlemleri 

>[!NOTE]
>Azure Cosmos DB tablo API anahtar teslimi genel dağıtım, düşük gecikme süresi okuma ve yazma, otomatik ikincil dizin oluşturma ve ayrılmış işleme gibi tablo depolama premium özellikleri sunar. Çoğu durumda, bu makalede çalışma Azure Cosmos DB tablo API ve Azure Table storage, ancak bu makale için PowerShell komutlarını Azure Cosmos DB tablo API'sine özel. Azure Table depolama birimi kullanıyorsanız bkz [gerçekleştirmek Azure Table depolama işlemleri Azure PowerShell ile](../storage/tables/table-storage-how-to-use-powershell.md).
>

Azure Cosmos DB tablo API, depolamak ve yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini sorgulamanızı sağlar. Hizmet ana bileşenlerinin tabloları, varlıkları ve özellikler mevcuttur. Bir tablo, varlıkları koleksiyonudur. Bir varlık özellikler kümesidir. Her varlığın tüm ad-değer çiftleri en çok 252 özellik olabilir. Bu makale, zaten Azure Cosmos DB tablo API kavramlarını olduğunuzu varsayar. Ayrıntılı bilgi için bkz: [Azure Cosmos DB tablo API giriş](table-introduction.md) ve [tablo API kullanarak bir .NET uygulaması oluşturma](create-table-dotnet.md).

Bu nasıl yapılır makalesi ortak tablo API işlemleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlıkları ekleyin
> * Sorgu Tablosu
> * Tablo varlıklarını silme

## <a name="prerequisites"></a>Önkoşullar

Azure PowerShell modülü sürümü 4.4.0 örnekler gerektiren veya sonraki bir sürümü. Bir PowerShell penceresinde çalıştırın `Get-Module -ListAvailable AzureRM` sürümü bulunamıyor. Hiçbir şey görünmez veya yükseltmek için bkz: gereken [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). 

Azure PowerShell yüklü veya güncelleştirildikten sonra modülü yüklemelisiniz **AzureRmStorageTable**, varlıkları yönetmek için komutlar vardır. Bu modül yüklemek için PowerShell bir yönetici kullanım farklı çalıştır ve **yükleme-Module** komutu.

```powershell
Install-Module AzureRmStorageTable
```

Ardından yerel olarak bu PowerShell cmdlet'lerini kullanmak için Azure Cosmos DB derlemeleri yükleyin. Bunun hakkında daha fazla yönerge için bkz: [Cosmos DB tablolar için Azure RM depolama tabloları PowerShell Modülü](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/).

Aşağıdaki alıştırmada denemek için bir Azure Cosmos DB veritabanı hesabınızın olması gerekir. Yeni Azure Cosmos DB oluşturma bir zaten yoksa, kullanarak hesap [Azure portal](https://portal.azure.com). Yeni bir veritabanı hesabı oluşturmanıza yardımcı olması için bkz: [Azure Cosmos DB: veritabanı hesabı oluşturma](create-table-dotnet.md#create-a-database-account).

Veritabanı hesabı adı ve kaynak grubu portaldan alın; komut dosyanızı tablolara erişen koymak için bu değerleri gerekir. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="create-a-table-or-reference-a-table"></a>Bir tablo oluşturun veya bir tablo başvurusu

Bir tablo oluşturmak veya bir tablo başvuru almak için kullanın **Get-AzureStorageTableTable**. Bu cmdlet mevcut olmayan bir tablo adı ile çağırırsanız, bu ada sahip yeni bir tablo oluşturur ve yeni bir tablo için bir başvuru döndürür. Tablo mevcut değilse, var olan tablo için bir başvuru döndürür.

```powershell
# set the name of the resource group in which your Cosmos DB Account resides.
$resourceGroup = "contosocosmosrg"
# if you want to make sure the resource group is valid, try this command
Get-AzureRmResourceGroup -Name $resourceGroup

# set the Cosmos DB account name 
$cosmosDBAccountName = "contosocosmostbl" 

# set the table name 
$tableName = "contosotable1"

# Get a reference to a table. This creates the table if it doesn't exist.
$storageTable = Get-AzureStorageTableTable `
  -resourceGroup $resourceGroup `
  -tableName $tableName `
  -cosmosDbAccount $cosmosDBAccountName 
```

PowerShell kullanarak Azure Cosmos DB hesabını tablolarda listelenemiyor, ancak portalında oturum açın ve tabloya bakın. Şimdi tablo varlıklarda yönetme bakalım.

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Bir tablo silme 

PowerShell Azure Cosmos DB silme tablolardan desteklemiyor. Bir tabloyu silmek için şu adrese gidin [Azure portal](https://portal.azure.com), kullanmakta olduğunuz Azure Cosmos DB hesabını bulun, sonra bulma ve tablo silme. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yeni bir kaynak grubu oluşturduysanız ve yeni bir Azure Cosmos DB hesabı bu gruba oluşturulan tüm varlıkları kaldırabilirsiniz kaynak grubu kaldırarak bu alıştırmada oluşturdunuz. Bu komut, kaynak grubunun yanı sıra grup içinde bulunan tüm kaynakları siler.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır bu makalede PowerShell, ortak tablo API işlemleriyle ilgili öğrenilen nasıl dahil olmak üzere: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlıkları ekleyin
> * Sorgu Tablosu
> * Tablo varlıklarını silme

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure depolama tablolardan PowerShell ile çalışma](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
