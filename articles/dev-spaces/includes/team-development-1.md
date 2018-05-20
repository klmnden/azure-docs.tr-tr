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
ms.openlocfilehash: 96a749c0cb59759e9294f52bd4f631d7fdc2275f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
Uygulama üzerinde çalışan tek Geliştirici değilmiş gibi şu ana kadar uygulamanızın kod çalışır durumda. Bu bölümde, nasıl Azure Dev alanları takım geliştirme kolaylaştırır öğreneceksiniz:
* Aynı geliştirme ortamında geliştiricilerin ekibi etkinleştirin.
* Her geliştirici kendi kodunda yalıtımı ve diğerleri parçalamak, korkusu olmadan yineleme destekler.
* Kod uçtan uca, kod tamamlama önce mocks oluşturmak veya bağımlılıkları benzetimini yapmak zorunda kalmadan sınayın.

## <a name="challenges-with-developing-microservices"></a>Mikro geliştirme ile zorlukları
Örnek uygulamanızı şu anda çok karmaşık değil. Ancak gerçek geliştirme zorluklar yakında daha fazla hizmet ekleyin ve geliştirme ekibi büyütmeleri olarak ortaya çıkan.

Kendinizi etkileşimde bulunan diğer hizmetler düzinelerce hizmetiyle çalışan görüntüleyin.

- Her şey geliştirme için yerel olarak çalıştırmak için belirtmeyi gerçekçi hale gelebilir. Geliştirme makinenizde tüm uygulamayı çalıştırmak için yeterli kaynağı olmayabilir. Veya, belki de uygulamanız genel olarak erişilebilir olması gereken uç noktaları (örneğin, uygulamanızı bir Web kancası için bir SaaS uygulamadan yanıt verir).

- Yalnızca üzerinde bağlı olan hizmetleri çalıştırmasına deneyin, ancak bu bağımlılıkları (örneğin, bağımlılıkları bağımlılıklarını) tam kapatması bilmeniz anlamına gelir. Veya, kolayca nasıl oluşturulacağı ve bunlar üzerinde işe yaramadı bağımlılıklarınızı çalıştırılamıyor bilmesinin bir konudur.
- Bazı geliştiriciler benzetimini yapma veya birçok Hizmet bağımlılıklarını, mocking çözümlemelere. Bu yaklaşım bazen yardımcı olabilir, ancak bu mocks yönetme kendi geliştirme çaba yakında alabilir. Ayrıca, bu yaklaşım, geliştirme ortamınızı üretime çok farklı arayan müşteri adayları ve zarif hatalar işi.
- Bu, uçtan uca test herhangi bir türünü yapmak zor izler. Tümleştirme sınaması sorunları daha sonra geliştirme döngüsü göreceğiniz anlamına gelir sonrası tamamlama yalnızca gerçekçi oluşabilir.

![](../media/common/microservices-challenges.png)


## <a name="work-in-a-shared-development-environment"></a>Paylaşılan geliştirme ortamında çalışma
Ayarlayabileceğiniz Azure Dev alanları ile bir *paylaşılan* Azure geliştirme ortamında. Her geliştirici yalnızca uygulamanın postalara odaklanabilirsiniz ve yinelemeli olarak geliştirebilirsiniz *önceden yürüttüğünüzde kodunuz* bir ortamda zaten tüm diğer hizmetler ve bunların senaryoları bağımlı bulut kaynakları içerir. Bağımlılıkları her zaman güncel ve geliştiriciler, üretim yansıtan bir şekilde çalışıyoruz.

## <a name="work-in-your-own-space"></a>Kendi alanında çalışır
Hizmetiniz için kod geliştirirken ve iade hazırsınız önce kodu genellikle iyi bir durumda olmayacaktır. Hala tekrarlayarak, test ve çözümlerle denemeler şekillendirme. Azure Dev alanları kavramı sağlar bir **alanı**, yalıtım ve ekip üyelerinizin sapması korkusu olmadan iş sağlar.

> [!Note]
> Devam etmek için her iki hizmet tüm VS Code pencerelerini kapatın ve ardından çalıştırın önce `azds up -d` her hizmetin kök klasör. (Bu bir önizleme kısıtlamadır.)

Burada Hizmetleri şu anda çalışan bir daha yakından bakalım. Çalıştırma `azds list` komut ve aşağıdakine benzer bir çıktı göreceksiniz:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     https://webfrontend-contosodev.1234abcdef.westeurope.aksapp.io
```

Her iki hizmet adlı bir alana çalıştığını alanı sütunda görüntülenir `default`. Ortak URL açar ve web uygulaması'na götürür herkes hem Hizmetleri aracılığıyla çalıştıran daha önce yazdığınız kod yolu çağırır. Şimdi geliştirmeye devam istediğinizi varsayalım `mywebapi`. Nasıl, kod değişiklikler yapabilir ve bunları test ve geliştirme ortamı kullanarak diğer geliştiriciler kesme değil mi? Bunu yapmak için kendi alan boşaltın ayarlarsınız.

## <a name="create-a-space"></a>Bir alanı oluşturma
Kendi sürümünü çalıştırmak için `mywebapi` dışındaki bir alana `default`, aşağıdaki komutu kullanarak kendi alan oluşturabilirsiniz:

``` 
azds space create --name scott
```

Yukarıdaki örnekte, böylece çalışıyorum içinde yer my eşleri için tanımlanabilen adımın yeni alan için kullandığınız, ancak herhangi bir şey gibi ve, 'sprint4' veya 'demo.' gibi anlamı hakkında esnek çağırabilirsiniz

Çalıştırma `azds space list` geliştirme ortamında tüm alanları listesini görmek için komutu. Bir yıldız işareti (*) şu anda seçili alanının yanında görüntülenir. Sizin durumunuzda, oluşturulduğunda 'tan' adlı alan otomatik olarak seçilir. İle herhangi bir zamanda başka bir alan seçin `azds space select` komutu.
