---
title: VS Code ile bulutta Kubernetes Node.js geliştirme ortamı oluşturma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: deb651170b0fd58f8c89b591f3e42b5b629f4095
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361483"
---
# <a name="get-started-on-azure-dev-spaces-with-nodejs"></a>Node.js ile Azure Dev Spaces'da Çalışmaya Başlama

[!INCLUDE[](includes/learning-objectives.md)]

[!INCLUDE[](includes/see-troubleshooting.md)]

Artık Azure’da Kubernetes tabanlı bir geliştirme ortamı oluşturmaya hazırsınız.

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme
Azure Dev Spaces, çok az yerel makine kurulumu gerektirir. Geliştirme ortamı yapılandırmanızın büyük bölümü bulutta depolanır ve diğer kullanıcılarla paylaşılabilir. İlk olarak [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)'yi indirip yükleyin.

> [!IMPORTANT]
> Azure CLI zaten yüklüyse, 2.0.32 veya üzeri bir sürüm kullandığınızdan emin olun.

[!INCLUDE[](includes/sign-into-azure.md)]

[!INCLUDE[](includes/use-dev-spaces.md)]

[!INCLUDE[](includes/install-vscode-extension.md)]

Kümenin oluşturulmasını beklerken kod yazmaya başlayabilirsiniz.

## <a name="create-a-nodejs-container-in-kubernetes"></a>Kubernetes’te bir Node.js kapsayıcısı oluşturma

Bu bölümde bir Node.js web uygulaması oluşturacak ve Kubernetes’teki bir kapsayıcı içinde çalıştıracaksınız.

### <a name="create-a-nodejs-web-app"></a>Node.js Web Uygulaması oluşturma
GitHub deposunu yerel ortamınıza indirmek için https://github.com/Azure/dev-spaces konumuna gidip **Kopyala veya İndir**’i seçerek GitHub’dan kodu indirin. Bu kılavuzun kodu `samples/nodejs/getting-started/webfrontend` içindedir.

[!INCLUDE[](includes/azds-prep.md)]

[!INCLUDE[](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>İçerik dosyası güncelleştirme
Azure Dev Spaces yalnızca kodu Kubernetes’te çalıştırmaya yönelik değildir; aynı zamanda kod değişikliklerinizin buluttaki bir Kubernetes ortamında uygulandığını hızla ve yinelenerek görmenizi sağlar.

1. `./public/index.html` dosyasını bulun ve HTML dosyasında bir düzenleme yapın. Örneğin, sayfanın arka plan rengini mavinin bir tonu ile değiştirin:

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

2. Dosyayı kaydedin. Birkaç dakika sonra, Terminal penceresinde çalışan kapsayıcı içindeki bir dosyanın güncelleştirildiğini söyleyen bir ileti göreceksiniz.
1. Tarayıcınıza gidip sayfayı yenileyin. Renk güncelleştirmenizi görürsünüz.

Ne oldu? HTML ve CSS gibi içerik dosyalarında düzenlemeler yapmak için Node.js işleminin yeniden başlatılması gerekmez; bu nedenle etkin bir `azds up` komutu, değiştirilmiş tüm içerik dosyalarını Azure’daki çalışan kapsayıcıya doğrudan otomatik olarak eşitler ve böylece içerik düzenlemelerinizi görmenin hızlı bir yolunu sağlar.

### <a name="test-from-a-mobile-device"></a>Mobil cihazdan test etme
Web uygulamasını bir mobil cihazda açarsanız, kullanıcı arabiriminin küçük bir cihazda düzgün şekilde gösterilmediğini fark edersiniz.

Bu sorunu gidermek için bir `viewport` meta etiketi ekleyin:
1. `./public/index.html` dosyasını açın
1. Mevcut `head` öğesine bir `viewport` meta etiketi ekleyin:

    ```html
    <head>
        <!-- Add this line -->
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    ```

1. Dosyayı kaydedin.
1. Cihazınızın tarayıcısını yenileyin. Şimdi web uygulamasının doğru şekilde işlendiğini görürsünüz. 

Bu örnek, bir uygulamayı kullanılması amaçlanan cihazlarda test edinceye kadar bazı sorunların nasıl bulunamadığını gösterir. Azure Dev Spaces sayesinde kodunuzda hızla yineleme yapabilir ve hedef cihazlardaki değişiklikleri doğrulayabilirsiniz.

### <a name="update-a-code-file"></a>Kod dosyasını güncelleştirme
Sunucu tarafı kod dosyalarının güncelleştirilmesi, Node.js uygulamasının yeniden başlatılması gerektiği için biraz daha fazla işlem gerektirir.

1. Terminal penceresinde `Ctrl+C` düğmesine basın (`azds up` hizmetini durdurmak için).
1. `server.js` adlı kod dosyasını açın ve hizmetin karşılama iletisini düzenleyin: 

    ```javascript
    res.send('Hello from webfrontend running in Azure!');
    ```

3. Dosyayı kaydedin.
1. Terminal penceresinde `azds up` komutunu çalıştırın. 

Bu işlem, kapsayıcı görüntüsünü yeniden derler ve Helm grafiğini yeniden dağıtır. Kod değişikliklerinizin uygulandığını görmek için tarayıcı sayfasını yeniden yükleyin.

Bununla birlikte, kod geliştirmek için sonraki bölümde öğreneceğiniz daha da *hızlı bir yöntem* mevcuttur. 

## <a name="debug-a-container-in-kubernetes"></a>Kubernetes’te bir kapsayıcının hatalarını ayıklama

[!INCLUDE[](includes/debug-intro.md)]

[!INCLUDE[](includes/init-debug-assets-vscode.md)]

### <a name="select-the-azds-debug-configuration"></a>AZDS hata ayıklama yapılandırmasını seçme
1. Hata Ayıklama görünümünü açmak için VS Code’un yan tarafındaki **Etkinlik Çubuğu** içinde Hata Ayıklama simgesine tıklayın.
1. Etkin hata ayıklama yapılandırması olarak **Program Başlat (AZDS)** öğesini seçin.

![](media/get-started-node/debug-configuration-nodejs.png)

> [!Note]
> Komut Paletinde herhangi bir Azure Dev Spaces komutu görmüyorsanız, [Azure Dev Spaces için VS Code uzantısını yüklediğinizden](get-started-nodejs.md#get-kubernetes-debugging-for-vs-code) emin olun.

### <a name="debug-the-container-in-kubernetes"></a>Kubernetes’te kapsayıcının hatalarını ayıklama
Kubernetes’te kodunuzun hatalarını ayıklamak için **F5**’e basın!

`up` komutuna benzer şekilde, hata ayıklamaya başladığınızda kod geliştirme ortamıyla eşitlenir ve bir kapsayıcı derlenip Kubernetes’e dağıtılır. Bu kez, hata ayıklayıcı uzak kapsayıcıya eklenir.

[!INCLUDE[](includes/tip-vscode-status-bar-url.md)]

Sunucu tarafı kod dosyasında (örneğin `server.js` içindeki `app.get('/api'...` içinde) bir kesme noktası belirleyin. Tarayıcı sayfasını yenilediğinizde veya 'Tekrar Söyleyin' düğmesine bastığınızda kesme noktasına basıp koda girebilirsiniz.

Kodun yerel olarak yürütülmesi durumunda olduğu gibi, çağrı yığını, yerel değişkenler, özel durum bilgileri vb. hata ayıklama bilgilerine tam erişiminiz vardır.

### <a name="edit-code-and-refresh-the-debug-session"></a>Kod düzenleme ve hata ayıklama oturumunu yenileme
Hata ayıklayıcı etkinken bir kod düzenlemesi yapın; örneğin, karşılama iletisini yeniden değiştirin:

```javascript
app.get('/api', function (req, res) {
    res.send('**** Hello from webfrontend running in Azure! ****');
});
```

Dosyayı kaydedin ve **Hata ayıklama eylemleri** bölmesinde **Yenile** düğmesine tıklayın. 

![](media/get-started-node/debug-action-refresh-nodejs.png)

Azure Dev Spaces, her kod düzenlemesi yapıldığında yeni bir kapsayıcı görüntüsünü yeniden derleme ve yeniden dağıtmayı içeren uzun süreli işlem yerine, hata ayıklama oturumları arasında Node.js işlemini yeniden başlatarak daha hızlı bir düzenleme/hata ayıklama döngüsü sağlar.

Tarayıcıda web uygulamasını yenileyin veya *Tekrar Söyleyin* düğmesine basın. Özel iletinizin kullanıcı arabiriminde görüntülendiğini görürsünüz.

### <a name="use-nodemon-to-develop-even-faster"></a>NodeMon ile daha da hızlı geliştirme
*Nodemon*, Node.js geliştiricilerinin hızlı geliştirme için kullandığı popüler bir araçtır. Sunucu tarafı kod düzenlemesi yapılan her durumda Node işlemini el ile yeniden başlatmak yerine, geliştiriciler genellikle Node projelerini *nodemon* ile dosya değişikliklerini izleyecek ve sunucu işlemini otomatik olarak yeniden başlatacak şekilde yapılandırır. Bu çalışma stilinde geliştirici bir kod değişikliği yaptıktan sonra yalnızca tarayıcısını yeniler.

Azure Dev Spaces ile, yerel olarak geliştirdiğiniz sırada kullandığınız geliştirme iş akışlarının birçoğunu kullanabilirsiniz. Bunu göstermek için, örnek `webfrontend` projesi *nodemon* kullanacak şekilde yapılandırılmıştır (`package.json` içinde bir geliştirme bağımlılığı olarak yapılandırılmıştır).

Aşağıdaki adımları deneyin:
1. VS Code hata ayıklayıcıyı durdurun.
1. VS Code’un yan tarafındaki **Etkinlik Çubuğu** içinde Hata Ayıklama simgesine tıklayın. 
1. Etkin hata ayıklama yapılandırması olarak **Ekle (AZDS)** öğesini seçin.
1. F5'e basın.

Bu yapılandırmada, kapsayıcı *nodemon* başlatacak şekilde yapılandırılmıştır. Sunucu kodu düzenlemeleri yapıldığında *nodemon*, tıpkı yerel olarak geliştirme sırasında yaptığı gibi Node işlemini otomatik olarak yeniden başlatır. 
1. `server.js` içindeki karşılama iletisini yeniden düzenleyin ve dosyayı kaydedin.
1. Değişikliklerinizin uygulandığını görmek için tarayıcıyı yenileyin veya *Tekrar Söyleyin* düğmesine tıklayın!

**Artık kod üzerinde hızla yineleme ve doğrudan Kubernetes’te hata ayıklamaya yönelik bir yönteminiz var!** Ardından, ikinci bir kapsayıcıyı nasıl oluşturabileceğinizi ve çağırabileceğinizi göreceksiniz.

## <a name="call-a-service-running-in-a-separate-container"></a>Ayrı kapsayıcıda çalıştırılan hizmeti çağırma

Bu bölümde `mywebapi` adlı ikinci bir hizmet oluşturacak ve bu hizmetin `webfrontend` tarafından çağrılmasını sağlayacaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>*mywebapi* için örnek kodu açma
Bu kılavuz için `samples` adlı klasörün altında zaten `mywebapi` örnek kodunuz olmalıdır (yoksa, https://github.com/Azure/dev-spaces adresine gidin ve **Kopyala veya İndir**'i seçerek GitHub deposunu indirin.) Bu bölümün kodu `samples/nodejs/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. *Ayrı bir VS Code penceresinde* `mywebapi` klasörünü açın.
1. F5'e basın ve hizmetin oluşturulup dağıtılmasını bekleyin. VS Code hata ayıklama çubuğu görüntülendiğinde hazır olduğunu anlarsınız.
1. Uç nokta URL'sini not alın; http://localhost:\<portnumber\> benzeri bir URL olacaktır. **İpucu: VS Code durum çubuğunda tıklanabilir bir URL görüntülenir.** Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamınızda çalışıyordur. Bunun localhost adresi olmasının nedeni `mywebapi` hizmetinin hiçbir genel uç nokta tanımlamamış olması ve buna yalnızca Kubernetes örneğinin içinden erişilebilmesidir. Size rahatlık sağlamak ve yerel makinenizden özel hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur.
1. `mywebapi` hazır olduğunda, tarayıcınızda localhost adresini açın. `mywebapi` hizmetinden bir yanıt görmeniz gerekir ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım.
1. `webfrontend` için VS Code penceresine geçin.
1. Şu kod satırlarını `server.js` dosyasının en üstüne ekleyin:
    ```javascript
    var request = require('request');
    var propagateHeaders = require('./propagateHeaders');
    ```

3. `/api` GET işleyicisinin kodunu *değiştirin*. İsteği işlerken, o da `mywebapi` hizmetine bir çağrı yapar ve her iki hizmetten de sonuçları döndürür.

    ```javascript
    app.get('/api', function (req, res) {
        request({
            uri: 'http://mywebapi',
            headers: propagateHeaders.from(req) // propagate headers to outgoing requests
        }, function (error, response, body) {
            res.send('Hello from webfrontend and ' + body);
        });
    });
    ```

Kubernetes’in DNS hizmeti bulma yönteminin hizmete `http://mywebapi` olarak başvuracak şekilde nasıl uygulandığına dikkat edin. **Geliştirme ortamınızdaki kod, üretimde çalışacağı şekilde çalışır**.

Yukarıdaki kod örneğinde `propagateHeaders` adlı bir yardımcı modül kullanılır. Bu yardımcı kod klasörünüze `azds prep` komutunu çalıştırdığınız zaman eklenmiştir. `propagateHeaders.from()` işlevi mevcut http.IncomingMessage nesnesindeki belirli üst bilgileri giden istek için bir headers nesnesine yayar. Daha sonra bunun takımlara işbirliğine dayalı geliştirme açısından nasıl yardımcı olduğunu göreceksiniz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. Varsayılan GET `/` işleyicisinde bir kesme noktası ayarlayın.
1. `webfrontend` projesinde, `http://mywebapi` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın.
1. `webfrontend` projesinde F5'e basın.
1. Web uygulamasını açın ve her iki hizmette de kodun üzerinden geçin. Web uygulamasında iki hizmet tarafından birleştirilmiş bir ileti görüntüleniyor olmalıdır: "Hello from webfrontend and Hello from mywebapi."

Bravo! Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

[!INCLUDE[](includes/team-development-1.md)]

Şimdi nasıl çalıştığını görün:
1. `mywebapi` için VS Code penceresine gidin ve varsayılan GET `/` işleyicisinde bir kod düzenlemesi yapın; örneğin:

    ```javascript
    app.get('/', function (req, res) {
        res.send('mywebapi now says something new');
    });
    ```

[!INCLUDE[](includes/team-development-2.md)]

[!INCLUDE[](includes/well-done.md)]

[!INCLUDE[](includes/clean-up.md)]




