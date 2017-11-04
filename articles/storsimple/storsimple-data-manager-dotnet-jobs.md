---
title: "Microsoft Azure StorSimple veri Yöneticisi işleri .NET SDK'yı kullanma | Microsoft Docs"
description: "StorSimple veri Yöneticisi işleri (özel olarak incelenmektedir) başlatmak için .NET SDK'sını kullanmayı öğrenin"
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
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 44d243a034b20b99faf284c8615e470bc6f9d020
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-net-sdk-to-initiate-data-transformation-private-preview"></a>Veri dönüştürme (özel olarak incelenmektedir) başlatmak için .net SDK'sını kullanın

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple cihaz verileri dönüştürmek için StorSimple veri Yöneticisi hizmeti içindeki veri dönüştürme özelliği nasıl kullanabileceğiniz açıklanır. Dönüştürülen veriler daha sonra diğer Azure hizmetleriyle bulutta tarafından mı tüketiliyor. Makale ayrıca veri dönüştürme işi başlatmak için örnek bir .NET konsol uygulaması oluşturmaya yardımcı olmak için bir kılavuz vardır ve tamamlanmasını izlemek.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapın:
*   Visual Studio 2012, 2013, 2015 veya yüklü 2017 sistemiyle.
*   Azure Powershell yüklü. [Azure Powershell indirme](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Veri dönüştürme işlemini başlatmak için yapılandırma ayarlarını (Bu ayarları almak için yönergeleri dahil burada).
*   Bir kaynak grubu içinde karma veri kaynağındaki doğru şekilde yapılandırılmış bir iş tanımı.
*   Tüm gerekli DLL'leri. Bu DLL'lerden karşıdan [GitHub deposunu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[betik](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) github'da depodan.

## <a name="step-by-step"></a>Adım adım

Veri dönüştürme işi başlatmak için .NET kullanmak için aşağıdaki adımları gerçekleştirin.

1. Yapılandırma parametreleri almak için aşağıdaki adımları uygulayın:
    1. Karşıdan `Get-ConfigurationParams.ps1` github depo komut `C:\DataTransformation` konumu.
    1. Çalıştırma `Get-ConfigurationParams.ps1` github deposuna betikten. Aşağıdaki komutu yazın:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        AppName ve ActiveDirectoryKey için herhangi bir değer geçirebilirsiniz.


2. Bu komut dosyasını aşağıdaki değerleri çıkarır:
    * İstemci kimliği
    * Kiracı Kimliği
    * Active Directory anahtar (aynı Yukarıda girilen)
    * Abonelik Kimliği

3. Visual Studio 2012 kullanarak, 2013 veya 2015, C# .NET konsol uygulaması oluşturun.

    1. Başlatma **Visual Studio 2012/2013/2015**.
    1. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın.
    2. **Şablonlar**’ı genişletin ve **Visual C#** seçeneğini belirleyin.
    3. Sağ taraftaki proje türleri listesinden **Konsol Uygulaması**’nı seçin.
    4. Girin **DataTransformationApp** için **adı**.
    5. Seçin **C:\DataTransformation** için **konumu**.
    6. Projeyi oluşturmak için **Tamam**'a tıklayın.

4.  Şimdi, mevcut tüm DLL'ler ekleyin [DLL'leri](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) klasörü olarak **başvuruları** oluşturduğunuz projesinde. Dll dosyaları indirmek için aşağıdakileri yapın:

    1. Visual Studio'da Git **Görünüm > Çözüm Gezgini**.
    1. Veri dönüştürme uygulama projesi solundaki oka tıklayın. Tıklatın **başvuruları** ve ardından sağ tıklatarak **Başvuru Ekle**.
    2. Paketleri klasör konumuna göz atın, tüm DLL'ler seçin ve tıklatın **Ekle**ve ardından **Tamam**.

5. Aşağıdaki **using** bildirimlerini projedeki kaynak dosyasına (Program.cs) ekleyin.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. Aşağıdaki kod, veri dönüştürme işi örneği başlatır. Bu konuda eklemek **Main yönteminin**. Yapılandırma parametrelerinin değerlerini daha önce edindiğiniz şekilde değiştirin. Değerlerini içinde takın **kaynak grubu adı** ve **karma veri kaynağı adı**. **Kaynak grubu adı** iş tanımı yapılandırılmışsa karma veri kaynağı barındıran sunucudur.

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. Hangi iş tanımı çalıştırılması gerektiğini parametrelerini belirtin

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (VEYA)

    Çalışma zamanı sırasında iş tanımı parametrelerini değiştirmek istiyorsanız, aşağıdaki kodu ekleyin:

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. Başlatma iş tanımı bir veri dönüştürme işi tetiklemek için aşağıdaki kodu ekleyin. Uygun takın **iş tanımı adını**.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. Bu iş StorSimple birim kök dizininin altındaki mevcut eşleşen dosyaları belirtilen kapsayıcı yükler. Bir dosyayı karşıya yüklendiğinde, bir ileti sırasına (aynı depolama hesabındaki kapsayıcı olarak) iş tanımı aynı ada sahip bırakılır. Bu ileti her dosyanın işlenmesi başlatmak için bir tetikleyici olarak kullanılabilir.

10. İş tetiklendi sonra iş için tamamlanma izlemek için aşağıdaki kodu ekleyin.

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).
