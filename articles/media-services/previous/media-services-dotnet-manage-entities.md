---
title: Varlıkları ve Media Services .NET SDK'sı ile ilgili öğeleri yönetme
description: .NET, varlıkları ve Media Services SDK'sı ile ilgili öğeleri yönetmeyi öğrenin.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 1bd8fd42-7306-463d-bfe5-f642802f1906
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: a686465b0006c2e9aac6e06cb4ab12d30921e8c5
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802686"
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Varlıkları ve Media Services .NET SDK'sı ile ilgili öğeleri yönetme
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-manage-entities.md)
> * [REST](media-services-rest-manage-entities.md)
> 
> 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Bu konuda, .NET ile Azure Media Services varlıkları yönetme gösterilmektedir.

1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. Örneğin, 1 Nisan 2017'de hesabınızda 31 Aralık 2016'dan daha eski olan tüm iş kayıtları otomatik olarak silinir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, bu konuda açıklanan kodu kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

## <a name="get-an-asset-reference"></a>Bir varlık başvurusu alın
Media Services'de mevcut bir varlığa bir başvuru alma sık gerçekleştirilen bir görevdir. Aşağıdaki kod örneği nasıl bir varlık başvurusu sayfasından varlıklar koleksiyonu sunucuda bağlam nesnesi, bir varlığı kimliğe göre edinebilirsiniz gösterir. Aşağıdaki kod örneği, mevcut IAsset nesneye bir başvuru almak için bir LINQ sorgusu kullanır.

```csharp
    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }
```

## <a name="list-all-assets"></a>Tüm varlıklar listesi
Depolama alanında sahip varlıklar sayısı arttıkça, varlıklarınızı listelemek yararlıdır. Aşağıdaki kod örneği, sunucu context nesnesinde varlıklar koleksiyonu üzerinden yineleme gösterilmektedir. Her varlık ile kod örneğinde ayrıca bazı özellik değerleri konsola yazar. Örneğin, her varlık, birçok medya dosyaları içerebilir. Kod örneği, her bir varlıkla ilişkili tüm dosyaları yazar.

```csharp
    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }

        // Display output in console.
        Console.Write(builder.ToString());
    }
```

## <a name="get-a-job-reference"></a>Proje başvuru alma

İşleme görevlerini Media Services kod ile çalışırken, sıklıkla bir kimliği temel alınarak varolan bir projeyi bir başvuru almak ihtiyacınız Aşağıdaki kod örneği, işleri koleksiyondan IJob nesneye bir başvuru almak gösterilmektedir.

Uzun süre çalışan bir kodlama işi başlatılırken bir proje başvurusu alın ve bir iş parçacığında iş durumunu kontrol etmeniz gerekebilir. Bu gibi durumlarda, yöntem bir iş parçacığından döndürüldüğünde bir işi yenilenmiş bir başvuru almak gerekir.

```csharp
    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }
```

## <a name="list-jobs-and-assets"></a>Liste işleri ve varlıkları
Önemli bir ilgili görev listesi varlıklarla ilişkili, Media Services işinde etkinleştirmektir. Aşağıdaki kod örneği, her IJob nesne listeleme gösterir ve sonra her bir iş için işle ilgili özellikleri görüntüler, ilgili tüm görevleri, tüm varlıklar ve tüm çıktı varlığı giriş. Bu örnekteki kod çok sayıda diğer görevler için yararlı olabilir. Örneğin, daha önce çalıştırdığınız bir veya daha fazla kodlama işi çıkış varlıklarından listelemek istiyorsanız, bu kod, çıktı varlıkları nasıl gösterir. Çıktı varlığına başvuru olduğunda, daha sonra içeriği diğer kullanıcılar veya uygulamalar için karşıdan veya URL'leri sağlama teslim edebilirsiniz. 

Varlıklar sunmaya yönelik seçenekler hakkında daha fazla bilgi için bkz. [.NET için Media Services SDK'sı ile varlıkları teslim](media-services-deliver-streaming-content.md).

```csharp
    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }

            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {

                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }

            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }

        }

        // Display output in console.
        Console.Write(builder.ToString());
    }
```

## <a name="list-all-access-policies"></a>Tüm erişim ilkeleri listesi
Media Services'de bir varlık veya dosyalar üzerinde bir erişim ilkesi tanımlayabilirsiniz. Bir erişim ilkesi, bir dosya veya bir varlık (ne tür erişim ve süresi) için izinleri tanımlar. Media Services kodunuzda, genellikle bir erişim ilkesi IAccessPolicy nesne oluşturarak ve ardından var olan bir varlıkla ilişkilendirme tanımlarsınız. Ardından Media Services varlıklara doğrudan erişim sağlamanıza olanak sağlayan bir ILocator nesnesi oluşturun. Bu belgeleri serisi birlikte gelen Visual Studio projesi oluşturma ve erişim ilkeleri ve bulucular varlıklarına atama işlemini gösteren birkaç kod örneği içerir.

Aşağıdaki kod örneği, sunucu üzerindeki tüm erişim ilkeleri listesinde gösterilmiştir ve her ilişkilendirilmiş izinleri türünü gösterir. Sunucu üzerindeki tüm ILocator nesneleri listelemek için erişim ilkelerini görüntülemek için başka bir kullanışlı yolu ve ardından her bir Bulucu için kendi ilişkili erişim ilkesi, AccessPolicy özelliğini kullanarak listeleyebilirsiniz.

```csharp
    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");

        }
    }
```
    
## <a name="limit-access-policies"></a>Sınırı erişim ilkeleri 

>[!NOTE]
> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). 

Örneğin, uygulamanızda yalnızca bir kez çalıştıracağınız aşağıdaki kod ile genel olarak ilkeleri oluşturabilirsiniz. Daha sonra kullanmak için bir günlük dosyasına kimlikleri günlüğe kaydedebilirsiniz:

```csharp
    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);
```

Daha sonra kodunuzda bu gibi mevcut kimlikleri kullanabilirsiniz:

```csharp
    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get the standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get the existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("The locator base path is " + originLocator.BaseUri.ToString());
```

## <a name="list-all-locators"></a>Tüm bulucuları listeler
Bir Bulucu varlık izinleri ile birlikte bir varlık konum ilişkilendirilmiş bir erişim ilkesi tarafından tanımlandığı şekilde erişmek için doğrudan bir yol sağlayan bir URL'dir. Her varlık, Bulucular özelliği ilişkili ILocator nesnelerinin bir koleksiyonunu olabilir. Sunucu bağlamı da içeren tüm bulucular Bulucular koleksiyonu vardır.

Aşağıdaki kod örneği, sunucudaki tüm bulucuları listeler. Her bir Bulucu için ilgili varlık ve erişim ilkesi kimliğini gösterir. Ayrıca varlık için izinler, sona erme tarihi ve tam yolunu türünü görüntüler.

Bir varlığa bir Bulucu yol yalnızca varlık için temel URL olduğunu unutmayın. Bir kullanıcı veya uygulama için Gözat tek tek dosyaları doğrudan bir yol oluşturmak için kodunuzu Bulucu yolu belirli bir dosya yolu eklemeniz gerekir. Bunun nasıl yapılacağı hakkında daha fazla bilgi için Ek Yardım konusuna [.NET için Media Services SDK'sı ile varlıkları teslim](media-services-deliver-streaming-content.md).

```csharp
    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }
```

## <a name="enumerating-through-large-collections-of-entities"></a>Varlıklar büyük koleksiyonlarına numaralandırma
Varlıkları sorgulanırken ortak REST v2 1000 sonuçları için sorgu sonuçları sınırladığı için tek seferde döndürülen 1000 varlıkların bir sınır yoktur. Skip ve Take varlıklar büyük koleksiyonlarına sıralanırken kullanmanız gerekir. 

Aşağıdaki işlev aracılığıyla sağlanan Media Services hesabı tüm işleri döngüde kalır. Media Services işleri koleksiyondaki 1000 işleri döndürür. İşlevi Atla kullanın yapar ve tüm işleri emin olmak için sınav zamanı numaralandırılan (1000'den fazla işleri hesabınızdaki olması durumunda).

```csharp
    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }

                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
```

## <a name="delete-an-asset"></a>Bir varlığı silme
Aşağıdaki örnek, bir varlığını siler.

```csharp
    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");

    }
```

## <a name="delete-a-job"></a>İş Sil
Bir işi silmek için durum özelliğinde gösterildiği gibi iş durumunu işaretlemeniz gerekir. İlk sıraya alınan, zamanlanan ya da işlem, gibi diğer bazı durumlarda işler iptal edildi ve daha sonra silinebilir tamamlandı veya iptal edilen işler silinebilir.

Aşağıdaki kod örneği, işi durumlarını denetleme ve sonra durumu tamamlandı ya da iptal edildiğinde silerek bir işi silmek için bir yöntemi gösterir. Bu kod, önceki bölümde bir projeye bir başvuru almak için bu konudaki bağlıdır: Bir proje başvurusu alın.

```csharp
    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;

        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);

            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }

        }
    }
```


## <a name="delete-an-access-policy"></a>Erişim ilkesini silme
Aşağıdaki kod örneği kimliği, ilke tabanlı bir erişim ilkesi için bir başvuru almak nasıl gösterir ve ilke silinemedi.

```csharp
    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

