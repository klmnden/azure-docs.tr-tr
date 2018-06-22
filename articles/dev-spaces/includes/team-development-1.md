---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 41418cb908f2bf149a3d0087728652b44cd6b19e
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825559"
---
Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırıyordunuz. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Paylaşılan bir geliştirme alanında veya gerektiğinde ayrı geliştirme alanlarında çalışarak bir geliştirici ekibinin aynı ortamda çalışmasını sağlayın.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız şu an için pek karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar.

Diğer düzinelerce hizmetle etkileşimde bulunan bir hizmet üzerinde çalıştığınızı düşünün.

- Geliştirme için her şeyi yerel olarak çalıştırmak pek mantıklı olmayabilir. Geliştirme makineniz uygulamanın tamamını çalıştıracak yeterli kaynaklara sahip olmayabilir. Ya da uygulamanız genel olarak erişilebilmesi gereken uç noktalara sahip olabilir (örneğin, uygulamanızın bir SaaS uygulamasındaki web kancasına yanıt vermesi durumunda).

- Yalnızca bağımlı olduğunuz hizmetleri çalıştırmayı deneyebilirsiniz, ancak bu durumda bağımlılıkların tam kapanışını bilmeniz gerekir (örneğin, bağımlılıkların bağımlılıkları). Ya da üzerinde çalışmadığınız için bağımlılıklarınızı oluşturup çalıştırmayı tam olarak bilmemenizle ilgili bir mesele olabilir.
- Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu yaklaşım bazen yardımcı olabilir, ancak bu sahteleri yönetmek için ayrı bir geliştirme çabası sarf etmek gerekebilir. Ayrıca bu yaklaşım, geliştirme alanınızın üretimden çok farklı görünmesine ve fark edilmeyen hataların ortaya çıkmasına yol açar.
- Bunun sonucunda da uçtan uca testin herhangi bir türünü yapmak zorlaşır. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

![](../media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

> [!Note]
> Devam etmeden önce, her iki hizmet için de tüm VS Code pencerelerini kapatın ve sonra hizmetin kök klasörlerinin her birinde `azds up -d` komutunu çalıştırın. (Bu bir Önizleme kısıtlamasıdır.)

Şimdi hizmetlerin çalışmakta olduğu yeri daha yakından inceleyelim. `azds list` komutunu çalıştırdığınızda aşağıdakine benzer bir çıktı görürsünüz:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     http://webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

Alan sütunu, her iki hizmetin de `default` adlı bir alanda çalıştığını gösterir. Genel URL’yi açıp web uygulamasına giden herkes, her iki hizmet üzerinden de çalışan, önceden yazmış olduğunuz kod yolunu çağırır. Şimdi `mywebapi` geliştirmeye devam istediğinizi varsayalım. Nasıl kod değişiklikleri yapabilir ve bunları test edebilir, öte yandan da geliştirme ortamını kullanan diğer geliştiricilerin işlemini kesintiye uğratmazsınız? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-space"></a>Alan oluşturma
Kendi `mywebapi` sürümünüzü `default` dışında bir alanda çalıştırmak için, aşağıdaki komutu kullanarak kendi alanınızı oluşturabilirsiniz:

``` 
azds space create --name scott
```

Yukarıdaki örnekte, iş arkadaşlarım bunun benim çalıştığım alan olduğunu anlayabilsin diye yeni alan için kendi adımı kullandım; ancak siz buna istediğiniz adı verebilir ve anlamı konusunda da esnek olabilirsiniz; örn. 'sprint4' veya 'demo.'

Geliştirme ortamındaki tüm alanların listesini görmek için `azds space list` komutunu çalıştırın. Şu anda seçili alanın yanında bir yıldız işareti (*) görüntülenir. Sizin durumunuzda, oluşturulduğu anda otomatik olarak 'scott' adlı alan seçildi. İstediğiniz zaman `azds space select` komutuyla başka bir alan seçebilirsiniz.
