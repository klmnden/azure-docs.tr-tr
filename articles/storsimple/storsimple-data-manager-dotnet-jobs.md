---
title: Microsoft Azure StorSimple veri Yöneticisi işleri için .NET SDK'sını kullanma | Microsoft Docs
description: StorSimple veri Yöneticisi işleri başlatmak için .NET SDK'sını kullanmayı öğrenin
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
ms.openlocfilehash: 80f01a926b94deebab59f8ef91bfc36a4600b5f0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60632405"
---
# <a name="use-the-net-sdk-to-initiate-data-transformation"></a>Veri dönüştürme başlatmak için .NET SDK'sını kullanma

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple cihaz verileri dönüştürmek için veri dönüştürme özelliği StorSimple veri Yöneticisi hizmeti içinde nasıl kullanabileceğiniz açıklanmaktadır. Dönüştürülen verileri, ardından bulutta diğer Azure Hizmetleri tarafından kullanılır.

Bir veri dönüşüm işi iki yolla başlatabilirsiniz:

- .NET SDK’yı kullanma
- Azure Otomasyonu runbook'u kullanın
 
  Bu makalede, bir veri dönüşüm işi başlatın ve ardından tamamlanmasını izlemek için bir .NET konsol uygulaması oluşturma işlemi açıklanmaktadır. Otomasyon aracılığıyla veri dönüştürme başlatma hakkında daha fazla bilgi için şuraya gidin [tetikleyici veri dönüştürme işleri kullanım Azure Otomasyonu runbook'una](storsimple-data-manager-job-using-automation.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapın:
*   Çalıştıran bir bilgisayar:

    - Visual Studio 2012, 2013, 2015 veya 2017.

    - Azure Powershell. [Azure Powershell indirme](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Bir kaynak grubu içinde doğru yapılandırılmış iş tanımı StorSimple veri Yöneticisi'nde.
*   Tüm gerekli DLL'lerin. Bu DLL'leri indirin [GitHub deposu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   [`Get-ConfigurationParams.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) GitHub deposundan betiği.

## <a name="step-by-step-procedure"></a>Adım adım yordam

Bir veri dönüşüm işi başlatmak için .NET kullanmak için aşağıdaki adımları gerçekleştirin.

1. Yapılandırma parametreleri almak için aşağıdaki adımları uygulayın:
    1. İndirme `Get-ConfigurationParams.ps1` içinde GitHub deposu betikten `C:\DataTransformation` konumu.
    1. Çalıştırma `Get-ConfigurationParams.ps1` GitHub deposundan bir betik. Aşağıdaki komutu yazın:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        AppName ve ActiveDirectoryKey için herhangi bir değer geçirebilirsiniz.

2. Bu betik, aşağıdaki değerleri çıkarır:
    * İstemci Kimliği
    * Kiracı Kimliği
    * Active Directory anahtarı (aynı Yukarıda girilen)
    * Abonelik Kimliği

        ![Yapılandırma parametreleri betik çıktısı](media/storsimple-data-manager-dotnet-jobs/get-config-parameters.png)

3. 2013 veya 2015, Visual Studio 2012 kullanarak, oluşturun bir C# .NET konsol uygulaması.

    1. Başlatma **Visual Studio 2012/2013/2015**.
    1. **Dosya > Yeni > Proje**'yi seçin.

        ![1 proje oluşturma](media/storsimple-data-manager-dotnet-jobs/create-new-project-7.png)        
    2. Seçin **yüklü > şablonları > Visual C# > konsol uygulaması**.
    3. Girin **DataTransformationApp** için **adı**.
    4. Seçin **C:\DataTransformation** için **konumu**.
    6. Projeyi oluşturmak için **Tamam**'a tıklayın.

        ![2 proje oluşturma](media/storsimple-data-manager-dotnet-jobs/create-new-project-1.png)

4. Şimdi tüm DLL'leri mevcut ekleyin [DLL'leri klasör](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) olarak **başvuruları** oluşturduğunuz projede. Dll dosyaları eklemek için aşağıdakileri gerçekleştirin:

   1. Visual Studio'da Git **Görüntüle > Çözüm Gezgini**.
   2. Veri dönüştürme uygulama projesi solundaki oka tıklayın. Tıklayın **başvuruları** ve ardından sağ tıklatarak **Başvuru Ekle**.
    
       ![DLL'leri 1 Ekle](media/storsimple-data-manager-dotnet-jobs/create-new-project-4.png)

   3. Paketleri klasör konumuna göz atın, tüm DLL'leri seçip tıklayın **Ekle**ve ardından **Tamam**.

       ![DLL'leri 2 Ekle](media/storsimple-data-manager-dotnet-jobs/create-new-project-6.png)

5. Aşağıdaki **using** bildirimlerini projedeki kaynak dosyasına (Program.cs) ekleyin.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```
    
6. Aşağıdaki kod, veri dönüştürme işi örneği başlatır. Bu ekleme **Main yöntemi**. Daha önce elde edilen yapılandırma parametrelerinin değerlerini değiştirin. Değerlerine takın **kaynak grubu adı** ve **ResourceName**. **ResourceGroupName** StorSimple veri iş tanımı yapılandırılmışsa Yöneticisi ile ilişkilidir. **ResourceName** StorSimple veri Yöneticisi'ni hizmetinizi adıdır.

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
   
7. Parametreleri ile iş tanımı çalıştırılması gerektiğini belirtin

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);
    ```

    (VEYA)

    Çalışma zamanı sırasında iş tanımı parametrelerini değiştirmek istiyorsanız, ardından aşağıdaki kodu ekleyin:

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

8. Başlatmadan sonra bir iş tanımı veri dönüşüm işi tetiklemesi için aşağıdaki kodu ekleyin. Uygun takın **iş tanımı adı**.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);
    Console.WriteLine("jobid: ", jobId);
    Console.ReadLine();

    ```
    Kod yapıştırıldıktan sonra çözümü oluşturun. Veri dönüştürme işi örneği başlatmak için kod parçacığı bir ekran görüntüsü aşağıda verilmiştir.

   ![Veri dönüştürme işi başlatmak için kod parçacığı](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet-1.png)

9. Bu proje kök dizininde eşleşen verileri dönüştürür ve dosya içinde bir StorSimple biriminin filtreler ve belirtilen kapsayıcısı/dosya paylaşımına koyar. Bir dosyaya dönüştürüldüğünde (aynı depolama hesabındaki kapsayıcısı/dosya paylaşımının) iş tanımı olarak aynı ada sahip bir depolama kuyruğuna bir ileti eklenir. Bu iletiyi bir daha fazla dosyanın işleme başlatmak için tetikleyici olarak kullanılabilir.

10. İş tetiklendikten sonra işin tamamlanmasını izlemek için aşağıdaki kodu kullanın. İş çalıştırma için bu kodu eklemek için zorunlu değildir.

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
    .NET kullanarak işini tetiklemek için kullanılan tüm kod örneği, bir ekran görüntüsü aşağıda verilmiştir.

    ![Tam .NET işini tetiklemek için kod parçacığı](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple veri Yöneticisi'ni kullanın, verilerinizi dönüştürmek için kullanıcı Arabirimi](storsimple-data-manager-ui.md).
