---
title: Bulutta .NET Core ve Visual Studio kullanarak Kubernetes geliştirme ortamı oluşturma | Microsoft Docs
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
ms.openlocfilehash: c2d92f26bec2045e7f1e8afff189d58d8c29f25a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099485"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core-and-visual-studio"></a>.NET Core ve Visual Studio ile Azure Dev Spaces’ı Kullanmaya Başlama

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
- Visual Studio kullanarak kapsayıcılarda yinelemeli kod geliştirin.
- İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
- Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.

[!INCLUDE[](includes/see-troubleshooting.md)]

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="get-the-visual-studio-tools"></a>Visual Studio araçlarını edinme
1. En son [Visual Studio 2017](https://www.visualstudio.com/vs/) sürümünü yükleme
1. Visual Studio yükleyicisinde aşağıdaki İş Yükünün seçili olduğundan emin olun:
    * ASP.NET ve web geliştirme
1. [Kubernetes için Visual Studio Araçları](https://aka.ms/get-azds-visualstudio)'nı yükleme

## <a name="create-a-web-app-running-in-a-container"></a>Kapsayıcıda çalışan bir web uygulaması oluşturma

Bu bölümde bir ASP.NET Core web uygulaması oluşturacak ve Kubernetes’teki bir kapsayıcı içinde çalıştıracaksınız.

### <a name="create-an-aspnet-web-app"></a>ASP.NET web uygulaması oluşturma

Visual Studio 2017’de yeni bir proje oluşturun. Şu anda, projenin bir **ASP.NET Core Web Uygulaması** olması gerekir. Projeyi '**webfrontend**' olarak adlandırın.

![](media/get-started-netcore-visualstudio/NewProjectDialog1.png)

**Web Uygulaması (Model-Görünüm-Denetleyici)** şablonunu seçin ve iletişim kutusunun üstündeki iki açılır listede **.NET Core** ve **ASP.NET Core 2.0**’ı hedeflediğinizden emin olun. Projeyi oluşturmak için **Tamam**'a tıklayın.

![](media/get-started-netcore-visualstudio/NewProjectDialog2.png)


### <a name="enable-dev-spaces-for-an-aks-cluster"></a>AKS kümesi için Azure Dev Spaces'i etkinleştirme

Yeni oluşturduğunuz projede, aşağıda gösterildiği gibi başlatma ayarları açılır listesinden **Azure Dev Spaces** seçeneğini belirleyin.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Daha sonra gösterilen iletişim kutusunda, uygun hesapla oturum açtığınızdan emin olun ve ardından mevcut bir Kubernetes kümesini seçin.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.PNG)

**Alan** açılır listesini şimdilik varsayılan `default` değerinde bırakın. Daha sonra bu seçenek hakkında daha fazla bilgi edineceksiniz. Web uygulamasına bir genel uç noktadan erişilebilmesi için **Genel Erişime Açık** onay kutusunu işaretleyin. Bu ayar zorunlu değildir ancak bu kılavuzun sonraki kısımlarında bazı kavramları göstermeye yardımcı olacaktır. Yine de endişelenmeyin, her iki durumda da Visual Studio kullanarak web sitenizde hata ayıklayabilirsiniz.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog2.png)

Kümeyi seçmek veya oluşturmak için **Tamam**’a tıklayın.

Azure Dev Spaces ile çalışacak şekilde etkinleştirilmemiş bir küme seçerseniz, yapılandırmak isteyip istemediğinizi soran bir ileti görürsünüz.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

**Tamam**’ı seçin.

 Bunu gerçekleştirmek için bir arka plan görevi başlatılır. Tamamlanması birkaç dakika sürer. Oluşturma işleminin devam edip etmediğini görmek için, aşağıdaki görüntüde gösterildiği gibi işaretçinizi durum çubuğunun sol alt köşesindeki **Arka plan görevleri** simgesinin üzerine getirin.

![](media/get-started-netcore-visualstudio/BackgroundTasks.PNG)

> [!Note]
> Geliştirme alanı başarıyla oluşturulana kadar uygulamanızda hata ayıklayamazsınız.

### <a name="look-at-the-files-added-to-project"></a>Projeye eklenen dosyalara bakın
Geliştirme alanının oluşturulmasını beklerken, geliştirme alanını kullanmayı seçmeniz sırasında projenize eklenmiş dosyalara bakın.

İlk olarak, `charts` adlı bir klasörün eklendiğini ve bu klasörün içinde uygulamanız için bir [Helm grafiği](https://docs.helm.sh) oluşturulduğunu görebilirsiniz. Bu dosyalar, uygulamanızı geliştirme alanına dağıtmak için kullanılır.

`Dockerfile` adlı bir dosyanın eklendiğini görürsünüz. Bu dosya, uygulamanızı standart Docker biçiminde paketlemek için gereken bilgileri içerir.

Son olarak geliştirme alanının ihtiyaç duyduğu geliştirme zamanı yapılandırmasını içeren `azds.yaml` adlı dosyayı görürsünüz.

![](media/get-started-netcore-visualstudio/ProjectFiles.png)

## <a name="debug-a-container-in-kubernetes"></a>Kubernetes’te bir kapsayıcının hatalarını ayıklama
Geliştirme alanı başarıyla oluşturulduktan sonra uygulamanızda hata ayıklayabilirsiniz. Kodda bir kesme noktası oluşturun. Örneğin, `Message` değişkeninin ayarlandığı `HomeController.cs` dosyasının 20. satırında. Hata ayıklamaya başlamak için **F5**’e tıklayın. 

Visual Studio, uygulamayı derleyip dağıtmak için geliştirme alanıyla iletişim kurar ve sonra web uygulaması çalışır durumdayken bir tarayıcı açar. Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamında çalışıyordur. Localhost adresinin nedeni, Azure Dev Spaces’in AKS’de çalışan kapsayıcıya geçici bir SSH tüneli oluşturmasıdır.

Kesme noktasını tetiklemek için sayfanın üst kısmındaki **Hakkında** bağlantısına tıklayın. Kodun yerel olarak yürütülmesi durumunda olduğu gibi, çağrı yığını, yerel değişkenler, özel durum bilgileri vb. hata ayıklama bilgilerine tam erişiminiz vardır.

## <a name="iteratively-develop-code"></a>Kodu yinelemeli geliştirme

Azure Dev Spaces yalnızca kodu Kubernetes’te çalıştırmaya yönelik değildir; aynı zamanda kod değişikliklerinizin buluttaki bir Kubernetes ortamında uygulandığını hızlıca ve yinelenerek görmenizi sağlar.

### <a name="update-a-content-file"></a>İçerik dosyası güncelleştirme
1. `./Views/Home/Index.cshtml` dosyasını bulun ve HTML dosyasında bir düzenleme yapın. Örneğin, `<h2>Application uses</h2>` olan 70. satırı `<h2>Hello k8s in Azure!</h2>` benzeri bir değerle değiştirin.
1. Dosyayı kaydedin.
1. Tarayıcınıza gidip sayfayı yenileyin. Web sayfasında güncelleştirilmiş HTML’in gösterildiğini görürsünüz.

Ne oldu? HTML ve CSS gibi içerik dosyalarında düzenleme yapılması için bir .NET Core web uygulamasında yeniden derleme yapılması gerekmez; bu nedenle, etkin bir F5 oturumu değiştirilmiş içerik dosyalarını AKS’deki çalışan kapsayıcı ile otomatik olarak eşitler ve böylece içerik düzenlemelerinizi hemen görebilirsiniz.

### <a name="update-a-code-file"></a>Kod dosyasını güncelleştirme
.NET Core uygulamasının güncelleştirilmiş uygulama ikili dosyalarını yeniden derleyip oluşturması gerektiğinden, kod dosyalarının güncelleştirilmesi biraz daha fazla iş gerektirir.

1. Visual Studio'daki hata ayıklayıcısını durdurun.
1. `Controllers/HomeController.cs` adlı kod dosyasını açın ve Hakkında sayfasında gösterilen iletiyi düzenleyin: `ViewData["Message"] = "Your application description page.";`
1. Dosyayı kaydedin.
1. Hata ayıklamaya yeniden başlamak için **F5**'e basın. 

Azure Dev Spaces, her kod düzenlemesi yapıldığında yeni bir kapsayıcı görüntüsünü yeniden derleme ve yeniden dağıtmayı içeren uzun süreli işlem yerine, mevcut kapsayıcı içindeki kodu artımlı bir şekilde yeniden derleyerek daha hızlı bir düzenleme/hata ayıklama döngüsü sağlar.

Tarayıcıda web uygulamasını yenileyin ve Hakkında sayfasına gidin. Özel iletinizin kullanıcı arabiriminde görüntülendiğini görürsünüz.


## <a name="call-another-container"></a>Başka bir kapsayıcı çağırma
Bu bölümde `mywebapi` adlı ikinci bir hizmet oluşturacak ve bu hizmetin `webfrontend` tarafından çağrılmasını sağlayacaksınız. Her hizmet ayrı kapsayıcılarda çalışır. Ardından her iki kapsayıcıda da hata ayıklayacaksınız.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>*mywebapi* için örnek kod indirme
Zamandan kazanmak adına örnek kodu bir GitHub deposundan indirelim. https://github.com/Azure/dev-spaces adresine gidip **Kopyala veya İndir**’i seçerek GitHub deposunu indirin. Bu bölümün kodu `samples/dotnetcore/getting-started/mywebapi` konumundadır.

### <a name="run-mywebapi"></a>*mywebapi* hizmetini çalıştırın
1. `mywebapi` projesini *ayrı bir Visual Studio penceresinde* açın.
1. Daha önce `webfrontend` projesinde yaptığınız gibi başlatma ayarları açılır listesinden **Azure Dev Spaces** seçeneğini belirleyin. Bu sefer yeni bir AKS kümesi oluşturmak yerine, önceden oluşturduğunuz ortamı seçin. Önceki seferde olduğu gibi, Alan açılır listesini varsayılan `default` değerinde bırakın ve **Tamam**’a tıklayın. Çıkış penceresinde, hata ayıklamaya başlarken süreci hızlandırmak adına Visual Studio’nun bu yeni hizmeti geliştirme alanınızda “hazırlamaya” başladığını fark edebilirsiniz.
1. F5'e bastıktan sonra hizmetin oluşturulup dağıtılmasını bekleyin. Visual Studio durum çubuğu turuncuya döndüğünde hazır olduğunu biliyor olacaksınız
1. **Çıkış** penceresindeki **AKS için Azure Dev Spaces** bölmesinde görüntülenen uç nokta URL’sini not edin. http://localhost:\<portnumber\> gibi görünür. Kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte Azure’daki geliştirme ortamında çalışıyordur.
2. `mywebapi` hazır olduğunda, tarayıcınızı localhost adresine açın ve `ValuesController` için varsayılan GET API’yi çağırmak üzere URL’ye `/api/values` öğesini ekleyin. 
3. Tüm adımları başarılı olursa, `mywebapi` hizmetinden şöyle bir yanıt görebilmelisiniz.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>*webfrontend*’den *mywebapi*’ye istek gönderme
Şimdi `webfrontend` uygulamasında `mywebapi` hizmetine istek gönderen bir kod yazalım. `webfrontend` projesinin bulunduğu Visual Studio penceresine geçin. `HomeController.cs` dosyasında Hakkında yönteminin kodunu aşağıdaki kodla *değiştirin*:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

Önceki kod örneğinde `azds-route-as` üst bilgisi gelen istekten giden isteğe iletilmektedir. Daha sonra bunun ekip senaryolarında nasıl daha verimli bir geliştirme deneyimine olanak sağladığını görürsünüz.

### <a name="debug-across-multiple-services"></a>Birden çok hizmette hata ayıklama
1. Bu noktada, `mywebapi` hizmetinin hata ayıklayıcısı ekli bir şekilde çalışmaya devam ediyor olması gerekir. Devam etmiyorsa, `mywebapi` projesinde F5'e basın.
1. `api/values/{id}` GET isteklerini işleyen `Controllers/ValuesController.cs` dosyasının içerdiği `Get(int id)` yönteminde bir kesme noktası ayarlayın.
1. Yukarıdaki kodu yapıştırdığınız `webfrontend` projesinde, `mywebapi/api/values` konumuna GET isteği göndermeden hemen önce bir kesme noktası ayarlayın.
1. `webfrontend` projesinde F5'e basın. Visual Studio yeniden uygun localhost bağlantı noktasına bir tarayıcı açar ve web uygulaması görüntülenir.
1. `webfrontend` projesindeki kesme noktasını tetiklemek için sayfanın üst kısmındaki **Hakkında** bağlantısına tıklayın. 
1. Devam etmek için F10'a basın. `mywebapi` projesindeki kesme noktası tetiklenir.
1. Devam etmek üzere F5’e basarak `webfrontend` projesindeki koda dönersiniz.
1. F5’e bir kez daha bastığınızda istek tamamlanır ve tarayıcıda bir sayfa döndürülür. Web uygulamasında Hakkında sayfası iki hizmet tarafından birleştirilmiş bir ileti görüntüler: "Hello from webfrontend and Hello from mywebapi."

Bravo! Artık her kapsayıcının ayrı ayrı geliştirilip dağıtılabileceği çok kapsayıcılı bir uygulamanız var.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırdınız. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Paylaşılan bir geliştirme alanında veya gerektiğinde ayrı geliştirme alanlarında çalışarak bir geliştirici ekibinin aynı ortamda çalışmasını sağlayın.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız şu an için pek karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar.

Diğer düzinelerce hizmetle etkileşimde bulunan bir hizmet üzerinde çalıştığınızı düşünün.

- Geliştirme için her şeyi yerel olarak çalıştırmak pek mantıklı olmayabilir. Geliştirme makineniz uygulamanın tamamını çalıştıracak yeterli kaynaklara sahip olmayabilir. Ya da uygulamanız genel olarak erişilebilmesi gereken uç noktalara sahip olabilir (örneğin, uygulamanızın bir SaaS uygulamasındaki web kancasına yanıt vermesi durumunda).
- Yalnızca bağımlı olduğunuz hizmetleri çalıştırmayı deneyebilirsiniz, ancak bu durumda bağımlılıkların tam kapanışını bilmeniz gerekir (örneğin, bağımlılıkların bağımlılıkları). Ya da üzerinde çalışmadığınız için bağımlılıklarınızı oluşturup çalıştırmayı tam olarak bilmemenizle ilgili bir mesele olabilir.
- Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu bazen yardımcı olabilir, ancak bu sahteleri yönetmek için ayrı bir geliştirme çabası sarf etmek gerekebilir. Ayrıca bu yöntem, geliştirici ortamınızın üretimden farklı durmasına ve fark edilmeyen hataların ortaya çıkmasına yol açar.
- Bunun sonucunda da uçtan uca testin herhangi bir türünü yapmak zorlaşır. Tümleştirme testi yalnızca bir yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

`webfrontend` ve `mywebapi` hizmetlerinizin ikisinin de **`default` geliştirme alanında** çalıştığından emin olmak için aşağıdakileri yapın.
1. Her iki hizmetin de F5/hata ayıklama oturumunu kapatın, ancak projeleri Visual Studio pencerelerinde açık tutun.
2. `mywebapi` projesiyle Visual Studio penceresine geçin ve Ctrl+F5 tuşuna basarak hizmeti hata ayıklayıcısı ekli olmadan çalıştırın
3. `webfrontend` projesiyle Visual Studio penceresine geçin ve Ctrl+F5 tuşuna basarak bu hizmeti de çalıştırın.

> [!Note]
> Ctrl+F5’e bastıktan sonra web sayfasının ilk olarak görüntülenmesinin ardından tarayıcınızı bazen yenilemeniz gerekir.

Ortak URL’yi açan ve web uygulamasına giden herkes, varsayılan `default` alanını kullanarak her iki hizmet üzerinden de çalışan yazmış olduğunuz kod yolunu çağırır. Şimdi `mywebapi` hizmetini geliştirmeye devam etmek istediğinizi varsayalım. Bunu geliştirme alanınızı kullanan diğer geliştiricilere müdahale etmeden nasıl yapabilirsiniz? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-new-dev-space"></a>Yeni bir geliştirme alanı oluşturma
Visual Studio içinde, hizmetinizi F5 veya Ctrl+F5 tuşlarıyla yenilediğinizde kullanılacak olan ek alanları oluşturabilirsiniz. Alana istediğiniz adı verebilir ve ne anlama geldiği konusunda esnek davranabilirsiniz (ör. `sprint4` veya `demo`).

Yeni bir alan oluşturmak için aşağıdakileri yapın:
1. `mywebapi` projesinin bulunduğu Visual Studio penceresine geçin.
2. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
3. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
4. Buradan, F5 veya Ctrl+F5 tuşlarına basarken kullanılacak olan küme ve/veya alanı değiştirebilir ya da oluşturabilirsiniz. *Daha önce oluşturduğunuz Azure Dev Space’in seçildiğinden emin olun*.
5. Alan açılan listesinde **<Yeni Alan Oluştur…>** seçeneğini belirleyin.

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. **Alan Ekle** iletişim kutusunda alanın adını yazın ve **Tamam**’a tıklayın. İş arkadaşlarınızın hangi alanda çalıştığınızı tanımlayabilmesi adına yeni alan için adınızı kullanabilirsiniz (örneğin, "scott").

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Artık proje özellikleri sayfasında AKS kümenizi ve seçili yeni Alanı görmeniz gerekir.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>*mywebapi* için güncelleştirme kodu

1. `mywebapi` projesindeki `Controllers/ValuesController.cs` dosyasında bulunan `string Get(int id)` yöntemine aşağıda gösterildiği şekilde bir kod değişikliği yapın:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Bu güncelleştirilmiş kod bloğunda bir kesme noktası ayarlayın (zaten önceden ayarladığınız bir kesme noktanız olabilir).
3. `mywebapi` hizmetini başlatmak için F5’e basın. Bu eylem, kümenizdeki hizmeti, bu durumda `scott` olan seçili alanı kullanarak başlatır.

Aşağıda, farklı alanların nasıl çalıştığını anlamanıza yardımcı olacak bir diyagram verilmiştir. Mor yol, `default` alanı üzerinden bir istek gösterir. Bu, URL’ye alan eklenmemesi durumunda varsayılan yoldur. Pembe yol, `default/scott` alanı üzerinden bir istek gösterir.

![](media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her bir geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir ortamda uçtan uca test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi yayma üst bilgilerinin uygulama kodunuzda iletilmesini gerektirir.

### <a name="test-code-running-in-the-defaultscott-space"></a>`default/scott` alanında kod çalışmasını test etme
`mywebapi` hizmetinin `webfrontend` ile birlikte yeni sürümünü test etmek üzere, tarayıcınızı `webfrontend` için genel erişim noktası URL’sine açın (örneğin, http://webfrontend.123456abcdef.eastus.aksapp.io) ve Hakkında sayfasına gidin. "Hello from webfrontend and Hello from mywebapi" özgün iletisini görmelisiniz.

Şimdi de http://scott.s.webfrontend.123456abcdef.eastus.aksapp.io gibi bir şekilde olması için "scott.s." kısmını URL’ye ekleyin ve tarayıcıyı yenileyin. `mywebapi` projenizde ayarladığınız kesme noktasının isabet edilmesi gerekir. Devam etmek için F5’e tıkladığınızda tarayıcınızda yeni "Hello from webfrontend and mywebapi now says something new." iletisini artık görmeniz gerekir. Bunun nedeni, `mywebapi` hizmetindeki güncelleştirilmiş kodunuza giden yolun `default/scott` alanında çalışmasıdır.

[!INCLUDE[](includes/well-done.md)]

[!INCLUDE[](includes/clean-up.md)]
