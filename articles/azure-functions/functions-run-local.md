---
title: İş ile Azure işlevleri çekirdek araçları | Microsoft Docs
description: Kod ve Azure işlevleri üzerinde çalıştırmadan önce komut istemi veya terminal yerel bilgisayarınızda Azure işlevlerini test etme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: glenga
ms.custom: 80e4ff38-5174-43
ms.openlocfilehash: 6c0732b33608105009eda9bba2e4970e8e12e652
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050570"
---
# <a name="work-with-azure-functions-core-tools"></a>İle Azure işlevleri çekirdek Araçları çalışma

Azure işlevleri temel araçları, geliştirme ve yerel bilgisayarınızda bir komut istemi veya terminal işlevlerinizi test sağlar. Yerel işlevlerinizi Canlı Azure hizmetlerine bağlanabilir ve tam işlevler çalışma zamanı'nı kullanarak yerel bilgisayarınızda işlevlerinizi hata ayıklaması yapabilirsiniz. Azure aboneliğinizde bir işlev uygulaması da dağıtabilirsiniz.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Yerel bilgisayarınızda işlevleri geliştirme ve temel araçları ile Azure'a yayımlama temel adımları izler:

> [!div class="checklist"]
> * [Temel araçları ve bağımlılıkları yükleyin.](#v2)
> * [Dile özgü şablondan bir işlev uygulaması projesi oluşturun.](#create-a-local-functions-project)
> * [Tetikleyici ve bağlama uzantıları kaydedin.](#register-extensions)
> * [Depolama ve diğer bağlantıların tanımlayın.](#local-settings-file)
> * [Bir işlev, bir tetikleyici ve dile özgü şablon oluşturun.](#create-func)
> * [İşlevi yerel olarak çalıştırma](#start)
> * [Projeyi Azure'da yayımlama](#publish)

## <a name="core-tools-versions"></a>Çekirdek araçları sürümleri

Azure işlevleri çekirdek araçları iki sürümü vardır. Kullandığınız sürümü, yerel geliştirme ortamınıza bağlıdır [dilinin seçim](supported-languages.md)ve gerekli destek düzeyi:

+ Sürüm 1.x: sürüm destekler çalışma zamanının 1.x. Araçlar'ın bu sürümü yalnızca Windows bilgisayarlarda desteklenir ve gelen yüklü bir [npm paket](https://docs.npmjs.com/getting-started/what-is-npm). Bu sürümle birlikte, resmi olarak desteklenmeyen Deneysel dillerde işlevleri oluşturabilirsiniz. Daha fazla bilgi için [Azure işlevleri'nde desteklenen diller](supported-languages.md)

+ [Sürüm 2.x](#v2): destekler [sürüm 2.x çalışma zamanı](functions-versions.md). Bu sürümü destekler [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux). Platforma özgü paket yöneticileri veya npm yükleme için kullanır.

Aksi belirtilmediği sürece, bu makaledeki örnekler için sürümü olan 2.x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

[Azure işlevleri temel araçları] yerel geliştirme bilgisayarınızda çalıştırabilirsiniz Azure işlevleri çalışma zamanı güç veren aynı çalışma zamanının bir sürümünü içerir. Ayrıca, işlev projelerini dağıtma işlevler oluşturun ve Azure'a bağlanmak için komutları sağlar.

### <a name="v2"></a>Sürüm 2.x

Sürüm 2.x Araçları, Azure işlevleri çalışma zamanı kullanan .NET Core üzerine yapılandırılan 2.x. Bu sürüm dahil olmak üzere, .NET Core 2.x desteklenen tüm platformlarda desteklenir [Windows](#windows-npm), [macOS](#brew), ve [Linux](#linux). 

> [!IMPORTANT]
> .NET Core yükleme gerekliliğini atlayabilir 2.x SDK'sını kullanarak [uzantı paketleri].

#### <a name="windows-npm"></a>Windows

Aşağıdaki adımlar, Windows üzerinde temel araçları yüklemek için npm kullanın. Ayrıca [Chocolatey](https://chocolatey.org/). Daha fazla bilgi için [temel araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Yükleme [Node.js], npm içerir. İçin sürüm 2.x Araçlar, yalnızca Node.js 8.5 ve sonraki sürümlerde desteklenir.

1. Temel Araçları paketi yükleyin:

    ```bash
    npm install -g azure-functions-core-tools
    ```
1. Kullanmayı planlamıyorsanız [uzantı paketleri], yükleme [Windows için .NET Core 2.x SDK](https://www.microsoft.com/net/download/windows).

#### <a name="brew"></a>Homebrew ile MacOS

Aşağıdaki adımları macOS üzerinde temel araçları yüklemek için Homebrew kullanın.

1. Yükleme [Homebrew](https://brew.sh/), zaten yüklü değilse.

1. Temel Araçları paketi yükleyin:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools
    ```
1. Kullanmayı planlamıyorsanız [uzantı paketleri], yükleme [.NET Core 2.x SDK macOS için](https://www.microsoft.com/net/download/macos).


#### <a name="linux"></a> APT ile Linux (Debian/Ubuntu)

Aşağıdaki adımları kullanın [APT](https://wiki.debian.org/Apt) Ubuntu/Debian Linux dağıtımınıza bağlı Core araçlarını yüklemek için. Diğer Linux dağıtımları için bkz: [temel araçları Benioku](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Microsoft ürün anahtarı olarak güvenilir kaydedin:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

1. Aşağıdaki tabloda, Ubuntu server uygun sürümlerinden birini çalıştığını doğrulayın. Apt kaynak eklemek için şunu çalıştırın:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux dağıtım | Version |
    | --------------- | ----------- |
    | Ubuntu 18.10    | `cosmic`    |
    | Ubuntu 18.04    | `bionic`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Mint 18    | `xenial`  |

1. Temel Araçları paketi yükleyin:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```
1. Kullanmayı planlamıyorsanız [uzantı paketleri], yükleme [.NET Core 2.x SDK'sı Linux](https://www.microsoft.com/net/download/linux).

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

İşlevleri proje dizini dosyalarını içeren [host.json](functions-host-json.md) ve [local.settings.json](#local-settings-file), tekil işlevler için kod içeren klasörleri yanı sıra. Bu dizinde bir Azure işlev uygulamasında eşdeğerdir. İşlevleri klasör yapısı hakkında daha fazla bilgi için bkz: [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md#folder-structure).

Sürüm 2.x başlatıldıktan ve varsayılan dil şablonları eklenen tüm işlevleri projeniz için varsayılan dili seçmenizi gerektirir. Sürümünde 1.x, belirttiğiniz dili her zaman bir işlev oluşturun.

Terminal penceresinde ya da bir komut isteminden proje ve yerel Git deposu oluşturmak için aşağıdaki komutu çalıştırın:

```bash
func init MyFunctionProj
```

Bir proje adı sağladığınızda, bu ada sahip yeni bir klasör oluşturulur ve başlatılır. Aksi takdirde, geçerli klasörde başlatılır.  
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

`func init` Aksi belirtilmediği sürece salt 2.x sürümü olan aşağıdaki seçeneklerini destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--csx`** | Bir C# betiği (.csx) projesi başlatır. Belirtmelisiniz `--csx` sonraki komutlarda. |
| **`--docker`** | Seçilen dayalı temel bir görüntü kullanarak bir kapsayıcı için bir Dockerfile oluşturma `--worker-runtime`. Özel bir Linux kapsayıcısında yayımlamak planlama yaparken bu seçeneği kullanın. |
| **`--force`** | Olduğunda bile var olan dosyalar projeye proje başlatın. Bu ayar, aynı ada sahip mevcut dosyaların üzerine yazar. Proje klasöründeki diğer dosyalar etkilenmez. |
| **`--no-source-control -n`** | Sürüm Git deposunda varsayılan oluşturulmasını engeller 1.x. Sürüm 2.x git deposu değil varsayılan olarak oluşturuldu. |
| **`--source-control`** | Bir git deposu oluşturulup oluşturulmadığını denetler. Varsayılan olarak, bir havuz oluşturulmadı. Zaman `true`, bir havuz oluşturulur. |
| **`--worker-runtime`** | Proje için dil çalışma zamanı ayarlar. Desteklenen değerler şunlardır: `dotnet`, `node` (JavaScript) `java`, ve `python`. Ne zaman ayarlanmadı, başlatma sırasında çalışma zamanı seçmeniz istenir. |

> [!IMPORTANT]
> Varsayılan olarak, sürüm 2.x Core Araçları'nın işlevi .NET çalışma zamanı için uygulama projeleri oluşturur [C# sınıf projeleri](functions-dotnet-class-library.md) (.csproj). Visual Studio veya Visual Studio Code ile kullanılan bu C# projeleri, test sırasında ve Azure'a yayımlarken derlenir. Bunun yerine oluşturup olan aynı C# betiği (.csx) çalışmak istiyorsanız sürümünde oluşturulan dosyaları 1.x ve Portalı'nda eklemeniz gerekir `--csx` oluşturup işlevleri dağıttığınızda parametresi.

## <a name="register-extensions"></a>Uzantılarını kaydetme

Sürüm 2.x Azure işlevleri çalışma zamanı sahip işlev uygulamanızda kullandığınız bağlama uzantıları (bağlama türleri) açıkça kaydedilecek.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Daha fazla bilgi için [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](./functions-bindings-expressions-patterns.md).

## <a name="local-settings-file"></a>Yerel ayarlar dosyası

Uygulama ayarları, bağlantı dizeleri ve Azure işlevleri çekirdek araçları için ayarları dosyası local.settings.json depolar. Local.settings.json dosyasında ayarları, yalnızca yerel olarak çalıştırılırken işlevleri araçları tarafından kullanılır. Varsayılan olarak, projeyi Azure'da yayımlandığında bu ayarlar otomatik olarak geçirilmez. Kullanım `--publish-local-settings` geçiş [yayımladığınızda](#publish) bu ayarlar, Azure işlev uygulamasında eklenir emin olmak için. Değerler **ConnectionStrings** hiçbir zaman yayımlanır. Dosya aşağıdaki yapıya sahiptir:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

| Ayar      | Açıklama                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Ayarlandığında `true`, tüm değerlerin bir yerel makine anahtarı kullanılarak şifrelenir. İle kullanılan `func settings` komutları. Varsayılan değer `false`. |
| **`Values`** | Uygulama ayarları ve yerel olarak çalıştırırken kullanılan bağlantı dizeleri koleksiyonu. Bu değerler, azure'daki işlev uygulamanızın uygulama ayarlarında gibi karşılık [ `AzureWebJobsStorage` ]. Birçok tetikleyiciler ve bağlamalar gibi bir bağlantı dizesi uygulama ayarına başvuran bir özelliği olan `Connection` için [Blob Depolama tetikleyicisi](functions-bindings-storage-blob.md#trigger---configuration). Tür özellikleri için tanımlanan bir uygulama ayarı ihtiyacınız `Values` dizisi. <br/>[`AzureWebJobsStorage`] gerekli bir uygulama dışındaki HTTP tetikleyici ayarlanıyor. <br/>Sürüm 2.x çalışma zamanı işlevleri gerektirir [ `FUNCTIONS_WORKER_RUNTIME` ] ayarını projeniz için temel araçları tarafından oluşturulur. <br/> Olduğunda [Azure storage öykünücüsü](../storage/common/storage-use-emulator.md) ayarlayabileceğiniz yerel olarak yüklü [ `AzureWebJobsStorage` ] için `UseDevelopmentStorage=true` ve temel araçları öykünücüsü kullanır. Geliştirme sırasında kullanışlıdır ancak gerçek depolama bağlantısı dağıtımdan önce test etmeniz gerekir. |
| **`Host`** | Bu bölümdeki ayarlarını yerel olarak çalıştırılırken işlevleri ana bilgisayar işlemi özelleştirin. |
| **`LocalHttpPort`** | Yerel işlevler ana çalıştırırken kullanılan varsayılan bağlantı noktasını ayarlar (`func host start` ve `func run`). `--port` Komut satırı seçeneği bu değerin üzerine göre önceliklidir. |
| **`CORS`** | İzin verilen çıkış noktaları tanımlar [çıkış noktaları arası kaynak paylaşımı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Kaynakları, boşluk virgülle ayrılmış bir liste olarak sağlanır. Joker karakter değeri (\*) desteklenir, her türlü kaynağa gelen isteklere izin verir. |
| **`CORSCredentials`** |  İzin vermek için true olarak ayarlanmış `withCredentials` istekleri |
| **`ConnectionStrings`** | Bu koleksiyon, işlev bağlamaları tarafından kullanılan bağlantı dizeleri için kullanmayın. Bu koleksiyon yalnızca genellikle bağlantı dizeleri alma çerçeveleri tarafından kullanılan `ConnectionStrings` gibi bir yapılandırma bölümünü dosya [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Bağlantı dizelerini bu nesne, sağlayıcı türü ortamı eklenir [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Bu koleksiyondaki öğelerin diğer uygulama ayarları ile Azure'a yayımlanmaz. Bu değerleri açıkça eklemelidir `Connection strings` , işlev uygulaması ayarları koleksiyonu. Oluşturuyorsanız bir [ `SqlConnection` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) işlev kodunuzu bağlantı dizesi değerindeki saklamalısınız **uygulama ayarları** , diğer bağlantılarla portalında. |

İşlev uygulaması ayarları değerleri, ortam değişkenleri olarak kodunuzda da okunabilir. Daha fazla bilgi için bu dile özgü başvuru konularında ortam değişkenleri bölümüne bakın:

* [C# önceden derlenmiş](functions-dotnet-class-library.md#environment-variables)
* [C# betiği (.csx)](functions-reference-csharp.md#environment-variables)
* [F#betik (.fsx)](functions-reference-fsharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

İçin geçerli bir depolama bağlantı dizesi ayarlandığında [ `AzureWebJobsStorage` ] ve öykünücü kullanılmıyor, aşağıdaki hata iletisi gösterilir:

> Local.settings.json içinde AzureWebJobsStorage için eksik değer. Bu HTTP dışındaki tüm tetikleyiciler için gereklidir. Çalıştırabileceğiniz ' func azure functionapp getirme-app-settings \<functionAppName\>' ya da local.settings.json içinde bir bağlantı dizesi belirtin.

### <a name="get-your-storage-connection-strings"></a>Depolama bağlantı dizeleri alma

Depolama öykünücüsü için geliştirme kullanırken, bile, bir gerçek depolama bağlantısı ile test etmek isteyebilirsiniz. Zaten olduğunu varsayarsak [bir depolama hesabı oluşturmuş](../storage/common/storage-create-storage-account.md), geçerli bir depolama bağlantı dizesi aşağıdaki yollardan biriyle alabilirsiniz:

+ Gelen [Azure portal]. Depolama hesabınıza, select gidin **erişim anahtarları** içinde **ayarları**, birini kopyalayın **bağlantı dizesi** değerleri.

  ![Bağlantı dizesini Azure portalından kopyalayın.](./media/functions-run-local/copy-storage-connection-portal.png)

+ Kullanım [Azure Depolama Gezgini](https://storageexplorer.com/) Azure hesabınıza bağlanmak için. İçinde **Gezgini**, aboneliğinizi genişletin, depolama hesabınızı seçin ve birincil veya ikincil bağlantı dizesini kopyalayın.

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

| Bağımsız Değişken     | Açıklama                            |
| ------------------------------------------ | -------------------------------------- |
| **`--csx`** | (Sürüm 2.x) Sürümünde kullanılan aynı C# betiği (.csx) şablonları oluşturur 1.x ve Portalı'nda. |
| **`--language -l`**| Gibi programlama dili, şablon C#, F#, veya JavaScript. Bu seçenek sürümünde gerekli 1.x. Sürüm 2.x değil Bu seçeneği kullanın veya alt çalışma zamanı ile eşleşen bir dil seçin. |
| **`--name -n`** | İşlev adı. |
| **`--template -t`** | Kullanım `func templates list` desteklenen her dil için kullanılabilir şablonların tam listesi görmek için komutu.   |

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

`host` Komut sürümünde yalnızca gerekli 1.x.

`func host start` Aşağıdaki seçeneklerini destekler:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--no-build`** | Hiçbir derleme geçerli proje çalıştırmadan önce yapın. Yalnızca dotnet projeler için. Varsayılan false olarak ayarlanır. Sürüm 2.x yalnızca. |
| **`--cert`** | Özel anahtarı içeren bir .pfx dosyası yolu. Yalnızca kullanılan `--useHttps`. Sürüm 2.x yalnızca. |
| **`--cors-credentials`** | Çıkış noktaları arası kimliği doğrulanmış istekler (yani, tanımlama bilgileri ve kimlik doğrulama üst bilgisi) izin sürüm 2.x yalnızca. |
| **`--cors`** | CORS çıkış noktaları, boşluk virgülle ayrılmış listesi. |
| **`--language-worker`** | Dil çalışan yapılandırmak için bağımsız değişkenler. Sürüm 2.x yalnızca. |
| **`--nodeDebugPort -n`** | Kullanmak düğüm hata ayıklayıcısı bağlantı noktası. Varsayılan: Launch.json veya 5858 arasında bir değer. Sürümü yalnızca 1.x. |
| **`--password`** | Parola veya bir .pfx dosyası için parolayı içeren dosya. Yalnızca kullanılan `--cert`. Sürüm 2.x yalnızca. |
| **`--port -p`** | Dinlemenin yapılacağı yerel bağlantı noktası. Varsayılan değer: 7071. |
| **`--pause-on-error`** | Duraklatma işlemi çıkmadan önce ek giriş. Yalnızca temel araçları bir tümleşik geliştirme ortamından (IDE) başlatma sırasında kullanılır.|
| **`--script-root --prefix`** | Çalıştırın veya dağıtılmış bir işlev uygulaması, kök yolunu belirtmek için kullanılır. Bu, bir alt klasöre proje dosyalarını oluşturmak derlenen projeler için kullanılır. Örneğin, bir C# sınıf kitaplığı projesi, host.json, local.settings.json ve function.json dosyaları oluşturduğunuzda oluşturulur bir *kök* bir yola sahip alt ister `MyProject/bin/Debug/netstandard2.0`. Bu durumda, ön eki olarak ayarlamak `--script-root MyProject/bin/Debug/netstandard2.0`. Azure'da çalışan işlev uygulamasını kök budur. |
| **`--timeout -t`** | Saniyeler içinde başlatılacak işlevleri konak için zaman aşımı. Varsayılan: 20 saniye.|
| **`--useHttps`** | Bağlama `https://localhost:{port}` yerine çok `http://localhost:{port}`. Varsayılan olarak, bu seçenek bilgisayarınızda güvenilen bir sertifika oluşturur.|

Bir C# sınıf kitaplığı projesi için (.csproj) eklemeniz gerekir `--build` kitaplığı .dll uzantısını oluşturmak için seçeneği.

İşlevleri ana bilgisayar başladığında, URL HTTP tetiklemeli işlevleri çıkarır:

```output
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

>[!IMPORTANT]
>Yerel olarak çalışırken, kimlik doğrulaması için HTTP uç noktaları zorlanmaz. Tüm yerel HTTP isteklerini olarak işlenir yani `authLevel = "anonymous"`. Daha fazla bilgi için [HTTP bağlama makale](functions-bindings-http-webhook.md#authorization-keys).

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
```

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

## <a name="publish"></a>Azure'da yayımlama

Azure işlevleri çekirdek araçları iki dağıtım türlerini destekler: işlev proje dosyalarını doğrudan işlev uygulamanız dağıtma [Zip dağıtma](functions-deployment-technologies.md#zip-deploy) ve [özel bir Docker kapsayıcısı dağıtma](functions-deployment-technologies.md#docker-container). Önceden olmalıdır [Azure aboneliğinizde bir işlev uygulamanız oluşturulurken](functions-cli-samples.md#create), kodunuzu dağıtmak. Derleme gerektiren projeler, böylece ikili dosyaları dağıtılabilir oluşturulmalıdır.

### <a name="project-file-deployment"></a>Dağıtım (proje dosyaları)

Kodunuzu yerel bir işlev uygulaması azure'da yayımlamak için kullanın `publish` komutu:

```bash
func azure functionapp publish <FunctionAppName>
```

Bu komut var olan işlev uygulamanızı Azure'a yayımlar. Yayımlamak çalışırsanız hata alırsınız bir `<FunctionAppName>` aboneliğinizde yok. Komut istemi veya terminal penceresinde Azure CLI kullanarak bir işlev uygulaması oluşturmak nasıl öğrenmek için bkz. [sunucusuz yürütme için bir işlev uygulaması oluşturma](./scripts/functions-cli-create-serverless.md). Varsayılan olarak, bu komutu çalıştırmak uygulamanızı sağlayacaktır [paketi çalıştırmak](run-functions-from-deployment-package.md) modu.

>[!IMPORTANT]
> Azure portalında bir işlev uygulaması oluşturduğunuzda, bu sürüm kullanır 2.x varsayılan olarak işlev çalışma zamanı. İşlev uygulaması kullanım sürümü yapmak için 1.x çalışma zamanı'ndaki yönergeleri izleyin [sürümünde çalışmasını 1.x](functions-versions.md#creating-1x-apps).
> Mevcut işlevleri sahip bir işlev uygulaması için çalışma zamanı sürümünü değiştiremezsiniz.

Aşağıdaki Yayımlama seçenekleri sürümleri, 1.x ve 2.x'i için geçerlidir:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Ayarları varsa üzerine yaz isteyen azure'a local.settings.json yayımlamak ayar zaten mevcut. Depolama öykünücüsü kullanıyorsanız, uygulama ayarının değiştirme bir [gerçek depolama bağlantısı](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | Uygulama ayarların üzerine yazmak için istemi bastır olduğunda `--publish-local-settings -i` kullanılır.|

Aşağıdaki Yayımlama seçenekleri yalnızca sürümünde desteklenen 2.x:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--publish-settings-only -o`** |  Yalnızca yayımlama ayarları ve içeriği atlayın. Komut istemi varsayılandır. |
|**`--list-ignored-files`** | .Funcignore dosyasını temel alan yayımlama sırasında sayılan dosyaların bir listesini görüntüler. |
| **`--list-included-files`** | .Funcignore dosyasını temel alan yayımlanan, dosyaların listesini görüntüler. |
| **`--nozip`** | Varsayılan kapatır `Run-From-Package` modunu devre dışı. |
| **`--build-native-deps`** | Python yayımlarken .wheels klasör oluşturmayı atlar uygulamalar çalışmaz. |
| **`--additional-packages`** | Yerel bağımlılıkları oluştururken yüklemek için paketler listesi. Örneğin: `python3-dev libevent-dev`. |
| **`--force`** | Bazı senaryolarda önceden yayımlama doğrulama yoksayın. |
| **`--csx`** | Bir C# betiği (.csx) projesini yayımlayın. |
| **`--no-build`** | DotNet işlevleri derlenmesini atla. |
| **`--dotnet-cli-params`** | Ne zaman yayımlama derlenmiş C# (.csproj) İşlevler, temel Araçlar 'dotnet build--çıktı bin/yayımlama' çağırır. Herhangi bir parametre için geçirilen komut satırına eklenir. |

### <a name="deployment-custom-container"></a>Dağıtım (özel kapsayıcı)

Azure işlevleri işlev projenizi dağıtmanıza olanak sağlar bir [özel Docker kapsayıcısı](functions-deployment-technologies.md#docker-container). Daha fazla bilgi için [Linux üzerinde özel görüntü kullanarak bir işlev oluşturma](functions-create-function-linux-custom-image.md). Özel kapsayıcı bir Dockerfile olmalıdır. Bir Dockerfile ile bir uygulama oluşturmak için--dockerfile seçeneğini kullanın `func init`.

```bash
func deploy
```

Aşağıdaki özel kapsayıcı dağıtım seçenekleri kullanılabilir:

| Seçenek     | Açıklama                            |
| ------------ | -------------------------------------- |
| **`--registry`** | Bir Docker kayıt defteri adını geçerli kullanıcı oturum. |
| **`--platform`** | İşlev uygulaması için ana bilgisayar platformu. Geçerli seçenekler: `kubernetes` |
| **`--name`** | İşlev uygulamasının adı. |
| **`--max`**  | İsteğe bağlı olarak, işlev uygulaması örnekleri dağıtmak için en fazla sayısını ayarlar. |
| **`--min`**  | İsteğe bağlı olarak, işlev uygulaması örnekleri dağıtmak için en az sayısını ayarlar. |
| **`--config`** | Bir isteğe bağlı dağıtım yapılandırma dosyası ayarlar. |

## <a name="monitoring-functions"></a>İzleme işlevleri

İşlevlerinizin yürütülmesini izlemek için önerilen yöntem, Azure Application Insights ile tümleştirerek ' dir. Bu tümleştirme, Azure portalında bir işlev uygulaması oluşturduğunuzda, sizin için varsayılan olarak gerçekleştirilir. Ancak, Azure CLI kullanarak işlev uygulamanızı oluşturmak, işlev uygulamanızı azure'da tümleştirme bitti değil.

İşlev uygulamanız için Application Insights'ı etkinleştirmek için:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Daha fazla bilgi için bkz. [İzleyici Azure işlevleri](functions-monitoring.md).
## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri çekirdek Araçları [açık kaynak ve GitHub üzerinde barındırılır](https://github.com/azure/azure-functions-cli).  
Bir hata veya özellik isteği için [açık bir GitHub sorunu](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure işlevleri temel araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
['FUNCTIONS_WORKER_RUNTIME']: functions-app-settings.md#functions_worker_runtime
[`AzureWebJobsStorage`]: functions-app-settings.md#azurewebjobsstorage
[Uzantı paketleri]: functions-bindings-register.md#local-development-with-azure-functions-core-tools-and-extension-bundles
