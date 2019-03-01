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
ms.openlocfilehash: 5d66dcaccc6ca2e40fbd516f535ec56c1baf6b17
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57195617"
---
### <a name="run-the-service"></a>Hizmeti çalıştırma

1. Hizmeti çalıştırmak için F5'e basın (veya Terminal Penceresine `azds up` yazın). Hizmet otomatik olarak yeni seçilen alanınızda çalışır _dev/scott_. 
1. Hizmetin kendi alanında çalıştırıldığını onaylamak için `azds list-up` komutunu yeniden çalıştırabilirsiniz. Bir örneğini göreceksiniz *mywebapi* şu anda çalışıyor _dev/scott_ alanı (çalışan sürümü _geliştirme_ hala çalışıyor, ancak bu listede değil).

    ```
    Name                      DevSpace  Type     Updated  Status
    mywebapi                  scott     Service  3m ago   Running
    mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
    webfrontend               dev       Service  26m ago  Running
    ```

1. Çalıştırma `azds list-uris`ve URL erişimi fark *webfrontend*.

    ```
    Uri                                                                        Status
    -------------------------------------------------------------------------  ---------
    http://localhost:53831 => mywebapi.scott:80                                Tunneled
    http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
    ```

1. URL ile kullanmak *scott.s* önek uygulamanıza gidin. Bu URL güncelleştirilen bildirim hala çözümler. Bu URL için benzersiz _dev/scott_ alanı. Özel URL Hizmetleri'nde ilk yol "tan" URL'ye istekleri dener gösterir _dev/scott_ , başarısız olursa, ancak alanı, bunlar geri döner hizmetlere _geliştirme_ alanı.

<!--
TODO: replace 2 & 3 with below once bug#753164 and PR#158827 get pushed to production.

You can confirm that your service is running in its own space by running `azds list-up` again. First, you'll notice an instance of *mywebapi* is now running in the _dev/scott_ space (the version running in _dev_ is still running but it is not listed). If you run `azds list-uris`, you will notice that the access point URL for *webfrontend* is prefixed with the text "scott.s.". This URL is unique to the _dev/scott_ space. The special URL signifies that requests sent to the "Scott URL" will try to first route to services in the _dev/scott_ space, but if that fails, they will fall back to services in the _dev_ space.

```
Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

```
Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```
-->

![](../articles/dev-spaces/media/common/space-routing.png)

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