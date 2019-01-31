---
title: 'Hızlı Başlangıç: Depolama hesabı - Azure depolama oluşturma'
description: Bu hızlı başlangıçta Azure portalı, Azure PowerShell veya Azure CLI kullanarak bir depolama hesabı oluşturmayı öğreneceksiniz. Azure depolama hesabı, Azure Depolama hizmetinde oluşturduğunuz veri nesnelerini depolamak ve bunlara erişmek için Microsoft Azure’da benzersiz bir ad alanı sağlar.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 09/18/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5266ca3f50a2d8163dbab95109cb967fb5a63ed8
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474590"
---
# <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu hızlı başlangıçta [Azure portalı](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) veya [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) kullanarak bir depolama hesabı oluşturmayı öğreneceksiniz.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Yok.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Bu hızlı başlangıçta, Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Geçerli sürümünüzü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

İki yöntemden biriyle Azure'da oturum açarak Azure CLI komutlarını çalıştırabilirsiniz:

- CLI komutlarını Azure portalında Azure Cloud Shell içinden çalıştırabilirsiniz 
- CLI yükleyip CLI komutlarını yerel olarak çalıştırabilirsiniz  

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure portalının sağ üst tarafında yer alan menüdeki **Cloud Shell** düğmesine tıklayın:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme bu hızlı başlangıçtaki adımları uygulamak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portaldaki Cloud Shell penceresini gösteren ekran görüntüsü](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

---

## <a name="log-in-to-azure"></a>Azure'da oturum açma

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://portal.azure.com)’da oturum açın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyerek kimlik doğrulaması yapın.

```powershell
Connect-AzAccount
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Cloud Shell’i başlatmak için [Azure portalında](https://portal.azure.com) oturum açın.

CLI yerel yüklemesinde oturum açmak için oturum açma komutunu çalıştırın:

```cli
az login
```

---

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Artık depolama hesabınızı oluşturmaya hazırsınız.

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu hızlı başlangıçta, yeni bir kaynak grubu oluşturma işlemi gösterilmektedir. 

**Genel amaçlı v2** depolama hesabı, tüm Azure Depolama hizmetlerine erişim sağlar: blob'lar, dosyalar, kuyruklar, tablolar ve diskler. Hızlı başlangıç bir genel amaçlı v2 depolama hesabı oluşturur, ancak herhangi bir türde depolama hesabı oluşturma adımları da benzerdir.   

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak kullanarak PowerShell ile yeni bir kaynak grubu oluşturun. [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu: 

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location 
```

İçin hangi bölgeyi belirteceğinizden emin değilseniz `-Location` parametresi, aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz [Get-AzLocation](/powershell/module/az.resources/get-azlocation) komutu:

```powershell
Get-AzLocation | select Location 
$location = "westus"
```

Ardından, yerel olarak yedekli depolama (LRS) ile bir genel amaçlı v2 depolama hesabı oluşturun. Kullanım [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutu: 

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 
```

Bölgesel olarak yedekli depolama (ZRS) (önizleme), coğrafi olarak yedekli depolama (GRS) veya okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) ile genel amaçlı v2 depolama hesabı oluşturmak için, aşağıdaki tabloda dilediğiniz değeri **SkuName** parametresinin yerine yerleştirin. 

|Çoğaltma seçeneği  |SkuName parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, [az group create](/cli/azure/group#az_group_create) komutunu kullanarak Azure CLI ile yeni bir kaynak grubu oluşturun. 

```azurecli-interactive
az group create \
    --name storage-quickstart-resource-group \
    --location westus
```

`--location` parametresi için hangi bölgeyi belirteceğinizden emin değilseniz, [az account list-locations](/cli/azure/account#az_account_list) komutuyla aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Ardından, yerel olarak yedekli depolama ile bir genel amaçlı v2 depolama hesabı oluşturun. [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutunu kullanın:

```azurecli-interactive
az storage account create \
    --name storagequickstart \
    --resource-group storage-quickstart-resource-group \
    --location westus \
    --sku Standard_LRS \
    --kind StorageV2
```

Bölgesel olarak yedekli depolama (ZRS Önizlemesi), coğrafi olarak yedekli depolama (GRS) veya okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) ile genel amaçlı v2 depolama hesabı oluşturmak için, aşağıdaki tabloda dilediğiniz değeri **sku** parametresinin yerine yerleştirin. 

|Çoğaltma seçeneği  |sku parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |

---

Kullanılabilir çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Depolama çoğaltma seçenekleri](storage-redundancy.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç tarafından oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silmeniz yeterlidir. Kaynak grubunun silinmesi, ilişkili depolama hesabını ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve listenin sağ tarafındaki **Daha fazla** düğmesine (**...**) sağ tıklayın.
3. **Kaynak grubunu sil**'i seçip onaylayın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutu: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için [az group delete](/cli/azure/group#az_group_delete) komutunu kullanın.

```azurecli-interactive
az group delete --name storage-quickstart-resource-group
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel amaçlı v2 standart depolama hesabı oluşturdunuz. Depolama hesabınıza/hesabınızdan blobları karşıya yüklemeyi ve indirmeyi öğrenmek için, Blob depolama hızlı başlangıcı ile devam edin.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [PowerShell kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Azure CLI kullanarak blobları ile çalışma](../blobs/storage-quickstart-blobs-cli.md)

---
