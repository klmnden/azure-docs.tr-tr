---
title: Sonuçları ya da bir veri deposu - Azure Batch için tamamlanan işler ve görevler günlüklerinden kalıcı | Microsoft Docs
description: Toplu iş görevlerini ve işleri kalıcı çıktı verileri için farklı seçenekler hakkında bilgi edinin. Azure depolama alanına veya başka bir veri deposuna verileri kalıcı hale getirebilirsiniz.
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
ms.openlocfilehash: b578abfa6fc0a10edc5daab40f8a0eea5e6653d9
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115071"
---
# <a name="persist-job-and-task-output"></a>İş ve görev çıktılarını kalıcı hale getirme

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Görev çıktısı bazı genel örnekleri şunlardır:

- Görev işlemleri giriş verileri oluşturulan dosyalar.
- Görev Yürütme ile ilişkili günlük dosyaları. 

Bu makalede, görev çıktısını kalıcı hale getirme için çeşitli seçenekler ve her seçenek en uygun olduğu senaryolar açıklanmaktadır.   

## <a name="about-the-batch-file-conventions-standard"></a>Batch dosya kuralları standart hakkında

Batch, görev Çıkış dosyalarını Azure depolama adlandırma kurallarına isteğe bağlı bir kümesini tanımlar. [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) bu kuralları açıklar. Dosya kuralları standart kapsayıcı ve blob için hedef yolu Azure Depolama'da iş ve görev adlarına göre bir belirtilen çıkış dosyası adını belirler.

Bu, çıkış veri dosyalarınızı adlandırmak için dosya kuralları standart kullanmaya karar aittir. Ayrıca, hedef kapsayıcı adı ve istediğiniz ancak blob. Dosya kuralları standart çıkış dosyalarını adlandırma için kullandığınız sonra çıktı dosyalarınızı görüntülenmek üzere kullanılabilir [Azure portalında][portal].

Dosya kuralları standart kullanabileceğiniz birkaç farklı yolu vardır:

- Çıktı dosyaları kalıcı hale getirmek için Batch hizmeti API'SİNİN kullanıyorsanız ad hedef kapsayıcılara ve blob'lara erişimi seçebilirsiniz dosya kuralları standardına göre. Batch hizmeti API'si, görev uygulamanızı değiştirmeden Çıkış dosyalarını istemci kodundan kalıcı hale getirmek sağlar.
- .NET ile geliştiriyorsanız kullanabileceğiniz [.NET için Azure Batch dosya kuralları Kitaplığı][nuget_package]. Bu kitaplık kullanmanın bir avantajı, çıktı dosyalarınızı kendi kimliği veya amaçlı göre sorgulama desteklemesidir. Yerleşik bir sorgulama işlevi, bir istemci uygulamasından ya da diğer görevlerin çıkış dosyalarına erişmek kolaylaştırır. Ancak, görev uygulamanızın dosya kuralları kitaplığı çağırmak için değiştirilmesi gerekir. Başvuru için daha fazla bilgi için bkz. [dosya kuralları kitaplığı için .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- .NET dışında bir dil ile geliştiriyorsanız, uygulamanızda dosya kuralları standart uygulayabilirsiniz.

## <a name="design-considerations-for-persisting-output"></a>Kalıcı çıkış için tasarım konuları 

Batch çözümünüzü tasarlarken, iş ve görev çıktıları ilgili aşağıdaki faktörleri göz önünde bulundurun.

* **İşlem düğümü ömrü**: düğümleri genellikle geçici, özellikle otomatik ölçeklendirme özellikli havuzlarınızdaki işlem. Bir düğüm üzerinde çalışan bir görev çıktısı, yalnızca düğümün var ve yalnızca dosya elde tutma süresi içinde görevi için ayarladığınız sırada kullanılabilir. Bir görev görev tamamlandıktan sonra gerekli olabilecek bir çıktı oluşturulur, ardından görev Çıkış dosyalarını Azure depolama gibi dayanıklı bir depolama alanına yüklemeniz gerekir.

* **Çıkış depolama**: Azure depolama, görev çıkışı için bir veri deposu olarak önerilir, ancak herhangi bir kalıcı depolama alanı kullanabilirsiniz. Görev çıktısını Azure depolama alanına yazılmasını Batch hizmeti API'ye tümleşiktir. Dayanıklı depolama, başka bir form kullanırsanız, görev kendiniz çıktısını kalıcı hale getirmek için uygulama mantığı yazmak gerekir.   

* **Çıkış alma**: görev çıktısını kalıcı, havuzunuzdaki işlem düğümlerine doğrudan ya da Azure depolama veya başka bir veri deposundan görev çıktısı alabilirsiniz. Doğrudan işlem düğümünden bir görevin çıkış almak için dosya adı ve çıkış konumunu düğümde gerekir. Azure depolama için görev çıktılarını kalıcı hale getirme, Azure depolama SDK'sı ile çıkış dosyalarını indirmek için Azure depolamadaki dosyanın tam yolu gerekir.

* **Çıkışını görüntüleme**: gittiğinizde Azure portal ve select Batch göreve **düğüm üzerindeki dosyalar**çıkış dosyalarının yalnızca ilginizi çeken, görev ile ilişkili tüm dosyaları ile sunulur. Yine, işlem düğümlerinde dosyaları yalnızca düğüm yok ve dosyayı elde tutma süresi içinde yalnızca görev için ayarladığınız sırada kullanılabilir. Azure Depolama'da kalıcı hale görev çıktısını görüntülemek için Azure portalı veya bir Azure depolama istemci uygulaması gibi kullanabileceğiniz [Azure Depolama Gezgini][storage_explorer]. Portalı veya başka bir aracı ile çıktı verilerini Azure Depolama'da görüntülemek için dosyanın konumunu bilmeniz ve ona doğrudan gidin.

## <a name="options-for-persisting-output"></a>Kalıcı çıkış seçenekleri

Senaryonuza bağlı olarak, görev çıktısını kalıcı hale getirmek için yapabileceğiniz birkaç farklı yaklaşım vardır:

- Batch hizmeti API'sini kullanın.  
- .NET için Batch dosya kuralları kitaplığı kullanın.  
- Batch dosya kuralları standart uygulamanızda uygulayın.
- Bir özel dosya taşıma çözümü uygulayın.

Aşağıdaki bölümlerde, her bir yaklaşıma daha ayrıntılı açıklanmaktadır.

### <a name="use-the-batch-service-api"></a>Batch hizmeti API'sini kullanma

Sürüm 2017-05-01 toplu işlem hizmeti Çıkış dosyalarını Azure Storage'da görev verileri belirtmek için destek ekler. zaman, [bir işe görev ekleme](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) veya [bir işe görev koleksiyonunu ekleme](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

Batch hizmeti API'si kalıcı görev veri gelen sanal makine yapılandırmasıyla oluşturulan havuzlar için bir Azure depolama hesabını destekler. Batch hizmeti API'si ile göreviniz çalışan uygulama değiştirmeden görev verileri kalıcı hale getirebilirsiniz. İsteğe bağlı olarak uyması [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) Azure depolamada kalıcı hale dosya adlandırma. 

Batch hizmeti API'si, görevin çıkış kalıcı hale getirmek için kullanın:

- Toplu iş görevleri ve iş yöneticisi görevleri sanal makine yapılandırmasıyla oluşturulan havuzlarda verileri kalıcı hale getirmek istediğiniz.
- Rastgele bir ada sahip bir Azure depolama kapsayıcısı verileri kalıcı hale getirmek istediğiniz.
- Şunlara göre adlı bir Azure depolama kapsayıcısına veriyi kalıcı hale getirmek istediğiniz [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> Batch hizmeti API'si, bulut hizmeti yapılandırmasıyla oluşturulan havuzlar, çalışan görevleri kalıcı verileri desteklemez. Cloud services yapılandırması çalıştıran havuzlarından çıktısını kalıcı hale getirme görevi hakkında daha fazla bilgi için bkz. [iş ve görev verilerini Azure Depolama'da kalıcı hale getirmek .NET için Batch dosya kuralları kitaplığı ile kalıcı ](batch-task-output-file-conventions.md)
> 
> 

Batch hizmeti API'si ile kalıcı hale getirme görev çıkışı hakkında daha fazla bilgi için bkz. [kalan görev veri Batch ile Azure depolama hizmeti API](batch-task-output-files.md). Ayrıca bkz: [PersistOutputs] [ github_persistoutputs] kodunuzla github'da hangi görev çıktısını kalıcı depolama devam ettirmek için .NET için Batch istemci kitaplığını nasıl yapılacağı açıklanır.

### <a name="use-the-batch-file-conventions-library-for-net"></a>.NET için Batch dosya kuralları kitaplığı kullanma

C# ve .NET ile batch çözümleri oluşturmak geliştiriciler [dosya kuralları kitaplığı için .NET] [ nuget_package] kalıcı hale getirmek için görev verileri bir Azure depolama hesabı, göre [toplu iş dosyası Standart kuralları](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Dosya kuralları kitaplığı taşıma Çıkış dosyalarını Azure depolama ve iyi bilinen bir biçimde hedef kapsayıcıları ve blobları adlandırma işler.

Dosya kuralları kitaplığı kimliği veya tam dosya URI'ler gerek kalmadan bulmak kolaylaştıran bir amaçla sorgulanırken Çıkış dosyalarını destekler. 

.NET için Batch dosya kuralları kitaplığı çıkış görev kalıcı hale getirmek için kullanın:

- Görevin çalışmaya devam ederken akışı verilerini Azure Depolama'da istiyorsunuz.
- Bulut hizmeti yapılandırmasını veya sanal makine yapılandırması ile oluşturulan havuzlar verileri kalıcı hale getirmek istediğiniz.
- İstemci uygulamanız veya işteki diğer görevler bulun ve Kimliğine veya amacına göre Görev çıkış dosyalarını indirmek gerekir. 
- İlk sonuçlar onay işaret eden veya erken karşıya yükleme gerçekleştirmek istediğiniz.
- Görev çıktısını Azure portalında görüntülemek istiyorsunuz.

.NET için dosya kuralları kitaplığı ile kalıcı hale getirme görev çıktısını hakkında daha fazla bilgi için bkz. [iş ve görev verilerini Azure Depolama'da kalıcı hale getirmek .NET için Batch dosya kuralları kitaplığı ile kalıcı ](batch-task-output-file-conventions.md). Ayrıca bkz: [PersistOutputs] [ github_persistoutputs] kodunuzla github'da hangi görev çıktısını kalıcı depolama devam ettirmek için .NET için dosya kuralları kitaplığı nasıl yapılacağı açıklanır.

[PersistOutputs] [ github_persistoutputs] GitHub üzerinde örnek proje görev çıktısını kalıcı depolama devam ettirmek için .NET için Batch istemci kitaplığını kullanma işlemini gösterir.

### <a name="implement-the-batch-file-conventions-standard"></a>Standart Batch dosya kuralları uygulama

.NET dışında bir dil kullanıyorsanız, uygulayabileceğiniz [Batch dosya kuralları standart](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) kendi uygulamanızda. 

Kendini kanıtlamış bir adlandırma şeması istediğinizde ya da görev çıktısını Azure portalında görüntülemek istediğiniz dosya kuralları adlandırma standardı kendiniz uygulamak isteyebilirsiniz.

### <a name="implement-a-custom-file-movement-solution"></a>Bir özel dosya taşıma çözümü

Ayrıca, kendi tam dosya taşıma çözümü uygulayabilirsiniz. Bu yaklaşım ne zaman kullanın:

- Azure depolama dışındaki bir veri deposuna görev verileri kalıcı hale getirmek istediğiniz. Azure SQL veya Azure DataLake gibi bir veri deposuna dosyaları karşıya yüklemek için özel betik veya yürütülebilir dosyayı bu konuma yüklenecek oluşturabilirsiniz. Ardından, komut satırında birincil yürütülebilir dosyanızın çalıştırdıktan sonra çağırabilirsiniz. Örneğin, bir Windows düğümünde bu iki komut çağırabilirsiniz: `doMyWork.exe && uploadMyFilesToSql.exe`
- İlk sonuçlar onay işaret eden veya erken karşıya yükleme gerçekleştirmek istediğiniz.
- Hata işleme üzerinde ayrıntılı denetim korumak istiyorsunuz. Örneğin, görev bağımlılık eylemleri karşıya yükleme eylemlerin belirli görev çıkış kodları kullanmak istiyorsanız kendi çözümünüzü uygulamak isteyebilirsiniz. Görev bağımlılık eylemleri hakkında daha fazla bilgi için bkz. [diğer görevlere bağlı görevlerin çalıştırılacak görev bağımlılıklarını oluşturma](batch-task-dependencies.md). 

## <a name="next-steps"></a>Sonraki adımlar

- Görev verileri kalıcı hale getirmek için Batch hizmet API'SİNDE yeni özellikleri kullanarak keşfedin [kalan görev veri Batch ile Azure depolama hizmeti API](batch-task-output-files.md).
- .NET için Batch dosya kuralları kitaplığı kullanma hakkında bilgi edinin [.NET için Azure Depolama'ya iş ve görev veri Batch dosya kuralları kitaplığı ile kalıcı](batch-task-output-file-conventions.md).
- Bkz: [PersistOutputs] [ github_persistoutputs] , hem Batch istemci kitaplığını .NET ve .NET için dosya kuralları kitaplığı için görev çıktısını kalıcı depolama devam ettirmek için nasıl kullanılacağını gösterir ve GitHub üzerinde örnek proje .

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs 
