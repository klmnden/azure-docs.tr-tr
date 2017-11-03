---
title: "Program aracılığıyla akış analizi işleri izleme | Microsoft Docs"
description: "Program aracılığıyla REST API'leri, Azure SDK'sı veya PowerShell oluşturulan akış analizi işleri izleme öğrenin."
keywords: ".NET izleme, iş izleme, izleme uygulaması"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 2ec02cc9-4ca5-4a25-ae60-c44be9ad4835
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 7e9d2f6f03fd539c59b105108fb46697bcd60f1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Program aracılığıyla bir akış analizi işi İzleyicisi oluşturma

Bu makalede, Stream Analytics işi için izlemeyi etkinleştirmek gösterilmiştir. REST API'leri, Azure SDK'sı veya PowerShell oluşturulan stream Analytics işlerini izleme varsayılan olarak etkin gerekmez. El ile Azure portalında iş İzleyici sayfasına giderek ve etkinleştir düğmesine tıkladığınızda etkinleştirebilirsiniz veya bu makaledeki adımları izleyerek bu işlemi otomatikleştirebilirsiniz. İzleme verilerini Stream Analytics işiniz için Azure portal, ölçümleri alanında görünecektir.

## <a name="prerequisites"></a>Ön koşullar

Bu işlem başlamadan önce aşağıdakilere sahip olmanız gerekir:

* Visual Studio 2017 veya 2015
* [Azure .NET SDK'sı](https://azure.microsoft.com/downloads/) indirilir ve yüklenir
* İzleme etkin olması gerekiyorsa var olan bir akış analizi işi

## <a name="create-a-project"></a>Proje oluşturma

1. Visual Studio C# .NET konsol uygulaması oluşturun.
2. Paket Yöneticisi Konsolu'nda NuGet paketlerini yüklemek için aşağıdaki komutları çalıştırın. İlk Azure Stream Analytics yönetimi .NET SDK ' dir. İzlemeyi etkinleştirmek için kullanılan Azure İzleyici SDK'sı ikinci adrestir. Bir kimlik doğrulaması için kullanılacak Azure Active Directory istemcidir.
   
   ```
   Install-Package Microsoft.Azure.Management.StreamAnalytics
   Install-Package Microsoft.Azure.Insights -Pre
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```
3. Aşağıdaki appSettings bölümünü App.config dosyasına ekleyin.
   
   ```
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
     <add key="JobName" value="YOUR JOB NAME" />
     <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
     <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
   </appSettings>
   ```
   İçin değerleri *Subscriptionıd* ve *ActiveDirectoryTenantId* Azure aboneliği ve Kiracı kimlikleri. Aşağıdaki PowerShell cmdlet'ini çalıştırarak bu değerleri alabilirsiniz:
   
   ```
   Get-AzureAccount
   ```
4. Aşağıdaki using deyimlerini projenin kaynak dosyasında (Program.cs).
   
   ```
     using System;
     using System.Configuration;
     using System.Threading;
     using Microsoft.Azure;
     using Microsoft.Azure.Management.Insights;
     using Microsoft.Azure.Management.Insights.Models;
     using Microsoft.Azure.Management.StreamAnalytics;
     using Microsoft.Azure.Management.StreamAnalytics.Models;
     using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```
5. Bir kimlik doğrulama yardımcı yöntemi ekleyin.
   
     ortak statik dizesi GetAuthorizationHeader()
   
         {
             AuthenticationResult result = null;
             var thread = new Thread(() =>
             {
                 try
                 {
                     var context = new AuthenticationContext(
                         ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                         ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
   
                     result = context.AcquireToken(
                         resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                         clientId: ConfigurationManager.AppSettings["AsaClientId"],
                         redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                         promptBehavior: PromptBehavior.Always);
                 }
                 catch (Exception threadEx)
                 {
                     Console.WriteLine(threadEx.Message);
                 }
             });
   
             thread.SetApartmentState(ApartmentState.STA);
             thread.Name = "AcquireTokenThread";
             thread.Start();
             thread.Join();
   
             if (result != null)
             {
                 return result.AccessToken;
             }
   
             throw new InvalidOperationException("Failed to acquire token");
     }

## <a name="create-management-clients"></a>Yönetim istemcileri oluşturma

Aşağıdaki kod, gerekli değişkenleri ve yönetim istemcilerin ayarlarsınız.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Var olan bir akış analizi işi için izlemeyi etkinleştir

Aşağıdaki kod için izlemeyi etkinleştirir bir **varolan** Stream Analytics işi. İlk kod parçası, belirli Stream Analytics işi hakkında bilgi almak için GET isteğini Stream Analytics Hizmeti'ne karşı gerçekleştirir. Kullandığı *kimliği* Put yöntemi PUT gönderir kod yarısı isteği Öngörüler hizmeti Stream Analytics işi için izlemeyi etkinleştirmek için ikinci bir parametre olarak özellik (GET isteği alındı).

>[!WARNING]
>Daha önce farklı Stream Analytics işi, Azure Portalı aracılığıyla ya da program aracılığıyla yoluyla için izleme etkinleştirdiyseniz, kod, aşağıda **önceden izleme etkin olduğunda kullandığınız aynı depolama hesabı adı sağlamanızı öneririz.**
> 
> Depolama hesabı bölgesine Stream Analytics işiniz oluşturduğunuz, iş için özel olarak bağlanır.
> 
> Tüm Stream Analytics işleri (ve tüm diğer Azure kaynakları) aynı bölgede paylaşmak izleme verilerini depolamak için bu depolama hesabı. Farklı bir depolama hesabı belirtirseniz, bu, bir akış analizi işleri veya diğer Azure kaynaklarına izlemede istenmeyen yan etkileri neden olabilir.
> 
> Değiştirmek için kullandığınız depolama hesabı adı `<YOUR STORAGE ACCOUNT NAME>` aşağıdaki kodda için izlemeyi etkinleştirme Stream Analytics işi ile aynı abonelikte olduğundan bir depolama hesabı olmalıdır.
> 
> 

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Destek alın

Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

