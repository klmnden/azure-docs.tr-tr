---
title: Premium Azure dosya paylaşımı oluşturma
description: Bu makalede, bir premium Azure dosya paylaşımı oluşturma işlemini öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/05/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 265a1cf0a8a5e1e099a4ec7a9f0d674e0c474dd4
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190106"
---
# <a name="how-to-create-an-premium-azure-file-share"></a>Bir premium Azure dosya paylaşımı oluşturma
Premium dosya paylaşımları (Önizleme), katı hal sürücüsü (SSD) depolama medyasında sunulur ve veritabanları ve yüksek performanslı bilgi işlem (HPC) barındırma gibi g/ç yoğunluklu iş yükleri için yararlıdır. Premium dosya paylaşımları adlı bir dosya deposundan hesabı bir özel amaçlı depolama hesabı türü içinde barındırılır. Premium dosya paylaşımları, yüksek performanslı ve yüksek aktarım hızı paylaşımları tutarlı düşük gecikme süresi ve yüksek IOPS sağlayan kurumsal ölçekli uygulamalar için tasarlanmıştır.

Bu makalede bu hesap türü kullanarak yeni oluşturma işlemi gösterilmektedir [Azure portalında](https://portal.azure.com/), Azure PowerShell ve Azure CLI.

## <a name="prerequisites"></a>Önkoşullar

Premium Azure dosya paylaşımlarını Azure kaynaklarına erişmek için bir Azure aboneliği gerekir. Bir aboneliğiniz zaten yoksa, oluşturup bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

## <a name="create-a-premium-file-share-using-the-azure-portal"></a>Premium, Azure portalını kullanarak bir dosya paylaşımı oluşturma

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com/) oturum açın.

### <a name="create-a-filestorage-preview-storage-account"></a>Dosya deposundan (Önizleme) depolama hesabı oluşturma

Artık depolama hesabınızı oluşturmak hazırsınız.

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. Bu makalede yeni bir kaynak grubu oluşturma işlemini gösterir.

1. Azure portalında **depolama hesapları** sol menüsünde.

    ![Azure portalı ana sayfası depolama hesabını seçin](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
1. Depolama hesabının oluşturulacağı aboneliği seçin.
1. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Aşağıdaki görüntüde gösterildiği gibi yeni kaynak grubunuz için bir ad girin.

1. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 - 24 karakter uzunluğunda olmalıdır ve yalnızca rakam ve küçük harf içerebilir.
1. Depolama hesabınız için bir konum seçin veya varsayılan konumu kullanın.
1. İçin **performans** seçin **Premium**.
1. Seçin **hesap türü** ve **dosya (Önizleme) deposundan**.
1. Bırakın **çoğaltma** kendi varsayılan değerine ayarlanmasından **yerel olarak yedekli depolama (LRS)**.

    ![Premium dosya paylaşımı için bir depolama hesabı oluşturma](media/storage-how-to-create-premium-fileshare/premium-files-storage-account.png)

1. Depolama hesabı ayarlarınızı gözden geçirmek ve hesabı oluşturmak için **Gözden Geçir + Oluştur**’u seçin.
1. **Oluştur**’u seçin.

Depolama hesabı kaynak oluşturulduktan sonra bu sayfaya gidin.

### <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

1. Depolama hesabı için sol taraftaki menüde kaydırarak **dosya hizmeti** bölümüne ve ardından **dosyalar (Önizleme)**.
1. Seçin **+ dosya paylaşımı** premium dosya paylaşımı oluşturmak için.
1. Dosya paylaşımınız için bir ad ve istenen kotayı girin ve ardından **Oluştur**.

> [!NOTE]
> Sağlanan paylaşım boyutları, dosya paylaşımları, sağlanan boyutu faturalandırılır paylaşımı kota tarafından belirtilirse, başvurmak [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/) daha fazla ayrıntı için.

   ![Premium dosya paylaşımı oluşturma](media/storage-how-to-create-premium-fileshare/create-premium-file-share.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silmeniz yeterlidir. Kaynak grubunu sildiğinizde ilişkili depolama hesabı ve bunun yanı sıra kaynak grubuyla ilişkili diğer kaynaklar da silinir.

## <a name="create-a-premium-file-share-using-powershell"></a>PowerShell kullanarak bir premium dosya paylaşımı oluşturma

### <a name="create-an-account-using-powershell"></a>PowerShell kullanarak hesap oluşturma

İlk olarak, en son sürümünü yükleyin [PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget) modülü.

Ardından, yükseltme, powershell modülü, Azure aboneliğinizde oturum açın bir kaynak grubu oluşturun ve bir depolama hesabı oluşturun.

### <a name="upgrade-your-powershell-module"></a>Yükseltme, PowerShell Modülü

Premium dosya paylaşımı PowerShell ile etkileşim için son Az.Storage modül yüklemeniz gerekir.

Yükseltilmiş izinlere sahip bir PowerShell oturumu açarak işleme başlayın.

Az.Storage modülünü yükleyin:

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowPrerelease -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

Kullanım `Connect-AzAccount` izleyin ve komut ekrandaki kimlik doğrulaması yapın.

```powershell
Connect-AzAccount
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PowerShell ile yeni bir kaynak grubu oluşturmak için kullanın [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu:

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-how-to-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-filestorage-preview-storage-account"></a>Dosya deposundan (Önizleme) depolama hesabı oluşturma

Powershell'den dosya deposundan (Önizleme) depolama hesabı oluşturmak için kullanın [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutu:

```powershell
$storageAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name "fileshowto" -SkuName "Premium_LRS" -Location "westus2" -Kind "FileStorage"
```

### <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

Bir dosya deposundan hesabınız olduğuna göre bir premium dosya paylaşımı oluşturabilirsiniz. Kullanım [yeni AzStorageShare](/powershell/module/az.storage/New-AzStorageShare) oluşturmak için cmdlet'i.

> [!NOTE]
> Sağlanan paylaşım boyutları, dosya paylaşımları, sağlanan boyutu faturalandırılır paylaşımı kota tarafından belirtilirse, başvurmak [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/) daha fazla ayrıntı için.

```powershell
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutu: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-a-premium-file-share-using-azure-cli"></a>Premium, Azure CLI kullanarak bir dosya paylaşımı oluşturma

Azure Cloud Shell'i başlatmak için oturum açın [Azure portalında](https://portal.azure.com).

CLI yerel yüklemesinde açmak oturum açma komutunu çalıştırın:

```cli
az login
```

### <a name="add-the-preview-storage-cli-extension"></a>Önizleme depolama CLI uzantısını ekleyin

Dosya paylaşımları Premium bir önizleme özelliği olduğundan, kabuğunuzun için Önizleme uzantıyı eklemek zorunda kalırsınız. Bunu yapmak için Cloud Shell veya yerel bir kabuk kullanarak aşağıdaki komutu girin: `az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure CLI ile yeni bir kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group create `
    --name files-howto-resource-group `
    --location westus2
```

### <a name="create-a-filestorage-storage-account"></a>Dosya deposundan depolama hesabı oluşturma

Azure CLI üzerinden bir dosya deposundan depolama hesabı oluşturmak için kullanın [az depolama hesabı oluşturma](/cli/azure/storage/account) komutu.

```azurecli-interactive
az storage account create `
    --name fileshowto `
    --resource-group files-howto-resource-group `
    --location westus `
    --sku Premium_LRS `
    --kind FileStorage
```

### <a name="get-the-storage-account-key"></a>Depolama hesabı anahtarını alma

Depolama hesabı anahtarları, bu makalede, depolama hesabınızdaki kaynaklara erişimi denetlemek, anahtarı bir premium dosya paylaşımı oluşturmak için kullanacağız. Bu anahtarlar, depolama hesabını oluşturduğunuzda otomatik olarak oluşturulur. [az storage account keys list](/cli/azure/storage/account/keys) komutunu kullanarak depolama hesabınızın depolama hesabı anahtarlarını alabilirsiniz:

```azurecli-interactive 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

### <a name="create-a-premium-file-share"></a>Premium dosya paylaşımı oluşturma

Bir dosya deposundan hesabınız olduğuna göre bir premium dosya paylaşımı oluşturabilirsiniz. Kullanım [az depolama alanı paylaşımı oluşturma](/cli/azure/storage/share) oluşturmak için komutu.

> [!NOTE]
> Sağlanan paylaşım boyutları, dosya paylaşımları, sağlanan boyutu faturalandırılır paylaşımı kota tarafından belirtilirse, başvurmak [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/) daha fazla ayrıntı için.

```azurecli-interactive
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için [az group delete](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir premium dosya paylaşımı oluşturdunuz. Bu hesap sunar performans hakkında bilgi edinmek için Planlama Kılavuzu'nun performans katmanı bölümüne geçin.

> [!div class="nextstepaction"]
> [Dosya Paylaşımı performans katmanları](storage-files-planning.md#file-share-performance-tiers)