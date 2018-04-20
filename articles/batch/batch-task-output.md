---
title: Sonuçları ya da bir veri deposu - Azure Batch tamamlanan işler ve görevler günlüklerine kalıcı | Microsoft Docs
description: Toplu iş görevlerini ve işleri kalıcı çıktı verileri için farklı seçenekler hakkında bilgi edinin. Verileri Azure Storage veya başka bir veri deposu devam edebilir.
services: batch
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cb8b1ca3514e27221e95cb2def823c8f89d151e5
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="persist-job-and-task-output"></a>İş ve görev çıktılarını kalıcı hale getirme

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Görev çıktısı ortak bazı örnekleri şunlardır:

- Görev işlemleri veri girdikten sonra oluşturulan dosyaları.
- Görev Yürütme ile ilişkili günlük dosyaları. 

Bu makalede, kalıcı görev çıktısı için çeşitli seçenekleri ve her seçeneğin en uygun olan senaryolar açıklanmaktadır.   

## <a name="about-the-batch-file-conventions-standard"></a>Toplu iş dosyası kuralları standardı hakkında

Toplu Görev çıkış dosyalarını Azure storage'da adlandırma kuralları isteğe bağlı bir kümesini tanımlar. [Toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) bu kuralları açıklar. Dosya kuralları standart hedef kapsayıcı ve blob yolu Azure Storage iş ve görev adlarına göre verilen çıktı dosyası adını belirler.

Çıktı veri dosyalarınızı adlandırma dosyası kuralları standardını kullanmaya karar olduğunu. Ayrıca, hedef kapsayıcı adı ve Bununla birlikte, istediğiniz blob. Dosya kuralları standart çıktı dosyalarını adlandırma için kullandığınız sonra çıktı dosyaları görüntülemek için kullanılabilir olan [Azure portal][portal].

Dosya kuralları standart kullanabileceğiniz birkaç farklı yolu vardır:

- Çıkış dosyaları kalıcı hale getirmek için Batch hizmeti API'si kullanıyorsanız, ad hedef kapsayıcılar ve bloblar için seçebilirsiniz dosyası kuralları standart göre. Batch hizmeti API'si, istemci kodu çıktı dosyalarını görev uygulamanızı değiştirmeden kalıcı hale getirmek sağlar.
- .NET ile geliştiriyorsanız, kullanabileceğiniz [.NET için Azure toplu işlem dosyası kuralları Kitaplığı][nuget_package]. Bu kitaplığı kullanarak bir avantajı, kendi kimliği veya amaca göre çıktı dosyaları sorgulama destekliyorsa olmasıdır. Yerleşik Sorgulama işlevi, bir istemci uygulaması veya diğer görevler çıkış dosyalara erişmek kolaylaştırır. Ancak, görev uygulamanızı dosyası kuralları kitaplığı çağırmak için değiştirilmesi gerekir. Daha fazla bilgi için bkz: başvurusu için [dosyası kuralları kitaplığı için .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- .NET dışında bir dil ile geliştiriyorsanız, uygulamanızda dosyası kuralları standart uygulayabilirsiniz.

## <a name="design-considerations-for-persisting-output"></a>Kalıcı çıktı için tasarım konuları 

Batch çözümünüzü tasarlarken, iş ve görev çıktıları ilgili aşağıdaki etmenleri dikkate alın.

* **İşlem düğümü ömrü**: düğümleri genellikle geçici, özellikle otomatik ölçeklendirme özellikli havuzlarında işlem. Bir düğüm üzerinde çalışan bir görev çıktısı, yalnızca bu düğümün mevcut ve yalnızca dosya saklama dönemi içinde görev için ayarladığınız sırada kullanılabilir. Bir görev görev tamamlandıktan sonra gerekli olabilecek bir çıktı oluşturursa, görev dayanıklı bir depolama alanına Azure depolama gibi kendi çıktı dosyaları göndermelisiniz.

* **Çıktı depolama**: görev çıktısı için bir veri deposu olarak Azure Storage önerilir, ancak herhangi dayanıklı bir depolama alanı kullanabilirsiniz. Görev çıktısını Azure depolama alanına yazılmasını Batch hizmeti API'si tümleştirilmiştir. Başka bir form dayanıklı depolama kullanırsanız, görev kendiniz çıktı kalıcı hale getirmek için uygulama mantığı yazmak gerekir.   

* **Çıktı alma**: görev çıktısı kalıcı varsa doğrudan havuzunuzdaki işlem düğümlerine veya Azure Storage veya başka bir veri deposundan görev çıktısını alabilirsiniz. Bir görevin çıkış doğrudan bir işlem düğümü almak için dosya adı ve çıkış konumunu düğümde gerekir. Görev çıktısını Azure depolama birimine devam ederse, Azure depolama SDK'sı çıktı dosyalarını indirmek için Azure storage'da dosyasının tam yolunu gerekir.

* **Çıktı görüntüleme**: zaman gidin Azure portal ve select toplu göreve **düğümde dosyaları**, görev ile ilişkili tüm dosyalar ile sunulan, yalnızca çıktı dosyaları ilgilendiğiniz. Yeniden, işlem düğümlerinde dosyaları yalnızca bu düğümün mevcut ve yalnızca dosyayı elde tutma süresi içinde görev için ayarladığınız sırada kullanılabilir. Azure depolama alanına kalıcı görev çıktısını görüntülemek için Azure portal veya bir Azure Storage istemci uygulaması gibi kullanabilirsiniz [Azure Storage Gezgini][storage_explorer]. Portal ya da başka bir aracıyla çıktı verilerini Azure depolama alanında görüntülemek için dosyanın konumunu bilmeniz ve kendisine doğrudan gidin.

## <a name="options-for-persisting-output"></a>Kalıcı çıkış seçenekleri

Senaryonuza bağlı olarak, görev çıktısını kalıcı hale getirmek için yapabileceğiniz birkaç farklı yaklaşım vardır:

- Batch hizmeti API kullanın.  
- Toplu iş dosyası kuralları kitaplığı için .NET kullanın.  
- Toplu iş dosyası kuralları standart uygulamanızda uygulayın.
- Bir özel dosya taşıma çözüm uygulayabilirsiniz.

Aşağıdaki bölümlerde, her iki yaklaşımın daha ayrıntılı açıklanmıştır.

### <a name="use-the-batch-service-api"></a>Batch hizmeti API'si kullanın

2017-05-01 sürümüyle Batch hizmeti için görev verileri Azure depolama alanında çıktı dosyaları belirtmek için destek ekler olduğunda, [işe görev ekleme](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) veya [işe görevleri koleksiyonu ekleme](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

Batch hizmeti API'si sanal makine yapılandırmasıyla oluşturulan havuzlarından bir Azure depolama hesabı kalıcı görev veri destekler. Batch hizmeti API'si ile göreviniz çalıştıran uygulama değiştirmeden görev veri devam edebilir. İsteğe bağlı olarak uygun [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) Azure depolama alanına kalıcı dosya adlandırma. 

Görev çıkış devam ettirmek için Batch hizmeti API'si kullanın:

- Toplu iş görevleri ve iş yöneticisi görevleri havuzlarındaki sanal makine yapılandırmasıyla oluşturulan verilerini kalıcı hale getirmek istediğiniz.
- Rastgele bir ada sahip bir Azure Storage kapsayıcısına veri kalıcı hale getirmek istediğiniz.
- Veri göre adlı bir Azure Storage kapsayıcısına kalıcı hale getirmek istediğiniz [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> Batch hizmeti API'si, bulut hizmet yapılandırmasıyla oluşturulan havuzlarında çalışan görevler kalıcı verileri desteklemez. Bulut Hizmetleri yapılandırmasını çalıştıran havuzlarından çıktı kalıcı görev hakkında daha fazla bilgi için bkz: [iş ve görev verileri Azure depolama kalıcı hale getirmek .NET için toplu iş dosyası kuralları kitaplığıyla Sürdür ](batch-task-output-file-conventions.md)
> 
> 

Batch hizmeti API'si ile kalıcı görev çıktısını hakkında daha fazla bilgi için bkz: [kalan görev verileri toplu ile Azure depolama hizmeti API](batch-task-output-files.md). Ayrıca [PersistOutputs] [github_persistoutputs] Örnek Proje Görev çıktısını dayanıklı depolama için kalıcı hale getirmek için .NET için toplu istemci kitaplığı kullanımı gösterilmiştir GitHub bakın.

### <a name="use-the-batch-file-conventions-library-for-net"></a>Toplu iş dosyası kuralları kitaplığı için .NET kullanın

C# ve .NET ile toplu çözümler derleme geliştiricileri kullanabilir [dosyası kuralları kitaplığı için .NET] [ nuget_package] kalıcı hale getirmek için görev verileri Azure depolama hesabı, için according [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Dosya kuralları kitaplığı Azure Storage ve bilinen bir biçimde hedef kapsayıcıları ve blobları adlandırma taşıma çıktı dosyaları işler.

Dosya kuralları kitaplık kimliği veya tam dosya URI'ler gerek kalmadan bulun kolaylaşır amacı, sorgulama çıktı dosyalarını destekler. 

Görev çıkış devam ettirmek için .NET için toplu iş dosyası kuralları kitaplığı kullanın:

- Görevin çalışmaya devam ederken, Azure Storage veri akışı istersiniz.
- Bulut hizmet yapılandırması veya sanal makine yapılandırması ile oluşturulan havuzlarından veri kalıcı hale getirmek istediğiniz.
- İstemci uygulamanız veya işteki diğer görevler bulun ve Görev çıkış dosyalarını kimlikle veya amacı indirmek gerekir. 
- İlk sonuçları onay işaret eden veya erken karşıya yükleme yapmak istiyorsunuz.
- Görev çıktısını Azure portalında görüntülemek istiyorsunuz.

.NET için kalıcı Görev çıkış dosyası kuralları Kitaplığı hakkında daha fazla bilgi için bkz: [iş ve görev verileri Azure depolama kalıcı hale getirmek .NET için toplu iş dosyası kuralları kitaplığıyla kalıcı ](batch-task-output-file-conventions.md). Ayrıca [PersistOutputs] [github_persistoutputs] örnek proje dosyası kuralları kitaplığının .NET için görev çıktısını dayanıklı depolama için kalıcı hale getirmek için nasıl kullanıldığını gösterir GitHub bakın.

Github'da [PersistOutputs] [github_persistoutputs] Örnek Proje .NET için toplu istemci Kitaplığı görev çıktısını dayanıklı depolama için kalıcı hale getirmek için nasıl kullanılacağını gösterir.

### <a name="implement-the-batch-file-conventions-standard"></a>Standart toplu işlem dosyası kuralları uygula

.NET dışında bir dil kullanıyorsanız, uygulayabileceğiniz [toplu iş dosyası kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) kendi uygulamanızda. 

Kanıtlanmış adlandırma şeması istediğinizde ya da görev çıktısını Azure portalında görüntülemek istediğiniz dosyası kuralları adlandırma standardı kendiniz uygulamak isteyebilirsiniz.

### <a name="implement-a-custom-file-movement-solution"></a>Bir özel dosya taşıma çözümü

Ayrıca kendi tam dosya taşıma çözümü uygulayabilirsiniz. Bu yaklaşımını durumlarda kullanın:

- Azure Storage dışındaki bir veri deposu için görev verilerini kalıcı hale getirmek istediğiniz. Azure SQL veya Azure DataLake gibi bir veri deposu dosyaları yüklemek için bir özel komut dosyası veya yürütülebilir dosyayı bu konuma yüklemek için oluşturabilirsiniz. Ardından komut satırında birincil yürütülebilir dosyanın çalıştırdıktan sonra çağırabilirsiniz. Örneğin, bir Windows düğümünde, şu iki komutu deniyor olabilir: `doMyWork.exe && uploadMyFilesToSql.exe`
- İlk sonuçları onay işaret eden veya erken karşıya yükleme yapmak istiyorsunuz.
- Hata işleme üzerinde ayrıntılı denetim korumak istiyorsunuz. Örneğin, belirli bir görev çıkış kodları bağlı belirli karşıya yükleme eylemleri için görev bağımlılık eylemleri kullanmak istiyorsanız, kendi çözümünüzü uygulamak isteyebilirsiniz. Görev bağımlılık eylemleri hakkında daha fazla bilgi için bkz: [diğer görevlere bağlı görevleri çalıştırmak için görev bağımlılıkları oluşturmak](batch-task-dependencies.md). 

## <a name="next-steps"></a>Sonraki adımlar

- Görev verilerde sürdürmek için yeni özellikler Batch hizmeti API kullanarak araştırmaya [kalan görev verileri toplu ile Azure depolama hizmeti API](batch-task-output-files.md).
- .NET için toplu iş dosyası kuralları kitaplığını kullanma hakkında bilgi edinin [iş ve görev verileri Azure depolama kalıcı hale getirmek .NET için toplu iş dosyası kuralları kitaplığıyla kalıcı ](batch-task-output-file-conventions.md).
- [PersistOutputs] [github_persistoutputs] Örnek Proje hem Batch istemci kitaplığı .NET ve .NET için dosyası kuralları kitaplığı için görev çıktısını dayanıklı depolama için kalıcı hale getirmek için nasıl kullanılacağı ortaya GitHub bakın.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
