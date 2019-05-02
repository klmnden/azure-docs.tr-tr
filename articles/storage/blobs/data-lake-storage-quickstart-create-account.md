---
title: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma | Microsoft Docs
description: Azure portalı, Azure PowerShell veya Azure CLI'yı kullanarak Data Lake depolama Gen2'ye ile erişim yeni bir depolama hesabı oluşturmak hızlı bir şekilde öğrenin.
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 18132ac4c218c766efdc9a9afae2cc3508c4f732
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939416"
---
# <a name="quickstart-create-an-azure-data-lake-storage-gen2-storage-account"></a>Hızlı Başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma

Azure Data Lake depolama Gen2 [bir hiyerarşik ad alanı destekler](data-lake-storage-introduction.md) yerel dizin tabanlı sağlayan dosya sistemi Hadoop dağıtılmış dosya sistemi (HDFS) ile çalışacak şekilde tasarlanmış. HDFS'den Data Lake Storage Gen2 verilerine erişim [ABFS sürücüsü](data-lake-storage-abfs-driver.md) aracılığıyla sağlanabilir.

Bu hızlı başlangıçta [Azure portal](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) veya [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) kullanarak hesap oluşturma adımları gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

|           | Önkoşul |
|-----------|--------------|
|Portal     | None         |
|PowerShell | Bu hızlı başlangıç PowerShell modülü Az.Storage sürümünü gerektirir **0,7** veya üzeri. Geçerli sürümünüzü bulmak için çalıştırın `Get-Module -ListAvailable Az.Storage` komutu. Bu komutu çalıştırdıktan sonra sonuç görünüyorsa ya da dışında bir sürüm varsa **0,7** görünür sonra powershell Modülü'ı yükseltmeniz gerekir. Bkz: [, powershell modülü yükseltme](#upgrade-your-powershell-module) başlığına.
|CLI        | Azure'da oturum açın ve Azure CLI komutları iki yoldan biriyle çalıştırın: <ul><li>CLI komutlarını Azure portalında Azure Cloud Shell içinden çalıştırabilirsiniz </li><li>CLI yükleyip CLI komutlarını yerel olarak çalıştırabilirsiniz</li></ul>|

Komut satırında çalışırken Azure Cloud Shell'i çalıştırabilir veya CLI'yı yerel ortama yükleyebilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure portalının sağ üst tarafında yer alan menüdeki **Cloud Shell** düğmesine tıklayın:

[![Cloud Shell](./media/data-lake-storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme bu hızlı başlangıçtaki adımları uygulamak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portaldaki Cloud Shell penceresini gösteren ekran görüntüsü](./media/data-lake-storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu hızlı başlangıç için Azure CLI 2.0.38 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-storage-account-with-azure-data-lake-storage-gen2-enabled"></a>Azure Data Lake Storage 2. Nesil etkin bir depolama hesabı oluşturma

Hesap oluşturmadan önce depolama hesapları veya oluşturduğunuz diğer Azure kaynakları için mantıksal kapsayıcı görevi görecek bir kaynak grubu oluşturmanız gerekir. Bu hızlı başlangıç tarafından oluşturulan kaynakları temizlemek isterseniz, kaynak grubunu silmeniz yeterlidir. Kaynak grubunun silinmesi, ilişkili depolama hesabını ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Data Lake Storage Gen2 özelliklerinden faydalanmak için yeni depolama hesaplarını **StorageV2 (genel amaçlı V2)** türünde oluşturmanız gerekir.  

Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure Depolama hesabına genel bakış](../common/storage-account-overview.md).

Depolama hesabınızı adlandırırken şu kuralları göz önünde bulundurun:

- Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
- Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. İki depolama hesabı aynı ada sahip olamaz.

## <a name="create-an-account-using-the-azure-portal"></a>Azure portalı kullanarak bir hesap oluşturma

[Azure Portal](https://portal.azure.com) oturum açın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure portalında bir kaynak grubu oluşturmak için şu adımları izleyin:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçin.
2. **Ekle**’ye tıklayarak yeni bir kaynak grubu ekleyin.
3. Yeni kaynak grubu için bir ad girin.
4. Yeni kaynak grubunun oluşturulacağı aboneliği seçin.
5. Kaynak grubu için konum seçin.
6. **Oluştur** düğmesine tıklayın.  

   ![Azure portalında kaynak grubu oluşturmayı gösteren ekran görüntüsü](./media/data-lake-storage-quickstart-create-account/create-resource-group.png)

### <a name="create-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı oluşturma

Azure portalında genel amaçlı v2 bir depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

> [!NOTE]
> Hiyerarşik ad alanı şu anda tüm genel bölgelerde kullanılabilir.

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Tüm hizmetler**'i seçin. Ardından **Depolama** bölümüne inin ve **Depolama hesapları**'nı seçin. Açılan **Depolama Hesapları** penceresinde **Ekle**'yi seçin.
2. Seçin, **abonelik** ve **kaynak grubu** daha önce oluşturduğunuz.
3. Depolama hesabınız için bir ad girin.
4. **Konum**'u **Batı ABD 2** olarak belirleyin
5. Şu alanları varsayılan değerlerinde bırakın: **Performans**, **hesap türü**, **çoğaltma**, **erişim katmanı**.
6. Depolama hesabını oluşturmak istediğiniz aboneliği seçin.
7. Seçin **sonraki: Gelişmiş >**
8. Altında değerleri bırakın **güvenlik** ve **sanal ağlar** alanlarını varsayılan değerlerine ayarlayın.
9. İçinde **Data Lake depolama Gen2** bölümünde kümesi **hiyerarşik ad alanı** için **etkin**.
10. Tıklayın **gözden geçir + Oluştur** depolama hesabı oluşturmak için.

    ![Azure portalında depolama hesabı oluşturmayı gösteren ekran görüntüsü](./media/data-lake-storage-quickstart-create-account/azure-data-lake-storage-account-create-advanced.png)

Depolama hesabınız portaldan oluşturulmuş olur.

### <a name="clean-up-resources"></a>Kaynakları temizleme

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve listenin sağ tarafındaki **Daha fazla** düğmesine (**...**) sağ tıklayın.
3. **Kaynak grubunu sil**'i seçip onaylayın.

## <a name="create-an-account-using-powershell"></a>PowerShell kullanarak hesap oluşturma

İlk olarak, en son sürümünü yükleyin [PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget) modülü.

Ardından, yükseltme, powershell modülü, Azure aboneliğinizde oturum açın bir kaynak grubu oluşturun ve bir depolama hesabı oluşturun.

### <a name="upgrade-your-powershell-module"></a>PowerShell modülünüzü yükseltme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell kullanarak Data Lake depolama 2. nesil ile etkileşimde bulunmak üzere, modül Az.Storage sürümü yüklemeniz gerekir **0,7** veya üzeri.

Yükseltilmiş izinlere sahip bir PowerShell oturumu açarak işleme başlayın.

Az.Storage modülünü yükleme

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

> [!NOTE]
> Hiyerarşik ad alanı şu anda tüm genel bölgelerde kullanılabilir.

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı oluşturma

Genel amaçlı v2 depolama hesabı, yerel olarak yedekli depolama (LRS) Powershell'den oluşturmak için kullanın [yeni AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) komutu:

```powershell
$location = "westus2"

New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 `
  -EnableHierarchicalNamespace $True
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutu: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-an-account-using-azure-cli"></a>Azure CLI'yı kullanarak hesap oluşturma

Azure Cloud Shell'i başlatmak için oturum açın [Azure portalında](https://portal.azure.com).

İçin CLI yerel yüklemesinde oturum açmak oturum açma komutunu çalıştırın:

```cli
az login
```

### <a name="add-the-cli-extension-for-azure-data-lake-gen-2"></a>Azure Data Lake Gen 2 için CLI uzantısını ekleyin

CLI'yı kullanarak Data Lake depolama 2. nesil ile etkileşimde bulunmak üzere kabuğunuz için bir uzantı eklemek zorunda kalırsınız.

Bunu yapmak için Cloud Shell veya yerel bir kabuk kullanarak aşağıdaki komutu girin: `az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure CLI ile yeni bir kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group create `
    --name storage-quickstart-resource-group `
    --location westus2
```

> [!NOTE]
> > Hiyerarşik ad alanı şu anda tüm genel bölgelerde kullanılabilir.

### <a name="create-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı oluşturma

Azure CLI’dan yerel olarak yedekli depolama ile genel amaçlı v2 depolama hesabı oluşturmak için [az storage account create](/cli/azure/storage/account) komutunu kullanın.

```azurecli-interactive
az storage account create `
    --name storagequickstart `
    --resource-group storage-quickstart-resource-group `
    --location westus2 `
    --sku Standard_LRS `
    --kind StorageV2 `
    --hierarchical-namespace true
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için [az group delete](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Data Lake Storage 2. Nesil özelliklerine sahip bir depolama hesabı oluşturdunuz. Karşıya yükleme ve BLOB Depolama hesabınızdan indirme öğrenmek için aşağıdaki konuya bakın.

* [AzCopy V10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
