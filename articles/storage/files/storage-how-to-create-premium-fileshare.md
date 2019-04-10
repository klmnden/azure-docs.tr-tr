---
title: Bir premium Azure dosya depolama hesabı oluşturma
description: Bu makalede, bir premium Azure dosya depolama hesabı ve bir premium dosya paylaşımı oluşturma hakkında bilgi edinin.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 72dec14dde47580313e57bb3b8d7315604929277
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288434"
---
# <a name="how-to-create-an-azure-premium-file-share"></a>Bir premium Azure dosya paylaşımı oluşturma

Dosya deposundan (Önizleme) depolama hesabı türü, premium performans özelliklerini taşıyan dosya paylaşımları oluşturmanıza olanak sağlayan, Azure dosyaları için yeni bir katman temsil eder. Bu dosya paylaşımlarını yüksek performanslı ve yüksek aktarım hızı paylaşımları tutarlı düşük gecikme süresi ve yüksek IOPS sağlayan kurumsal ölçekli uygulamalar için tasarlanmıştır.

Bu makalede bu hesap türü kullanarak yeni oluşturma işlemi gösterilmektedir [Azure portalında](https://portal.azure.com/), Azure PowerShell ve Azure CLI.

## <a name="prerequisites"></a>Önkoşullar

Azure depolamaya erişmek için bir Azure aboneliği gerekir. Bir aboneliğiniz zaten yoksa, oluşturup bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

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

    ![Premium dosyalar depolama hesabı oluşturma](media/storage-how-to-create-premium-fileshare/premium-files-storage-account.png)

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

### <a name="upgrade-your-powershell-module"></a>PowerShell modülünüzü yükseltme

PowerShell ile premium dosyalarla etkileşim kurmak için en son Az.Storage modül yüklemeniz gerekir.

Yükseltilmiş izinlere sahip bir PowerShell oturumu açarak işleme başlayın.

Az.Storage modülünü yükleyin:

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowPrerelease -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

Kullanım `Login-AzAccount` izleyin ve komut ekrandaki kimlik doğrulaması yapın.

```powershell
Login-AzAccount
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

### <a name="add-the-cli-extension-for-azure-premium-files"></a>Azure premium dosyaları için CLI uzantısını ekleyin

CLI kullanarak premium dosyaları ile etkileşimde bulunmak üzere kabuğunuz için bir uzantı eklemek zorunda kalırsınız.

Bunu yapmak için Cloud Shell veya yerel bir kabuk kullanarak aşağıdaki komutu girin: `az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure CLI ile yeni bir kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group create `
    --name files-howto-resource-group `
    --location westus2
```

### <a name="create-a-filestorage-preview-storage-account"></a>Dosya deposundan (Önizleme) depolama hesabı oluşturma

Azure CLI üzerinden bir dosya deposundan (Önizleme) depolama hesabı oluşturmak için kullanın [az depolama hesabı oluşturma](/cli/azure/storage/account) komutu.

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

Bu makalede, bir premium dosyaları depolama hesabı oluşturdunuz. Bu hesap sunar performans hakkında bilgi edinmek için Planlama Kılavuzu'nun performans katmanı bölümüne geçin.

> [!div class="nextstepaction"]
> [Dosya Paylaşımı performans katmanları](storage-files-planning.md#file-share-performance-tiers)