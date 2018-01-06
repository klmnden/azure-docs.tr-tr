---
title: "Visual Studio kullanarak Azure işlevleri geliştirme | Microsoft Docs"
description: "Geliştirme ve Azure işlevleri için Visual Studio 2017 Azure işlevleri araçları kullanarak test öğrenin."
services: functions
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: glenga
ms.openlocfilehash: ed1d8298123597fe8330b54f89fd580095f21ec7
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-tools-for-visual-studio"></a>Visual Studio için Azure işlevleri araçları  

Visual Studio 2017 için Azure işlevleri araçları, geliştirme, test ve C# işlevleri için Azure dağıtmanızı sağlar Visual Studio için bir uzantısıdır. İlk Azure işlevleri deneyiminizi varsa hakkında daha fazla bilgi edinebilirsiniz [Azure işlevleri giriş](functions-overview.md).

Azure işlevleri araçları aşağıdaki avantajları sağlar: 

* Düzenleme, yapı ve işlevleri yerel geliştirme bilgisayarınızda çalıştırın. 
* Azure işlevleri projenizi doğrudan Azure yayımlayın. 
* İşlev bağlamaları tanımları bağlama için ayrı bir function.json Bakımı yerine doğrudan C# kodunda bildirmek için Web işleri öznitelikleri kullanın.
* Geliştirme ve önceden derlenmiş C# işlevleri dağıtın. Önceden derlenmiş işlevleri bir daha iyi soğuk başlangıç daha performans C# betik tabanlı işlevleri sağlar. 
* Tüm Visual Studio geliştirme avantajları yaparken işlevlerinizi C# kod. 

Bu konu Azure işlevleri araçları Visual Studio 2017 için C# işlevlerinizi geliştirmek için nasıl kullanılacağını gösterir. Ayrıca, projenizin bir .NET derlemesi olarak Azure yayımlama öğrenin.

> [!IMPORTANT]
> Yerel geliştirme aynı işlev uygulaması portal geliştirme ile bir arada kullanmayın. Dağıtım işlemi, bir işlev uygulaması için bir yerel projeden yayımladığınızda, portalda geliştirilen işlevleri üzerine yazar.

## <a name="prerequisites"></a>Önkoşullar

Azure işlevleri araçları Azure geliştirme iş yükü dahil [Visual Studio 2017 sürüm 15.4](https://www.visualstudio.com/vs/), veya sonraki bir sürümü. Eklediğinizden emin olun **Azure geliştirme** Visual Studio 2017 yüklemenizdeki iş yükü:

![Azure geliştirme iş yüküyle Visual Studio 2017’yi yükleyin](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Oluşturma ve dağıtma işlevleri için ayrıca gerekir:

* Etkin bir Azure aboneliği. Bir Azure aboneliğiniz yoksa [serbest hesapları](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) kullanılabilir.

* Bir Azure depolama hesabı. Bir depolama hesabı oluşturmak için bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-an-azure-functions-project"></a>Azure işlevleri projesi oluşturma 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a>Projeyi yerel geliştirme için yapılandırın

Azure işlevleri şablonunu kullanarak yeni bir proje oluşturduğunuzda, aşağıdaki dosyaları içeren boş C# projesinde alın:

* **Host.JSON**: işlevleri konak yapılandırmanıza olanak sağlar. Bu ayarlar hem de yerel olarak ve Azure içinde çalışırken geçerlidir. Daha fazla bilgi için bkz: [host.json başvuru](functions-host-json.md).
    
* **Local.Settings.JSON**: işlevleri yerel olarak çalıştırırken kullanılan ayarları bulundurur. Bu ayarlar, Azure tarafından kullanılmaz, tarafından kullanılan [Azure işlevleri çekirdek Araçları](functions-run-local.md). Bu dosya, diğer Azure Hizmetleri için bağlantı dizelerini gibi ayarlarını belirtmek için kullanın. Yeni bir anahtar ekleyin **değerleri** dizi projenizdeki işlevleri gerektirdiği her bağlantı için. Daha fazla bilgi için bkz: [yerel ayarları dosyasına](functions-run-local.md#local-settings-file) Azure işlevleri çekirdek araçları konu başlığı.

İşlevler çalışma zamanı bir Azure Storage hesabı dahili olarak kullanır. Tüm HTTP ve Web kancalarını dışında türleri tetiklemek için ayarlamanız gerekir **Values.AzureWebJobsStorage** geçerli bir Azure depolama hesabı bağlantı dizesi anahtar.

[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

 Depolama hesabı bağlantı dizesi ayarlamak için:

1. Visual Studio'da açın **Cloud Explorer**, genişletin **depolama hesabı** > **depolama hesabınız**seçeneğini belirleyip **özellikleri** ve kopyalama **birincil bağlantı dizesi** değeri.   

2. Projenizdeki local.settings.json dosyasını açın ve değerini ayarlama **AzureWebJobsStorage** anahtar bağlantı dizesine kopyalanır.

3. Benzersiz anahtarlara eklemek için önceki adımı yineleyin **değerleri** dizi işlevlerinizi tarafından gereken diğer bağlantılar için.  

## <a name="create-a-function"></a>İşlev oluşturma

Önceden derlenmiş işlevlerde işlevi tarafından kullanılan bağlamaları kodda öznitelikleri uygulama tarafından tanımlanır. Sağlanan şablonlardan işlevlerinizi oluşturmak için Azure işlevleri araçları kullandığınızda, bu öznitelikler için uygulanır. 

1. **Çözüm Gezgini**’nde, proje düğümünüze sağ tıklayın ve **Yeni** > **Öğe Ekle**’yi seçin. Seçin **Azure işlevi**, bir **adı** sınıfı ve tıklatın **Ekle**.

2. Tetikleyici seçin, bağlama özelliklerini ayarlamak ve tıklatın **oluşturma**. Aşağıdaki örnekte bir kuyruk depolama oluşturma işlevi tetiklendiğinde ayarları gösterir. 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    Adlı bir bağlantı dizesi anahtar **QueueStorage** sağlanır, local.settings.json dosyasında tanımlanmış. 
 
3. Yeni eklenen sınıfını inceleyin. Statik bkz **çalıştırmak** ile öznitelikli yöntemi, **FunctionName** özniteliği. Bu öznitelik, yöntemi işlevi için giriş noktası olarak gösterir. 

    Örneğin, aşağıdaki C# sınıfı, temel bir sıra tetiklenen depolama işlevini temsil eder:

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    Bağlama özgü öznitelik giriş noktası yönteme sağlanan her bağlama parametresi uygulanır. Öznitelik parametre olarak bağlama bilgilerini alır. Önceki örnekte, ilk parametresine sahip bir **QueueTrigger** tetiklenen sıra işlevini belirten uygulanan, öznitelik. Kuyruk adı ve bağlantı dizesi ayarı adı için parametre olarak geçirilen **QueueTrigger** özniteliği.

## <a name="testing-functions"></a>İşlevleri test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.  

İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin.  Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

Çalışan proje ile dağıtılan işlevi test edersiniz gibi kodunuzu test edebilirsiniz. Daha fazla bilgi için bkz: [Azure işlevleri, kodunuzu test etmek için stratejileri](functions-test-a-function.md). Hata ayıklama modunda çalışırken, kesme noktaları Visual Studio'da beklendiği gibi ulaşıldığından. 

Örneği tetiklenen sıra işlevi test etme için bkz: [tetiklenen sıra işlevi hızlı başlangıç Öğreticisi](functions-create-storage-queue-triggered-function.md#test-the-function).  

Azure işlevleri çekirdek araçlarını kullanma hakkında daha fazla bilgi için bkz: [kod ve yerel olarak Azure işlevlerini test](functions-run-local.md).

## <a name="publish-to-azure"></a>Azure’da Yayımlama

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
>Local.settings.json eklediğiniz herhangi bir ayarı ayrıca Azure işlevi uygulamada eklenmesi gerekir. Bu ayarlar otomatik olarak eklenmez. Bu yollardan biriyle işlevi uygulamanıza gerekli ayarları ekleyebilirsiniz:
>
>* [Azure portalını kullanarak](functions-how-to-use-azure-function-app-settings.md#settings).
>* [Kullanarak `--publish-local-settings` Azure işlevleri çekirdek araçları publish seçeneği](functions-run-local.md#publish).
>* [Azure CLI kullanarak](/cli/azure/functionapp/config/appsettings#set). 

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri araçları hakkında daha fazla bilgi için sık sorulan sorular bölümüne bakın [Azure işlevleri için Visual Studio 2017 Araçları](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog postası.

Azure işlevleri çekirdek araçları hakkında daha fazla bilgi için bkz: [kod ve yerel olarak Azure işlevlerini test](functions-run-local.md).  
.NET sınıf kitaplıkları işlevleri geliştirme hakkında daha fazla bilgi için bkz: [Azure işlevleri C# Geliştirici Başvurusu](functions-dotnet-class-library.md). Bu konu ayrıca bağlamaları Azure işlevleri tarafından desteklenen çeşitli türlerde bildirmek için öznitelikleri kullanma örnekleri bağlar.    
