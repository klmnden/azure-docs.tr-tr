---
title: Varsayılan blob yolu Değiştir | Microsoft Docs
description: Bir Azure işlevi, bir blob dosya yolu yeniden adlandırmak için ayarlama hakkında bilgi edinin
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: cdaf991c25c23dee4f87b44142c1482bf892bcf2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723811"
---
# <a name="change-a-blob-path-from-the-default-path"></a>Varsayılan yolu bir blob yolunu değiştirme

StorSimple veri Yöneticisi hizmeti verileri dönüştürür, varsayılan olarak, dönüştürülen bloblar bir kapsayıcıda depolama belirtilen hedef depo oluşturma sırasında yerleştirir. Blobları bu konumda geldikçe bu bloblarda alternatif bir konuma taşımak isteyebilirsiniz. Bu makalede, bir varsayılan blob dosya yolu yeniden adlandırın ve bu nedenle blobları için farklı bir konuma taşımak için bir Azure işlevini ayarlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

StorSimple veri Yöneticisi hizmetinizde doğru yapılandırılmış iş tanımı olduğundan emin olun.

## <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Bir Azure işlevi oluşturmak için aşağıdaki adımları gerçekleştirin:

1. [Azure Portal](https://portal.azure.com/) gidin.

2. Tıklayın **+ kaynak Oluştur**. İçinde **arama** kutusuna **işlev uygulaması** basın **Enter**. Seçin ve tıklayın **işlev uygulaması** görüntülenen uygulama listesinde.

    ![Arama kutusuna "İşlev uygulaması"](./media/storsimple-data-manager-change-default-blob-path/search-function-app.png)

3. **Oluştur**’a tıklayın.

    ![İşlev uygulaması penceresi "Oluştur" düğmesine](./media/storsimple-data-manager-change-default-blob-path/create-function-app.png)

4. Üzerinde **işlev uygulaması** yapılandırma dikey penceresinde aşağıdaki adımları gerçekleştirin:

    1. Benzersiz bir sağlamak **uygulama adı**.
    2. Açılır listeden seçin **abonelik**. Bu abonelik, StorSimple veri Yöneticisi hizmeti ile ilişkilendirilmiş olanla aynı olması gerekir.
    3. Seçin **Yeni Oluştur** kaynak grubu.
    4. İçin **barındırma planı** açılan listesinden **tüketim planı**.
    5. İşlevinizin çalıştırıldığı bir konum belirtin. StorSimple veri Yöneticisi hizmeti ve iş tanımıyla ilişkili depolama hesabına yerleştirildiği aynı bölgede kullanmanız gerekir.
    6. Mevcut bir depolama hesabını seçin veya yeni bir depolama hesabı oluşturun. Bir depolama hesabı, işlev için dahili olarak kullanılır.

        ![Yeni işlev uygulaması yapılandırma verilerini girin](./media/storsimple-data-manager-change-default-blob-path/function-app-parameters.png)

    7. **Oluştur**’a tıklayın. İşlev uygulaması oluşturulur.
     
        ![Oluşturulan işlev uygulaması](./media/storsimple-data-manager-change-default-blob-path/function-app-created.png)

5. Seçin **işlevleri**, tıklatıp **+ yeni işlev**.

    ![Tıklama + yeni işlev](./media/storsimple-data-manager-change-default-blob-path/create-new-function.png)

6. Seçin **C#** dil için. İçinde bir dizi şablon kutucuk seçin **C#** içinde **QueueTrigger-CSharp** Döşe.

7. İçinde **kuyruk tetikleyicisi**:

    1. Girin bir **adı** işleviniz için.
    2. İçinde **kuyruk adı** veri dönüştürme iş tanımı adı yazın.
    3. Altında **depolama hesabı bağlantısı**, tıklayın **yeni**. Depolama hesapları listesinden, iş tanımıyla ilişkili hesabı seçin. Bağlantı adı (vurgulu) not edin. Daha sonra Azure işlev adı gereklidir.

        ![Yeni bir C# işlevi](./media/storsimple-data-manager-change-default-blob-path/new-function-parameters.png)

    4. **Oluştur**’a tıklayın. **İşlevi** oluşturulur.

     
10. İşlev penceresinde çalıştırın _.csx_ dosya.

    ![Yeni bir C# işlevi](./media/storsimple-data-manager-change-default-blob-path/new-function-run-csx.png)
    
    Aşağıdaki adımları uygulayın.

    1. Aşağıdaki kodu yapıştırın:

        ```
        using System;
        using System.Configuration;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Queue;
        using Microsoft.WindowsAzure.Storage;
        using System.Collections.Generic;
        using System.Linq;

        public static void Run(QueueItem myQueueItem, TraceWriter log)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

            string storageAccUriEndswith = "windows.net/";
            string uri = myQueueItem.TargetLocation.Replace("%20", " ");
            log.Info($"Blob Uri: {uri}");

            // Remove storage account uri string
            uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

            string containerName = uri.Substring(0, uri.IndexOf("/")); 

            // Remove container name string
            uri = uri.Substring(containerName.Length + 1);

            // Current blob path
            string blobName = uri; 

            string volumeName = uri.Substring(containerName.Length + 1);
            volumeName = uri.Substring(0, uri.IndexOf("/"));

            // Remove volume name string
            uri = uri.Substring(volumeName.Length + 1);

            string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
            string newBlobName = uri.Substring(newContainerName.Length + 1);

            log.Info($"Container name: {containerName}");
            log.Info($"Volume name: {volumeName}");
            log.Info($"New container name: {newContainerName}");

            log.Info($"Blob name: {blobName}");
            log.Info($"New blob name: {newBlobName}");

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Container reference
            CloudBlobContainer container = blobClient.GetContainerReference(containerName);
            CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
            newContainer.CreateIfNotExists();

            if(!container.Exists())
            {
                log.Info($"Container - {containerName} not exists");
                return;
            }

            if(!newContainer.Exists())
            {
                log.Info($"Container - {newContainerName} not exists");
                return;
            }

            CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
            if (!blob.Exists())
            {
                // Skip to copy the blob to new container, if source blob doesn't exist
                log.Info($"The specified blob does not exist.");
                log.Info($"Blob Uri: {blob.Uri}");
                return;
            }

            CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
            if (!blobCopy.Exists())
            {
                blobCopy.StartCopy(blob);
                // Delete old blob, after copy to new container
                blob.DeleteIfExists();
                log.Info($"Blob file path renamed completed successfully");
            }
            else
            {
                log.Info($"Blob file path renamed already done");
                // Delete old blob, if already exists.
                blob.DeleteIfExists();
            }
        }

        public class QueueItem
        {
            public string SourceLocation {get;set;}
            public long SizeInBytes {get;set;}
            public string Status {get;set;}
            public string JobID {get;set;}
            public string TargetLocation {get; set;}
        }

        ```

    2. Değiştirin **STORAGE_CONNECTIONNAME** 11, depolama hesabı bağlantısı ile'on satır (adım 7 c bakın).

        ![Depolama bağlantı adı Kopyala](./media/storsimple-data-manager-change-default-blob-path/new-function-storage-connection-name.png)

    3. **Kaydet** işlevi.

        ![İşlev Kaydet](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. İşlev tamamlamak için aşağıdaki adımları uygulayarak bir daha fazla dosya ekleyin:

    1. Tıklayın **dosyaları görüntüleyin**.

       !["Dosyaları görüntüleme" bağlantısı](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    2. **+ Ekle**'ye tıklayın.
        
        !["Dosyaları görüntüleme" bağlantısı](./media/storsimple-data-manager-change-default-blob-path/new-function-add-file.png)
    
    3. Tür **project.json**ve tuşuna **Enter**. İçinde **project.json** dosyasında, aşağıdaki kodu yapıştırın:

        ```
        {
        "frameworks": {
            "net46":{
            "dependencies": {
                "windowsazure.storage": "8.1.1"
            }
            }
        }
        }

        ```

    
    4. **Kaydet**’e tıklayın.

        !["Dosyaları görüntüleme" bağlantısı](./media/storsimple-data-manager-change-default-blob-path/new-function-project-json.png)

Bir Azure işlevi oluşturdunuz. Bu işlev, yeni bir blob veri dönüşüm işi tarafından üretildiği her seferinde tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md)
