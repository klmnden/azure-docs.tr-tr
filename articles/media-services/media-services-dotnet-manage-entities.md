---
title: "Varlıklar ve ilgili varlıklar Media Services .NET SDK'sı ile yönetme"
description: "Varlıklar ve Media Services SDK'sı ile ilgili varlıklar için .NET yönetmeyi öğrenin."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1bd8fd42-7306-463d-bfe5-f642802f1906
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 5efe16a09808267d0797521f9e1df2b60aec9cbb
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Varlıklar ve ilgili varlıklar Media Services .NET SDK'sı ile yönetme
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-manage-entities.md)
> * [REST](media-services-rest-manage-entities.md)
> 
> 

Bu konu Azure Media Services .NET varlıklarıyla yönetme gösterir. 

>[!NOTE]
> 1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. Örneğin, 1 Nisan 2017 üzerinde 31 Aralık 2016'den daha eski hesabınızda herhangi bir işi kaydının otomatik olarak silinir. İş/görevi bilgileri arşivlemek ihtiyacınız varsa, bu konuda açıklanan kodu kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

## <a name="get-an-asset-reference"></a>Bir varlık başvurusu alın
Media Services ile var olan bir varlık için bir başvuru almak sık bir görevdir. Aşağıdaki kod örneğinde nasıl bir varlık başvurusu sunucuda varlıklar koleksiyonundan bağlam nesnesi, bir varlığı kimliğe göre alabileceğiniz gösterir Aşağıdaki kod örneğinde bir LINQ Sorgu varolan IAsset nesneye bir başvurusu almak için kullanır.

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

## <a name="list-all-assets"></a>Tüm varlıklar listesi
Depolama alanına sahip varlıklar sayısı arttıkça, varlıklarınızı listelemek yararlıdır. Aşağıdaki kod örneğinde sunucu context nesnesinde varlıklar koleksiyonu yinelemek gösterilmektedir. Her varlık ile kod örneği de bazı özellik değerleri konsola yazar. Örneğin, her varlık birçok medya dosyaları içerebilir. Kod örneği, her varlık ile ilişkili tüm dosyalar çıkışı yazar.

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

## <a name="get-a-job-reference"></a>Bir iş başvurusu alın

Media Services kod görevlerinde işleme ile çalışırken, genellikle bir kimliğine göre varolan bir projeyi bir başvuru almanız gereken Aşağıdaki kod örneğinde işler koleksiyonunun bir IJob nesne için bir başvuru almak nasıl gösterir.

Uzun süre çalışan kodlama işi başlatılırken bir proje başvurusu alın ve bir iş parçacığında iş durumunu kontrol etmeniz gerekebilir. Bu gibi durumlarda, yöntem bir iş parçacığından döndüğünde bir işi yenilenmiş bir başvuru almak gerekir.

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

## <a name="list-jobs-and-assets"></a>Liste işler ve varlıklar
Bir önemli ilgili liste varlıklarına ilişkili işlerini Media Services ile bir görevdir. Aşağıdaki kod örneği, her IJob Nesne listesinde gösterilmiştir ve sonra her bir iş için iş hakkında özelliklerini görüntüler, ilgili tüm görevleri, tüm varlıklar ve tüm çıktı varlıklar giriş. Bu örnekteki kod çok sayıda diğer görevler için yararlı olabilir. Örneğin, daha önce çalıştırdığınız bir veya daha fazla kodlama işleri çıkış varlıklarından listesinde istiyorsanız, bu kod çıkış varlıklar erişmek nasıl gösterir. Bir çıkış varlığına başvuru sahip olduğunuzda, sonra içeriği diğer kullanıcılar veya uygulamalar için karşıdan veya URL'leri sağlama sunabilir. 

Varlık teslim etmek için seçenekleri hakkında daha fazla bilgi için bkz: [teslim varlıklar .NET için Media Services SDK'sı ile](media-services-deliver-streaming-content.md).

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

## <a name="list-all-access-policies"></a>Tüm erişim ilkeleri listesi
Media Services'de bir varlık veya dosyalarından bir erişim ilkesi tanımlayabilirsiniz. Bir erişim ilkesi, bir dosya veya bir varlık (erişim ve süresini hangi türü) için izinleri tanımlar. Media Services kodunuzda, genellikle bir erişim ilkesi IAccessPolicy nesneyi oluşturarak ve var olan bir varlıkla ilişkilendirme tanımlarsınız. Ardından varlıklar Media Services doğrudan erişim sağlamak olanak sağlayan bir ILocator nesnesi oluşturun. Bu belge seri eşlik Visual Studio projesi oluşturmak ve erişim ilkeleri ve bulucular varlıklarına atamak nasıl gösteren birkaç kod örnekleri içerir.

Aşağıdaki kod örneğinde, sunucu üzerindeki tüm erişim ilkeleri listelemek nasıl gösterir ve her ilişkilendirilmiş izinleri türünü gösterir. Erişim ilkeleri görüntülemek için başka bir yararlı sunucu üzerindeki tüm ILocator nesneleri listelemek için yoludur ve ardından her Bulucu için onun ilişkili erişim ilkesi kendi AccessPolicy özelliğini kullanarak listeleyebilirsiniz.

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
    
## <a name="limit-access-policies"></a>Sınır erişim ilkeleri 

>[!NOTE]
> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). 

Örneğin, yalnızca bir kez uygulamanızda çalıştırırsınız: Aşağıdaki kod ile genel bir ilke kümesi oluşturabilirsiniz. Daha sonra kullanmak için bir günlük dosyasına kimlikleri kaydedebilirsiniz:

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

Daha sonra kodunuzda bu gibi varolan kimlikleri kullanabilirsiniz:

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

## <a name="list-all-locators"></a>Tüm Bulucular listesi
Bir Bulucu Bulucu'nın ilişkili erişim ilkesi tarafından tanımlanan varlığı izinleri ile birlikte bir varlık erişmek için doğrudan bir yol sağlayan bir URL'dir. Her varlık, kendi Bulucular özellikte ilişkili ILocator nesnelerinin bir koleksiyonu olabilir. Sunucu bağlamı da tüm bulucular içeren bir konum belirleyicisi koleksiyon sahiptir.

Aşağıdaki kod örneğinde, sunucu üzerindeki tüm bulucular listeler. Her Bulucu için ilgili varlık ve erişim ilkesi kimliğini gösterir. Ayrıca varlık için izinleri, sona erme tarihini ve tam yolu türünü görüntüler.

Bir Bulucu yolu bir varlık için varlık yalnızca temel URL olduğunu unutmayın. Bir kullanıcı veya uygulama için Gözat tek tek dosyalar için doğrudan bir yol oluşturmak için kodunuzu Bulucu yoluna belirli dosya yolu eklemeniz gerekir. Bunun nasıl yapılacağı hakkında daha fazla bilgi için Ek Yardım konusuna [teslim varlıklar .NET için Media Services SDK'sı ile](media-services-deliver-streaming-content.md).

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

## <a name="enumerating-through-large-collections-of-entities"></a>Varlıkları büyük koleksiyonlarına numaralandırma
Varlıkları sorgulanırken ortak REST v2 1000 sonuçları için sorgu sonuçları sınırladığından aynı anda döndürülen 1000 varlıkların bir sınırı yoktur. Atlama ve alma varlıklar büyük koleksiyonlarına numaralandırılırken kullanmanız gerekir. 

Sağlanan Media Services hesabı tüm işlere aşağıdaki işlevi döngüsü. Media Services 1000 işleri işleri koleksiyondaki döndürür. İşlevi atlayın yapar ve tüm işleri emin olmak için Al numaralandırılan (1000'den fazla iş hesabınızı olması durumda).

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

## <a name="delete-an-asset"></a>Bir varlığı silme
Aşağıdaki örnekte bir varlığını siler.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");

    }

## <a name="delete-a-job"></a>Bir işi Sil
Bir işi silmek için durumu özelliğinde belirtildiği gibi iş durumunu işaretlemeniz gerekir. İlk sıraya alınan, zamanlanan ya da işlem, gibi diğer bazı durumlarda işler iptal edildi ve bunlar daha sonra silinebilir tamamlandı veya iptal edilen işler silinebilir.

Aşağıdaki kod örneği, işi durumlarını denetleme ve durumu tamamlandı ya da iptal edildiğinde sonra silerek bir işi silmek için bir yöntemi gösterir. Bu kod bir iş için bir başvuru almak için bu konunun önceki bölümde bağlıdır: bir iş başvurusu alın.

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


## <a name="delete-an-access-policy"></a>Erişim ilkesini Sil
Aşağıdaki kod örneğinde ilkesine bağlı kimliği, bir erişim ilkesi için bir başvuru almak nasıl gösterir ve ardından ilkeyi silin.

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



## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

