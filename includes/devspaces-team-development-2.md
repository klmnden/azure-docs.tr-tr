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
ms.openlocfilehash: e0f768b876b49ec006ce98decf121d73d334b6d8
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439537"
---
### <a name="run-the-service"></a>Hizmeti çalıştırma

Hizmeti çalıştırmak için F5'e basın (ya da türü `azds up` Terminal penceresinde) hizmeti çalıştırmak için. Hizmet otomatik olarak yeni seçilen alanınızda çalışır _dev/scott_. Hizmetinizi kendi alanında çalıştırarak çalışır durumda olduğunu doğrulamak `azds list-up`:

```cmd
$ azds list-up

Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

Örneği fark *mywebapi* şu anda çalışıyor _dev/scott_ alanı. Çalıştırma sürümünü _geliştirme_ hala çalışıyor, ancak bu listede değil.

Çalıştırarak geçerli alanı URL'leri listesi `azds list-uris`.

```cmd
$ azds list-uris

Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```

Genel erişim noktası URL'si fark *webfrontend* ile önek *scott.s*. Bu URL için benzersiz _dev/scott_ alanı. Bu URL ön eki istekleri için giriş denetleyicisini söyler _dev/scott_ hizmet sürümü. Geliştirme boşluklarla bu URL ile bir istek işlendiğinde, giriş denetleyicisine ilk isteği yönlendirmek çalıştığında *webfrontend* hizmeti _dev/scott_ alanı. İçin başarısız olursa, istek iletilir *webfrontend* hizmeti _geliştirme_ alan bir geri dönüş olarak. Ayrıca Kubernetes kullanarak localhost hizmete erişmek için localhost URL'ye fark *bağlantı noktası iletme* işlevselliği. URL'ler ve Azure Dev boşluk yönlendirme hakkında daha fazla bilgi için bkz. [nasıl Azure geliştirme alanları çalışır ve olan yapılandırılmış](../articles/dev-spaces/how-dev-spaces-works.md).



![Alanı yönlendirme](../articles/dev-spaces/media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir alanda test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi uygulama kodunuzun yayma üst bilgilerini iletmesini gerektirir.

### <a name="test-code-in-a-space"></a>Alanda kodu test etme
Yeni sürümünüzü test etmek için *mywebapi* ile *webfrontend*, genel erişim noktası URL'sini tarayıcınızda *webfrontend* ve hakkında sayfasına gidin. Yeni iletinizin görüntülendiğini görmelisiniz.

Şimdi, URL'nin "scott.s." bölümünü kaldırın ve tarayıcıyı yenileyin. Eski davranışı görmeniz gerekir (ile *mywebapi* çalışan sürümü _geliştirme_).

Sonra bir _geliştirme_ boşluk, her zaman en son değişikliklerinizi içeren ve uygulamanızı tasarlanmıştır varsayılarak yönlendirme olarak DevSpace'nın uzay tabanlı avantajlarından yararlanmak için Bu öğretici bölümünde açıklanan, Umarım kolay olur bkz. nasıl geliştirme boşluklar önemli ölçüde daha büyük uygulama bağlamında yeni özellikleri test size yardımcı olabilir. Dağıtmak zorunda yerine _tüm_ hizmetler için özel alanınızı öğesinden türetilen özel bir alan oluşturabilirsiniz _geliştirme_ve "yedekleme" yalnızca gerçekten üzerinde çalıştığınız hizmetler. Bu, çalışan en son sürüme geri varsayarak bulabileceğinden kadar Hizmetleri özel alanınızı dışında yararlanarak rest geliştirme alanları yönlendirme altyapısını işleyecek _geliştirme_ alanı. Ve daha iyi yine de _birden çok_ geliştiriciler etkin olarak kullanılmak üzere farklı Hizmetleri aynı anda kendi alanı birbiriyle kesintiye uğratmadan.

### <a name="well-done"></a>Bravo!
Başlangıç kılavuzunu tamamladınız! Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
> * Kapsayıcılarda yinelemeli kod geliştirin.
> * İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
> * Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.
> * Temel bir kolayca daha büyük bir mikro hizmet uygulaması bağlamında izole edilmiş değişiklikleri test etmek için geliştirme alanları'nı kullanarak işlevler oluşturun

Azure geliştirme alanları incelediniz göre [geliştirme alanınızın bir ekip üyesiyle paylaşmak](../articles/dev-spaces/how-to/share-dev-spaces.md) ve İşbirliği yapmaya başlayın.

## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```