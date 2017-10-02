---
title: "Azure Dosya paylaşımı oluşturma | Microsoft Docs"
description: "Azure portalı, PowerShell ve Azure CLI kullanarak Azure Dosyaları'nda bir Azure dosya paylaşımı oluşturma."
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: bc01e5427f32e9532e39694f6de9f0b1146eda35
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---
# <a name="create-a-file-share-in-azure-files"></a>Azure Dosyaları'nda bir dosya paylaşımı oluşturma
[Azure portalını](https://portal.azure.com/), Azure Storage PowerShell cmdlet'lerini, Azure Storage istemcisi kitaplıklarını veya Azure Storage REST API'sini kullanarak Azure dosya paylaşımları oluşturabilirsiniz. Bu öğreticide şunları öğreneceksiniz:
* [Azure portalını kullanarak Azure Dosya paylaşımı oluşturma](#Create file share through the Portal)
* [PowerShell kullanarak Azure Dosya paylaşımı oluşturma](#Create file share using PowerShell)
* [CLI kullanarak Azure Dosya paylaşımı oluşturma](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Ön koşullar
Azure Dosya paylaşımı oluşturmak için zaten var olan bir Depolama Hesabı kullanabilir veya [yeni bir Azure Depolama Hesabı oluşturabilirsiniz](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). PowerShell ile Azure Dosya paylaşımı oluşturmak için depolama hesabınızın hesap anahtarı ve adı gerekir. PowerShell veya CLI kullanmayı planlıyorsanız Depolama hesabının anahtarı gerekir.

## <a name="create-file-share-through-the-azure-portal"></a>Azure portalı üzerinden dosya paylaşımı oluşturma
1. **Azure portalındaki Depolama Hesabı dikey penceresine gidin**:    
    ![Depolama Hesabı dikey penceresi](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Dosya Paylaşımı ekleme düğmesine tıklayın**:    
    ![Dosya Paylaşımı ekleme düğmesine tıklayın](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Ad ve Kota belirtin. Kota şu an en çok 5 TiB olabilir**:    
    ![Yeni dosya paylaşımı için ad ve istenen kotayı sağlayın](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Yeni dosya paylaşımınızı görüntüleyin**: ![Yeni dosya paylaşımınızı görüntüleyin](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Dosyayı karşıya yükleyin**: ![Dosyayı karşıya yükleyin](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Dosya paylaşımınıza göz atın ve dizinlerinizle dosyalarınız yönetin**: ![Dosya paylaşımına göz atın](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>PowerShell üzerinden dosya paylaşımı oluşturma
PowerShell’i kullanmaya hazırlamak için Azure PowerShell cmdlet’lerini indirin ve yükleyin. Yükleme noktası ve yükleme yönergeleri için bkz. [Azure PowerShell’i yükleme ve yapılandırma](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).

> [!Note]  
> En güncel Azure PowerShell modülünü indirmeniz ve yüklemeniz veya yükseltmeniz önerilir.

1. **Depolama hesabınız ve anahtarınız için bir bağlam oluşturun** Bağlam, depolama hesabı adını ve hesap anahtarını kapsar. Hesap anahtarını [Azure portalından](https://portal.azure.com/) kopyalama yönergeleri için bkz. [Depolama erişim anahtarlarını görüntüleme ve kopyalama](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Yeni dosya paylaşımını oluşturun**:    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarının ve dosyaların adlandırılması hakkında tüm ayrıntılara ulaşmak için bkz. [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Bunlara Başvuruda Bulunma](https://msdn.microsoft.com/library/azure/dn167011.aspx)

## <a name="create-file-share-through-command-line-interface-cli"></a>Komut Satırı Arabirimi (CLI) üzerinden dosya paylaşımı oluşturma
1. **Komut Satırı Arabirimi (CLI) kullanmaya hazırlanmak için Azure CLI’yi indirin ve yükleyin.**  
    Bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-az-cli2.md) ve [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli.md).

2. **Paylaşımı oluşturmak istediğiniz depolama hesabına bir bağlantı dizesi oluşturun.**  
    ```<storage-account>``` ve ```<resource_group>``` değerlerini aşağıdaki örnekte olduğu gibi depolama hesabı adı ve kaynak grubuyla değiştirin:

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

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
