---
title: Depolamaya ve Blob depolamadan Azure Depolama Gezgini ile veri taşıma | Microsoft Docs
description: Azure Depolama Gezgini’ni kullanarak Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 464290901959ee052059b092b737e369928bd19b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837798"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Azure Storage Gezgini kullanarak Azure Blob Storage gelen ve veri taşıma
Azure Storage Gezgini, Windows, macOS ve Linux Azure Storage verilerle çalışmanıza olanak sağlayan bir Microsoft ücretsiz bir araçtır. Bu konu, karşıya yükleme ve verileri Azure blob depolama alanından indirmek için kullanımını açıklar. Aracı yüklenebilir [Microsoft Azure Storage Gezgini](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Tarafından sağlanan kodlarıyla ayarlanan VM kullanıyorsanız [veri bilimi sanal makinelerle](virtual-machines.md), sonra da Azure Storage Gezgini VM üzerinde zaten yüklü.
> 
> [!NOTE]
> Azure blob depolama tam bir giriş için bkz [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu belge, Azure aboneliği, bir depolama hesabı ve o hesap için karşılık gelen depolama anahtarı sahip olduğunuzu varsayar. Karşıya yükleme/veri yüklemeden önce Azure depolama hesabı adı ve hesap anahtarınızın bilmeniz gerekir. 

* Bir Azure aboneliği ayarlamak için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler ve hesabı ve anahtarı bilgilerini almak için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md). Azure Storage Gezgini aracıyla hesabına bağlamak için bu anahtarı gerektiği depolama hesabınız için erişim anahtarı not edin.
* Azure Storage Gezgini aracı yüklenebilir [Microsoft Azure Storage Gezgini](http://storageexplorer.com/). Yükleme sırasında Varsayılanları kabul edin.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanın
Aşağıdaki adımları nasıl Azure Storage Gezgini kullanarak verileri karşıya yükleme/indirme belge. 

1. Microsoft Azure Storage Gezgini başlatın.
2. Ortaya çıkarmak için **hesabınızda oturum açın...**  seçin **Azure hesap ayarlarını** simgesine ve sonra **Hesap Ekle** ve kimlik bilgilerini girin. ![Bir Azure depolama hesabı ekleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Ortaya çıkarmak için **Azure Storage Bağlan** seçin **Azure depolama Bağlan** simgesi. ![Azure depolama birimini bağlayın](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Azure storage hesabınızdan erişim anahtarını girin **Azure Storage Bağlan** Sihirbazı'nı ve ardından **sonraki**. ![Azure depolama birimini bağlayın](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Depolama hesabı adı girin **hesap adı** kutusuna ve ardından **sonraki**. ![Harici depolama ekleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Eklenen depolama hesabı listelenmiş olmalıdır. Bir depolama hesabında blob kapsayıcısı oluşturmak için **Blob kapsayıcıları** bu hesap, select düğümünde **Blob kapsayıcısı oluşturmak**ve bir ad girin.
7. Verileri bir kapsayıcıya yüklemek için hedef kapsayıcıyı seçin ve **karşıya** düğmesini.![ Depolama hesapları](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Tıklayın **...**  sağındaki **dosyaları** kutusunda, dosya sisteminden karşıya tıklatıp bir veya birden çok dosya seçin **karşıya** dosyaları karşıya başlamaya.![ Dosyaları karşıya yükleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Karşıdan yükle ve'ı tıklatın karşılık gelen kapsayıcıda blob seçerek veri indirmek için **karşıdan**. ![Dosyaları indirme](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

