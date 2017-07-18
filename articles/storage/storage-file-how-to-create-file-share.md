---
title: "Azure Dosya Paylaşımı oluşturma | Microsoft Docs"
description: "Azure portal, PowerShell ve Azure CLI kullanarak Azure Dosya depolamada bir Azure dosya paylaşımı nasıl oluşturulur?"
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
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: bfe12fbdd50e82404ff49b1e34adc03913e50905
ms.contentlocale: tr-tr
ms.lasthandoff: 07/12/2017

---
# <a name="create-a-file-share-in-azure-file-storage"></a>Azure Dosya depolamada bir Dosya Paylaşımı oluşturma
[Azure portalını](https://portal.azure.com/), Azure Depolama PowerShell cmdlet’lerini, Azure Depolama istemci kitaplıklarını veya Azure Depolama REST API’sini kullanarak Azure dosya paylaşımları oluşturabilirsiniz. Bu öğreticide şunlar gösterilecek:
* [Azure portalını kullanarak Azure Dosya paylaşımı oluşturma](#Create file share through the Portal)
* [PowerShell kullanarak Azure Dosya paylaşımı oluşturma](#Create file share using PowerShell)
* [CLI kullanarak Azure Dosya paylaşımı oluşturma](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Ön koşullar
Azure Dosya paylaşımı oluşturmak için zaten var olan bir depolama hesabı kullanabilir veya [yeni bir Azure depolama hesabı oluşturabilirsiniz](storage-create-storage-account.md). PowerShell ile Azure Dosya paylaşımı oluşturmak için depolama hesabınızın hesap anahtarı ve adı gerekir. PowerShell veya CLI kullanmayı planlıyorsanız Depolama hesabının anahtarı gerekir.

## <a name="create-file-share-through-the-portal"></a>Portal üzerinden dosya paylaşımı oluşturma
1. **Azure Portal’ındaki Depolama Hesabı dikey penceresine gidin**:    
    ![Depolama Hesabı Dikey Penceresi](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)

2. **Dosya Paylaşımı ekleme düğmesine tıklayın**:    
    ![Dosya Paylaşımı ekleme düğmesine tıklayın](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)

3. **Ad ve Kota belirtin. Kota şu an en çok 5 TB olabilir**:    
    ![Yeni dosya paylaşımı için ad ve istenen kotayı sağlayın](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)

4. **Yeni dosya paylaşımınızı görüntüleyin**: ![Yeni dosya paylaşımınızı görüntüleyin](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)

5. **Dosyayı karşıya yükleyin**: ![Dosyayı karşıya yükleyin](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)

6. **Dosya paylaşımınıza göz atın ve dizinlerinizle dosyalarınız yönetin**: ![Dosya paylaşımına göz atın](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>PowerShell üzerinden dosya paylaşımı oluşturma
PowerShell’i kullanmaya hazırlamak için Azure PowerShell cmdlet’lerini indirin ve yükleyin. Yükleme noktası ve yükleme yönergeleri için bkz. [Azure PowerShell’i yükleme ve yapılandırma](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).

> [!Note]  
> En güncel Azure PowerShell modülünü indirmeniz ve yüklemeniz veya yükseltmeniz önerilir.

1. **Depolama hesabınız ve anahtarınız için bir bağlam oluşturun** Bağlam, depolama hesabı adını ve hesap anahtarını kapsar. Hesap anahtarını [Azure portalından](https://portal.azure.com/) kopyalama yönergeleri için bkz. [Depolama erişim anahtarlarını görüntüleme ve kopyalama](storage-create-storage-account.md#view-and-copy-storage-access-keys).

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
    ```<storage-account>``` ve ```<resource_group>``` değerlerini aşağıdaki örnekte olduğu gibi depolama hesabı adı ve kaynak grubuyla değiştirin.

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
* [Dosya Paylaşımını Bağlama - Windows](storage-file-how-to-use-files-windows.md)
* [Dosya Paylaşımını Bağlama - Linux](storage-how-to-use-files-linux.md)
* [Dosya Paylaşımını Bağlama - macOS](storage-file-how-to-use-files-mac.md)

Azure File Storage hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

* [SSS](storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-file-connection-problems.md)
