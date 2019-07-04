---
title: Visual Studio Code kullanarak Azure işlevleri geliştirme | Microsoft Docs
description: Geliştirme ve Azure işlevleri için Visual Studio Code uzantısıyla Azure işlevlerini test etme hakkında bilgi edinin.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: glenga
ms.openlocfilehash: 17550e148ea61eb69a20fc6a3215dfb63b65f18e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452702"
---
# <a name="develop-azure-functions-using-visual-studio-code"></a>Visual Studio Code kullanarak Azure işlevleri geliştirme

[Visual Studio Code için Azure işlevleri uzantısı] yerel olarak geliştirme ve Azure işlevleri'ni dağıtma olanak sağlar. Bu deneyim, Azure işlevleri ile ilk ise, daha fazla bilgi edinebilirsiniz [Azure işlevleri giriş](functions-overview.md).

Azure işlevleri uzantısı aşağıdaki avantajları sağlar: 

* Düzenleme, derleme ve yerel geliştirme bilgisayarınızda işlevleri çalıştırın. 
* Azure işlevleri projenizi doğrudan Azure'da yayımlayabilirsiniz. 
* İşlevlerinizi tüm Visual Studio Code'un avantajları yaparken çeşitli dillerde yazma. 

Uzantı ile Azure işlevleri sürüm 2.x çalışma zamanı tarafından desteklenen aşağıdaki dillerde kullanılabilir: 

* [C#derlenmiş](functions-dotnet-class-library.md) 
* [C# betiği](functions-reference-csharp.md)<sup>*</sup>
* [JavaScript](functions-reference-node.md)
* [Java](functions-reference-java.md)
* [PowerShell](functions-reference-powershell.md)
* [Python](functions-reference-python.md)

<sup>*</sup>Gerektiren, [ayarlamak C# betik dilini varsayılan proje olarak](#c-script-projects).

Bu makaledeki örnekler şu anda yalnızca JavaScript (Node.js) için kullanılabilir ve C# sınıf kitaplığı işlevleri.  

Bu makalede, Azure işlevleri uzantı işlevleri geliştirmek ve bunları Azure'a yayımlama için nasıl kullanılacağını hakkında ayrıntılar sağlar. Bu makaleyi okuyun önce karşılamanız gereken [Visual Studio Code kullanarak ilk işlevinizi oluşturma](functions-create-first-function-vs-code.md).

> [!IMPORTANT]
> Yerel geliştirme portalı geliştirme aynı işlev uygulaması ile karıştırmayın. Bir işlev uygulaması için yerel bir projeden yayımladığınızda, dağıtım işlemi portalda geliştirilen tüm işlevleri üzerine yazar.

## <a name="prerequisites"></a>Önkoşullar

Yükleme ve çalıştırmadan önce [Azure işlevleri uzantısı][Visual Studio Code için Azure işlevleri uzantısı], aşağıdaki gereksinimleri karşılaması gerekir:

* [Visual Studio Code](https://code.visualstudio.com/) birinde yüklü [desteklenen platformlar](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Aboneliğinizde bir Azure depolama hesabı gibi gereken diğer kaynakların oluşturulmasını, size [Visual Studio Code kullanarak yayımlama](#publish-to-azure).

> [!IMPORTANT]
> İşlevleri yerel olarak geliştirebilir ve başlatmak ve yerel olarak çalıştırmak zorunda kalmadan Azure'da yayımlayın. İşlev yerel olarak Azure işlevleri çekirdek araçları dahil olmak üzere bir otomatik indirilmesi çalıştırmak için ek gereksinimler vardır. Daha fazla bilgi için bkz. [yerel olarak çalıştırmak için ek gereksinimler](#additional-requirements-to-run-locally). 

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Azure İşlevleri projesi oluşturma

Uzantı işlevleri kullanarak ilk işlevinizi birlikte bir işlev uygulaması projesi oluşturmanıza olanak sağlar. Aşağıdaki adımlar, HTTP ile tetiklenen bir işlev içinde yeni işlevler projesi oluşturma işlemini gösterir. [HTTP tetikleyicisi](functions-bindings-http-webhook.md) göstermek için en basit işlevi tetikleyici şablonudur.

1. Gelen **Azure: İşlevleri**, Create FUNCTION simgesini seçin.

    ![İşlev oluşturma](./media/functions-develop-vs-code/create-function.png)

1. İşlev uygulaması projenizi için bir klasör seçin ve ardından **işlevi projeniz için bir dil seçmek**. 

1. Seçin **HTTP tetikleyicisi** işlev şablonu için veya tercih edebilir **şimdilik atla** bir işlev olmayan bir proje oluşturmak için. Her zaman [projenize bir işlev ekleme](#add-a-function-to-your-project) daha sonra. 

    ![HTTP tetikleyicisi şablonunu seçin](./media/functions-develop-vs-code/create-function-choose-template.png)

1. Tür `HTTPTrigger` işlev adı ve Enter tuşuna basın için seçip **işlevi** yetkilendirme. Bu yetkilendirme düzeyi vermenizi gerektirir bir [işlev anahtarı](functions-bindings-http-webhook.md#authorization-keys) işlevi uç noktası çağrısı yapıldığında.

    ![İşlevi kimlik doğrulamasını seçme](./media/functions-develop-vs-code/create-function-auth.png)

    HTTP ile tetiklenen işlevin şablonu kullanılarak, seçtiğiniz dilde bir işlev oluşturulur.

    ![Visual Studio Code’daki HTTP ile tetiklenen işlev şablonu](./media/functions-develop-vs-code/new-function-full.png)

Proje şablonu, seçtiğiniz dilde bir proje oluşturur, gerekli bağımlılıkları yükler. Herhangi bir dil için yeni proje aşağıdaki dosyaları içerir:

* **host.json**: İşlevleri konak yapılandırmanıza olanak sağlar. Bu ayarlar hem de yerel olarak ve azure'da çalışırken geçerlidir. Daha fazla bilgi için [host.json başvurusu](functions-host-json.md).

* **Local.Settings.JSON**: İşlevleri yerel olarak çalıştırırken kullanılan ayarları tutar. Bu ayarlar, yalnızca yerel olarak çalıştırırken kullanılır. Daha fazla bilgi için [yerel ayarları dosyası](#local-settings-file).

    >[!IMPORTANT]
    >Local.settings.json dosyasında parolaları içerdiğinden, gerekir dışarıda Bu, proje kaynak denetimi.

Bu noktada, giriş ekleyin ve işleviniz tarafından çıktı bağlaması [function.json dosyasını değiştirerek](#javascript-2), ya da [parametre ekleyerek bir C# sınıf kitaplığı işlevi](#c-class-library-2).

Ayrıca [projenize yeni bir işlev ekleme](#add-a-function-to-your-project).

## <a name="install-binding-extensions"></a>Bağlama uzantılarını yükleme

HTTP ve Zamanlayıcı Tetikleyicileri dışında bağlamaları uzantı paketleri içinde uygulanır. Onları gerektiren bağlamalar ve Tetikleyiciler için uzantı paketleri yüklemeniz gerekir. Bağlama uzantıları yükleme yolu, proje dile bağlıdır.

### <a name="javascript"></a>JavaScript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>C\# sınıf kitaplığı

Çalıştırma [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) projenizde ihtiyacınız uzantı paketleri yüklemek için Terminal penceresinde komutu. Aşağıdaki örnek, uygulayan bağlamaları Blob, kuyruk ve tablo depolama için Azure depolama uzantıyı yükler.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

## <a name="add-a-function-to-your-project"></a>Bir işlev projenize ekleyin.

Önceden tanımlanmış işlevler tetikleyici şablonlardan birini kullanarak varolan bir projeye yeni bir işlev ekleyebilirsiniz. Yeni Tetikleyici işlevi eklemek için komut paletini açın ardından aramak ve komutu çalıştırmak için F1 tuşuna basın. **Azure işlevleri: İşlev oluştur...** . Tetikleyici türünüzü seçin ve tetikleyicinin gerekli öznitelikleri tanımlamak için yönergeleri izleyin. Tetikleyicinizin bir hizmete bağlanmak için bir erişim anahtarı veya bağlantı dizesi gerektiriyorsa, işlev tetikleyicisi oluşturmadan önce hazır olun. 

Bu işlemin sonuçlarının proje dilinizi bağlıdır:

### <a name="javascript"></a>JavaScript

Yeni bir function.json dosya ve yeni bir JavaScript kod dosyası içeren projede yeni bir klasör oluşturulur.

### <a name="c-class-library"></a>C\# sınıf kitaplığı

Yeni bir C# sınıf kitaplığı (.cs) dosyası projenize eklenir.

## <a name="add-input-and-output-bindings"></a>Giriş ve çıkış bağlamaları

İşlevinizin giriş ve çıkış bağlamaları ekleyerek genişletebilirsiniz. Bunu yaptığınız gibi proje dile bağlıdır. Bağlamaları hakkında daha fazla bilgi için bkz: [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md). 

Aşağıdaki örnekte adlı bir depolama kuyruğuna bağlanmak `outqueue`, depolama hesabı için bağlantı dizesi ayarlama burada `MyStorageConnection` local.settings.json uygulama ayarı. 

### <a name="javascript"></a>JavaScript

Visual Studio Code istemleri kullanışlı bir dizi izleyerek function.json dosyanıza bağlamaları eklemenizi sağlar. Bir bağlamayı oluşturmak için sağ tıklatın (Ctrl + tıklama macos'ta) `function.json` seçin ve dosya işlevi klasörünüzde **bağlama Ekle...** . 

![Bağlama eklemek için var olan bir JavaScript işlevi ](media/functions-develop-vs-code/function-add-binding.png)

Yeni bir tanımlamak için örnek yönergeleri verilmiştir depolama çıkış bağlaması:

| İstem | Değer | Açıklama |
| -------- | ----- | ----------- |
| **Bağlama yönünü seçin** | `out` | Bağlama bir çıkış bağlaması var. |
| **Bağlama yönünü seçin...** | `Azure Queue Storage` | Bir Azure depolama kuyruğu bağlaması bağlamadır. |
| **Kodunuzda bu bağlamayı tanımlamak için kullanılan ad** | `msg` | Kodunuzda başvurulan bağlama parametresi tanımlayan ad. |
| **Kuyruk iletisi gönderilir** | `outqueue` | Bağlama Yazar Kuyruğun adı. Zaman *queueName* yok, ilk kullanımda bağlama oluşturur. |
| **"Local.setting.json" ayarı seçin** | `MyStorageConnection` | Depolama hesabı için bağlantı dizesi içeren bir uygulama ayarı adı. `AzureWebJobsStorage` Ayarı, işlev uygulaması ile oluşturduğunuz depolama hesabının bağlantı dizesini içerir. |

Bu örnekte, aşağıdaki bağlama eklenen `bindings` function.json dosyanızdaki dizisi:

```javascript
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```

Bu gibi durumlarda, aynı bağlama tanım ayrıca doğrudan, function.json ekleyebilirsiniz.

İşlev kodunuzda `msg` bağlama erişilen `context`, aşağıdaki örnekte olduğu gibi:

```javascript
context.bindings.msg = "Name passed to the function: " req.query.name;
```

Daha fazla bilgi için bkz. [kuyruk depolama çıkış bağlaması](functions-bindings-storage-queue.md#output---javascript-example) başvuru.

### <a name="c-class-library"></a>C\# sınıf kitaplığı

Güncelleştirme için aşağıdaki parametreyi eklemek için işlevi yöntemi `Run` yöntemi tanımı:

```cs
[Queue("outqueue"),StorageAccount("MyStorageConnection")] ICollector<string> msg
```

Bu kod aşağıdaki eklemenizi gerektirir `using` deyimi:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

`msg` Parametresi bir `ICollector<T>` bağlamayı çıktısıysa işlev yazılır iletileri koleksiyonunu temsil eden tür tamamlar. Bir veya daha fazla ileti koleksiyona işlevi tamamlandığında, kuyruğa gönderilen eklersiniz.

Daha fazla bilgi için bkz. [kuyruk depolama çıkış bağlaması](functions-bindings-storage-queue.md#output---c-example) başvuru.

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="publish-to-azure"></a>Azure'a Yayımlama

Visual Studio Code, işlevler projenizi doğrudan Azure’da dağıtmanıza olanak sağlar. Süreç kapsamında, Azure abonelik bir işlev uygulaması ve ilgili kaynakları oluşturursunuz. İşlev uygulaması, işlevlerinize ilişkin bir yürütme bağlamı sağlar. Proje, Azure aboneliğinizdeki yeni işlev uygulamasında paketlenir ve dağıtılır.

Visual Studio Code'dan yayımlarken, iki dağıtım yöntemlerinden birini kullanılır:

* [Çalışma alanından-etkin paket dağıtma zip](functions-deployment-technologies.md#zip-deploy): Azure işlevleri dağıtımların çoğu için kullanılır.
* [Dış paket URL'si](functions-deployment-technologies.md#external-package-url): dağıtım Linux uygulamaları için kullanılan bir [tüketim planı](functions-scale.md#consumption-plan).

### <a name="quick-function-app-creation"></a>Hızlı bir işlev uygulaması oluşturma

Varsayılan olarak, Visual Studio Code, işlev uygulamanız tarafından gereken Azure kaynakları için değerleri otomatik olarak oluşturur. Bu değerler, seçtiğiniz işlev uygulamasının adı temel alır. Azure'da yeni bir işlev uygulaması projenizi yayımlamak için varsayılanları kullanarak bir örnek için bkz: [Visual Studio Code hızlı başlangıç makalesi](functions-create-first-function-vs-code.md#publish-the-project-to-azure).

Açık oluşturulan kaynakların adlarını sağlamak istiyorsanız, Gelişmiş seçenekleri kullanarak yayımlama etkinleştirmeniz gerekir.

### <a name="enabled-publishing-with-advanced-create-options"></a>Gelişmiş ile etkin yayımlama oluşturma seçenekleri

Ayarlar üzerinde denetim size uygulamaları oluştururken Azure işlevleri ile ilişkili, Gelişmiş ayarları etkinleştirmek için Azure işlevleri uzantısını güncelleştirin.

1. Tıklayın **Dosya > Tercihler > ayarları**

1. Gezinmek **kullanıcı ayarları > uzantılar > Azure işlevleri**

1. Onay için **Azure işlevi: Gelişmiş oluşturma**

### <a name="publish-to-a-new-function-app-in-azure-with-advanced-creation"></a>Yeni bir işlev uygulaması azure'da Gelişmiş oluşturma ile yayımlama

Aşağıdaki adımlarda oluşturulan yeni bir işlev uygulaması için projenizi yayımlamak Gelişmiş kullanarak oluşturma seçenekleri.

1. İçinde **Azure: İşlevleri** alan, işlev uygulaması simgesine Dağıt'ı seçin.

    ![İşlev uygulaması ayarları](./media/functions-develop-vs-code/function-app-publish-project.png)

1. Değil oturum açma, siz istenirse **Azure'da oturum aç**. Ayrıca **ücretsiz bir Azure hesabı oluşturun**. Tarayıcıdan sonra başarılı oturum açma, Visual Studio Code için geri dönün.

1. Birden fazla aboneliğiniz varsa **bir abonelik seçin** seçin işlev uygulaması için **+ oluştur yeni işlev uygulamanızı Azure'a**.

1. Komut istemlerini, aşağıdaki bilgileri sağlayın:

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----------- |
    | Azure işlev uygulaması seçin | + Azure'da yeni işlev uygulaması oluşturma | Sonraki isteminde yeni işlev uygulamanızı tanımlayan genel olarak benzersiz bir ad yazın ve Enter tuşuna basın. İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir. |
    | Bir işletim sistemi seçin | Windows | İşlev uygulaması, Windows üzerinde çalışır |
    | Bir barındırma planı seçin | Tüketim planı | Sunucusuz [tüketim barındırma planı](functions-scale.md#consumption-plan) kullanılır. |
    | Yeni uygulamanız için bir çalışma zamanı seçin | Proje dilinizi | Çalışma zamanı yayımlamakta olduğunuz proje eşleşmesi gerekir. |
    | Yeni kaynaklar için bir kaynak grubu seçin | Yeni kaynak grubu oluşturun | Sonraki satırında bir kaynak grubu adı gibi tür `myResourceGroup`, ve enter tuşuna basın. Ayrıca, mevcut bir kaynak grubunu seçebilirsiniz. |
    | Bir depolama hesabı seçin | Yeni depolama hesabı oluşturma | Sonraki satırında yeni depolama hesabı genel olarak benzersiz bir ad türü kullanılan Enter tuşuna basın ve işlev uygulaması. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Ayrıca, mevcut bir hesabı seçebilirsiniz. |
    | Yeni kaynaklar için bir konum seçin | bölge | Ayrıca, kendinize veya işlevlerinizin erişeceği diğer hizmetlere yakın bir [bölgede](https://azure.microsoft.com/regions/) yer alan bir konum seçin. |

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Seçin **görünümü çıkış** oluşturduğunuz Azure kaynaklarını oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimi dahil olmak üzere.

## <a name="republish-project-files"></a>Proje dosyaları yeniden yayımlayın

Ayarladığınızda [sürekli dağıtım](functions-continuous-deployment.md), bağlı kaynak konumda güncelleştirilmiş kaynak dosyalarını azure'daki işlev uygulamanızın güncelleştirilir. Bu geliştirme uygulama öneririz, ancak Visual Studio Code, proje dosyası güncelleştirmeleri de yeniden yayımlayabilirsiniz. 

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.

1. Visual Studio Code'da komut paletini açın için F1 tuşuna basın. Arayın ve seçin komut Paleti'nde `Azure Functions: Deploy to function app...`.

1. Değil oturum açma, siz istenirse **Azure'da oturum aç**. Tarayıcıdan sonra başarılı oturum açma, Visual Studio Code için geri dönün. Birden fazla aboneliğiniz varsa **bir abonelik seçin** işlevi uygulamanızı içeren.

1. Azure'da mevcut işlevi uygulamanızı seçin. İşlev uygulaması tüm dosyaların üzerine hakkında bir uyarı seçin **Dağıt** uyarı bildirimi ve devam edin. 

Proje yeniden, paketlenmiş ve Azure'a karşıya yüklendi. Varolan projeyi yeni paketi tarafından değiştirilir ve işlev uygulamasını yeniden başlatır.

## <a name="get-deployed-function-url"></a>Dağıtılmış bir işlev URL'sini Al

HTTP ile tetiklenen bir işlev çağrısı yapabilmek için işlev uygulamanıza dağıtıldığında işlev URL'si gerekir. Bu URL gerekli içerir [işlev anahtarları](functions-bindings-http-webhook.md#authorization-keys). Uzantı, bu URL'ler için dağıtılan işlevlerinizi almak için kullanabilirsiniz.

1. komut paletini açın ardından aramak ve komutu çalıştırmak için F1 tuşuna **Azure işlevleri: İşlev URL'sini kopyalama**.

1. Azure ve ardından çağırmak istediğiniz belirli HTTP tetikleyici işlevi uygulamanızı seçin için istemleri izleyin. 

URL panoya iletilen tüm gerekli anahtarlarla birlikte kopyalanır işlevi `code` sorgu parametresi. Uzak işlev GET istekleri için bir tarayıcı veya POST istekleri göndermek için bir HTTP aracını kullanın.  

## <a name="run-functions-locally"></a>İşlevleri yerel olarak çalıştırma

Azure işlevleri uzantı işlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak tanır. Yerel çalışma zamanı, işlev uygulamanızı azure'da barındıran aynı çalışma zamanıdır. Den okunan yerel ayarları [local.settings.json dosyasında](#local-settings-file).

### <a name="additional-requirements-to-run-locally"></a>Yerel olarak çalıştırmak için ek gereksinimler

Yerel olarak işlevler projeniz çalıştırılabilmesi için de bu ek gereksinimleri karşılaması gerekir:

* Sürümünü yüklemek 2.x [Azure işlevleri çekirdek Araçları](functions-run-local.md#v2). Temel Araçları Paketi indirilir ve yüklü, otomatik olarak projeyi yerel olarak başlattığınızda. İndirme ve yükleme biraz zaman alabilir bu nedenle tüm Azure işlevleri çalışma zamanı temel araçlar içerir.

* Seçtiğiniz dile özgü gereksinimleri yükleyin:

    | Dil | Gereksinim |
    | -------- | --------- |
    | **C#** | [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[.NET core CLI araçları](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)   |
    | **Java** | [Java uzantı için hata ayıklayıcı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3+](https://maven.apache.org/) |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> |  
    | **Python** | [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)<br/>[Python 3.6 +](https://www.python.org/downloads/)|

    <sup>*</sup>Etkin LTS ve Bakım LTS sürümleri (8.11.1 ve önerilen 10.14.1).

### <a name="configure-the-project-to-run-locally"></a>Yerel olarak çalıştırmak için proje yapılandırma

İşlevler çalışma zamanı HTTP ve Web kancaları dışındaki tüm tetikleyici türleri için dahili olarak bir Azure depolama hesabı kullanır. Yani, ayarlamalısınız **Values.AzureWebJobsStorage** geçerli bir Azure depolama hesabı bağlantı dizesi için anahtar.

Bu bölümde kullanan [Visual Studio Code için Azure depolama uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) ile [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/) bağlanmak ve depolama bağlantı dizesini almak için.   

Depolama hesabı bağlantı dizesi ayarlamak için:

1. Visual Studio'da açın **Cloud Explorer**, genişletme **depolama hesabı** > **depolama hesabınızı**, ardından **özellikleri**ve kopyalama **PRIMARY CONNECTION Strıng'i** değeri.

2. Projenizdeki local.settings.json dosyasını açın ve değerini ayarlama **AzureWebJobsStorage** anahtar bağlantı dizesine kopyalanır.

3. Benzersiz anahtarlar için eklemek için önceki adımı yineleyin **değerleri** işlevleriniz tarafından gerekli herhangi bir bağlantı için bir dizi.

Daha fazla bilgi için [yerel ayarları dosyası](#local-settings-file).

### <a name="debugging-functions-locally"></a>İşlevleri yerel olarak hata ayıklama  

İşlevlerinizi hata ayıklamak için F5 tuşuna basın. Zaten indirmediyseniz [temel araçları][azure işlevleri temel araçları], bunu yapmak istenir. Ne zaman temel araçları yüklü ve çalışıyor, çıkış terminalde gösterilir. Çalışan ile aynıdır `func host start` çekirdek Araçları komut terminalden, ancak ek derleme görevleri ve ekli bir hata ayıklayıcı.  

Azure'da dağıtıldığında olduğu gibi projenin ile çalışan işlevlerinizi tetikleyebilirsiniz. Hata ayıklama modunda çalışırken, kesme noktaları Visual Studio Code'da beklendiği gibi ulaşıldığından.

İstek URL'si için HTTP Tetikleyicileri terminal çıktısında görüntülenir. HTTP tetikleyici işlevi anahtarlar, yerel olarak çalıştırılırken kullanılmaz. Daha fazla bilgi için [kodunuzu Azure işlevleri'nde test stratejileri](functions-test-a-function.md).  

Daha fazla bilgi için bkz. [iş ile Azure işlevleri çekirdek Araçları][azure işlevleri temel araçları].

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Varsayılan olarak, projeyi Azure'da yayımlandığında bu ayarlar otomatik olarak geçirilmez. Yayımlama işlemi tamamlandıktan sonra işlev uygulamanızı local.settings.json ayarlarını Azure'da yayımlama seçeneği sunulur. Daha fazla bilgi için bkz. [yayımlama uygulama ayarları](#publish-application-settings).

Değerler **ConnectionStrings** hiçbir zaman yayımlanır.

İşlev uygulaması ayarları değerleri, ortam değişkenleri olarak kodunuzda da okunabilir. Daha fazla bilgi için bu dile özgü başvuru makalelerimize ortam değişkenleri bölümüne bakın:

* [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
* [C# betiği (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

## <a name="application-settings-in-azure"></a>Azure uygulama ayarları

Projenizi local.settings.json dosyasında ayarları uygulama ayarlarında, bir Azure işlev uygulaması ile aynı olması gerekir. Azure işlev uygulaması da local.settings.json dosyasına eklediğiniz herhangi bir ayarı eklenmesi gerekir. Proje yayımladığınızda, bu ayarlar otomatik olarak karşıya yüklenmemiş. Benzer şekilde, işlev uygulamanızı oluşturmak, tüm ayarlarını [portalında](functions-how-to-use-azure-function-app-settings.md#settings) yerel projenize indirilmelidir.

### <a name="publish-application-settings"></a>Uygulama ayarları yayımlama

Gerekli ayarları işlev uygulamanızı azure'da yayımlamak için en kolay yolu kullanmaktır **karşıya yükleme ayarları** projenizi başarıyla yayımladıktan sonra görüntülenen bağlantı.

![Dağıtım tamamlandı karşıya yükleme uygulama ayarları](./media/functions-develop-vs-code/upload-app-settings.png)

Kullanarak ayarları yayımlayabilirsiniz `Azure Functions: Upload Local Setting` komut Paleti'nde komutu. Tek tek ayarları kullanarak Azure uygulama ayarlarında eklenir `Azure Functions: Add New Setting...` komutu. 

> [!TIP]
> Yayımlamadan önce local.settings.json dosyanızı kaydedin emin olun.

Yerel dosya şifrelenmişse, şifresi, yayımlanan ve yeniden şifrelenir. Ayarları konumlarının her ikisinde de farklı değerlerle varsa, devam etmek nasıl seçme istenir.

Mevcut uygulama ayarlarında görüntülemek **Azure: İşlevleri** aboneliğiniz, işlev uygulamanızı genişleterek alan ve **uygulama ayarları**.

![Visual Studio code'da görünümü işlev uygulaması ayarı](./media/functions-develop-vs-code/view-app-settings.png)

### <a name="download-settings-from-azure"></a>Azure'dan indirme ayarları

Uygulama ayarlarını Azure'da oluşturduysanız, bunları, local.settings.json dosyasına indirebilirsiniz. kullanarak `Azure Functions: Download Remote Settings...` komutu. 

Yerel dosya şifrelenmişse, karşıya gibi ile şifresi, güncelleştirme ve yeniden şifrelenir. Ayarları konumlarının her ikisinde de farklı değerlerle varsa, devam etmek nasıl seçme istenir.

## <a name="monitoring-functions"></a>İzleme işlevleri

Olduğunda, [yerel olarak çalıştırma](#run-functions-locally), günlük verilerini Terminal konsola akış. Bir işlev uygulaması ile Azure işlevleri projenizi çalışırken günlük veri de alabilirsiniz. Ya da akış günlükleri neredeyse gerçek zamanlı günlük verilerini görmek için Azure için bağlanın veya işlev uygulamanızı nasıl davrandığını bir daha kapsamlı anlamak için Application ınsights'ı etkinleştirebilirsiniz.

### <a name="streaming-logs"></a>Akış günlükleri

Bir uygulama geliştirirken, genellikle günlük kaydı bilgilerini neredeyse gerçek zamanlı görmek yararlı olur. İşlevleriniz tarafından oluşturulan günlük dosyalarının akışını görüntüleyebilirsiniz. Aşağıdaki çıktı, akış günlükleri için bir HTTP isteğinin bir örneği ile tetiklenen işlev.

![Akış günlükleri için HTTP tetikleyici çıkışı](media/functions-develop-vs-code/streaming-logs-vscode-console.png) 

Daha fazla bilgi için bkz. [akış günlüklerini](functions-monitoring.md#streaming-logs). 

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

> [!NOTE]
> Akış günlükleri işlevleri konak yalnızca tek bir örneğini destekler. İşlevinizi birden fazla örneğe ölçeklendirildiğinde diğer örneklerindeki verilerin günlük stream'de gösterilmez. [Canlı ölçümleri Stream](../azure-monitor/app/live-stream.md) Application Insights'ta birden çok örneği desteklenir. Ayrıca, gerçek zamanlı, akış analizini de dayanır ancak [veri örneklenen](functions-monitoring.md#configure-sampling).

### <a name="application-insights"></a>Application Insights

İşlevlerinizin yürütülmesini izlemek için önerilen yöntem, işlev uygulamanızı Azure Application Insights ile tümleştirerek ' dir. Bu tümleştirme, Azure portalında bir işlev uygulaması oluşturduğunuzda, sizin için varsayılan olarak gerçekleştirilir. Ancak, Visual Studio yayımlama sırasında işlev uygulamanızı oluşturmak, işlev uygulamanızı azure'da tümleştirme bitti değil.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Daha fazla bilgi için bkz. [İzleyici Azure işlevleri](functions-monitoring.md).

## <a name="c-script-projects"></a>C\# kod projeleri

Varsayılan olarak, tüm C# projeleri olarak oluşturulan [ C# sınıf kitaplığı projelerine derlenen](functions-dotnet-class-library.md). Bunun yerine ile çalışmayı tercih ederseniz C# betik projeleri seçmelisiniz C# betik Azure işlevleri uzantı ayarları varsayılan dil olarak.

1. Tıklayın **Dosya > Tercihler > ayarları**.

1. Gezinmek **kullanıcı ayarları > uzantılar > Azure işlevleri**.

1. Seçin **C #Script** gelen **Azure işlevi: Proje dili**.

Bu noktada, temel alınan temel araçları için yapılan çağrılar dahil `--csx` oluşturur ve yayımlar seçeneği C# komut proje dosyalarını (.csx). Belirtilen bir varsayılan dili ile tüm oluşturulmuş projeler varsayılan olarak C# kod projeleri. Varsayılan olarak ayarlandığında proje dili seçmeniz istendiğinde değil. Diğer dil projeleri oluşturmak için bu ayarı değiştirmek veya kullanıcı settings.json dosyasından kaldırmanız gerekir. Bu ayar kaldırdıktan sonra bir proje oluşturduğunuzda dilinizi yeniden istenir.

## <a name="command-palette-reference"></a>Komut paleti başvurusu

Azure işlevleri uzantı, işlev uygulamalarınızı Azure ile etkileşim kurmak için Azure alanında yararlı bir grafik arabirim sağlar. Aynı işlevselliği, komutlar komut Paleti'nde (F1) olarak da kullanılabilir. Azure işlevleri özgü aşağıdaki komutlar kullanılabilir:

|Azure işlevleri komutu  | Açıklama  |
|---------|---------|
|**Yeni ayarları Ekle...**  |  Azure'da yeni bir uygulama ayarı oluşturur. Daha fazla bilgi için bkz. [yayımlama uygulama ayarları](#publish-application-settings). Ayrıca gerekebilir [yerel ayarlarınızı bu ayar indirme](#download-settings-from-azure). |
| **Dağıtım Kaynağı Yapılandır...** | İşlev uygulamanızı azure'da yerel bir Git deposuna bağlanın. Daha fazla bilgi için bkz. [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md). |
| **GitHub deposuna Bağlan...** | İşlev uygulamanızı bir GitHub deposuna bağlanın. |
| **İşlev URL'sini kopyalama** | Alır, uzak bir HTTP URL'sini Azure'da çalışan bir işlev tetiklenir. Daha fazla bilgi için bkz. nasıl [dağıtılan işlev URL'sini Al](#get-deployed-function-url). |
| **Azure işlev uygulaması oluşturma...** | Azure aboneliğinizdeki yeni bir işlev uygulaması oluşturur. Daha fazla bilgi için bkz. nasıl [azure'da yeni bir işlev uygulaması için Yayımlama](#publish-to-azure).        |
| **Ayarları şifresini çözme** | Şifresini çözmek için kullanabilirler [yerel ayarları](#local-settings-file) şifrelenmiş kullanarak `Azure Functions: Encrypt Settings`.  |
| **İşlev uygulaması Sil...** | Azure aboneliğinizde bir işlev uygulamanız kaldırır. App Service planında başka hiçbir uygulama olduğunda, size çok silme seçeneğine verilir. Depolama hesapları ve kaynak grupları gibi diğer kaynaklar silinmez. Tüm kaynakları kaldırmak için bunun yerine gereken [kaynak grubunu silin](functions-add-output-binding-storage-queue-vs-code.md#clean-up-resources). Yerel projenizi etkilenmez. |
|**İşlevi Sil...**  | Var olan bir işlevi bir Azure işlev uygulamasında kaldırır. Bu silme işlemi, yerel proje etkilemez, çünkü bunun yerine işlevi yerel olarak kaldırmayı deneyin ve ardından [projenizi yeniden yayımlanması](#republish-project-files). |
| **Proxy silin...** | İşlev uygulamanızı azure'da bir Azure işlevleri proxy kaldırır. Proxy hakkında daha fazla bilgi için bkz: [iş ile Azure işlevleri proxy'leri](functions-proxies.md). |
| **Ayar silme...** | Bir Azure işlevi uygulaması ayarında siler. Local.settings.json dosyanızdaki ayarlarını etkilemez. |
| **Depodan bağlantısını kes...**  | Kaldırma [sürekli dağıtım](functions-continuous-deployment.md) Azure işlev uygulamasında bir kaynak denetim deposu arasındaki bağlantı. |
| **Uzak bağlantı ayarları indir...** | Ayarlar seçtiğiniz işlev uygulamanızı Azure'a, local.settings.json dosyasına indirir. Yerel dosya şifrelenmişse, şifresi, güncelleştirme ve yeniden şifrelenir. Ayarları konumlarının her ikisinde de farklı değerlerle varsa, devam etmek nasıl seçme istenir. Değişiklikleri local.settings.json dosyanıza bu komutu çalıştırmadan önce kaydettiğinizden emin olun. |
| **Ayarları Düzenle...** | Azure'da var olan bir işlev uygulama ayarı değerini değiştirir. Local.settings.json dosyanızdaki ayarlarını etkilemez.  |
| **Şifreleme ayarları** | Tek tek öğeleri şifreler `Values` içindeki dizi [yerel ayarları](#local-settings-file). Bu dosyadaki `IsEncrypted` ayrıca kümesine `true`, kullanmadan önce ayarları şifresini çözmek için yerel çalışma zamanı söyler. Değerli bilgi sızıntısı riskini azaltmak için yerel ayarları şifreleyin. Azure'da uygulama ayarları depolanan her zaman şifrelenir. |
| **Şimdi, işlevi yürütme** | Başlatan bir [Zamanlayıcı tetiklenen işlevi](functions-bindings-timer.md) azure'da el ile test amaçları için. Azure işlevleri HTTP olmayan tetikleme hakkında daha fazla bilgi için bkz: [olmayan HTTP ile tetiklenen bir işlev el ile çalıştırmak](functions-manually-run-non-http.md). |
| **VS Code ile kullanım için proje başlatılıyor...** | Gerekli Visual Studio Code proje dosyaları var olan bir işlevler projeye ekler. Temel araçları ile oluşturulmuş bir proje ile çalışmak için bu komutu kullanın. |
| **Azure işlevleri çekirdek araçları güncelleştirme yüklemesi** | Yükler veya güncelleştirmeler [Azure işlevleri temel araçları] yerel olarak çalıştırmak için kullanılır. |
| **Yeniden dağıtma**  | Azure'da belirli bir dağıtım için bağlı bir Git deposundan proje dosyalarını yeniden dağıtma olanak sağlar. Visual Studio Code yerel güncelleştirmeleri yayımlamayı [projenizi yeniden yayımlamanız](#republish-project-files). |
| **Ayarları yeniden adlandır...** | Azure'da var olan bir işlev uygulama ayarı anahtar adını değiştirir. Local.settings.json dosyanızdaki ayarlarını etkilemez. Azure ayarlarında yeniden adlandırıldıktan sonra yapmanız gerekenler [bu değişiklikleri yerel projeye Yükle](#download-settings-from-azure). |
| **Yeniden başlatma** | İşlev uygulamasını Azure'da yeniden başlatır. Güncelleştirmeleri dağıtmak, işlev uygulamasını başlatır. |
| **AzureWebJobStorage Ayarla...**| Ayarlar `AzureWebJobStorage` uygulama ayarı. Bu ayar, Azure işlevleri tarafından gereklidir ve Azure işlev uygulaması oluşturulduğunda ayarlanır. |
| **Start** | Durdurulmuş bir işlev uygulaması, Azure'da başlatır. | 
| **Akış günlükleri Başlat** | İşlev uygulaması için akış günlükleri Azure'da başlatır. Akış günlükleri neredeyse gerçek zamanlı olarak bu bilgileri görmeniz gerekiyorsa uzaktan Azure'da sorun giderme sırasında kullanın. Daha fazla bilgi için bkz. [akış günlüklerini](#streaming-logs). |
| **Durdur** | Azure'da çalışan bir işlev uygulaması kapatıldığında aşağı. |
| **Akış günlükleri Durdur** | Azure işlev uygulaması için akış günlükleri durdurur. |
| **Yuva ayarı olarak değiştir** | Etkin olduğunda, bir uygulama ayarı için bir dağıtım yuvasını devam ettiğini emin olur. |
| **Azure işlevleri çekirdek Araçları'nı kaldırın** | Uzantısı için gereken Azure işlevleri çekirdek Araçları'nı kaldırır. |
| **Yerel ayarlar karşıya yükle...** | Local.settings.json dosyanızdan ayarlar seçtiğiniz işlev uygulamanızı Azure'a yükler. Yerel dosya şifrelenmişse, şifresi, karşıya yüklenen ve yeniden şifrelenir. Ayarları konumlarının her ikisinde de farklı değerlerle varsa, devam etmek nasıl seçme istenir. Değişiklikleri local.settings.json dosyanıza bu komutu çalıştırmadan önce kaydettiğinizden emin olun. |
| **Github'da işlemesini görüntüle** | İşlev uygulamanızı bir depoya bağlı olduğunda, belirli bir dağıtımda bulunan en son işlemeyi gösterir. |
| **Dağıtım günlüklerini görüntüle** | İşlev uygulaması için belirli bir dağıtım için günlüklere Azure'da gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri çekirdek araçları hakkında daha fazla bilgi için bkz: [kod ve Azure işlevleri yerel olarak test](functions-run-local.md).

.NET sınıf kitaplıkları olarak işlevleri geliştirme hakkında daha fazla bilgi için bkz. [Azure işlevleri C# Geliştirici Başvurusu](functions-dotnet-class-library.md). Bu makalede ayrıca bağlamaları Azure işlevleri tarafından desteklenen çeşitli türlerde bildirmek için öznitelikleri kullanma örnekleri bağlantılarını içerir.    

[Visual Studio Code için Azure İşlevleri uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
[Azure işlevleri temel araçları]: functions-run-local.md