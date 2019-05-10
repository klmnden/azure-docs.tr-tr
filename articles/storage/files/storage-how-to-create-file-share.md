---
title: Azure dosya paylaşımı oluşturma | Microsoft Docs
description: Azure portalı, PowerShell ve Azure CLI kullanarak Azure Dosyaları'nda bir Azure dosya paylaşımı oluşturma.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d945d5b79c274aa8e142203c56b27eb673e36741
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510524"
---
# <a name="create-a-file-share-in-azure-files"></a>Azure Dosyaları'nda bir dosya paylaşımı oluşturma
Kullanarak Azure dosya paylaşımları oluşturabilirsiniz [Azure portalında](https://portal.azure.com/), Azure Storage PowerShell cmdlet'lerini, Azure Storage istemcisi kitaplıklarını veya Azure depolama REST API'si. Bu öğreticide şunları öğreneceksiniz:
* Azure portalını kullanarak Azure dosya paylaşımı oluşturma
* [PowerShell kullanarak Azure dosya paylaşımı oluşturma](#create-file-share-through-powershell)
* [CLI kullanarak Azure dosya paylaşımı oluşturma](#create-file-share-through-command-line-interface-cli)

## <a name="prerequisites"></a>Önkoşullar
Azure dosya paylaşımı oluşturmak için zaten var olan bir Depolama Hesabı kullanabilir veya [yeni bir Azure Depolama Hesabı oluşturabilirsiniz](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). PowerShell ile Azure dosya paylaşımı oluşturmak için depolama hesabınızın hesap anahtarı ve adı gerekir. Powershell veya CLI kullanmayı planlıyorsanız depolama hesabı anahtarı gerekir.

## <a name="create-a-file-share-through-the-azure-portal"></a>Azure portalı üzerinden dosya paylaşımı oluşturma
1. **Azure portalında depolama hesabı dikey penceresine gidin**:    
    ![Depolama Hesabı dikey penceresi](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Dosya Paylaşımı ekleme düğmesine tıklayın**:    
    ![Dosya Paylaşımı ekleme düğmesine tıklayın](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Ad ve Kota belirtin. Kota'nın geçerli değeri en fazla 5 TiB olabilir**:    
    ![Yeni dosya paylaşımı için ad ve istenen kotayı sağlayın](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Yeni dosya paylaşımınızı görüntüleyin**:  ![Yeni dosya paylaşımınızı görüntüleyin](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Bir dosyayı karşıya yüklemeyi**:  ![Bir dosyayı karşıya yükleyin](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Dosya paylaşımınıza göz atın ve dizinlerinizle dosyalarınız yönetin**:  ![Dosya paylaşımına göz atın](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>PowerShell üzerinden dosya paylaşımı oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell’i kullanmaya hazırlamak için Azure PowerShell cmdlet’lerini indirin ve yükleyin. Bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) yükleme noktası ve yükleme yönergeleri için.

> [!Note]  
> En güncel Azure PowerShell modülünü indirmeniz ve yüklemeniz veya yükseltmeniz önerilir.

1. **Depolama hesabınız ve anahtarınız için bir bağlam oluşturun** Bağlam, depolama hesabı adını ve hesap anahtarını kapsar. Hesap anahtarını kopyalama yönergeleri [Azure portalında](https://portal.azure.com/), bkz: [depolama hesabı erişim anahtarlarını](../common/storage-account-manage.md#access-keys).

    ```powershell
    $storageContext = New-AzStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Yeni dosya paylaşımını oluşturun**:    
    
    ```powershell
    $share = New-AzStorageShare logs -Context $storageContext
    ```

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara için bkz: [adlandırma ve başvuran paylaşımları, dizinleri, dosyaları ve meta verileri](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-file-share-through-command-line-interface-cli"></a>Komut Satırı Arabirimi (CLI) üzerinden dosya paylaşımı oluşturma
1. **Komut satırı arabirimi (CLI) kullanmaya hazırlanmak için indirin ve Azure CLI'yı yükleyin.**  
    Bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

2. **Paylaşımı oluşturmak istediğiniz depolama hesabına bir bağlantı dizesi oluşturun.**  
    Değiştirin ```<storage-account>``` ve ```<resource_group>``` aşağıdaki örnekte, depolama hesabı adı ve kaynak grubu ile:

   ```azurecli
    current_env_conn_string=$(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. **Dosya paylaşımı oluşturma**
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Dosya Paylaşımını Bağlama - Windows](storage-how-to-use-files-windows.md)
* [Dosya Paylaşımını Bağlama - Linux](../storage-how-to-use-files-linux.md)
* [Dosya Paylaşımını Bağlama - macOS](storage-how-to-use-files-mac.md)

Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

* [SSS](../storage-files-faq.md)
* [Windows’da sorun giderme](storage-troubleshoot-windows-file-connection-problems.md)      
* [Linux’ta sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)   
