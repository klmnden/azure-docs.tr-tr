---
title: 'Öğretici: Azure SignalR hizmeti kimlik doğrulaması ile Azure işlevleri'
description: Bu öğreticide, Azure SignalR Hizmeti istemcilerinin kimliğini doğrulamayı öğreneceksiniz
author: sffamily
ms.service: signalr
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/18/2018
ms.author: zhshang
ms.openlocfilehash: 34cbb4d2c8a1e84499961802ca7bd07408375345
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409409"
---
# <a name="tutorial-azure-signalr-service-authentication-with-azure-functions"></a>Öğretici: Azure SignalR hizmeti kimlik doğrulaması ile Azure işlevleri

Azure İşlevleri, App Service Kimlik Doğrulaması ve SignalR Hizmeti ile kimlik doğrulaması ve özel mesajlaşma özelliklerine sahip bir sohbet odası oluşturma öğreticisi.

## <a name="introduction"></a>Giriş

### <a name="technologies-used"></a>Kullanılan teknolojiler

* [Azure İşlevleri](https://azure.microsoft.com/services/functions/?WT.mc_id=serverlesschatlab-tutorial-antchu): Kullanıcıların kimliğini doğrulamak ve sohbet iletisi göndermek için kullanılan arka uç API'si
* [Azure SignalR Hizmeti](https://azure.microsoft.com/services/signalr-service/?WT.mc_id=serverlesschatlab-tutorial-antchu): Yeni iletileri bağlı sohbet istemcilerine yayınlar
* [Azure Depolama](https://azure.microsoft.com/services/storage/?WT.mc_id=serverlesschatlab-tutorial-antchu): Sohbet istemcisi arabirimi için statik web sitesini barındırır

### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi oluşturmak için aşağıdaki yazılımlar gereklidir.

* [Git](https://git-scm.com/downloads)
* [Node.js](https://nodejs.org/en/download/) (Sürüm 10.x)
* [.NET SDK](https://www.microsoft.com/net/download) (Sürüm 2.x, İşlevler uzantıları için gereklidir)
* [Azure İşlevleri Temel Araçları](https://github.com/Azure/azure-functions-core-tools) (Sürüm 2)
* [Visual Studio Code](https://code.visualstudio.com/) (VS Code) ve aşağıdaki uzantılar
  * [Azure İşlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) - VS Code'da Azure İşlevleri ile çalışmak için
  * [Canlı Sunucu](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) - Test amacıyla yerel ortamda web sayfası sunmak için

## <a name="sign-into-the-azure-portal"></a>Azure portalda oturum açma

[Azure portala](https://portal.azure.com/) gidin ve kimlik bilgilerinizle oturum açın.

## <a name="create-an-azure-signalr-service-instance"></a>Azure SignalR Hizmeti örneği oluşturma

Azure İşlevleri uygulamasını yerel ortamda derleyecek ve test edeceksiniz. Uygulama, Azure'da önceden oluşturulması gereken bir SignalR Hizmeti örneğine erişecek.

1. Yeni bir Azure kaynağı oluşturmak için **Kaynak oluştur** (**+**) düğmesine tıklayın.

1. **SignalR Hizmeti** araması yapın ve sonuçlardan seçin. **Oluştur**’a tıklayın.

    ![Yeni SignalR Service](media/signalr-authenticate-azure-functions/signalr-quickstart-new.png)

1. Aşağıdaki bilgileri girin.

    | Ad | Değer |
    |---|---|
    | Kaynak adı | SignalR Hizmeti örneği için benzersiz bir ad |
    | Kaynak grubu | Yeni bir kaynak grubu oluşturma |
    | Konum | Size yakın bir konum seçin |
    | Fiyatlandırma Katmanı | Ücretsiz |

1. **Oluştur**’a tıklayın.

## <a name="initialize-the-function-app"></a>İşlev uygulamasını başlatma

### <a name="create-a-new-azure-functions-project"></a>Yeni bir Azure İşlevleri projesi oluşturma

1. Yeni bir VS Code penceresinde menüden `File > Open Folder` seçeneğini kullanarak uygun konumda boş bir klasör oluşturun ve açın. Bu derleyeceğiniz uygulamanın ana proje klasörü olacaktır.

1. VS Code Azure İşlevleri uzantısını kullanarak ana proje klasöründe bir İşlev uygulaması başlatın.
    1. Menüden **Görünüm > Komut Paleti** yolunu izleyerek (kısayol `Ctrl-Shift-P`, macOS: `Cmd-Shift-P`) VS Code'da Komut Paletini açın.
    1. Arama **Azure işlevleri: Yeni proje oluşturma** komut ve bu seçeneği belirleyin.
    1. Ana proje klasörü görünmelidir. Bu klasörü seçin (veya "Gözat" düğmesini kullanarak bulun).
    1. Dil seçmeniz istendiğinde **JavaScript**'i seçin.

    ![İşlev uygulaması oluşturma](media/signalr-authenticate-azure-functions/signalr-create-vscode-app.png)

### <a name="install-function-app-extensions"></a>İşlev uygulaması uzantılarını yükleme

Bu öğreticide Azure SignalR Hizmeti ile etkileşim kurmak için Azure İşlevleri bağlamaları kullanılır. Diğer bağlamalar gibi SignalR Hizmeti bağlamaları da kullanılabilmesi için Azure İşlevleri Temel Araçları CLI aracılığıyla yüklenmesi gereken bir uzantı olarak sunulur.

1. Bir terminal seçerek VS Code'da açın **Görüntüle > tümleşik Terminal** menüsünden (Ctrl -\`).

1. Geçerli dizinin ana proje dizini olduğundan emin olun.

1. SignalR Hizmeti işlev uygulaması uzantısını yükleyin.

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.SignalRService -v 1.0.0-preview1-10002
    ```

### <a name="configure-application-settings"></a>Uygulama ayarlarını yapılandırma

Azure İşlevleri çalışma zamanını yerel ortamda çalıştırma ve hata ayıklama sırasında uygulama ayarları **local.settings.json** dosyasından okunur. Bu uygulamayı güncelleştirerek oluşturduğunuz SignalR Hizmeti örneğinin bağlantı dizesini ekleyin.

1. VS Code'un Gezgin bölmesinde **local.settings.json** dosyasını seçerek açın.

1. Dosyanın içeriğini aşağıdakilerle değiştirin.

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureSignalRConnectionString": "<signalr-connection-string>",
            "WEBSITE_NODE_DEFAULT_VERSION": "10.0.0",
            "FUNCTIONS_WORKER_RUNTIME": "node"
        },
        "Host": {
            "LocalHttpPort": 7071,
            "CORS": "*"
        }
    }
    ```

    * Azure SignalR Hizmeti bağlantı dizesini `AzureSignalRConnectionString` adlı bir ayara girin. Azure portaldan Azure SignalR Hizmeti kaynağının **Anahtarlar** sayfasındaki değeri alın. Birincil veya ikincil bağlantı dizesini kullanabilirsiniz.
    * `WEBSITE_NODE_DEFAULT_VERSION` ayarı yerel ortamda kullanılmaz ancak uygulama Azure'a dağıtıldığında kullanılması gerekir.
    * `Host` bölümü yerel İşlevler ana bilgisayarı için bağlantı noktası ve CORS ayarlarını yapılandırır (Azure'da çalışırken bu ayarın bir etkisi yoktur).

    ![SignalR Hizmeti anahtarını alma](media/signalr-authenticate-azure-functions/signalr-get-key.png)

1. Dosyayı kaydedin.

    ![Yerel ayarları güncelleştirme](media/signalr-authenticate-azure-functions/signalr-update-local-settings.png)

## <a name="create-a-function-to-authenticate-users-to-signalr-service"></a>Kullanıcıların SignalR Hizmetinde kimlik doğrulamasını sağlayacak işlevi oluşturma

Sohbet uygulaması tarayıcıda ilk açıldığında Azure SignalR Hizmetine bağlanmak için gerekli bağlantı kimlik bilgilerine ihtiyaç duyar. Bu bağlantı bilgilerini döndürmek için işlev uygulamanızda *SignalRInfo* adlı bir HTTP ile tetiklenen işlev oluşturacaksınız.

1. VS Code komut paletini açın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Arayın ve seçin **Azure işlevleri: İşlev oluşturma** komutu.

1. İstendiğinde aşağıdaki bilgileri girin.

    | Ad | Değer |
    |---|---|
    | İşlev uygulamasının klasörü | Ana proje klasörünü seçin |
    | Şablon | HTTP Tetikleyicisi |
    | Ad | SignalRInfo |
    | Yetkilendirme düzeyi | Anonim |

    Yeni işlevi içeren **SignalRInfo** adlı bir klasör oluşturulur.

1. İşlev bağlamalarını yapılandırmak için **SignalRInfo/function.json** dosyasını açın. Dosyanın içeriğini aşağıdaki kodla değiştirin. Bu kod bir istemcinin `chat` adlı Azure SignalR Hizmeti hub'ına bağlanması için geçerli kimlik bilgileri oluşturan bir giriş bağlaması ekler.

    ```json
    {
        "disabled": false,
        "bindings": [
            {
                "authLevel": "anonymous",
                "type": "httpTrigger",
                "direction": "in",
                "name": "req"
            },
            {
                "type": "http",
                "direction": "out",
                "name": "res"
            },
            {
                "type": "signalRConnectionInfo",
                "name": "connectionInfo",
                "userId": "",
                "hubName": "chat",
                "direction": "in"
            }
        ]
    }
    ```

    `signalRConnectionInfo` bağlamasındaki `userId` özelliği kimliği doğrulanmış SignalR Hizmeti bağlantısı oluşturmak için kullanılır. Yerel ortamda geliştirme için bu özelliği boş bırakın. İşlev uygulaması Azure'a dağıtıldığında bu özelliği kullanacaksınız.

1. İşlevin gövdesini görüntülemek için **SignalRInfo/index.js** dosyasını açın. Dosyanın içeriğini aşağıdaki kodla değiştirin.

    ```javascript
    module.exports = function (context, req, connectionInfo) {
        context.res = { body: connectionInfo };
        context.done();
    };
    ```

    Bu işlev giriş bağlamasındaki SignalR bağlantısı bilgilerini alır ve HTTP yanıtı gövdesinde istemciye döndürür.

## <a name="create-a-function-to-send-chat-messages"></a>Sohbet iletisi göndermek için işlev oluşturma

Web uygulaması, sohbet iletisi göndermek için bir HTTP API'sine ihtiyaç duyar. SignalR Hizmetini kullanarak bağlı olan tüm istemcilere ileti gönderen *SendMessage* adlı bir HTTP ile tetiklenen işlev oluşturacaksınız.

1. VS Code komut paletini açın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Arayın ve seçin **Azure işlevleri: İşlev oluşturma** komutu.

1. İstendiğinde aşağıdaki bilgileri girin.

    | Ad | Değer |
    |---|---|
    | İşlev uygulamasının klasörü | ana proje klasörünü seçin |
    | Şablon | HTTP Tetikleyicisi |
    | Ad | SendMessage |
    | Yetkilendirme düzeyi | Anonim |

    Yeni işlevi içeren **SendMessage** adlı bir klasör oluşturulur.

1. İşlev bağlamalarını yapılandırmak için **SendMessage/function.json** dosyasını açın. Dosyanın içeriğini aşağıdaki kodla değiştirin.
    ```json
    {
        "disabled": false,
        "bindings": [
            {
                "authLevel": "anonymous",
                "type": "httpTrigger",
                "direction": "in",
                "name": "req",
                "route": "messages",
                "methods": [
                    "post"
                ]
            },
            {
                "type": "http",
                "direction": "out",
                "name": "res"
            },
            {
                "type": "signalR",
                "name": "signalRMessages",
                "hubName": "chat",
                "direction": "out"
            }
        ]
    }
    ```
    Bu kod özgün işlevde iki değişiklik yapar:
    * Yolu `messages` olarak değiştirir ve HTTP tetikleyicisini **POST** HTTP metoduyla sınırlar.
    * `chat` adlı SignalR Hizmeti hub'ına bağlı olan tüm istemcilere ileti gönderen bir SignalR Hizmeti çıkış bağlaması ekler.

1. Dosyayı kaydedin.

1. İşlevin gövdesini görüntülemek için **SendMessage/index.js** dosyasını açın. Dosyanın içeriğini aşağıdaki kodla değiştirin.

    ```javascript
    module.exports = function (context, req) {
        const message = req.body;
        message.sender = req.headers && req.headers['x-ms-client-principal-name'] || '';

        let recipientUserId = '';
        if (message.recipient) {
            recipientUserId = message.recipient;
            message.isPrivate = true;
        }

        context.bindings.signalRMessages = [{
            'userId': recipientUserId,
            'target': 'newMessage',
            'arguments': [ message ]
        }];
        context.done();
    };
    ```

    Bu işlev HTTP isteğinin gövdesini alır ve SignalR Hizmetine bağlı istemcilere göndererek her istemcide `newMessage` adlı bir işlevi çağırır.

    İşlev gönderenin kimliğini okuyabilir ve ileti gövdesine eklenebilecek bir *alıcı* değeriyle iletinin tek bir kullanıcıya gönderilmesini sağlayabilir. Bu işlevler öğreticinin ilerleyen bölümlerinde kullanılacaktır.

1. Dosyayı kaydedin.

## <a name="create-and-run-the-chat-client-web-user-interface"></a>Sohbet istemcisi web kullanıcı arabirimini oluşturma ve çalıştırma

Sohbet uygulamasının arabirimi, Vue JavaScript çerçevesiyle oluşturulan basit bir tek sayfalı uygulamadır (SPA). İşlev uygulamasından ayrı barındırılacaktır. Yerel ortamda web arabirimini çalıştırmak için Canlı Sunucu VS Code uzantısını kullanacaksınız.

1. VS Code'da ana proje klasörünün kök dizininde **content** adlı yeni bir klasör oluşturun.

1. **content** klasöründe **index.html** adlı bir dosya oluşturun.

1. Aşağıdaki içeriği kopyalayıp **[index.html](https://raw.githubusercontent.com/Azure-Samples/signalr-service-quickstart-serverless-chat/master/docs/demo/chat-with-auth/index.html)** dosyasına yapıştırın.

1. Dosyayı kaydedin.

1. **F5** tuşuna basarak işlev uygulamasını yerel ortamda çalıştırın ve bir hata ayıklayıcısı ekleyin.

1. İle **index.html** açın, VS Code komut paletini açarak Canlı sunucuyu başlatın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`) seçerek **Canlı sunucusu: Canlı sunucusuyla açık**. Canlı Sunucu uygulamayı bir tarayıcıda açar.

1. Uygulama açılır. Sohbet kutusuna ileti yazıp Enter tuşuna basın. Yeni iletileri görmek için uygulamayı yenileyin. Kimlik doğrulaması yapılandırılmadığından tüm iletiler "anonim" olarak gönderilir.

## <a name="deploy-to-azure-and-enable-authentication"></a>Azure'a dağıtma ve kimlik doğrulamasını etkinleştirme

Buraya kadar işlev uygulamasını ve sohbet uygulamasını yerel ortamda çalıştırdınız. Şimdi bunları Azure'a dağıtıp kimlik doğrulamasını ve uygulamanın özel mesajlaşma özelliğini etkinleştireceksiniz.

### <a name="log-into-azure-with-vs-code"></a>VS Code ile Azure'da oturum açma

1. VS Code komut paletini açın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Arayın ve seçin **Azure: Oturum** komutu.

1. Yönergeleri izleyerek tarayıcınızda oturum açma işlemlerini tamamlayın.

### <a name="configure-function-app-for-authentication"></a>İşlev uygulamasını kimlik doğrulaması için yapılandırma

Şu ana kadar sohbet uygulaması anonim çalışıyordu. Kullanıcıların kimliğini doğrulamak için Azure'da [App Service Kimlik Doğrulamasını](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) kullanacaksınız. Kimliği doğrulanmış olan kullanıcının kimliği veya kullanıcı adı *SignalRConnectionInfo* bağlamasına iletilerek kullanıcı olarak kimliği doğrulanan bağlantı bilgileri oluşturulabilir.

İleti gönderirken uygulama bağlı tüm istemcilere veya yalnızca belirli bir kullanıcı için kimliği doğrulanmış olan istemcilere gönderme seçenekleri arasında seçim yapabilir.

1. VS Code'da **SendMessage/function.json** dosyasını açın.

1. *SignalRConnectionInfo* bağlamasının *userId* özelliğine bir [bağlama ifadesi](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#binding-expressions-and-patterns) ekleyin: `{headers.x-ms-client-principal-name}`. Bu ifade değeri kimliği doğrulanmış kullanıcının kullanıcı adı olarak ayarlar. Öznitelik şimdi aşağıdaki gibi görünmelidir.

    ```json
    {
        "type": "signalRConnectionInfo",
        "name": "connectionInfo",
        "userId": "{headers.x-ms-client-principal-name}",
        "hubName": "chat",
        "direction": "in"
    }
    ```

1. Dosyayı kaydedin.

### <a name="deploy-function-app"></a>İşlev uygulamasını dağıtma

1. VS Code komut paletini açın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`) seçip **Azure işlevleri: İşlev uygulamasına dağıtın**.

1. İstendiğinde aşağıdaki bilgileri girin.

    | Ad | Değer |
    |---|---|
    | Dağıtılacak klasör | Ana proje klasörünü seçin |
    | Abonelik | Aboneliğinizi seçme |
    | İşlev uygulaması | **Yeni İşlev Uygulaması Oluştur**'u seçin |
    | İşlev uygulamasının adı | Benzersiz bir ad girin |
    | Kaynak grubu | SignalR Hizmeti örneğinin bulunduğu kaynak grubunu seçin |
    | Depolama hesabı | **Yeni depolama hesabı oluştur**'u seçin |
    | Depolama hesabı adı | Benzersiz bir ad girin (3-24 karakter, yalnızca alfasayısal) |
    | Konum | Size yakın bir konum seçin |

    Azure'da yeni bir işlev uygulaması oluşturulur ve dağıtım başlar. Dağıtımın tamamlanmasını bekleyin.

### <a name="upload-function-app-local-settings"></a>İşlev uygulaması yerel ayarlarını karşıya yükleme

1. VS Code komut paletini açın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Arayın ve seçin **Azure işlevleri: Karşıya yükleme yerel ayarları** komutu.

1. İstendiğinde aşağıdaki bilgileri girin.

    | Ad | Değer |
    |---|---|
    | Yerel ayarlar dosyası | local.settings.json |
    | Abonelik | Aboneliğinizi seçme |
    | İşlev uygulaması | Önceden dağıtılmış işlev uygulamasını seçin |
    | İşlev uygulamasının adı | Benzersiz bir ad girin |

Yerel ayarlar, Azure'daki işlev uygulamasına yüklenir. Geçerli ayarların üzerine yazmak isteyip istemediğiniz sorulduğunda **Tümüne evet**'i seçin.

### <a name="enable-function-app-cross-origin-resource-sharing-cors"></a>İşlev uygulamasında çıkış noktaları arası kaynak paylaşımını (CORS) etkinleştirme

**local.settings.json** dosyasında CORS seçeneği mevcut olsa da Azure'daki işlev uygulamasına yüklenmez. Bunu ayrıca ayarlamanız gerekir.

1. VS Code komut paletini açın (`Ctrl-Shift-P`, macOS: `Cmd-Shift-P`).

1. Arayın ve seçin **Azure işlevleri: Açık portalında** komutu.

1. İşlev uygulamasını Azure portalda açmak için aboneliği ve işlev uygulaması adını seçin.

1. Açılan portalda **Platform özellikleri** sekmesinde **CORS**'yi seçin.

    ![CORS'yi bulun](media/signalr-authenticate-azure-functions/signalr-find-cors.png)

1. `*` değerine sahip bir giriş ekleyin.

1. Var olan diğer tüm girişleri kaldırın.

1. CORS ayarlarını kalıcı hale getirmek için **Kaydet**'e tıklayın.

    ![CORS'yi ayarlama](media/signalr-authenticate-azure-functions/signalr-set-cors.png)

> [!NOTE]
> Gerçek bir uygulamada CORS'ye tüm çıkış noktalarında izin vermek yerine (`*`) yalnızca gerekli olan etki alanları için belirli CORS girişleri kullanarak daha güvenli bir yaklaşım benimsemeniz gerekir.

### <a name="update-the-web-app"></a>Web uygulamasını güncelleştirme

1. Azure portalda işlev uygulamasının genel bakış sayfasına gidin.

1. İşlev uygulamasının URL'sini kopyalayın.

    ![URL'yi alın](media/signalr-authenticate-azure-functions/signalr-get-url.png)

1. VS Code'da **index.html** dosyasını açın ve `apiBaseUrl` değerini işlev uygulamasının URL'siyle değiştirin.

1. Uygulama Azure Active Directory, Facebook, Twitter, Microsoft hesabı veya Google ile kimlik doğrulaması gerçekleştirilecek şekilde yapılandırılabilir. `authProvider` değerini ayarlayarak kullanmak istediğiniz kimlik doğrulaması sağlayıcısını seçin.

1. Dosyayı kaydedin.

### <a name="deploy-the-web-application-to-blob-storage"></a>Web uygulamasını blob depolamaya dağıtma

Web uygulamasını Azure Blob Depolama'nın statik web siteleri özelliğini kullanarak barındıracağız.

1. Yeni bir Azure kaynağı oluşturmak için **Yeni** (**+**) düğmesine tıklayın.

1. **Depolama** bölümünde **Depolama hesabı**'nı seçin.

1. Aşağıdaki bilgileri girin.

    | Ad | Değer |
    |---|---|
    | Ad | Blob depolama hesabı için benzersiz bir ad |
    | Hesap türü | StorageV2 (genel amaçlı V2) |
    | Konum | Diğer kaynaklarınızla aynı bölgeyi seçin |
    | Çoğaltma | Yerel olarak yedekli depolama (LRS) |
    | Performans | Standart |
    | Erişim katmanı | Sık Erişimli |
    | Kaynak grubu | Bu öğreticideki diğer kaynakların bulunduğu kaynak grubunu seçin |

1. **Oluştur**’a tıklayın.

    ![Depolama oluşturma](media/signalr-authenticate-azure-functions/signalr-create-storage.png)

1. Oluşturulan depolama hesabını Azure portalda açın.

1. Soldaki gezinti bölmesinden **Statik web sitesi (önizleme)** öğesini seçin.

1. **Etkinleştir**’i seçin.

1. **Dizin belgesi adı** olarak `index.html` yazın.

1. **Kaydet**’e tıklayın.

    ![Statik siteleri yapılandırma](media/signalr-authenticate-azure-functions/signalr-static-websites.png)

1. Sayfadaki **$web** bağlantısına tıklayarak blob kapsayıcısını açın.

1. **Karşıya Yükle**'ye tıklayıp **content** klasöründeki **index.html** adlı dosyayı yükleyin.

1. **Statik web sitesi** sayfasına dönün. **Birincil uç nokta** değerini not edin. Bu, web uygulamasının URL'sidir.

### <a name="enable-app-service-authentication"></a>App Service Kimlik Doğrulamayı Etkinleştirme

App Service Kimlik Doğrulaması; Azure Active Directory, Facebook, Twitter, Microsoft hesabı ve Google ile kimlik doğrulamayı destekler.

1. İşlev uygulaması portalda açık durumdayken **Platform özellikleri** sekmesini bulun ve **Kimlik Doğrulaması/Yetkilendirme**'yi seçin.

1. App Service Kimlik Doğrulama ayarını **Açık** duruma getirin.

1. **İsteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** menüsünde "{Seçtiğiniz kimlik doğrulaması sağlayıcısı} ile oturum aç"ı seçin.

1. **İzin Verilen Dış Yönlendirme URL'leri** bölümünde önceden not ettiğiniz depolama hesabı birincil web uç noktası URL'sini girin.

1. Yapılandırmayı tamamlamak için seçtiğiniz oturum açma hizmeti sağlayıcısının belgelerini inceleyin.

    - [Azure Active Directory](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad)
    - [Facebook](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-facebook)
    - [Twitter](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-twitter)
    - [Microsoft hesabı](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-microsoft)
    - [Google](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-google)

### <a name="try-the-application"></a>Uygulamayı deneyin

1. Bir tarayıcıdan depolama hesabınızın birincil web uç noktasına gidin.

1. Seçtiğiniz kimlik doğrulaması sağlayıcısıyla kimlik doğrulaması gerçekleştirmek için **Oturum aç**'ı seçin.

1. Ana sohbet kutusunu kullanarak genel ileti gönderin.

1. Sohbet geçmişindeki kullanıcı adlarından birine tıklayarak özel ileti gönderin. İletiler yalnızca seçilen alıcıya gönderilir.

Tebrikler! Gerçek zamanlı bir sunucusuz sohbet uygulaması dağıttınız!

![Tanıtım](media/signalr-authenticate-azure-functions/signalr-serverless-chat.gif)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için Azure portalı kullanarak kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure İşlevleri’ni Azure SignalR Hizmeti ile birlikte kullanmayı öğrendiniz. Azure İşlevleri için SignalR Hizmeti bağlamalarıyla gerçek zamanlı sunucusuz uygulama derleme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure İşlevleri ile Gerçek Zamanlı Uygulama Derleme](signalr-overview-azure-functions.md)