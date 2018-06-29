---
title: Çekirdek araçları ile Azure işlevleri çalışma | Microsoft Docs
description: Kod ve Azure işlevlerini çalıştırmadan önce komut istemi veya terminal, yerel bilgisayarınızda Azure işlevlerini test öğrenin.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/26/2018
ms.author: glenga
ms.openlocfilehash: 5c582b080ec6f2cff801758fc4bff4f7d07fd7df
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083078"
---
# <a name="work-with-azure-functions-core-tools"></a>Çekirdek araçları ile Azure işlevleri çalışma

Azure işlevleri çekirdek araçları geliştirmek ve komut istemi veya terminal, yerel bilgisayarınızda işlevlerinizi test olanak sağlar. Yerel işlevlerinizi Azure Hizmetleri Canlı bağlanabilir ve tam işlevleri çalışma zamanı kullanarak, yerel bilgisayarınızda işlevlerinizi ayıklayabilirsiniz. Bu gibi durumlarda, bir işlev uygulaması bile Azure aboneliğinize dağıtabilirsiniz.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## <a name="core-tools-versions"></a>Çekirdek araçları sürümleri

Azure işlevleri çekirdek araçları iki sürümü vardır. Kullandığınız sürüm, yerel geliştirme ortamı, dil seçimi ve gerekli destek düzeyine bağlıdır:

+ [Sürüm 1.x](#v1): sürüm destekler 1.x genellikle kullanılabilir (GA) çalışma zamanı. Araçları'nın bu sürümü yalnızca Windows bilgisayarlarında desteklenir ve gelen yüklü bir [npm paket](https://docs.npmjs.com/getting-started/what-is-npm). Bu sürümle birlikte, resmi olarak desteklenmez Deneysel dillerde işlevleri oluşturabilirsiniz. Daha fazla bilgi için bkz: [Azure işlevlerinde desteklenen diller](supported-languages.md)

+ [Sürüm 2.x](#v2): sürüm destekler çalışma zamanının 2.x. Bu sürüm destekler [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux). Platforma özgü paket yöneticileri veya npm yükleme için kullanır. 2.x çalışma zamanı gibi çekirdek Araçları'nın bu sürümü şu anda önizlemede değil.

Aksi belirtilmediği sürece, bu makaledeki örneklerde sürümü için olan 2.x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

[Azure işlevleri çekirdek Araçları] yerel geliştirme bilgisayarınızda çalıştırabilirsiniz Azure işlevleri çalışma zamanı'nın temelini oluşturan aynı çalışma zamanı sürümü içerir. Ayrıca, İşlevler oluşturmak, Azure'a bağlanmak ve işlev projeleri dağıtmak için komutları sağlar.

### <a name="v1"></a>Sürüm 1.x

Araçlar özgün sürümü işlevleri 1.x çalışma zamanı kullanır. Bu sürüm, .NET Framework (4.7.1) kullanır ve yalnızca Windows bilgisayarlarda desteklenir. Sürüm 1.x Araçları yüklemeden önce şunları yapmalısınız [NodeJS yükleme](https://docs.npmjs.com/getting-started/installing-node), npm içerir.

Sürüm 1.x araçlarını yüklemek için aşağıdaki komutu kullanın:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Sürüm 2.x

>[!NOTE]
> Azure işlevleri çalışma zamanı 2.0 Önizleme aşamasındadır ve Azure işlevlerinin şu anda tüm özellikler desteklenir. Daha fazla bilgi için bkz: [Azure işlevleri sürümleri](functions-versions.md) 

Sürüm 2.x araçları kullanan Azure işlevleri çalışma zamanı .NET Core üzerinde oluşturulmuş 2.x. Tüm platformlarda .NET Core 2.x destekler dahil olmak üzere, bu sürümü desteklenmiyor [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux).

#### <a name="windows-npm"></a>Windows

Windows çekirdek araçlarını yüklemek için npm aşağıdaki adımları kullanın. Aynı zamanda [Chocolatey](https://chocolatey.org/). Daha fazla bilgi için bkz: [çekirdek araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Yükleme [Windows için .NET Core 2.0](https://www.microsoft.com/net/download/windows).

2. Yükleme [Node.js], npm içerir. Sürümü için Araçlar, yalnızca Node.js 8.5 ve sonraki sürümleri 2.x desteklenir.

3. Çekirdek araçları paketini yükleyin:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>MacOS Homebrew ile

Aşağıdaki adımları Homebrew üzerinde macOS çekirdek araçlarını yüklemek için kullanın.

1. Yükleme [macOS .NET Core 2.0](https://www.microsoft.com/net/download/macos).

2. Yükleme [Homebrew](https://brew.sh/), henüz yüklü değilse.

3. Çekirdek araçları paketini yükleyin:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) APT ile

Aşağıdaki adımları kullanın [APT](https://wiki.debian.org/Apt) Ubuntu/Debian Linux dağıtım noktasında çekirdek araçlarını yüklemek için. Diğer Linux dağıtımları için bkz: [çekirdek araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Yükleme [Linux için .NET Core 2.0](https://www.microsoft.com/net/download/linux).

2. Microsoft ürün anahtarı güvenilir olarak kaydedin:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Ubuntu server uygun sürümlerinden birini tablosundan çalışıp çalışmadığını denetleyin. Apt kaynak eklemek için çalıştırın:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux dağıtım | Sürüm |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Naneli 18    | `xenial`  |

4. Çekirdek araçları paketini yükleyin:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

İşlevler proje dizinine dosyaları içeren [host.json](functions-host-json.md) ve [local.settings.json](#local-settings-file), tekil işlevler için kod içeren klasörleri boyunca. Bu dizinde bir işlev uygulaması Azure eşdeğeridir. İşlevler klasör yapısı hakkında daha fazla bilgi için bkz: [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md#folder-structure).

Sürüm 2.x başlatıldıktan ve kullanım varsayılan dil şablonlarını eklenen tüm işlevleri projeniz için bir varsayılan dil seçmenizi gerektirir. Sürümünde 1.x, belirlediğiniz dili her zaman bir işlev oluşturun.

Terminal penceresi veya bir komut isteminden, proje ve yerel Git deposu oluşturmak için aşağıdaki komutu çalıştırın:

```bash
func init MyFunctionProj
```

Sürümünde 2.x komutu çalıştırdığınızda, bir çalışma zamanı projeniz için seçmeniz gerekir. JavaScript işlevleri geliştirmek düşünüyorsanız seçin **düğümü**:

```output
Select a worker runtime:
dotnet
node
```

Yukarı/bir dil seçmek için aşağı ok tuşlarını kullanarak, ardından Enter tuşuna basın. Çıkış için bir JavaScript projesini aşağıdaki gibi görünür:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

Yerel bir Git deposu olmadan projesi oluşturmak için kullanın `--no-source-control [-n]` seçeneği.

## <a name="register-extensions"></a>Uzantılarını kaydetme

Sürümünde 2.x Azure işlevleri çalışma zamanına sahip açıkça işlevi uygulamanızda kullandığınız bağlama uzantılar (bağlama türleri) kaydetmek.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Daha fazla bilgi için bkz: [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Dosya local.settings.json uygulama ayarları, bağlantı dizeleri ve Azure işlevleri çekirdek araçları ayarlarını saklar. Aşağıdaki yapı vardır:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*"
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```

| Ayar      | Açıklama                            |
| ------------ | -------------------------------------- |
| **Isencrypted** | Ayarlandığında **doğru**, tüm değerleri yerel makine anahtarı kullanılarak şifrelenir. İle kullanılan `func settings` komutları. Varsayılan değer **false**. |
| **Değerler** | Uygulama ayarları ve yerel olarak çalıştırırken kullanılan bağlantı dizeleri koleksiyonu. Bu değerleri uygulama ayarlarında, azure'da işlevi uygulamanız gibi karşılık gelen **AzureWebJobsStorage** ve **AzureWebJobsDashboard**. Birçok Tetikleyicileri ve bağlamaları gibi bağlantı dizesi uygulama ayarı için başvuran özelliğine sahip **bağlantı** için [Blob Depolama tetikleyici](functions-bindings-storage-blob.md#trigger---configuration). Tür özellikleri için tanımlanan bir uygulama ayarı gereksinim **değerleri** dizi. <br/>**AzureWebJobsStorage** HTTP dışında Tetikleyiciler için gerekli uygulama ayarıdır. Olduğunda [Azure storage öykünücüsü](../storage/common/storage-use-emulator.md) ayarlayabileceğiniz yerel olarak yüklenmiş **AzureWebJobsStorage** için `UseDevelopmentStorage=true` ve çekirdek araçları öykünücüsü kullanır. Bu geliştirme sırasında yararlıdır, ancak dağıtmadan önce gerçek depolama bağlantısı ile test etmeniz gerekir. |
| **Ana Bilgisayar** | Bu bölümdeki ayarlarını işlevleri ana bilgisayar işlemi yerel olarak çalıştırırken özelleştirin. |
| **LocalHttpPort** | Yerel işlevler ana çalıştırırken kullanılan varsayılan bağlantı noktasını ayarlar (`func host start` ve `func run`). `--port` Komut satırı seçeneği bu değerin üzerine göre önceliklidir. |
| **CORS** | İzin verilen çıkış noktası tanımlar [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Çıkış boşluk virgülle ayrılmış bir liste olarak sağlanır. Joker karakter değeri (\*) desteklenir, her türlü kaynağa gelen isteklere izin verir. |
| **ConnectionStrings** | Bu koleksiyon, işlev bağlamaları tarafından kullanılan bağlantı dizeleri için kullanmayın. Bu koleksiyon yalnızca bağlantı dizeleri almalısınız çerçeveleri tarafından kullanılan **ConnectionStrings** gibi bir yapılandırma bölümünü dosya [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Bu nesne bağlantı dizeleri, sağlayıcı türü ortamıyla eklenir [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Bu koleksiyondaki öğeler için Azure diğer uygulama ayarlarıyla yayımlanmaz. Bu değerleri açıkça eklemelidir **bağlantı dizeleri** bölümünü **uygulama ayarları** işlevi uygulamanız için. |

İşlev uygulama ayarlarının değerleri, ortam değişkenleri olarak kodunuzda de okunabilir. Daha fazla bilgi için bu dile özgü başvuru konuları ortam değişkenleri bölümüne bakın:

+ [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
+ [C# betik (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Local.settings.json dosyasındaki ayarları, yalnızca yerel olarak çalıştırırken işlevleri araçları tarafından kullanılır. Projeyi Azure'a yayımlandığında varsayılan olarak, bu ayarlar otomatik olarak geçirilmez. Kullanım `--publish-local-settings` geçiş [yayımladığınızda](#publish) bu ayarlar, azure'da işlev uygulaması eklenir emin olmak için. Değerler **ConnectionStrings** hiçbir zaman yayımlanır.

İçin hiç geçerli bir depolama bağlantı dizesi ayarlandığında **AzureWebJobsStorage** ve öykünücü kullanılmadığından, aşağıdaki hata iletisi gösterilir:  

>Local.settings.json eksik değeri AzureWebJobsStorage için. Bu, HTTP dışındaki tüm tetikleyiciler için gereklidir. Çalıştırabilirsiniz ' func azure functionapp fetch-app-settings <functionAppName>' veya local.settings.json içinde bir bağlantı dizesini belirtin.

### <a name="get-your-storage-connection-strings"></a>Depolama bağlantı dizelerinizi Al

Depolama öykünücüsü geliştirme için kullanıldığında, bile, gerçek depolama bağlantısı ile test etmek isteyebilirsiniz. Zaten sahip olduğu varsayılarak [bir depolama hesabı oluşturuldu](../storage/common/storage-create-storage-account.md), geçerli bir depolama bağlantı dizesi aşağıdaki yollardan biriyle alabilirsiniz:

+ Gelen [Azure portal]. Depolama hesabınıza, select gidin **erişim anahtarları** içinde **ayarları**, aşağıdakilerden birini kopyalamak **bağlantı dizesi** değerleri.

  ![Azure portalından bağlantı dizesini kopyalayın](./media/functions-run-local/copy-storage-connection-portal.png)

+ Kullanım [Azure Storage Gezgini](http://storageexplorer.com/) Azure hesabınıza bağlanmak için. İçinde **Explorer**aboneliğinizi genişletin, depolama hesabınızı seçin ve birincil veya ikincil bağlantı dizesini kopyalayın. 

  ![Depolama Gezgini'nde bağlantı dizesini kopyalayın](./media/functions-run-local/storage-explorer.png)

+ Bağlantı dizesi Azure aşağıdaki komutlardan birini ile indirebilir çekirdek araçları kullanın:

    + Tüm ayarlar, var olan bir işlev uygulaması yükleyin:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + Bağlantı dizesi, belirli bir depolama hesabı için alın:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    Zaten Azure'da oturum açtığınızda, bunu yapmak için istenir.

## <a name="create-func"></a>Bir işlev oluşturun

Bir işlev oluşturmak için aşağıdaki komutu çalıştırın:

```bash
func new
```

Sürümünde çalıştırdığınızda 2.x `func new` işlevi uygulamanızı varsayılan dilde bir şablonu seçmesi istenir, ardından işlevinizi için bir ad seçmeniz istenir. Sürümünde 1.x, ayrıca dili seçmeniz istenir.

```output
Select a language: Select a template:
Blob trigger
Cosmos DB trigger
Event Grid trigger
HTTP trigger
Queue trigger
SendGrid
Service Bus Queue trigger
Service Bus Topic trigger
Timer trigger
```

Aşağıdaki sıra tetikleyici çıktıda görüldüğü gibi işlev kodu sağlanan işlev ada sahip bir alt klasör oluşturulur:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

Ayrıca, bu seçenekler şu bağımsız değişkenleri kullanarak komutu belirtebilirsiniz:

| Bağımsız değişken     | Açıklama                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| Dil C#, F # ve JavaScript gibi programlama şablonu. Bu seçeneği sürümünde gerekli 1.x. Sürümünde 2.x, bu seçeneği kullanmamanız veya projenizin varsayılan dili seçin. |
| **`--template -t`** | Şablon adı değerlerden biri olabilir:<br/><ul><li>`Blob trigger`</li><li>`Cosmos DB trigger`</li><li>`Event Grid trigger`</li><li>`HTTP trigger`</li><li>`Queue trigger`</li><li>`SendGrid`</li><li>`Service Bus Queue trigger`</li><li>`Service Bus Topic trigger`</li><li>`Timer trigger`</li></ul> |
| **`--name -n`** | İşlev adı. |

Örneğin, bir JavaScript HTTP tetikleyici içinde tek bir komut oluşturmak için çalıştırın:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Sıra tetiklenen bir işlev içinde tek bir komut oluşturmak için çalıştırın:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>İşlevler yerel olarak çalıştırma

İşlevler proje çalıştırmak için işlevleri konak çalıştırın. Ana bilgisayar için projedeki tüm işlevleri Tetikleyicileri sağlar:

```bash
func host start
```

`func host start` Aşağıdaki seçenekleri destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | Dinlemenin yapılacağı yerel bağlantı noktası. Varsayılan değer: 7071. |
| **`--debug <type>`** | Başlatır konak hata ayıklama bağlantı noktası ile açmak ekleyebilirsiniz böylece **func.exe** gelen işlem [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) veya [Visual Studio 2017](functions-dotnet-class-library.md). *\<Türü\>* seçenekleri `VSCode` ve `VS`.  |
| **`--cors`** | Boşluk CORS çıkış noktası virgülle ayrılmış listesi. |
| **`--nodeDebugPort -n`** | Kullanmak düğüm hata ayıklayıcı için bağlantı noktası. Varsayılan: Launch.json'u veya 5858 arasında bir değer. |
| **`--debugLevel -d`** | Konsol izleme düzeyini (kapalı, ayrıntılı, bilgi, uyarı veya hata). Varsayılan: bilgisi.|
| **`--timeout -t`** | Saniye cinsinden başlatmaya işlevleri ana bilgisayar için zaman aşımı. Varsayılan: 20 saniye.|
| **`--useHttps`** | Bağlamak `https://localhost:{port}` yerine çok `http://localhost:{port}`. Varsayılan olarak, bu seçenek bilgisayarınızda güvenilen bir sertifika oluşturur.|
| **`--pause-on-error`** | İşlem çıkmadan önce ek giriş duraklatılıyor. Çekirdek araçları Visual Studio veya VS Code başlatılırken kullanılır.|

İşlevler ana bilgisayar başladığında, URL, HTTP tetiklemeli işlevleri çıkarır:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="passing-test-data-to-a-function"></a>Bir işlev için test veri geçirme

İşlevlerinizi yerel olarak test etmek için [işlevleri konak Başlat](#start) ve uç noktaları HTTP isteklerini kullanarak yerel sunucuda çağırın. Çağırmanız endpoint işlevi türüne bağlıdır.

>[!NOTE]  
> Bu konudaki örnekler, terminal ya da bir komut isteminden HTTP istekleri göndermek için cURL aracını kullanın. Yerel sunucuya HTTP istekleri göndermek için tercih ettiğiniz bir aracı kullanabilirsiniz. CURL aracı, Linux tabanlı sistemlerinde varsayılan olarak kullanılabilir. Windows, önce indirmeniz gerekir ve yükleme [cURL aracı](https://curl.haxx.se/).

İşlevlerini test etme hakkında daha fazla genel bilgi için bkz: [Azure işlevleri, kodunuzu test etmek için stratejileri](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>HTTP ve Web kancası işlevleri tetiklendi

Aşağıdaki çağrı HTTP ve Web kancası yerel olarak çalıştırmak için uç nokta tetiklenen işlevleri:

    http://localhost:{port}/api/{function_name}

Aynı sunucu adını ve işlevleri konak dinlediği bağlantı noktasını kullandığınızdan emin olun. Bu işlev ana bilgisayarı başlatma sırasında oluşturulan çıktıda bakın. Tetik tarafından desteklenen herhangi bir HTTP yöntemini kullanarak bu URL çağırabilirsiniz. 

Aşağıdaki cURL komutunu Tetikleyicileri `MyHttpTrigger` olan bir GET isteğine hızlı başlangıç işlevinden _adı_ parametresine geçirilen sorgu dizesi içinde. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
Aşağıdaki örnek, bir POST isteğini geçirme adlı aynı işlevidir _adı_ istek gövdesindeki:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

Sorgu dizesinde veri geçirme bir tarayıcıdan GET istekleri yapabilirsiniz. Diğer tüm HTTP yöntemleri için cURL, Fiddler, Postman veya benzer bir HTTP test etme aracını kullanmanız gerekir.  

#### <a name="non-http-triggered-functions"></a>HTTP olmayan tetiklenen işlevleri

İşlevler HTTP tetikleyiciler ve Web kancalarını dışındaki tüm türleri için yerel bir yönetim uç nokta çağırarak işlevlerinizi test edebilirsiniz. Yerel sunucuda bir HTTP POST isteği ile Bu uç noktası çağrılmadan işlevi tetikler. İsteğe bağlı olarak, test verileri POST isteğinin gövdesinde yürütme geçirebilirsiniz. Bu işlev benzer **Test** Azure portalında sekmesi.  

HTTP olmayan işlevleri tetiklemek için aşağıdaki yönetici uç noktası arayın:

    http://localhost:{port}/admin/functions/{function_name}

Bir işlev yönetici uç noktasına test veri iletmek için bir POST isteği ileti gövdesinde veri sağlamanız gerekir. İleti gövdesi şu JSON biçimini olması gerekir:

```JSON
{
    "input": "<trigger_input>"
}
````

`<trigger_input>` İşlevi tarafından beklenen biçimde veri değeri içeriyor. POST aşağıdaki cURL örnektir bir `QueueTriggerJS` işlevi. Bu durumda, giriş sırada bulunması beklenen iletisi eşdeğer bir gruba bir dizedir.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Kullanarak `func run` sürüm komutunu 1.x

>[!IMPORTANT]  
> `func run` Komutu sürümünde desteklenmiyor 2.x Araçları. Daha fazla bilgi için Ek Yardım konusuna [Azure işlevleri çalışma zamanı sürümlerini hedefleyen nasıl](set-runtime-version.md).

Bir işlev doğrudan kullanarak da çağırabilirsiniz `func run <FunctionName>` ve işlevi için giriş verileri sağlar. Bu komut işlevini kullanarak çalıştırmaya benzer **Test** Azure portalında sekmesi. 

`func run` Aşağıdaki seçenekleri destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Satır içi içeriği. |
| **`--debug -d`** | Bir hata ayıklayıcısı işlevi çalıştırmadan önce ana bilgisayar işleme iliştirilemiyor.|
| **`--timeout -t`** | Yerel işlevler ana hazır olana kadar (saniye cinsinden) beklenecek süre.|
| **`--file -f`** | İçerik kullanılacak dosya adı.|
| **`--no-interactive`** | Giriş istemez. Otomasyon senaryoları için kullanışlıdır.|

Örneğin, bir HTTP tetiklemeli işlevini çağırın ve İçerik gövdesi geçirmek için aşağıdaki komutu çalıştırın:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Günlük dosyaları yerel olarak görüntüleme

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Azure'a yayımlama

Bir işlev uygulaması Azure işlevleri proje yayımlamak için kullanın `publish` komutu:

```bash
func azure functionapp publish <FunctionAppName>
```

Aşağıdaki seçenekleri kullanabilirsiniz:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Yayımlama ayarları local.settings.json üzerine isteyen Azure içinde ayarı zaten mevcut. Depolama öykünücüsü kullanıyorsanız, uygulama ayarının değiştirme bir [gerçek depolama bağlantısı](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | İle birlikte kullanılmalıdır `-i`. Azure AppSettings farklı olması durumunda ile yerel değerin üzerine yazar. Komut istemi varsayılandır.|

Bu komut var olan bir işlev uygulamaya azure'da yayımlar. Bir hata oluşursa, `<FunctionAppName>` aboneliğinizde yok. Komut istemi veya terminal penceresinden Azure CLI kullanarak bir işlev uygulaması oluşturmayı öğrenmek için bkz: [sunucusuz yürütme için bir işlev uygulaması oluşturma](./scripts/functions-cli-create-serverless.md).

`publish` Komutu işlevleri proje dizininin içeriğini yükler. Dosyaları yerel olarak silerseniz `publish` komutu silinmez Azure'dan. Azure dosyaları kullanarak silebilirsiniz [Kudu aracı](functions-how-to-use-azure-function-app-settings.md#kudu) içinde [Azure portal].  

>[!IMPORTANT]  
> Azure üzerinde bir işlev uygulaması oluşturduğunuzda, bu sürüm kullanır 1.x varsayılan işlevi çalışma zamanı. İşlev uygulama kullanım sürümü yapmak için çalışma zamanı 2.x uygulama ayarı ekleme `FUNCTIONS_EXTENSION_VERSION=beta`.  
Bu ayar işlev uygulamanıza eklemek için aşağıdaki Azure CLI kodunu kullanın:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri çekirdek Araçları [kaynak açın ve GitHub üzerinde barındırılan](https://github.com/azure/azure-functions-cli).  
Bir hata veya özellik isteği dosyasına [GitHub sorunu açmak](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure işlevleri çekirdek araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
