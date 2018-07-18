---
title: İş ile Azure işlevleri çekirdek araçları | Microsoft Docs
description: Kod ve Azure işlevleri üzerinde çalıştırmadan önce komut istemi veya terminal yerel bilgisayarınızda Azure işlevlerini test etme hakkında bilgi edinin.
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
ms.openlocfilehash: 44485d04dad3ff9dfc6067a3737989c5d273541f
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116189"
---
# <a name="work-with-azure-functions-core-tools"></a>İle Azure işlevleri çekirdek Araçları çalışma

Azure işlevleri temel araçları, geliştirme ve yerel bilgisayarınızda bir komut istemi veya terminal işlevlerinizi test sağlar. Yerel işlevlerinizi Canlı Azure hizmetlerine bağlanabilir ve tam işlevler çalışma zamanı'nı kullanarak yerel bilgisayarınızda işlevlerinizi hata ayıklaması yapabilirsiniz. Azure aboneliğinizde bir işlev uygulaması da dağıtabilirsiniz.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## <a name="core-tools-versions"></a>Çekirdek araçları sürümleri

Azure işlevleri çekirdek araçları iki sürümü vardır. Kullandığınız sürümü, yerel geliştirme ortamı, dil seçimi ve gerekli destek düzeyine bağlıdır:

+ [Sürüm 1.x](#v1): sürümünü destekleyen genel kullanıma (GA) çalışma zamanının 1.x. Araçlar'ın bu sürümü yalnızca Windows bilgisayarlarda desteklenir ve gelen yüklü bir [npm paket](https://docs.npmjs.com/getting-started/what-is-npm). Bu sürümle birlikte, resmi olarak desteklenmeyen Deneysel dillerde işlevleri oluşturabilirsiniz. Daha fazla bilgi için [Azure işlevleri'nde desteklenen diller](supported-languages.md)

+ [Sürüm 2.x](#v2): sürümünü destekleyen 2.x çalışma zamanı. Bu sürümü destekler [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux). Platforma özgü paket yöneticileri veya npm yükleme için kullanır. 2.x çalışma zamanı gibi çekirdek Araçları'nın bu sürümü şu anda Önizleme aşamasındadır.

Aksi belirtilmediği sürece, bu makaledeki örnekler için sürümü olan 2.x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

[Azure işlevleri temel araçları] yerel geliştirme bilgisayarınızda çalıştırabilirsiniz Azure işlevleri çalışma zamanı güç veren aynı çalışma zamanının bir sürümünü içerir. Ayrıca, işlev projelerini dağıtma işlevler oluşturun ve Azure'a bağlanmak için komutları sağlar.

### <a name="v1"></a>Sürüm 1.x

İşlevler 1.x çalışma zamanı araçlarını orijinal sürümünü kullanır. Bu sürümü .NET Framework (4.7.1'i) kullanır ve yalnızca Windows bilgisayarlarda desteklenir. Sürüm 1.x Araçları'nı yüklemek için önce [Nodejs'yi yüklemeniz](https://docs.npmjs.com/getting-started/installing-node), npm içerir.

Sürüm 1.x araçlarını yüklemek için aşağıdaki komutu kullanın:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Sürüm 2.x

>[!NOTE]
> Azure işlevleri çalışma zamanı 2.0 Önizleme aşamasındadır ve Azure işlevleri'nin şu anda tüm özellikler desteklenir. Daha fazla bilgi için [Azure işlevleri sürümleri](functions-versions.md) 

Sürüm 2.x Araçları, Azure işlevleri çalışma zamanı kullanan .NET Core üzerine yapılandırılan 2.x. Bu sürüm dahil olmak üzere, .NET Core 2.x desteklenen tüm platformlarda desteklenir [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux).

#### <a name="windows-npm"></a>Windows

Aşağıdaki adımlar, Windows üzerinde temel araçları yüklemek için npm kullanın. Ayrıca [Chocolatey](https://chocolatey.org/). Daha fazla bilgi için [temel araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Yükleme [Windows için .NET Core 2.1](https://www.microsoft.com/net/download/windows).

2. Yükleme [Node.js], npm içerir. İçin sürüm 2.x Araçlar, yalnızca Node.js 8.5 ve sonraki sürümlerde desteklenir.

3. Temel Araçları paketi yükleyin:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>Homebrew ile MacOS

Aşağıdaki adımları macOS üzerinde temel araçları yüklemek için Homebrew kullanın.

1. Yükleme [macOS için .NET Core 2.1](https://www.microsoft.com/net/download/macos).

2. Yükleme [Homebrew](https://brew.sh/), zaten yüklü değilse.

3. Temel Araçları paketi yükleyin:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> APT ile Linux (Debian/Ubuntu)

Aşağıdaki adımları kullanın [APT](https://wiki.debian.org/Apt) Ubuntu/Debian Linux dağıtımınıza bağlı Core araçlarını yüklemek için. Diğer Linux dağıtımları için bkz: [temel araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Yükleme [Linux için .NET Core 2.1](https://www.microsoft.com/net/download/linux).

2. Microsoft ürün anahtarı olarak güvenilir kaydedin:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Aşağıdaki tabloda, Ubuntu server uygun sürümlerinden birini çalıştığını doğrulayın. Apt kaynak eklemek için şunu çalıştırın:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux dağıtım | Sürüm |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Naneli 18    | `xenial`  |

4. Temel Araçları paketi yükleyin:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

İşlevleri proje dizini dosyalarını içeren [host.json](functions-host-json.md) ve [local.settings.json](#local-settings-file), tekil işlevler için kod içeren klasörleri boyunca. Bu dizinde bir Azure işlev uygulamasında eşdeğerdir. İşlevleri klasör yapısı hakkında daha fazla bilgi için bkz: [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md#folder-structure).

Sürüm 2.x başlatıldıktan ve varsayılan dil şablonları eklenen tüm işlevleri projeniz için varsayılan dili seçmenizi gerektirir. Sürümünde 1.x, belirttiğiniz dili her zaman bir işlev oluşturun.

Terminal penceresinde ya da bir komut isteminden proje ve yerel Git deposu oluşturmak için aşağıdaki komutu çalıştırın:

```bash
func init MyFunctionProj
```

Sürüm 2.x komutu çalıştırdığınızda, bir çalışma zamanı projeniz için seçmeniz gerekir. JavaScript işlevleri geliştirmeyi planlıyorsanız, seçin **düğüm**:

```output
Select a worker runtime:
dotnet
node
```

Yukarı/bir dil seçmek için aşağı ok tuşlarını, Enter tuşuna basın. Çıkış için JavaScript proje aşağıdaki örnekteki gibi görünür:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

Yerel bir Git deposu oluşturmadan projeyi oluşturmak için kullanın `--no-source-control [-n]` seçeneği.

## <a name="register-extensions"></a>Uzantılarını kaydetme

Sürüm 2.x Azure işlevleri çalışma zamanı sahip işlev uygulamanızda kullandığınız bağlama uzantıları (bağlama türleri) açıkça kaydedilecek.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Daha fazla bilgi için [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Uygulama ayarları, bağlantı dizeleri ve Azure işlevleri çekirdek araçları için ayarları dosyası local.settings.json depolar. Bunu, aşağıdaki yapıya sahiptir:

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
| **Isencrypted** | Ayarlandığında **true**, tüm değerlerin bir yerel makine anahtarı kullanılarak şifrelenir. İle kullanılan `func settings` komutları. Varsayılan değer **false**. |
| **Değerler** | Uygulama ayarları ve yerel olarak çalıştırırken kullanılan bağlantı dizeleri koleksiyonu. Bu değerler, azure'daki işlev uygulamanızın uygulama ayarlarında gibi karşılık **AzureWebJobsStorage** ve **AzureWebJobsDashboard**. Birçok tetikleyiciler ve bağlamalar gibi bir bağlantı dizesi uygulama ayarına başvuran bir özelliği olan **bağlantı** için [Blob Depolama tetikleyicisi](functions-bindings-storage-blob.md#trigger---configuration). Tür özellikleri için tanımlanan bir uygulama ayarı ihtiyacınız **değerleri** dizisi. <br/>**AzureWebJobsStorage** dışındaki HTTP Tetikleyicileri için gerekli uygulama ayardır. Olduğunda [Azure storage öykünücüsü](../storage/common/storage-use-emulator.md) ayarlayabileceğiniz yerel olarak yüklü **AzureWebJobsStorage** için `UseDevelopmentStorage=true` ve temel araçları öykünücüsü kullanır. Geliştirme sırasında kullanışlıdır ancak gerçek depolama bağlantısı dağıtımdan önce test etmeniz gerekir. |
| **Ana Bilgisayar** | Bu bölümdeki ayarlarını yerel olarak çalıştırılırken işlevleri ana bilgisayar işlemi özelleştirin. |
| **LocalHttpPort** | Yerel işlevler ana çalıştırırken kullanılan varsayılan bağlantı noktasını ayarlar (`func host start` ve `func run`). `--port` Komut satırı seçeneği bu değerin üzerine göre önceliklidir. |
| **CORS** | İzin verilen çıkış noktaları tanımlar [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Kaynakları, boşluk virgülle ayrılmış bir liste olarak sağlanır. Joker karakter değeri (\*) desteklenir, her türlü kaynağa gelen isteklere izin verir. |
| **ConnectionStrings** | Bu koleksiyon, işlev bağlamaları tarafından kullanılan bağlantı dizeleri için kullanmayın. Bu koleksiyon yalnızca bağlantı dizeleri almalısınız çerçeveleri tarafından kullanılan **ConnectionStrings** gibi bir yapılandırma bölümünü dosya [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Bağlantı dizelerini bu nesne, sağlayıcı türü ortamı eklenir [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Bu koleksiyondaki öğelerin diğer uygulama ayarları ile Azure'a yayımlanmaz. Bu değerleri açıkça eklemelidir **bağlantı dizeleri** bölümünü **uygulama ayarları** işlev uygulamanız için. |

İşlev uygulaması ayarları değerleri, ortam değişkenleri olarak kodunuzda da okunabilir. Daha fazla bilgi için bu dile özgü başvuru konularında ortam değişkenleri bölümüne bakın:

+ [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
+ [C# betiği (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Local.settings.json dosyasında ayarları, yalnızca yerel olarak çalıştırılırken işlevleri araçları tarafından kullanılır. Varsayılan olarak, projeyi Azure'da yayımlandığında bu ayarlar otomatik olarak geçirilmez. Kullanım `--publish-local-settings` geçiş [yayımladığınızda](#publish) bu ayarlar, Azure işlev uygulamasında eklenir emin olmak için. Değerler **ConnectionStrings** hiçbir zaman yayımlanır.

İçin geçerli bir depolama bağlantı dizesi ayarlandığında **AzureWebJobsStorage** ve öykünücü kullanılmıyor, aşağıdaki hata iletisi gösterilir:  

>Local.settings.json içinde AzureWebJobsStorage için eksik değer. Bu HTTP dışındaki tüm tetikleyiciler için gereklidir. Çalıştırabileceğiniz ' func azure functionapp getirme-app-settings <functionAppName>' ya da local.settings.json içinde bir bağlantı dizesi belirtin.

### <a name="get-your-storage-connection-strings"></a>Depolama bağlantı dizeleri alma

Depolama öykünücüsü için geliştirme kullanırken, bile, bir gerçek depolama bağlantısı ile test etmek isteyebilirsiniz. Zaten olduğunu varsayarsak [bir depolama hesabı oluşturmuş](../storage/common/storage-create-storage-account.md), geçerli bir depolama bağlantı dizesi aşağıdaki yollardan biriyle alabilirsiniz:

+ Gelen [Azure portal]. Depolama hesabınıza, select gidin **erişim anahtarları** içinde **ayarları**, birini kopyalayın **bağlantı dizesi** değerleri.

  ![Bağlantı dizesini Azure portalından kopyalayın.](./media/functions-run-local/copy-storage-connection-portal.png)

+ Kullanım [Azure Depolama Gezgini](http://storageexplorer.com/) Azure hesabınıza bağlanmak için. İçinde **Gezgini**, aboneliğinizi genişletin, depolama hesabınızı seçin ve birincil veya ikincil bağlantı dizesini kopyalayın. 

  ![Depolama Gezgini'nde bağlantı dizesini kopyalayın](./media/functions-run-local/storage-explorer.png)

+ Bağlantı dizesi aşağıdaki komutlardan birini ile azure'dan indirmek için temel araçları kullanın:

    + Tüm ayarlar, var olan bir işlev uygulamasından indirin:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + Belirli bir depolama hesabı için bağlantı dizesini alın:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    Zaten Azure'da oturum değil, bunu yapmak istenir.

## <a name="create-func"></a>Bir işlev oluşturma

Bir işlev oluşturmak için aşağıdaki komutu çalıştırın:

```bash
func new
```

Sürümünde, çalıştırdığınızda 2.x `func new` işlev uygulamanız varsayılan dilde bir şablon seçmeniz istenir, ardından işleviniz için bir ad seçmeniz istenir. Sürümünde 1.x, dili seçmeniz istenir.

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

Kuyruk tetikleyicisi çıktıda görüldüğü gibi işlev kodunu sağlanan işlev adı ile bir alt klasör oluşturulur:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

Bu seçenekler şu bağımsız değişkenler kullanarak komutu belirtebilirsiniz:

| Bağımsız değişken     | Açıklama                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| Programlama dili, C#, F # veya JavaScript gibi şablon. Bu seçenek sürümünde gerekli 1.x. Sürüm 2.x değil Bu seçeneği kullanın veya projenizin varsayılan dili seçin. |
| **`--template -t`** | Şablon adı değerlerden biri olabilir:<br/><ul><li>`Blob trigger`</li><li>`Cosmos DB trigger`</li><li>`Event Grid trigger`</li><li>`HTTP trigger`</li><li>`Queue trigger`</li><li>`SendGrid`</li><li>`Service Bus Queue trigger`</li><li>`Service Bus Topic trigger`</li><li>`Timer trigger`</li></ul> |
| **`--name -n`** | İşlev adı. |

Örneğin, bir JavaScript HTTP tetikleyici tek bir komutta oluşturmak için çalıştırın:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Kuyruk ile tetiklenen bir işlev tek bir komutta oluşturmak için çalıştırın:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>İşlevleri yerel olarak çalıştırma

İşlevler projesini çalıştırmak için işlevleri konak çalıştırın. Konak, projedeki tüm işlevler için Tetikleyiciler sağlar:

```bash
func host start
```

`func host start` Aşağıdaki seçeneklerini destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | Dinlemenin yapılacağı yerel bağlantı noktası. Varsayılan değer: 7071. |
| **`--debug <type>`** | Başlatır hata ayıklama bağlantı konakla açın, böylece ekleyebileceğiniz **func.exe** gelen işlem [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) veya [Visual Studio 2017](functions-dotnet-class-library.md). *\<Türü\>* Seçenekler `VSCode` ve `VS`.  |
| **`--cors`** | CORS çıkış noktaları, boşluk virgülle ayrılmış listesi. |
| **`--nodeDebugPort -n`** | Kullanmak düğüm hata ayıklayıcısı bağlantı noktası. Varsayılan: Launch.json veya 5858 arasında bir değer. |
| **`--debugLevel -d`** | Konsolunun izleme düzeyini (kapalı, ayrıntı, Info, WARNING veya error). Varsayılan: bilgi.|
| **`--timeout -t`** | Saniyeler içinde başlatılacak işlevleri konak için zaman aşımı. Varsayılan: 20 saniye.|
| **`--useHttps`** | Bağlama `https://localhost:{port}` yerine çok `http://localhost:{port}`. Varsayılan olarak, bu seçenek bilgisayarınızda güvenilen bir sertifika oluşturur.|
| **`--pause-on-error`** | Duraklatma işlemi çıkmadan önce ek giriş. Visual Studio veya VS Code Core Araçları'nı başlatma sırasında kullanılır.|

İşlevleri ana bilgisayar başladığında, URL HTTP tetiklemeli işlevleri çıkarır:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="passing-test-data-to-a-function"></a>Bir işlev test veri geçirme

İşlevlerinizi yerel olarak test etmek için [işlevleri konağını Başlat](#start) ve HTTP isteklerini kullanarak yerel sunucuda uç noktalarını çağırın. Uç noktası çağrısı işlevi türüne bağlıdır.

>[!NOTE]  
> Bu konudaki örnekler, terminal veya komut istemi HTTP istekleri göndermek için cURL aracını kullanın. Yerel sunucuya HTTP istekleri göndermek için seçtiğiniz bir aracı kullanabilirsiniz. CURL aracını, Linux tabanlı sistemler üzerinde varsayılan olarak kullanılabilir. Windows üzerinde indirmeniz ve yüklemeniz [cURL aracını](https://curl.haxx.se/).

İşlevlerini test etme ile ilgili daha fazla genel bilgi için bkz: [kodunuzu Azure işlevleri'nde test stratejileri](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>İşlevleri HTTP ve Web kancası tetiklendi

Aşağıdaki çağrı uç noktası, HTTP ve Web kancası yerel olarak çalıştırmak için tetiklenen İşlevler:

    http://localhost:{port}/api/{function_name}

Aynı sunucu adını ve işlevleri konak dinlediği bağlantı noktasını kullandığınızdan emin olun. Bu işlev konak başlatma sırasında oluşturulan çıktıyı görürsünüz. Tetik tarafından desteklenen herhangi bir HTTP yöntemini kullanarak bu URL'yi çağırabilirsiniz. 

Aşağıdaki cURL komutu Tetikleyicileri `MyHttpTrigger` bir GET isteği ile hızlı başlangıç işlevden _adı_ geçirilen sorgu dizesi parametresi. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
Aşağıdaki örnek, bir POST isteğinde geçirme adlı aynı işlevidir _adı_ istek gövdesindeki:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

Sorgu dizesinde geçirilen bir tarayıcıdan GET istekleri yapabilir. Diğer tüm HTTP yöntemleri için cURL, Fiddler, Postman veya benzer bir HTTP test aracı kullanmanız gerekir.  

#### <a name="non-http-triggered-functions"></a>HTTP olmayan tetiklenen İşlevler

İşlevleri HTTP Tetikleyicileri ve Web kancaları dışındaki tüm türleri için yerel bir yönetim uç noktasını çağırarak işlevlerinizi test edebilirsiniz. Bu uç nokta bir HTTP POST isteği ile yerel sunucuda çağırma işlevi tetikler. İsteğe bağlı olarak bir POST isteğinin gövdesi yürütme için test verilerini geçirebilirsiniz. Bu işlevsellik aşağıdakine benzer **Test** Azure portalında sekmesi.  

HTTP olmayan işlevler tetiklemek için aşağıdaki yönetici uç noktası çağırırsınız:

    http://localhost:{port}/admin/functions/{function_name}

Bir işlev yönetici uç noktası için test verilerini geçirmek için bir POST isteğinin ileti gövdesinde veri sağlamanız gerekir. İleti gövdesinde şu JSON biçimini olması gerekiyor:

```JSON
{
    "input": "<trigger_input>"
}
````

`<trigger_input>` Değer işlevi tarafından beklenen biçimde veriler içeriyor. Bir GÖNDERİ için aşağıdaki cURL örnek, bir `QueueTriggerJS` işlevi. Bu durumda, giriş iletinin kuyrukta bulunması beklenen değerine eşdeğer olan bir dizedir.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Kullanarak `func run` sürüm komutunu 1.x

>[!IMPORTANT]  
> `func run` Komut sürümünde desteklenmeyen 2.x araçlar. Daha fazla bilgi için Ek Yardım konusuna [Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl](set-runtime-version.md).

Doğrudan kullanarak bir işlev de çağırabilirsiniz `func run <FunctionName>` ve işlevi için giriş verilerini sağlayın. Bu komut, bir işlevi kullanarak çalışan benzer **Test** Azure portalında sekmesi. 

`func run` Aşağıdaki seçeneklerini destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Satır içi içeriği. |
| **`--debug -d`** | Bir hata ayıklayıcı işlevi çalıştırmadan önce konak işlemine iliştirin.|
| **`--timeout -t`** | (Saniye cinsinden) yerel işlevler konak hazır olana kadar beklenecek süre.|
| **`--file -f`** | İçerik kullanılacak dosya adı.|
| **`--no-interactive`** | Giriş istemez. Otomasyon senaryoları için kullanışlıdır.|

Örneğin, bir HTTP ile tetiklenen işlevi çağırabilir ve İçerik gövdesi geçirmek için aşağıdaki komutu çalıştırın:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Günlük dosyaları yerel olarak görüntüleme

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Azure'da yayımlama

Bir işlev uygulaması ile Azure işlevleri projenizi yayımlamak için kullanın `publish` komutu:

```bash
func azure functionapp publish <FunctionAppName>
```

Aşağıdaki seçenekleri kullanabilirsiniz:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Ayarları varsa üzerine yaz isteyen azure'a local.settings.json yayımlamak ayar zaten mevcut. Depolama öykünücüsü kullanıyorsanız, uygulama ayarının değiştirme bir [gerçek depolama bağlantısı](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | İle birlikte kullanılmalıdır `-i`. Azure'da AppSettings yerel değeriyle farklı olması durumunda üzerine yazılır. Komut istemi varsayılandır.|

Bu komut var olan işlev uygulamanızı Azure'a yayımlar. Bir hata oluşursa, `<FunctionAppName>` aboneliğinizde mevcut değil. Komut istemi veya terminal penceresinde Azure CLI kullanarak bir işlev uygulaması oluşturmak nasıl öğrenmek için bkz. [sunucusuz yürütme için bir işlev uygulaması oluşturma](./scripts/functions-cli-create-serverless.md).

`publish` Komut işlevleri proje dizininin içeriğini yükler. Dosyaları yerel olarak silerseniz `publish` komut silinmez Azure'dan. Kullanarak Azure dosyaları silebilirsiniz [Kudu aracı](functions-how-to-use-azure-function-app-settings.md#kudu) içinde [Azure portal].  

>[!IMPORTANT]  
> Azure'da bir işlev uygulaması oluşturduğunuzda, sürümünü kullanır. varsayılan olarak işlev çalışma zamanının 1.x. İşlev uygulaması kullanım sürümü yapmak 2.x çalışma zamanı, uygulama ayarı ekleme `FUNCTIONS_EXTENSION_VERSION=beta`.  
Bu ayar işlev uygulamanıza eklemek için aşağıdaki Azure CLI kod kullanın:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri çekirdek Araçları [açık kaynak ve GitHub üzerinde barındırılır](https://github.com/azure/azure-functions-cli).  
Bir hata veya özellik isteği için [açık bir GitHub sorunu](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure işlevleri temel araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
