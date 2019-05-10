---
title: Depolama hesabı - Azure depolama oluşturma
description: Nasıl yapılır bu makalede, Azure portalı, Azure PowerShell veya Azure CLI kullanarak bir depolama hesabı oluşturmayı öğrenin. Azure depolama hesabı, Azure Depolama hizmetinde oluşturduğunuz veri nesnelerini depolamak ve bunlara erişmek için Microsoft Azure’da benzersiz bir ad alanı sağlar.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: article
ms.date: 05/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8375f4c54dc436ecf0694ec5f629c81d3591594d
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65234162"
---
# <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure depolama hesabınız Azure Storage veri nesnelerinizi içerir: bloblar, dosyalar, kuyruklar, tablolar ve diskler. Depolama hesabı, Azure Storage verilerinizin herhangi bir dünyada HTTP veya HTTPS üzerinden erişilebilir için benzersiz bir ad alanı sağlar. Azure depolama hesabınızdaki veriler, dayanıklı ve yüksek oranda kullanılabilir, güvenli ve yüksek düzeyde ölçeklenebilir.

Nasıl yapılır bu makalede, kullanarak bir depolama hesabı oluşturmak bilgi [Azure portalında](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), veya bir [Azure Resource Manager Şablon](../../azure-resource-manager/resource-group-overview.md).  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Yok.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Bu nasıl yapılır makalesi, Azure PowerShell modülü Az 0.7 veya sonraki bir sürümü gerektirir. Geçerli sürümünüzü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure'da oturum açın ve Azure CLI komutları iki yoldan biriyle çalıştırın:

- Azure Cloud Shell'de, Azure portal, CLI komutlarını çalıştırabilirsiniz.
- CLI yükleyip CLI komutlarını yerel olarak çalıştırabilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI yüklenmiş ve hesabınızla kullanmak üzere yapılandırılmış. Tıklayın **Cloud Shell** düğmesi Azure portalının sağ üst bölümündeki menüsünde:

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme bu nasıl yapılır makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portaldaki Cloud Shell penceresini gösteren ekran görüntüsü](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Azure CLI 2.0.4 bir sürümünü çalıştırıyorsanız bu nasıl yapılır makalesi gerektiriyor veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Yok.

---

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[Azure Portal](https://portal.azure.com) oturum açın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Azure aboneliğinizde oturum açın `Connect-AzAccount` izleyin ve komut ekrandaki kimlik doğrulaması yapın.

```powershell
Connect-AzAccount
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Cloud Shell'i başlatmak için oturum açın [Azure portalında](https://portal.azure.com).

CLI yerel yüklemesinde açmak için çalıştırma [az login](/cli/azure/reference-index#az-login) komutu:

```cli
az login
```

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Yok

---

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Artık depolama hesabınızı oluşturmaya hazırsınız.

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu makalede yeni bir kaynak grubu oluşturma işlemini gösterir.

**Genel amaçlı v2** depolama hesabı, tüm Azure Depolama hizmetlerine erişim sağlar: blob'lar, dosyalar, kuyruklar, tablolar ve diskler. Burada özetlenen adımları genel amaçlı v2 depolama hesabı oluşturma, ancak herhangi bir türde depolama hesabı oluşturmak için adımları da buradakilere benzer.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak kullanarak PowerShell ile yeni bir kaynak grubu oluşturun. [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

İçin hangi bölgeyi belirteceğinizden emin değilseniz `-Location` parametresi, aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz [Get-AzLocation](/powershell/module/az.resources/get-azlocation) komutu:

```powershell
Get-AzLocation | select Location
$location = "westus"
```

Ardından, genel amaçlı v2 depolama hesabı kullanarak okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) ile oluşturun [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutu. Depolama hesabınızın adı Azure'da benzersiz olması gerekir, böylece köşeli ayraçlar içindeki yer tutucu değerini, kendi benzersiz bir değer ile değiştirin unutmayın:

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2 
```

Farklı çoğaltma seçeneği ile genel amaçlı v2 depolama hesabı oluşturmak için aşağıdaki tabloda dilediğiniz değeri yerine **SkuName** parametresi.

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
    --name storage-resource-group \
    --location westus
```

`--location` parametresi için hangi bölgeyi belirteceğinizden emin değilseniz, [az account list-locations](/cli/azure/account#az_account_list) komutuyla aboneliğiniz için desteklenen bölgelerin bir listesini alabilirsiniz.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Ardından, genel amaçlı v2 depolama hesabı kullanarak okuma erişimli coğrafi olarak yedekli depolama ile oluşturma [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create) komutu. Depolama hesabınızın adı Azure'da benzersiz olması gerekir, böylece köşeli ayraçlar içindeki yer tutucu değerini, kendi benzersiz bir değer ile değiştirin unutmayın:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location westus \
    --sku Standard_RAGRS \
    --kind StorageV2
```

Farklı çoğaltma seçeneği ile genel amaçlı v2 depolama hesabı oluşturmak için aşağıdaki tabloda dilediğiniz değeri yerine **sku** parametresi.

|Çoğaltma seçeneği  |sku parametresi  |
|---------|---------|
|Yerel olarak yedekli depolama (LRS)     |Standard_LRS         |
|Alanlar arası yedekli depolama (ZRS)     |Standard_ZRS         |
|Coğrafi olarak yedekli depolama (GRS)     |Standard_GRS         |
|Okuma erişimli coğrafi olarak yedekli depolama (GRS)     |Standard_RAGRS         |

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Bir depolama hesabı oluşturmak için Resource Manager şablonu dağıtmak için Azure Powershell veya Azure CLI'yı kullanabilirsiniz. Bu nasıl yapılır makalesinde kullanılan şablon dandır [Azure Resource Manager hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Komut dosyalarını çalıştırmak için seçin **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından **yapıştırın**.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-file "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

Şablon oluşturma hakkında bilgi edinmek için bkz:

- [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).
- [Depolama hesabı şablon başvurusu](/azure/templates/microsoft.storage/allversions).
- [Ek depolama hesabı şablon örnekleri](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage).

---

Kullanılabilir çoğaltma seçenekleri hakkında daha fazla bilgi için bkz. [Depolama çoğaltma seçenekleri](storage-redundancy.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu nasıl yapılır makalesi tarafından oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili depolama hesabını ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

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
az group delete --name storage-resource-group
```

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için Azure PowerShell veya Azure CLI'yı kullanın.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde bir genel amaçlı v2 standart depolama hesabı oluşturdunuz. Karşıya yükleme ve BLOB Depolama hesabınızdan indirme öğrenmek için bir Blob Depolama hızlı başlangıçları devam edin.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [PowerShell kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Azure CLI kullanarak blobları ile çalışma](../blobs/storage-quickstart-blobs-cli.md)

# <a name="templatetabtemplate"></a>[Şablon](#tab/template)

> [!div class="nextstepaction"]
> [Azure portalını kullanarak bloblarla çalışma](../blobs/storage-quickstart-blobs-portal.md)

---
