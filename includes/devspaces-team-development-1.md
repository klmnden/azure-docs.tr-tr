---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: include
manager: yuvalm
ms.openlocfilehash: 40c1be20df845b975c023616e38cbb932c985735
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439538"
---
# <a name="team-development-with-azure-dev-spaces"></a>Azure Dev Spaces ile ekip geliştirmesi

Bu öğretici sayesinde nasıl bir geliştirici takımı ile geliştirme alanları kullanarak Kubernetes kümesinde aynı anda çalışabileceğiniz öğreneceksiniz.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme
Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırıyordunuz. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Bir geliştirici takımı ile bir paylaşılan geliştirme alanında veya ayrı geliştirme alanları gerektiği şekilde çalışarak aynı ortamda çalışmak için etkinleştirin.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız şu an için pek karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar. Geliştirme için her şeyi yerel olarak çalıştırmak pek mantıklı olmayabilir.

* Geliştirme makinenizde tek seferde ihtiyacınız olan her hizmeti çalıştırmak için yeterli kaynağı olmayabilir.
* Bazı hizmetler genel olarak erişilebilir olması gerekebilir. Örneğin, bir hizmet için bir Web kancası yanıt veren bir uç noktaya sahip gerekebilir.
* Hizmetlerin bir alt kümesini çalıştırmak istiyorsanız, tüm hizmetler arasında tam bağımlılık hiyerarşi bilmeniz gerekir. Bu zor olabilir belirleme, özellikle Hizmetleri sayısı artırın.
* Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu yaklaşım yardımcı olabilir, ancak bu mocks yönetme geliştirme maliyetini yakında etkileyebilir. Ayrıca, bu yaklaşım, katlamayı küçük hatalar sağlayan üretim çok farklı isteyen geliştirme ortamınızı yol açar.
* Bu, her tür tümleştirme testi yapılması zor izler. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](../articles/dev-spaces/media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

## <a name="use-dev-spaces-for-team-development"></a>Takım geliştirme için geliştirme alanları kullanın
Somut örneği kullanarak bu fikirleri gösterelim bizim *webfrontend* -> *mywebapi* örnek uygulama. Biz bir geliştirici, Scott, gereken yere bir değişiklik yapmak için bir senaryoyu düşünün *mywebapi* hizmeti ve *yalnızca* hizmet. *Webfrontend* Scott'ın güncelleştirmenin bir parçası değiştirmek zorunda kalmazsınız.

_Olmadan_ geliştirme alanları'nı kullanarak, Scott hiçbirinin ideal geliştirme ve test kendi güncelleştirme birkaç şekilde olabilir:
* TÜM bileşenler yerel olarak çalıştırın. Bu, Docker'ın yüklü ve MiniKube ile daha güçlü bir geliştirme makinesi gerektirir.
* TÜM bileşenleri ayrılmış bir ad alanında Kubernetes kümesinde çalışır. Bu yana *webfrontend* değil, bu küme kaynaklarının boşa harcanmasına değiştiriyor.
* YALNIZCA çalıştırma *mywebapi*ve test etmek için el ile REST çağrılarını yapın. Baştan sona tam akışı test etmek değildir.
* Kod geliştirme odaklı ekleyin *webfrontend* farklı bir örneğine istekleri göndermek geliştiricinin sağlayan *mywebapi*. Bu karmaşıklaştırır *webfrontend* hizmeti.

### <a name="set-up-your-baseline"></a>Temel ayarlayın
İlk biz hizmetlerimizin temel dağıtmanız gerekir. Bu dağıtım, yerel kodunuzu iade sürüm karşılaştırması davranışını kolayca karşılaştırabilmeniz "en son bilinen iyi" temsil eder. Ardından bu taban çizgisine göre yaptığımız değişiklikleri için test edebilmesi dayalı bir alt alanı oluşturacağız *mywebapi* daha büyük uygulama bağlamında.

1. Kopya [geliştirme alanları örnek uygulama](https://github.com/Azure/dev-spaces): `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Uzak dalı kullanıma alın *azds_updates*: `git checkout -b azds_updates origin/azds_updates`
1. Seçin _geliştirme_ alanı: `azds space select --name dev`. Bir üst geliştirme alanı seçmeniz istendiğinde seçin  _\<hiçbiri\>_.
1. Gidin _mywebapi_ dizin ve yürütün: `azds up -d`
1. Gidin _webfrontend_ dizin ve yürütün: `azds up -d`
1. Yürütme `azds list-uris` genel uç noktası için görmek için _webfrontend_

> [!TIP]
> Yukarıdaki adımları el ile bir temel ayarlamanız, ancak takımlar kullanım otomatik olarak temel önem koduyla güncel tutmak için CI/CD öneririz.
>
> Kullanıma sunduğumuz [Azure DevOps ile CI/CD ayarlama Kılavuzu](../articles/dev-spaces/how-to/setup-cicd.md) aşağıdaki diyagrama benzer bir iş akışı oluşturmak için.
>
> ![Örnek CI/CD diyagramı](../articles/dev-spaces/media/common/ci-cd-complex.png)

Bu noktada, temel çalıştırıyor olmalıdır. `azds list-up --all` komutunu çalıştırdığınızda aşağıdakine benzer bir çıktı görürsünüz:

```
$ azds list-up --all

Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      dev       Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      dev       Pod      3m ago   Running
webfrontend                   dev       Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  dev       Pod      1m ago   Running
```

Her iki hizmet adında boşluk çalıştığını DevSpace sütunu gösterir _geliştirme_. Genel URL açılır ve web uygulamasına gider herkes her iki hizmet çalıştıran iade kod yolu çağırır. Artık geliştirmeye devam etmek istediğinizi düşünelim _mywebapi_. Nasıl kod değişiklikleri yapabilir ve bunları test edebilir, öte yandan da geliştirme ortamını kullanan diğer geliştiricilerin işlemini kesintiye uğratmazsınız? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-dev-space"></a>Geliştirme alanı oluşturma
Kendi sürümünüzü çalıştırılacak _mywebapi_ dışındaki bir alana _geliştirme_, aşağıdaki komutu kullanarak kendi alan oluşturabilirsiniz:

```cmd
azds space select --name scott
```

Sorulduğunda, _geliştirme_ olarak **üst geliştirme alanı**. Sunduğumuz yeni bir alanı başka bir deyişle _dev/scott_, alanından derleyeceği _geliştirme_. Bu durumun test sırasında bize nasıl yardımcı olacağını birazdan göreceksiniz.

Bizim tanıtım kuramsal ile tutarak adını kullandık _scott_ yeni alan eşleri kimin içinde çalıştığını tanımlayabilmeniz için. Ancak olabilir herhangi bir şey gibi ve ne anlama geldiği hakkında esnek olarak adlandırılan, ister _sprint4_ veya _tanıtım_. Her durumda, _geliştirme_ bu uygulamanın bir parçası üzerinde çalışan tüm geliştiriciler için temel olarak görev yapar:

![](../articles/dev-spaces/media/common/ci-cd-space-setup.png)

Geliştirme ortamındaki tüm alanların listesini görmek için `azds space list` komutunu çalıştırın. _Seçili_ sütunu gösterir, boşluk (true/false) şu anda seçtiniz. Sizin durumunuzda alanını adlı _dev/scott_ oluşturulduğunda otomatik olarak seçilir. İstediğiniz zaman `azds space select` komutuyla başka bir alan seçebilirsiniz.

Şimdi nasıl çalıştığını görelim.