---
title: 'Azure hızlı başlangıç: Azure portalı kullanarak uygulamalarda yüksek kullanılabilirlik için bir Front Door profili oluşturma'
description: Bu hızlı başlangıç makalesinde yüksek oranda kullanılabilir ve yüksek performanslı global web uygulamanız için Front Door oluşturma adımları anlatılmaktadır.
services: front-door
documentationcenter: ''
author: sharad4u
editor: ''
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2018
ms.author: sharadag
ms.openlocfilehash: 6bcd5bcc2463ec1ab9dcc97644d5046c31bfc78b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461995"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application"></a>Hızlı Başlangıç: Bir ön kapısı yüksek oranda kullanılabilir bir küresel web uygulaması oluşturma

Bu hızlı başlangıçta global web uygulamanız için yüksek oranda kullanılabilirlik ve yüksek performans sunan bir Front Door profili oluşturma adımları anlatılmaktadır. 

Bu hızlı başlangıçta anlatılan senaryo, bir web uygulamasının farklı Azure bölgelerinde çalışan iki örneğini kapsamaktadır. Kullanıcı trafiğinin uygulamayı çalıştıran en yakın site arka ucu kümesine yönlendirilmesine yardımcı olmak için eşit [ağırlıklı ve aynı öncelik düzeyine sahip arka uçları](front-door-routing-methods.md) temel alan bir Front Door yapılandırması oluşturulmaktadır. Front Door, web uygulamasını sürekli izler ve en yakın site kullanım dışı olduğunda bir sonraki kullanılabilir arka uca otomatik yük devretme gerçekleştirir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 
[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıç için bir web uygulamasının farklı Azure bölgelerinde (*Doğu ABD* ve *Batı Avrupa*) çalışan iki örneğini dağıtmış olmanız gerekir. İki web uygulaması örneği de Etkin/Etkin modunda çalışıyor olmalıdır. Başka bir deyişle birinin yük devretme durumunda devreye girdiği Etkin/Beklemede yapılandırmasından farklı olarak ikisinin de her an trafik kabul eder durumda olması gerekir.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Web** > **Web Uygulaması** > **Oluştur**'u seçin.
2. **Web Uygulaması** sayfasında aşağıdaki bilgileri girdikten veya seçtikten sonra değer belirtilmeyen yerler için varsayılan ayarları kabul edin:

     | Ayar         | Değer     |
     | ---              | ---  |
     | Ad           | Web uygulamanız için benzersiz bir ad girin  |
     | Kaynak grubu          | **Yeni**'yi seçin ve *myResourceGroupFD1* yazın. |
     | Uygulama hizmeti planı/Konumu         | **Yeni**'yi seçin.  App Service planına *myAppServicePlanEastUS* yazın ve **Tamam**'ı seçin. 
     |      Location  |   Doğu ABD        |
    |||

3. **Oluştur**’u seçin.
4. Web Uygulaması başarıyla dağıtıldığında varsayılan bir web sitesi oluşturulur.
5. 1-3 arası adımları tekrarlayarak aşağıdaki ayarlarla farklı bir Azure bölgesinde ikinci bir web sitesi oluşturun:

     | Ayar         | Değer     |
     | ---              | ---  |
     | Ad           | Web Uygulamanız için benzersiz bir ad girin  |
     | Kaynak grubu          | **Yeni**'yi seçin ve *myResourceGroupFD2* yazın. |
     | Uygulama hizmeti planı/Konumu         | **Yeni**'yi seçin.  App Service planına *myAppServicePlanWestEurope* yazın ve **Tamam**'ı seçin. 
     |      Location  |   Batı Avrupa      |
    |||


## <a name="create-a-front-door-for-your-application"></a>Uygulamanız için Front Door oluşturma
### <a name="a-add-a-frontend-host-for-front-door"></a>A. Front Door için ön uç ana bilgisayar adı ekleme
Kullanıcı trafiğini iki arka uç arasındaki en düşük gecikme süresine göre yönlendiren bir Front Door yapılandırması oluşturun.

1. Ekranın sol üst kenarından **Kaynak oluştur** > **Ağ** > **Front Door** > **Oluştur**’u seçin.
2. **Front Door Oluştur** sayfasında temel bilgileri ekleyin ve Front Door'ın yapılandırılmasını istediğiniz aboneliği belirtin. Diğer Azure kaynaklarında olduğu gibi bir Kaynak Grubu ve yeni bir Kaynak Grubu oluşturuyorsanız bölge belirtmeniz gerekir. Son olarak Front Door'unuza bir ad vermeniz gerekir.
3. Temel bilgileri girdikten sonra tanımlamanız gerek ilk adım yapılandırmanın **ön uç ana bilgisayar adı** olacaktır. Sonucun şuna benzer geçerli bir etki alanı adı olması gerekir: `myappfrontend.azurefd.net`. Bu ana bilgisayar adının genel olarak benzersiz olması gerekir ancak doğrulama işlemi Front Door tarafından gerçekleştirilecektir. 

### <a name="b-add-application-backend-and-backend-pools"></a>B. Uygulama arka ucunu ve arka uç havuzlarını ekleme

Bir sonraki adımda Front Door'un uygulamanızın bulunduğu yeri bilmesi için uygulama arka uçlarınızı bir arka uç havuzu halinde yapılandırmanız gerekir. 

1. Arka uç eklemek için '+' simgesine tıklayın ve ardından arka uç havuzunuz için bir **ad** girin, örneğin: `myBackendPool`.
2. Ardından önceden oluşturduğunuz web sitelerinizi eklemek için Arka Uç Ekle'ye tıklayın.
3. **Hedef ana bilgisayar türü** için 'App Service' seçin, web sitesini oluşturduğunuz aboneliği seçin ve **Hedef ana bilgisayar adı** bölümünde ilk web sitesi olan *myAppServicePlanEastUS.azurewebsites.net* web sitesini seçin.
4. Kalan alanları şimdilik olduğu gibi bırakın ve **Ekle**'ye tıklayın.
5. 2-4 arasındaki adımları tekrarlayarak *myAppServicePlanWestEurope.azurewebsites.net* adlı diğer web sitesini ekleyin.
6. İsteğe bağlı olarak arka uç havuzu için Sistem Durumu Yoklamaları ve Yük Dengeleme ayarlarını güncelleştirebilirsiniz ancak varsayılan değerler de yeterli olacaktır. **Ekle**'ye tıklayın.


### <a name="c-add-a-routing-rule"></a>C. Yönlendirme kuralı ekleme
Son olarak Yönlendirme kuralları bölümünde '+' simgesine tıklayarak bir yönlendirme kuralı yapılandırın. Bu kural ön uç ana bilgisayarınızı arka uç havuzuyla eşlemek için gereklidir ve temelde `myappfrontend.azurefd.net` hedefine ulaşan bir isteği `myBackendPool` arka ucuna yönlendirme işlemidir. Front Door'unuza yönlendirme kuralı eklemek için **Ekle**'ye tıklayın. Artık Front Door'u oluşturmak için hazırsınız, **Gözden Geçir ve Oluştur**'a tıklayabilirsiniz.

>[!WARNING]
> Front Door'unuzdaki her ön uç ana bilgisayar adı ile ilişkilendirilmiş varsayılan yola ('/\*') sahip yönlendirme kuralı olduğundan **emin olmanız** gerekir. Başka bir deyişle tüm yönlendirme kurallarınızın arasında ön uç ana bilgisayar adlarınızın her biri için varsayılan yolda ('/\*') tanımlanmış en az bir yönlendirme kuralı olmalıdır. Aksi takdirde son kullanıcı trafiği doğru şekilde yönlendirilmeyebilir.

## <a name="view-front-door-in-action"></a>Front Door'u uygulamalı olarak görme
Front Door'u oluşturduğunuzda yapılandırmanın dünyanın her yerine dağıtılması birkaç dakika sürecektir. İşlem tamamlandığında oluşturduğunuz ana bilgisayar adına gidin. Bunun için bir web tarayıcısı açın ve `myappfrontend.azurefd.net` URL'sini yazın. İsteğiniz otomatik olarak arka uç havuzunda belirtilen arka uçlardan en yakın olana yönlendirilir. 

### <a name="view-front-door-handle-application-failover"></a>Front Door'un uygulama yük devretme adımlarını görüntüleme
Front Door'un anında genel yük devretme özelliğini uygulamalı olarak görmek istiyorsanız oluşturduğunuz web sitelerinden birini durdurabilirsiniz. Arka uç havuzunun Sistem Durumu Yoklama ayarlarına bağlı olarak trafik anında diğer web sitesi dağıtımına devredilir. Ayrıca bu davranışı test etmek için arka uç havuzu yapılandırmasında Front Door'unuzun arka ucunu da devre dışı bırakabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında kaynak grubunu, web uygulamalarını ve tüm ilgili kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta yüksek oranda kullanılabilirlik ve maksimum performans gerektiren web uygulamaları için kullanıcı trafiğini yönlendirmenizi sağlayan bir Front Door oluşturdunuz. Trafik yönlendirme hakkında daha fazla bilgi için Front Door tarafından kullanılan [Yönlendirme Yöntemlerini](front-door-routing-methods.md) inceleyin.