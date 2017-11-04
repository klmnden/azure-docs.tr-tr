---
title: "Varsayılan blob yolu Değiştir | Microsoft Docs"
description: "Bir blob dosya yolu (özel olarak incelenmektedir) yeniden adlandırmak için bir Azure işlevini ayarlamak öğrenin"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 057d4d7370207859617eb63238bf425bfa6d3e16
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="change-a-blob-path-from-the-default-path-private-preview"></a>Varsayılan yol (özel olarak incelenmektedir) blob yolunu değiştirme

Bu makalede, bir varsayılan blob dosya yolu yeniden adlandırmak için bir Azure işlevini ayarlamak açıklar. 

## <a name="prerequisites"></a>Ön koşullar

Bir kaynak grubu içinde karma veri kaynağındaki doğru şekilde yapılandırılmış bir iş tanımı olduğundan emin olun.

## <a name="create-an-azure-function"></a>Bir Azure işlevi oluşturma

Bir Azure işlevi oluşturmak için aşağıdakileri yapın:

1. [Azure Portal](http://portal.azure.com/) gidin.

2. Sol bölmenin en üstünde tıklatın **yeni**. 

3. İçinde **arama** kutusuna **işlev uygulaması**, ve ardından Enter tuşuna basın.

    ![Arama kutusuna "İşlev uygulaması" yazın](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. İçinde **sonuçları** tıklatın **işlev uygulaması**.

    ![Sonuçlar listesinde işlevi Uygulama kaynağı seçin](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    **İşlev uygulaması** penceresi açılır.

5. **Oluştur**'a tıklayın.

    ![İşlev uygulaması penceresini "Oluştur" düğmesine](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. Üzerinde **işlev uygulaması** yapılandırma dikey penceresinde aşağıdakileri yapın:

    a. İçinde **uygulama adı** kutusunda, uygulama adını yazın.
    
    b. İçinde **abonelik** abonelik adını yazın.

    c. Altında **kaynak grubu**, tıklatın **Yeni Oluştur**ve ardından kaynak grubunun adını yazın.

    d. İçinde **barındırma planına** kutusuna **tüketim planlama**.

    e. İçinde **konumu** konumunu yazın.

    f. Altında **depolama hesabı**, mevcut bir depolama hesabını seçin veya yeni bir depolama hesabı oluşturun. Bir depolama hesabı işlevi için dahili olarak kullanılır.

    ![Yeni işlev uygulaması yapılandırma verilerini girin](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. **Oluştur**'a tıklayın.  
    İşlev uygulaması oluşturulur.

8. Sol bölmede **daha fazla hizmet**, ve ardından aşağıdakileri yapın:
    
    a. İçinde **filtre** kutusuna **uygulama hizmetleri**.
    
    b. Tıklatın **uygulama hizmetleri**. 

    ![Sol bölmede "Daha fazla hizmet" bağlantısına](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. Uygulama hizmetleri listesinde, işlev uygulaması adına tıklayın.  
    İşlev uygulama sayfası açılır.

10. Sol bölmede **yeni işlev**, ve ardından aşağıdakileri yapın: 

    a. İçinde **dil** listesinde **C#**.
    
    b. Şablon döşeme dizisinde seçin **QueueTrigger CSharp**.

    c. İçinde **işlevinizi ad** işlevinizi için bir ad yazın.

    d. İçinde **sıra adı** veri dönüştürme işi tanımı adınızı yazın.

    e. Altında **depolama hesabı bağlantısı**, tıklatın **yeni**ve ardından veri dönüştürme işi karşılık gelen bir hesap seçin.  
        Bağlantı adı not edin. Daha sonra Azure işlevinde adı gerekiyor.

       ![Yeni bir C# işlevi oluşturma](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. **Oluştur**'a tıklayın.  
    **İşlevi** penceresi açılır.

11. İçinde **işlevi** penceresinde çalıştırın, _.csx_ dosyasını bulun ve ardından aşağıdakileri yapın:

    a. Aşağıdaki kodu yapıştırın:

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

    b. Değiştir **STORAGE_CONNECTIONNAME** üzerinde 11, depolama hesabı bağlantınızdaki satır (noktası 8 c bakın).

    c. En üstünde sol tıklayın **kaydetmek**.

    ![İşlev Kaydet](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. İşlev tamamlamak için aşağıdakileri yaparak bir daha fazla dosya ekleyin:

    a. Tıklatın **dosyaları görüntüleyebilir**.

       !["Dosyaları Görüntüle" bağlantısını](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. **Ekle**'ye tıklayın.
    
    c. Tür **project.json**, ve ardından Enter tuşuna basın.
    
    d. İçinde **project.json** dosya, aşağıdaki kodu yapıştırın:

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

    e. **Kaydet** düğmesine tıklayın.

Bir Azure işlevi oluşturdunuz. Bu işlev, yeni blob veri dönüştürme işlemi tarafından oluşturulan her zaman tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md)
