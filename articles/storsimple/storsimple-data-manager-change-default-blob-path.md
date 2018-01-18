---
title: "Varsayılan blob yolu Değiştir | Microsoft Docs"
description: "Bir blob dosya yolu yeniden adlandırmak için bir Azure işlevini ayarlamak öğrenin"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: f73d9dcedee5165af752b9e10fb70de860e8e98b
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="change-a-blob-path-from-the-default-path"></a>Varsayılan yolu bir blob yolunu değiştirme

StorSimple veri Yöneticisi hizmeti veri dönüşümleri, varsayılan olarak, dönüştürülmüş BLOB'ları bir depolama kapsayıcısı belirtildiği gibi hedef depo oluşturma sırasında yerleştirir. BLOB'ları bu konumda geldikçe bu BLOB'ları farklı bir konuma taşımak isteyebilirsiniz. Bu makalede, bir varsayılan blob dosya yolu yeniden adlandırın ve bu nedenle BLOB'ları farklı bir konuma taşımak için bir Azure işlevi ayarlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

StorSimple veri Yöneticisi hizmetinize doğru yapılandırılmış iş tanımı olduğundan emin olun.

## <a name="create-an-azure-function"></a>Bir Azure işlevi oluşturma

Bir Azure işlevi oluşturmak için aşağıdaki adımları gerçekleştirin:

1. [Azure Portal](http://portal.azure.com/) gidin.

2. Tıklatın **+ kaynak oluşturma**. İçinde **arama** kutusuna **işlev uygulaması** ve basın **Enter**. Tıklatıp **işlev uygulaması** görüntülenen uygulamalar listesinde.

    ![Arama kutusuna "İşlev uygulaması" yazın](./media/storsimple-data-manager-change-default-blob-path/search-function-app.png)

3. **Oluştur**’a tıklayın.

    ![İşlev uygulaması penceresini "Oluştur" düğmesine](./media/storsimple-data-manager-change-default-blob-path/create-function-app.png)

4. Üzerinde **işlev uygulaması** yapılandırma dikey penceresinde, aşağıdaki adımları gerçekleştirin:

    1. Benzersiz bir sağlamak **uygulama adı**.
    2. Açılır listeden seçin **abonelik**. Bu abonelik StorSimple veri Yöneticisi hizmetiniz ile ilişkili bir ile aynı olmalıdır.
    3. Seçin **Yeni Oluştur** kaynak grubu.
    4. İçin **barındırma planına** açılır listesinden, **tüketim planlama**.
    5. İşlevinizi çalıştığı bir konum belirtin. StorSimple veri Yöneticisi hizmeti ve iş tanımı ile ilişkilendirilmiş depolama hesabının bulunduğu aynı bölgede istiyor.
    6. Mevcut bir depolama hesabını seçin veya yeni bir depolama hesabı oluşturun. Bir depolama hesabı işlevi için dahili olarak kullanılır.

        ![Yeni işlev uygulaması yapılandırma verilerini girin](./media/storsimple-data-manager-change-default-blob-path/function-app-parameters.png)

    7. **Oluştur**’a tıklayın. İşlev uygulaması oluşturulur.
     
        ![Oluşturulan işlev uygulaması](./media/storsimple-data-manager-change-default-blob-path/function-app-created.png)

5. Seçin **işlevleri**, tıklatıp **+ yeni işlev**.

    ![Tıklatın + yeni işlevi](./media/storsimple-data-manager-change-default-blob-path/create-new-function.png)

6. Seçin **C#** dil için. Şablon döşeme dizisinde seçin **C#** içinde **QueueTrigger CSharp** döşeme.

7. İçinde **sıra tetikleyici**:

    1. Girin bir **adı** işlevinizi için.
    2. İçinde **sıra adı** , veri dönüştürme iş tanımı adını yazın.
    3. Altında **depolama hesabı bağlantısı**, tıklatın **yeni**. Depolama hesapları listesinden iş tanımıyla ilişkili hesabı seçin. Bağlantı adı (vurgulanan) not edin. Daha sonra Azure işlevinde adı gerekiyor.

        ![Yeni bir C# işlevi oluşturma](./media/storsimple-data-manager-change-default-blob-path/new-function-parameters.png)

    4. **Oluştur**’a tıklayın. **İşlevi** oluşturulur.

     
10. İşlev penceresinde çalıştırın _.csx_ dosya.

    ![Yeni bir C# işlevi oluşturma](./media/storsimple-data-manager-change-default-blob-path/new-function-run-csx.png)
    
    Aşağıdaki adımları gerçekleştirin.

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

    2. Değiştir **STORAGE_CONNECTIONNAME** üzerinde 11, depolama hesabı bağlantınızdaki satır (adım 7 c bakın).

        ![Depolama bağlantı adı kopyalayın](./media/storsimple-data-manager-change-default-blob-path/new-function-storage-connection-name.png)

    3. **Kaydet** işlevi.

        ![İşlev Kaydet](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. İşlevi tamamlamak için aşağıdaki adımları uygulayarak bir daha fazla dosya ekleyin:

    1. Tıklatın **dosyaları görüntüleyebilir**.

       !["Dosyaları Görüntüle" bağlantısını](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    2. Tıklatın **+ Ekle**.
        
        !["Dosyaları Görüntüle" bağlantısını](./media/storsimple-data-manager-change-default-blob-path/new-function-add-file.png)
    
    3. Tür **project.json**ve tuşuna basarak **Enter**. İçinde **project.json** dosya, aşağıdaki kodu yapıştırın:

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

        !["Dosyaları Görüntüle" bağlantısını](./media/storsimple-data-manager-change-default-blob-path/new-function-project-json.png)

Bir Azure işlevi oluşturdunuz. Bu işlev, yeni blob veri dönüştürme işlemi tarafından oluşturulan her zaman tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md)
