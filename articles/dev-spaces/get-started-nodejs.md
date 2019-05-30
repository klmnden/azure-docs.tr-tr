---
title: VS Code ile bulutta Kubernetes Node.js geliştirme ortamı oluşturma
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/26/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s
ms.openlocfilehash: e461f210dc5b2d0dda0eabd5ea80dfcdc9ccebfb
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392801"
---
# <a name="get-started-on-azure-dev-spaces-with-nodejs"></a>Node.js ile Azure geliştirme alanlarında çalışmaya başlama

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da geliştirme için iyileştirilmiş Kubernetes tabanlı bir ortam (bir _dev space_) oluşturun.
- VS Code'u ve komut satırını kullanarak kapsayıcılarda yinelemeli kod geliştirin.
- Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.

> [!Note]
> **Takılı kalarak,** herhangi bir zamanda bkz [sorun giderme](troubleshooting.md) bölümü.

## <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme
Azure Dev Spaces, çok az yerel makine kurulumu gerektirir. Geliştirme ortamı yapılandırmanızın büyük bölümü bulutta depolanır ve diğer kullanıcılarla paylaşılabilir. İlk olarak [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) indirip yükleyin.

### <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma
Azure'da oturum açın. Bir terminal penceresine aşağıdaki komutu yazın:

```cmd
az login
```

> [!Note]
> Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Birden çok Azure aboneliğiniz varsa...
Şunu çalıştırarak aboneliklerinizi görüntüleyebilirsiniz: 

```cmd
az account list
```
JSON çıkışında `isDefault: true` bulunan aboneliği bulun.
Kullanmak istediğiniz abonelik bu değilse, varsayılan aboneliği değiştirebilirsiniz:

```cmd
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Azure Dev Spaces için bir Kubernetes kümesi oluşturma

Komut isteminde, kaynak grubunu oluşturma bir [Azure geliştirme alanları destekleyen bir bölge][supported-regions].

```cmd
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```cmd
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

Kümenin oluşturulması birkaç dakika sürer.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>AKS kümenizi Azure Dev Spaces kullanacak şekilde yapılandırma

AKS kümenizi içeren kaynak grubuyla AKS kümesi adınızı kullanarak aşağıdaki Azure CLI komutunu girin. Komut, kümenizi Azure Dev Spaces desteğiyle yapılandırır.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

> [!IMPORTANT]
> Azure geliştirme alanları yapılandırma işlemini kaldıracak `azds` kümedeki varsa, ad alanı.

## <a name="get-kubernetes-debugging-for-vs-code"></a>VS Code için Kubernetes hata ayıklaması edinin
Kubernetes hata ayıklaması gibi zengin özellikler VS Code kullanarak .NET Core ve Node.js geliştiricileri için kullanılabilir.

1. Yüklü değilse, [VS Code](https://code.visualstudio.com/Download)’u yükleyin.
1. [VS Azure Dev Spaces uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) indirip yükleyin. Uzantının Market sayfasında ve yeniden VS Code’da Yükle’ye bir kez tıklayın. 

## <a name="create-a-nodejs-container-in-kubernetes"></a>Kubernetes’te bir Node.js kapsayıcısı oluşturma

Bu bölümde bir Node.js web uygulaması oluşturacak ve Kubernetes’teki bir kapsayıcı içinde çalıştıracaksınız.

### <a name="create-a-nodejs-web-app"></a>Node.js Web Uygulaması oluşturma
GitHub deposunu yerel ortamınıza indirmek için https://github.com/Azure/dev-spaces konumuna gidip **Kopyala veya İndir**’i seçerek GitHub’dan kodu indirin. Bu kılavuzun kodu `samples/nodejs/getting-started/webfrontend` içindedir.

## <a name="prepare-code-for-docker-and-kubernetes-development"></a>Docker ve Kubernetes geliştirme için Code hazırlayın
Şimdiye kadar, yerel olarak çalıştırılabilen temel bir web uygulamanız vardı. Şimdi uygulamanın kapsayıcısını tanımlayan varlıklar oluşturup Kubernetes’de nasıl dağıtılacağını belirleyerek uygulamayı kapsayıcılı hale getireceksiniz. Bu görev Azure Dev Spaces ile kolayca gerçekleştirilebilir: 

1. VS Code’u başlatın ve `webfrontend` klasörünü açın. (Hata ayıklama varlıkları eklemek veya projeyi geri yüklemek için tüm varsayılan istemleri yoksayabilirsiniz.)
1. VS Code’da Tümleşik Terminal’i açın (**Görünüm > Tümleşik Terminal** menüsünü kullanarak).
1. Şu komutu çalıştırın (geçerli klasörünüzün **webfrontend** olduğundan emin olun):

    ```cmd
    azds prep --public
    ```

Azure CLI’nin `azds prep` komutu varsayılan ayarlarla Docker ve Kubernetes varlıklarını oluşturur:
* `./Dockerfile`, uygulamanın kapsayıcı görüntüsünü açıklar, kaynak kodunun nasıl derlendiğini ve kapsayıcının içinde çalıştırıldığını belirtir.
* `./charts/webfrontend` altındaki [Helm grafiği](https://docs.helm.sh), kapsayıcının Kubernetes'de nasıl dağıtıldığını açıklar.

Şimdilik bu dosyaların tüm içeriğini anlamanız gerekli değildir. Bununla birlikte, **geliştirme aşamasından üretim aşamasına kadar aynı Kubernetes ve Docker kod yapılandırmalı varlıklarının kullanılabildiğini, bu şekilde farklı ortamlarda daha tutarlı sonuçlar sağlanabildiğini** belirtmek gerekir.
 
`prep` komutuyla `./azds.yaml` adlı bir dosya da oluşturulur ve bu dosya Azure Dev Spaces’in yapılandırma dosyasıdır. Azure’da yinelemeli bir geliştirme deneyimi sağlamak için Docker ve Kubernetes yapıtlarını ek yapılandırmayla tamamlar.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma
Şimdi kodumuzu çalıştıralım! Terminal penceresinde, bu komutu webfrontend **kök kod klasöründen** çalıştırın:

```cmd
azds up
```

Komutun çıkışını gözden geçirin; ilerledikçe bazı şeyler göreceksiniz:
- Kaynak kod Azure Dev Space ile eşitlenir.
- Kod klasörünüzdeki Docker varlıkları tarafından belirtildiği gibi Azure'da bir kapsayıcı görüntüsü oluşturulur.
- Kod klasörünüzdeki Helm grafiği tarafından belirtildiği gibi kapsayıcı görüntüsünü kullanan Kubernetes nesneleri oluşturulur.
- Kapsayıcının uç noktaları hakkındaki bilgiler görüntülenir. Bizim örneğimizde, genel bir HTTP URL'si görmeyi bekleriz.
- Yukarıdaki aşamaların başarıyla tamamlandığını varsayarsak, kapsayıcı başlatılırken `stdout` (ve `stderr`) çıkışını görmeye başlamalısınız.

> [!Note]
> Bu adımlar, `up` komutu ilk kez çalıştırıldığında biraz uzun sürer ama izleyen çalıştırmalar daha hızlı olacaktır.

### <a name="test-the-web-app"></a>Web uygulamasını test etme
`up` komutu tarafından oluşturulan genel URL hakkındaki bilgiler için konsol çıkışını tarayın. Şu biçimde olacaktır: 

```
(pending registration) Service 'webfrontend' port 'http' will be available at <url>
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
```

Tarayıcı penceresinde bu URL'yi açın; web uygulaması yükünü görmelisiniz. Kapsayıcı yürütülürken, terminal penceresine `stdout` ve `stderr` çıkışının akışı yapılır.

> [!Note]
> İlk çalıştırmada genel DNS hizmetinin hazır duruma gelmesi birkaç dakika sürebilir. Genel URL çözmezse alternatif kullanabileceğiniz `http://localhost:<portnumber>` konsol çıkışında görüntülenen URL. Localhost URL'sini kullanırsanız kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte AKS'de çalışıyordur. Size rahatlık sağlamak ve yerel makinenizden hizmetle etkileşimi kolaylaştırmak için, Azure Dev Spaces Azure'da çalıştırılan kapsayıcıya geçici bir SSH tüneli oluşturur. DNS kaydı hazır olduğunda geri gelip genel URL'yi deneyebilirsiniz.

### <a name="update-a-content-file"></a>İçerik dosyası güncelleştirme
Azure Dev Spaces yalnızca kodu Kubernetes’te çalıştırmaya yönelik değildir; aynı zamanda kod değişikliklerinizin buluttaki bir Kubernetes ortamında uygulandığını hızlıca ve yinelenerek görmenizi sağlar.

1. `./public/index.html` dosyasını bulun ve HTML dosyasında bir düzenleme yapın. Örneğin, sayfanın arka plan rengini mavinin bir tonu ile değiştirin:

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

2. Dosyayı kaydedin. Birkaç dakika sonra, Terminal penceresinde çalışan kapsayıcı içindeki bir dosyanın güncelleştirildiğini söyleyen bir ileti göreceksiniz.
1. Tarayıcınıza gidip sayfayı yenileyin. Renk güncelleştirmenizi görürsünüz.

Ne oldu? HTML ve CSS gibi içerik dosyalarında düzenlemeler yapmak için Node.js işleminin yeniden başlatılması gerekmez; bu nedenle etkin bir `azds up` komutu, değiştirilmiş tüm içerik dosyalarını Azure’daki çalışan kapsayıcıya doğrudan otomatik olarak eşitler ve böylece içerik düzenlemelerinizi görmenin hızlı bir yolunu sağlar.

### <a name="test-from-a-mobile-device"></a>Mobil cihazdan test etme
webfrontend genel URL'sini kullanarak web uygulamasını bir mobil cihazdan açın. Uzun adresi el ile girmemek için URL'yi masaüstü bilgisayarınızda kopyalayıp cihazınıza gönderebilirsiniz. Web uygulaması mobil cihazınızda yüklendiğinde, kullanıcı arabiriminin küçük bir cihazda düzgün şekilde gösterilmediğini fark edersiniz.

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

Bu komut, kapsayıcı görüntüsünü yeniden derler ve Helm grafiğini yeniden dağıtır. Kod değişikliklerinizin uygulandığını görmek için tarayıcı sayfasını yeniden yükleyin.

Bununla birlikte, kod geliştirmek için sonraki bölümde öğreneceğiniz daha da *hızlı bir yöntem* mevcuttur. 

## <a name="debug-a-container-in-kubernetes"></a>Kubernetes’te bir kapsayıcının hatalarını ayıklama

Bu bölümde, Azure'da çalıştırılan kapsayıcımızda doğrudan hata ayıklamak için VS Code kullanacaksınız. Ayrıca daha hızlı bir düzenleme-çalıştırma-test döngüsü elde etmeyi öğreneceksiniz.

![](media/common/edit-refresh-see.png)

> [!Note]
> Herhangi bir zamanda **kilitlenirseniz** [Sorun giderme](troubleshooting.md) bölümüne başvurun veya bu sayfada bir yorum paylaşın.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Hata ayıklama varlıklarını VS Code uzantısıyla başlatma
VS Code'un Azure Dev Space'imizle iletişim kurabilmesi için önce kod projenizi yapılandırmalısınız. Azure Dev Spaces için VS Code uzantısı hata ayıklama yapılandırmasını ayarlamak için yardımcı komutu sağlar. 

**Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

Bu, `.vscode` klasörünün altında Azure Dev Spaces için hata ayıklama yapılandırması ekler. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.

![](media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>AZDS hata ayıklama yapılandırmasını seçme
1. Hata Ayıklama görünümünü açmak için VS Code’un yan tarafındaki **Etkinlik Çubuğu** içinde Hata Ayıklama simgesine tıklayın.
1. Etkin hata ayıklama yapılandırması olarak **Program Başlat (AZDS)** öğesini seçin.

![](media/get-started-node/debug-configuration-nodejs2.png)

> [!Note]
> Komut Paletinde herhangi bir Azure Dev Spaces komutu görmüyorsanız, [Azure Dev Spaces için VS Code uzantısını yüklediğinizden](get-started-nodejs.md#get-kubernetes-debugging-for-vs-code) emin olun.

### <a name="debug-the-container-in-kubernetes"></a>Kubernetes’te kapsayıcının hatalarını ayıklama
Kubernetes’te kodunuzun hatalarını ayıklamak için **F5**’e basın!

`up` komutuna benzer şekilde, hata ayıklamaya başladığınızda kod geliştirme ortamıyla eşitlenir ve bir kapsayıcı derlenip Kubernetes’e dağıtılır. Bu kez, hata ayıklayıcı uzak kapsayıcıya eklenir.

> [!Tip]
> VS Code durum çubuğunda tıklanabilir bir URL görüntülenir.

![](media/common/vscode-status-bar-url.png)

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

Azure Dev Spaces ile, yerel olarak geliştirdiğiniz sırada kullandığınız geliştirme iş akışlarının birçoğunu kullanabilirsiniz. Bu noktayı vurgulamak için, örnek `webfrontend` projesi *nodemon* kullanacak şekilde yapılandırılmıştır (`package.json` içinde bir geliştirme bağımlılığı olarak yapılandırılmıştır).

Aşağıdaki adımları deneyin:
1. VS Code hata ayıklayıcıyı durdurun.
1. VS Code’un yan tarafındaki **Etkinlik Çubuğu** içinde Hata Ayıklama simgesine tıklayın. 
1. Etkin hata ayıklama yapılandırması olarak **Ekle (AZDS)** öğesini seçin.
1. F5'e basın.

Bu yapılandırmada, kapsayıcı *nodemon* başlatacak şekilde yapılandırılmıştır. Sunucu kodu düzenlemeleri yapıldığında *nodemon*, tıpkı yerel olarak geliştirme sırasında yaptığı gibi Node işlemini otomatik olarak yeniden başlatır. 
1. `server.js` içindeki karşılama iletisini yeniden düzenleyin ve dosyayı kaydedin.
1. Değişikliklerinizin uygulandığını görmek için tarayıcıyı yenileyin veya *Tekrar Söyleyin* düğmesine tıklayın!

**Artık kod üzerinde hızla yineleme ve doğrudan Kubernetes’te hata ayıklamaya yönelik bir yönteminiz var!** Ardından, ikinci bir kapsayıcıyı nasıl oluşturabileceğinizi ve çağırabileceğinizi göreceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok hizmet geliştirme hakkında bilgi edinin](multi-service-nodejs.md)


[supported-regions]: about.md#supported-regions-and-configurations