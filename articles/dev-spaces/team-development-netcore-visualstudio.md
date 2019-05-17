---
title: .NET Core ve Visual Studio kullanarak Azure geliştirme alanları ile takım geliştirme
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
author: DrEsteban
ms.author: stevenry
ms.date: 12/09/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s '
ms.openlocfilehash: c3a988a831ad1069e5988f9c67e92a85a7a44840
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65765199"
---
# <a name="team-development-with-azure-dev-spaces"></a>Azure Dev Spaces ile ekip geliştirmesi

Bu öğretici sayesinde nasıl bir geliştirici takımı ile geliştirme alanları kullanarak Kubernetes kümesinde aynı anda çalışabileceğiniz öğreneceksiniz.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırdınız. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Bir geliştirici takımı ile bir paylaşılan geliştirme alanında veya ayrı geliştirme alanları gerektiği şekilde çalışarak aynı ortamda çalışmak için etkinleştirin.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanızı, şu anda karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar.

* Geliştirme makinenizde tek seferde ihtiyacınız olan her hizmeti çalıştırmak için yeterli kaynağı olmayabilir.
* Bazı hizmetler genel olarak erişilebilir olması gerekebilir. Örneğin, bir hizmet için bir Web kancası yanıt veren bir uç noktaya sahip gerekebilir.
* Hizmetlerin bir alt kümesini çalıştırmak istiyorsanız, tüm hizmetler arasında tam bağımlılık hiyerarşi bilmeniz gerekir. Özellikle hizmetler sayısı arttıkça Bu hiyerarşi belirlenmesi zor olabilir.
* Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu yaklaşım yardımcı olabilir, ancak bu mocks yönetme geliştirme maliyetini yakında etkileyebilir. Ayrıca, bu yaklaşım oluşan küçük hatalar için yol açabilir üretim, farklı isteyen geliştirme ortamınızı yol açar.
* Bu, her tür tümleştirme testi yapılması zor izler. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

## <a name="use-dev-spaces-for-team-development"></a>Takım geliştirme için geliştirme alanları kullanın
Somut örneği kullanarak bu fikirleri gösterelim bizim *webfrontend* -> *mywebapi* örnek uygulama. Biz bir geliştirici, Scott, gereken yere bir değişiklik yapmak için bir senaryoyu düşünün *mywebapi* hizmeti ve *yalnızca* hizmet. *Webfrontend* Scott'ın güncelleştirmenin bir parçası değiştirmek zorunda kalmazsınız.

_Olmadan_ geliştirme alanları'nı kullanarak, Scott hiçbirinin ideal geliştirme ve test kendi güncelleştirme birkaç şekilde olabilir:
* Docker'ın yüklü ve MiniKube ile daha güçlü bir geliştirme makinesi gerektiren tüm bileşenler yerel olarak çalıştırın.
* TÜM bileşenleri ayrılmış bir ad alanında Kubernetes kümesinde çalışır. Bu yana *webfrontend* , yalıtılmış bir kullanarak değiştirme olmayan küme kaynaklarının boşa harcanmasına ad alanıdır.
* YALNIZCA çalıştırma *mywebapi*ve test etmek için el ile REST çağrılarını yapın. Bu tür akışını uçtan uça tam test değil.
* Kod geliştirme odaklı ekleyin *webfrontend* farklı bir örneğine istekleri göndermek geliştiricinin sağlayan *mywebapi*. Bu kod ekleme karmaşıklaştırır *webfrontend* hizmeti.

### <a name="set-up-your-baseline"></a>Temel ayarlayın
İlk biz hizmetlerimizin temel dağıtmanız gerekir. Bu dağıtım, yerel kodunuzu iade sürüm karşılaştırması davranışını kolayca karşılaştırabilmeniz "en son bilinen iyi" temsil eder. Ardından bu taban çizgisine göre yaptığımız değişiklikleri için test edebilmesi dayalı bir alt alanı oluşturacağız *mywebapi* daha büyük uygulama bağlamında.

1. Kopya [geliştirme alanları örnek uygulama](https://github.com/Azure/dev-spaces): `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Uzak bir dalı kullanıma alın *azds_updates*: `git checkout -b azds_updates origin/azds_updates`
1. Her iki hizmetin de F5/hata ayıklama oturumunu kapatın, ancak projeleri Visual Studio pencerelerinde açık tutun.
1. İle Visual Studio penceresine geçiş _mywebapi_ proje.
1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
1. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
1. Seçin **değişiklik** olacak alanı oluşturmak için kullanılabilir F5 ya da Ctrl + F5 hizmet.
1. Alan açılır menüden seçin  **\<yeni alan oluştur... \>**.
1. Üst alanı emin olun kümesine  **\<hiçbiri\>**, alan adı girin **geliştirme**. Tamam'a tıklayın.
1. Çalıştırmak için CTRL + F5 tuşlarına basın _mywebapi_ bağlı hata ayıklayıcı olmadan.
1. İle Visual Studio penceresine geçiş _webfrontend_ proje ve de çalıştırmak için Ctrl + F5 tuşuna basın.

> [!Note]
> Ctrl+F5’e bastıktan sonra web sayfasının ilk olarak görüntülenmesinin ardından tarayıcınızı bazen yenilemeniz gerekir.

> [!TIP]
> Yukarıdaki adımları el ile bir temel ayarlamanız, ancak takımlar kullanım otomatik olarak temel önem koduyla güncel tutmak için CI/CD öneririz.
>
> Kullanıma sunduğumuz [Azure DevOps ile CI/CD ayarlama Kılavuzu](how-to/setup-cicd.md) aşağıdaki diyagrama benzer bir iş akışı oluşturmak için.
>
> ![Örnek CI/CD diyagramı](media/common/ci-cd-complex.png)

Genel URL açılır ve web uygulamasına gider herkes çalıştığı varsayılan kullanarak her iki hizmet aracılığıyla yazdığınız kod yolu çağıracağı _geliştirme_ alanı. Artık geliştirmeye devam etmek istediğinizi düşünelim *mywebapi* -nasıl, bunu ve geliştirme alanı kullanarak diğer geliştiriciler kesme değil mi? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-new-dev-space"></a>Yeni bir geliştirme alanı oluşturma
Visual Studio içinde, hizmetinizi F5 veya Ctrl+F5 tuşlarıyla yenilediğinizde kullanılacak olan ek alanları oluşturabilirsiniz. Alana istediğiniz adı verebilir ve ne anlama geldiği konusunda esnek davranabilirsiniz (ör. _sprint4_ veya _tanıtım_).

Yeni bir alan oluşturmak için aşağıdakileri yapın:
1. İle Visual Studio penceresine geçiş *mywebapi* proje.
2. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
3. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
4. Buradan, F5 veya Ctrl+F5 tuşlarına basarken kullanılacak olan küme ve/veya alanı değiştirebilir ya da oluşturabilirsiniz. *Daha önce oluşturduğunuz Azure Dev Space’in seçildiğinden emin olun*.
5. Alan açılır menüden seçin  **\<yeni alan oluştur... \>**.

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. İçinde **alanı Ekle** iletişim kutusunda, üst alan kümesine **geliştirme**ve yeni alanınız için bir ad girin. İş arkadaşlarınızın hangi alanda çalıştığınızı tanımlayabilmesi adına yeni alan için adınızı kullanabilirsiniz (örneğin, "scott"). **Tamam**'ı tıklatın.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Artık proje özellikleri sayfasında AKS kümenizi ve seçili yeni Alanı görmeniz gerekir.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>*mywebapi* için güncelleştirme kodu

1. İçinde *mywebapi* proje olun bir kod değişikliği için `string Get(int id)` dosyasındaki yöntemi `Controllers/ValuesController.cs` gibi:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Bu güncelleştirilmiş kod bloğunda bir kesme noktası ayarlayın (zaten önceden ayarladığınız bir kesme noktanız olabilir).
3. Başlatmak için F5'e basın _mywebapi_ hizmeti, hizmeti kümenizde seçili alanı kullanarak başlar. Seçili alanı bu durumda, _scott_.

Aşağıda, farklı alanların nasıl çalıştığını anlamanıza yardımcı olacak bir diyagram verilmiştir. Bir isteği aracılığıyla mor yolunu gösterir _geliştirme_ boşluk URL'sine eklenir, kullanılan varsayılan yolu alan. Bir isteği aracılığıyla pembe yolunu gösterir _dev/scott_ alanı.

![](media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her bir geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir ortamda uçtan uca test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi yayma üst bilgilerinin uygulama kodunuzda iletilmesini gerektirir.

### <a name="test-code-running-in-the-devscott-space"></a>Test çalıştırma kod _dev/scott_ alanı
Yeni sürümünüzü test etmek için *mywebapi* birlikte *webfrontend*, genel erişim noktası URL'sini tarayıcınızda *webfrontend* (örneğin, http://dev.webfrontend.123456abcdef.eus.azds.io)ve hakkında sayfasına gidin. "Hello from webfrontend and Hello from mywebapi" özgün iletisini görmelisiniz.

Şimdi de http://scott.s.dev.webfrontend.123456abcdef.eus.azds.io gibi bir şekilde olması için "scott.s." kısmını URL’ye ekleyin ve tarayıcıyı yenileyin. Ayarladığınız kesme noktası, *mywebapi* proje isabet. Devam etmek için F5’e tıkladığınızda tarayıcınızda yeni "Hello from webfrontend and mywebapi now says something new." iletisini artık görmeniz gerekir. Bunun nedeni, güncelleştirilmiş kodunuzdaki yolu *mywebapi* çalıştığı _dev/scott_ alanı.

Sonra bir _geliştirme_ her zaman en son değişikliklerinizi ve uygulamanızı varsayılarak içeren alan uzay tabanlı Bu öğretici bölümünde anlatıldığı gibi yönlendirme DevSpace'nın yararlanmak için tasarlanmıştır, Umarım kolayca görebilir olur nasıl geliştirme boşluklar önemli ölçüde daha büyük uygulama bağlamında yeni özellikleri test size yardımcı olabilir. Dağıtmak zorunda yerine _tüm_ hizmetler için özel alanınızı öğesinden türetilen özel bir alan oluşturabilirsiniz _geliştirme_ve "yedekleme" yalnızca gerçekten üzerinde çalıştığınız hizmetler. Bu, çalışan en son sürüme geri varsayarak bulabileceğinden kadar Hizmetleri özel alanınızı dışında yararlanarak rest geliştirme alanları yönlendirme altyapısını işleyecek _geliştirme_ alanı. Ve daha iyi yine de _birden çok_ geliştiriciler etkin olarak kullanılmak üzere farklı Hizmetleri aynı anda kendi alanı birbiriyle kesintiye uğratmadan.

### <a name="well-done"></a>Bravo!
Başlangıç kılavuzunu tamamladınız! Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
> * Kapsayıcılarda yinelemeli kod geliştirin.
> * İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
> * Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.
> * Temel bir kolayca daha büyük bir mikro hizmet uygulaması bağlamında izole edilmiş değişiklikleri test etmek için geliştirme alanları'nı kullanarak işlevler oluşturun

Artık Azure Dev Spaces'i öğrendiğinize göre, [Dev Space'inizi bir takım üyesiyle paylaşın](how-to/share-dev-spaces.md) ve onların birlikte işbirliği yapmasının ne kadar kolay olduğunu görmelerine yardımcı olun.

## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
