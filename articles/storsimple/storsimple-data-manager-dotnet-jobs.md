---
title: "Microsoft Azure StorSimple veri Yöneticisi işleri .NET SDK'yı kullanma | Microsoft Docs"
description: "StorSimple veri Yöneticisi işleri başlatmak için .NET SDK'sını kullanmayı öğrenin"
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
ms.openlocfilehash: 7ecb3ed41a8a05f3ced2488226fa0380107b1b43
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="use-the-net-sdk-to-initiate-data-transformation"></a>Veri dönüştürme başlatmak için .net SDK'sını kullanın

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple cihaz verileri dönüştürmek için StorSimple veri Yöneticisi hizmeti içindeki veri dönüştürme özelliği nasıl kullanabileceğiniz açıklanır. Dönüştürülen veriler daha sonra diğer Azure hizmetleriyle bulutta tarafından mı tüketiliyor.

İki yolla veri dönüştürme işi başlatabilirsiniz:

 - .NET SDK’yı kullanma
 - Azure Otomasyonu runbook kullanın
 
 Bu makalede, veri dönüştürme işi başlatmak ve tamamlanmasını izlemek için örnek bir .NET konsol uygulaması oluşturmak nasıl ayrıntıları verilmektedir. Veri dönüştürme Otomasyon aracılığıyla başlatmak hakkında daha fazla bilgi için şuraya gidin [tetikleyici veri dönüştürme işleri için kullanım Azure Otomasyonu runbook'unu](storsimple-data-manager-job-using-automation.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapın:
*   Çalıştıran bir bilgisayar:

    - Visual Studio 2012 2013, 2015 veya 2017.

    - Azure Powershell. [Azure Powershell indirme](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Bir kaynak grubu içinde doğru yapılandırılmış iş tanımı StorSimple veri Yöneticisi'nde.
*   Tüm gerekli DLL'leri. Bu DLL'lerden karşıdan [GitHub deposunu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   [`Get-ConfigurationParams.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1)GitHub depo betikten.

## <a name="step-by-step-procedure"></a>Adım adım yordam

Veri dönüştürme işi başlatmak için .NET kullanmak için aşağıdaki adımları gerçekleştirin.

1. Yapılandırma parametreleri almak için aşağıdaki adımları uygulayın:
    1. Karşıdan `Get-ConfigurationParams.ps1` GitHub depo komut `C:\DataTransformation` konumu.
    1. Çalıştırma `Get-ConfigurationParams.ps1` GitHub deposuna betikten. Aşağıdaki komutu yazın:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        AppName ve ActiveDirectoryKey için herhangi bir değer geçirebilirsiniz.

2. Bu komut dosyasını aşağıdaki değerleri çıkarır:
    * İstemci Kimliği
    * Kiracı Kimliği
    * Active Directory anahtar (aynı Yukarıda girilen)
    * Abonelik Kimliği

        ![Yapılandırma parametreleri betik çıktısı](media/storsimple-data-manager-dotnet-jobs/get-config-parameters.png)

3. Visual Studio 2012 kullanarak, 2013 veya 2015, C# .NET konsol uygulaması oluşturun.

    1. Başlatma **Visual Studio 2012/2013/2015**.
    1. Seçin **Dosya > Yeni > Proje**.

        ![1 proje oluşturma](media/storsimple-data-manager-dotnet-jobs/create-new-project-7.png)        
    2. Seçin **yüklü > şablonları > Visual C# > konsol uygulaması**.
    3. Girin **DataTransformationApp** için **adı**.
    4. Seçin **C:\DataTransformation** için **konumu**.
    6. Projeyi oluşturmak için **Tamam**'a tıklayın.

        ![2 proje oluşturma](media/storsimple-data-manager-dotnet-jobs/create-new-project-1.png)

4.  Şimdi, mevcut tüm DLL'ler ekleyin [DLL'leri klasörü](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) olarak **başvuruları** oluşturduğunuz projesinde. Dll dosyaları indirmek için aşağıdakileri gerçekleştirin:

    1. Visual Studio'da Git **Görünüm > Çözüm Gezgini**.
    2. Veri dönüştürme uygulama projesi solundaki oka tıklayın. Tıklatın **başvuruları** ve ardından sağ tıklatarak **Başvuru Ekle**.
    
        ![DLL'leri 1 ekleme](media/storsimple-data-manager-dotnet-jobs/create-new-project-4.png)

    3. Paketleri klasör konumuna göz atın, tüm DLL'ler seçin ve tıklatın **Ekle**ve ardından **Tamam**.

        ![DLL'leri 2 ekleme](media/storsimple-data-manager-dotnet-jobs/create-new-project-6.png)

5. Aşağıdaki **using** bildirimlerini projedeki kaynak dosyasına (Program.cs) ekleyin.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```
    
6. Aşağıdaki kod, veri dönüştürme işi örneği başlatır. Bu konuda eklemek **Main yönteminin**. Yapılandırma parametrelerinin değerlerini daha önce edindiğiniz şekilde değiştirin. Değerlerini içinde takın **kaynak grubu adı** ve **ResourceName**. **ResourceGroupName** StorSimple veri iş tanımı yapılandırılmışsa Yöneticisi ile ilişkilidir. **ResourceName** , StorSimple veri Yöneticisi hizmetin adıdır.

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
   Kod yapıştırıldıktan sonra çözümü oluşturun. Veri dönüştürme işi örneği başlatmak için kod parçacığını bir ekran görüntüsü aşağıda verilmiştir.

   ![Veri dönüştürme işlemini başlatmak için kod parçacığını](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet-1.png)

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
 .NET kullanarak iş tetiklemek için kullanılır tüm kod örnek bir ekran görüntüsü aşağıda verilmiştir.

 ![Tam bir .NET iş tetiklemek için kod parçacığını](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).
