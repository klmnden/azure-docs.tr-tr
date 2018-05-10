---
title: Geliştirme ve Azure işlevleri yerel olarak çalıştırma | Microsoft Docs
description: Kod ve Azure işlevlerini çalıştırmadan önce yerel bilgisayarınızda Azure işlevlerini test öğrenin.
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
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 523ef25fe0d3227d526acbdee2c7cf2660fc4f25
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="code-and-test-azure-functions-locally"></a>Yerel kod ve test Azure işlevleri

Sırada [Azure portal] geliştirmek için Araçlar tam kümesi ve test Azure işlevleri, geliştiricilerin çoğu tercih yerel geliştirme deneyimi sağlar. Azure işlevleri geliştirmek ve yerel bilgisayarınızda işlevlerinizi test etmek için sık kullanılan kod düzenleyicisini ve yerel geliştirme araçları kullanın kolaylaştırır. İşlevlerinizi Azure olayları tetikleyebilir ve yerel bilgisayarınızda, C# ve JavaScript işlevleri ayıklayabilirsiniz. 

Visual Studio C# Geliştirici, Azure işlevleri de olup olmadığını [Visual Studio 2017 ile tümleşir](functions-develop-vs.md).

>[!IMPORTANT]  
> Aynı işlev uygulaması portal geliştirme ile yerel geliştirme karışık kullanmayın. Oluşturduğunuzda ve yerel bir proje işlevlerden yayımlama korumak veya portalında proje kodunu değiştirmek denemek.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

[Azure işlevleri çekirdek Araçları] yerel geliştirme bilgisayarınızda çalıştırabilirsiniz Azure işlevleri çalışma zamanı, yerel bir sürümüdür. Bir öykünücü veya benzetici değil. Powers Azure işlevleri çalışma zamanı olur. Azure işlevleri çekirdek araçları iki sürümü vardır:

+ [Sürüm 1.x](#v1): sürüm destekler çalışma zamanının 1.x. Bu sürüm yalnızca Windows bilgisayarlarda desteklenir ve gelen yüklü bir [npm paket](https://docs.npmjs.com/getting-started/what-is-npm).
+ [Sürüm 2.x](#v2): sürüm destekler çalışma zamanının 2.x. Bu sürüm destekler [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux). Platforma özgü paket yöneticileri veya npm yükleme için kullanır. 

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

1. Yükleme [Homebrew](https://brew.sh/), henüz yüklü değilse.

2. Çekirdek araçları paketini yükleyin:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) APT ile

Aşağıdaki adımları kullanın [APT](https://wiki.debian.org/Apt) Ubuntu/Debian Linux dağıtım noktasında çekirdek araçlarını yüklemek için. Diğer Linux dağıtımları için bkz: [çekirdek araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Yükleme [Linux için .NET Core 2.0](https://www.microsoft.com/net/download/linux).

1. Microsoft ürün anahtarı güvenilir olarak kaydedin:

  ```bash
  curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
  sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
  ```

2.  Akış, paketini ayarlayın değiştirme `<version>` tablosundan uygun sürüm adı ile aşağıdaki komutta:

  ```bash
  sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-<version>-prod <version> main" > /etc/apt/sources.list.d/dotnetdev.list'
  sudo apt-get update
  ```

  | Linux dağıtım | `<version>` |
  | --------------- | ----------- |
  | Ubuntu 17.10    | `artful`    |
  | Ubuntu 17.04    | `zesty`     |
  | Ubuntu 16.04/Linux Naneli 18    | `xenial`  |

3. Çekirdek araçları paketini yükleyin:

  ```bash
  sudo apt-get install azure-functions-core-tools
  ```

## <a name="run-azure-functions-core-tools"></a>Azure işlevleri çekirdek araçlarını çalıştırma
 
Azure işlevleri çekirdek araçları aşağıdaki komut diğer adları ekler:
* **FUNC**
* **azfun**
* **azurefunctions**

Bu diğer adları hiçbirini kullanılabilir nerede `func` örneklerde gösterilmiştir.

```
func init MyFunctionProj
```

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

Yerel olarak çalıştırırken, işlevleri proje dosyaları içeren bir dizindir [host.json](functions-host-json.md) ve [local.settings.json](#local-settings-file). Bu dizinde bir işlev uygulaması Azure eşdeğeridir. Azure işlevleri klasör yapısı hakkında daha fazla bilgi için bkz: [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md#folder-structure).

Terminal penceresi veya bir komut isteminden, proje ve yerel Git deposu oluşturmak için aşağıdaki komutu çalıştırın:

```
func init MyFunctionProj
```

Çıktı aşağıdaki gibi görünür:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

Yerel bir Git deposu olmadan projesi oluşturmak için kullanın `--no-source-control [-n]` seçeneği.

## <a name="register-extensions"></a>Uzantılarını kaydetme

Sürümünde 2.x Azure işlevleri çalışma zamanı açıkça kaydetmeniz gerekir [uzantıları bağlama](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/README.md) işlevi uygulamanızda kullanan. 

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Daha fazla bilgi için bkz: [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Dosya local.settings.json uygulama ayarları, bağlantı dizeleri ve Azure işlevleri çekirdek araçları ayarlarını saklar. Aşağıdaki yapı vardır:

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>" 
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
| **Değerler** | Uygulama ayarlarını yerel olarak çalıştırırken kullanılan koleksiyonu. **AzureWebJobsStorage** ve **AzureWebJobsDashboard** örnekler; tam bir listesi için bkz: [uygulama ayarları başvurusu](functions-app-settings.md). Birçok Tetikleyicileri ve bağlamaları gibi bir uygulama ayarı için başvuran özelliğine sahip **bağlantı** Blob Depolama tetikleyici için. Tür özellikleri için tanımlanan bir uygulama ayarı gereksinim **değerleri** dizi. Bu değer yüzde işaretleri, örneğin kaydırma tarafından bir uygulama ayarı adı için ayarladığınız herhangi bir bağlama özelliği için de geçerlidir `%AppSettingName%`. |
| **Ana Bilgisayar** | Bu bölümdeki ayarlarını işlevleri ana bilgisayar işlemi yerel olarak çalıştırırken özelleştirin. | 
| **LocalHttpPort** | Yerel işlevler ana çalıştırırken kullanılan varsayılan bağlantı noktasını ayarlar (`func host start` ve `func run`). `--port` Komut satırı seçeneği bu değerin üzerine göre önceliklidir. |
| **CORS** | İzin verilen çıkış noktası tanımlar [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Çıkış boşluk virgülle ayrılmış bir liste olarak sağlanır. Joker karakter değeri (\*) desteklenir, her türlü kaynağa gelen isteklere izin verir. |
| **ConnectionStrings** | İşlevlerinizi için veritabanı bağlantı dizelerini içerir. Bu nesne bağlantı dizeleri, sağlayıcı türü ortamıyla eklenir **System.Data.SqlClient**.  | 

Bu ayarlar, ortam değişkenleri olarak kodunuzda de okunabilir. Daha fazla bilgi için bu dile özgü başvuru konuları ortam değişkenleri bölümüne bakın:

+ [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
+ [C# betik (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Local.settings.json dosyasındaki ayarları, yalnızca yerel olarak çalıştırırken işlevleri araçları tarafından kullanılır. Projeyi Azure'a yayımlandığında varsayılan olarak, bu ayarlar otomatik olarak geçirilmez. Kullanım `--publish-local-settings` geçiş [yayımladığınızda](#publish) bu ayarlar, azure'da işlev uygulaması eklenir emin olmak için.

İçin hiç geçerli bir depolama bağlantı dizesi ayarlandığında **AzureWebJobsStorage**, aşağıdaki hata iletisi gösterilir:  

>Local.settings.json eksik değeri AzureWebJobsStorage için. Bu, HTTP dışındaki tüm tetikleyiciler için gereklidir. Çalıştırabilirsiniz ' func azure functionapp fetch-app-settings <functionAppName>' veya local.settings.json içinde bir bağlantı dizesini belirtin.
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma

Bağlantı dizeleri için bir değer ayarlamak için aşağıdaki seçeneklerden birini yapabilirsiniz:
* Bağlantı dizesi girin [Azure Storage Gezgini](http://storageexplorer.com/).
* Aşağıdaki komutlardan birini kullanın:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    Her iki komutları azure'a ilk oturum açma için gerektirir.

<a name="create-func"></a>
## <a name="create-a-function"></a>İşlev oluşturma

Bir işlev oluşturmak için aşağıdaki komutu çalıştırın:

```
func new
``` 
`func new` Aşağıdaki isteğe bağlı bağımsız değişkenler destekler:

| Bağımsız değişken     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | Dil C#, F # ve JavaScript gibi programlama şablonu. |
| **`--template -t`** | Şablon adı. |
| **`--name -n`** | İşlev adı. |

Örneğin, bir JavaScript HTTP tetikleyicisi oluşturmak için çalıştırın:

```
func new --language JavaScript --template "Http Trigger" --name MyHttpTrigger
```

Sıra tetiklenen bir işlev oluşturmak için çalıştırın:

```
func new --language JavaScript --template "Queue Trigger" --name QueueTriggerJS
```
<a name="start"></a>
## <a name="run-functions-locally"></a>İşlevler yerel olarak çalıştırma

İşlevler proje çalıştırmak için işlevleri konak çalıştırın. Ana bilgisayar için projedeki tüm işlevleri Tetikleyicileri sağlar:

```
func host start
```

`func host start` Aşağıdaki seçenekleri destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | Dinlemenin yapılacağı yerel bağlantı noktası. Varsayılan değer: 7071. |
| **`--debug <type>`** | Seçenekler şunlardır: `VSCode` ve `VS`. |
| **`--cors`** | Boşluk CORS çıkış noktası virgülle ayrılmış listesi. |
| **`--nodeDebugPort -n`** | Kullanmak düğüm hata ayıklayıcı için bağlantı noktası. Varsayılan: Launch.json'u veya 5858 arasında bir değer. |
| **`--debugLevel -d`** | Konsol izleme düzeyini (kapalı, ayrıntılı, bilgi, uyarı veya hata). Varsayılan: bilgisi.|
| **`--timeout -t`** | Saniye cinsinden başlatmaya işlevleri ana bilgisayar için zaman aşımı. Varsayılan: 20 saniye.|
| **`--useHttps`** | Bağlamak https://localhost:{port} yerine çok http://localhost:{port}. Varsayılan olarak, bu seçenek bilgisayarınızda güvenilen bir sertifika oluşturur.|
| **`--pause-on-error`** | İşlem çıkmadan önce ek giriş duraklatılıyor. Azure işlevleri çekirdek araçları bir tümleşik geliştirme ortamı (IDE) başlatılırken yararlıdır.|

İşlevler ana bilgisayar başladığında, URL, HTTP tetiklemeli işlevleri çıkarır:

```
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>VS Code'u veya Visual Studio'da hata ayıklama

Bir hata ayıklayıcısı eklemeniz için geçmesi `--debug` bağımsız değişkeni. JavaScript işlevleri hata ayıklamak için Visual Studio Code kullanın. C# işlevleri için Visual Studio'yu kullanın.

C# işlevleri hata ayıklamak için kullanmak `--debug vs`. Aynı zamanda [Azure işlevleri Visual Studio 2017 Araçları](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

Ana bilgisayar'ı başlatın ve JavaScript hata ayıklama kurulumu için çalıştırın:

```
func host start --debug vscode
```

> [!IMPORTANT]
> Hata ayıklama, yalnızca Node.js 8.x desteklenir. Node.js 9.x desteklenmiyor. 

Ardından, Visual Studio Code içinde **hata ayıklama** görünümü, select **eklemek için Azure işlevleri**. Kesme noktaları ekleme, değişkenleri inceleyin ve kod üzerinden adım.

![Visual Studio Code ile JavaScript hata ayıklama](./media/functions-run-local/vscode-javascript-debugging.png)

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

```
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
Aşağıdaki örnek, bir POST isteğini geçirme adlı aynı işlevidir _adı_ istek gövdesindeki:

```
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

```
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

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Günlük dosyaları yerel olarak görüntüleme

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Azure'a yayımlama

Bir işlev uygulaması Azure işlevleri proje yayımlamak için kullanın `publish` komutu:

```
func azure functionapp publish <FunctionAppName>
```

Aşağıdaki seçenekleri kullanabilirsiniz:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Yayımlama ayarları local.settings.json üzerine isteyen Azure içinde ayarı zaten mevcut.|
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

[Azure işlevleri çekirdek Araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
