---
title: Konak yük dengeli Azure web apps bölgenin tepesindeki
description: Bölgenin tepesinde bir diğer ad kaydı yük dengeli web uygulamalarını barındırmak için Azure DNS kullanma
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 11/3/2018
ms.author: victorh
ms.openlocfilehash: b08eae072c2fbe420401424baf97a25b4cbbe87b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60790751"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Konak yük dengeli Azure web apps bölgenin tepesindeki

DNS protokolü, bir A veya AAAA kaydı bölgenin tepesindeki dışında herhangi bir şeyi atamayı engeller. Bir örnek bölge tepesinde contoso.com'dur. Bu kısıtlama, yük dengeli uygulamalarda arkasında Traffic Manager sahip uygulama sahipleri için bir sorunu gösterir. Traffic Manager profilinde bölgenin tepesinde kaydını işaret etmek mümkün değildir. Sonuç olarak, uygulama sahipleri, geçici bir çözüm kullanmanız gerekir. Uygulama katmanındaki bir yeniden yönlendirme başka bir etki bölge tepesinde yeniden yönlendirmeniz gerekir. Contoso.com yeniden yönlendirme için www örneğidir\.contoso.com. Bu düzenleme tek bir yeniden yönlendirme işlevine yönelik bir hata noktası sunar.

Diğer ad kayıtlarını ile bu sorun artık yok. Artık uygulama sahipleri kendi bölge tepesinde kayıt dış uç noktaları olan bir Traffic Manager profiline işaret edebilir. Uygulama sahibi kendi DNS bölgesi içinde başka bir etki alanı için kullanılan aynı Traffic Manager profilini işaret edebilir.

Örneğin, contoso.com ve www\.contoso.com aynı Traffic Manager profiline işaret edebilir. Traffic Manager profilini yapılandırılmış yalnızca harici son noktaları olduğu sürece bu durum geçerlidir.

Bu makalede, etki alanı apex için bir diğer ad kaydı oluşturun ve yapılandırın, web uygulamalarınız için Traffic Manager profili bitiş noktalarını öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Birlikte test edilecek Azure DNS içinde barındırabileceğiniz bir etki alanı adınızın olması gerekir. Bu etki alanı üzerinde tam denetime sahip olmanız gerekir. Tam denetim, etki alanı için ad sunucusu (NS) kayıtlarını ayarlama olanağını kapsar.

Etki alanınızı Azure DNS'de barındırmak yönergeler için bkz: [Öğreticisi: Etki alanınızı Azure DNS'de konak](dns-delegate-domain-azure-dns.md).

Bu öğreticide örnek olarak contoso.com etki alanı kullanılmaktadır ancak sizin kendi etki alanı adınızı kullanmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu makalede kullanılan tüm kaynakları tutmak için bir kaynak grubu oluşturun.

## <a name="create-app-service-plans"></a>App Service planları oluşturma

Yapılandırma bilgileri için aşağıdaki tabloyu kullanarak kaynak grubunuzda iki Web App Service planı oluşturun. Bir App Service planı oluşturma hakkında daha fazla bilgi için bkz. [bir Azure App Service planında yönetme](../app-service/app-service-plan-manage.md).


|Ad  |İşletim sistemi  |Location  |Fiyatlandırma Katmanı  |
|---------|---------|---------|---------|
|ASP-01     |Windows|Doğu ABD|Geliştirme ve Test D1 paylaşılan|
|ASP-02     |Windows|Orta ABD|Geliştirme ve Test D1 paylaşılan|

## <a name="create-app-services"></a>Uygulama hizmetleri oluşturma

Bir App Service planı her iki web uygulaması oluşturun.

1. Sol üst köşesinde Azure portal sayfasının üzerinde tıklayın **kaynak Oluştur**.
2. Tür **Web uygulaması** arama çubuğu ve Enter tuşuna basın.
3. Tıklayın **Web uygulaması**.
4. **Oluştur**’a tıklayın.
5. Varsayılanları kabul edin ve iki web uygulaması'nı yapılandırmak için aşağıdaki tabloyu kullanın:

   |Ad<br>(içinde benzersiz olmalıdır. azurewebsites.net)|Kaynak Grubu |App Service planı/konumu
   |---------|---------|---------|
   |App-01|Var olanı kullan<br>Kaynak grubunuzu seçin|ASP 01(East US)|
   |App-02|Var olanı kullan<br>Kaynak grubunuzu seçin|ASP 02(Central US)|

### <a name="gather-some-details"></a>Bazı ayrıntılar toplayın

Şimdi uygulamaları için IP adresi ve ana bilgisayar adını not edin gerekir.

1. Kaynak grubunuzu açın ve ilk uygulamanızı tıklayın (**uygulama-01** Bu örnekte).
2. Sol sütunda tıklayın **özellikleri**.
3. Adresi altında **URL**, altında **giden IP adresleri** listedeki ilk IP adresini not edin. Traffic Manager uç noktalarınızı yapılandırdığınızda, daha sonra bu bilgileri kullanır.
4. Yineleme için **uygulama-02**.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Kaynak grubunuzda bir Traffic Manager profili oluşturun. Varsayılan ayarları kullanın ve trafficmanager.net ad alanı içinde benzersiz bir ad yazın.

Traffic Manager profili oluşturma hakkında daha fazla bilgi için bkz: [hızlı başlangıç: Yüksek oranda kullanılabilir web uygulaması için bir Traffic Manager profili oluşturma](../traffic-manager/quickstart-create-traffic-manager-profile.md).

### <a name="create-endpoints"></a>Uç noktalar oluşturma

Artık iki web uygulaması uç noktaları oluşturabilirsiniz.

1. Kaynak grubunuzu açın ve Traffic Manager profilinize tıklayın.
2. Sol sütunda tıklayın **uç noktaları**.
3. **Ekle**'yi tıklatın.
4. Uç noktaları yapılandırmak için aşağıdaki tabloyu kullanın:

   |Tür  |Ad  |Hedef  |Location  |Özel üst bilgi ayarları|
   |---------|---------|---------|---------|---------|
   |Dış uç noktası     |Bitiş-01|App-01 için kayıtlı IP adresi|Doğu ABD|konak:\<uygulama-01 için kaydettiğiniz URL'si\><br>Örnek: **konak: uygulama-01.azurewebsites.net**|
   |Dış uç noktası     |Bitiş-02|App-02 için kayıtlı IP adresi|Orta ABD|konak:\<uygulama-02 kaydettiğiniz URL'si\><br>Örnek: **konak: uygulama-02.azurewebsites.net**

## <a name="create-dns-zone"></a>DNS bölgesi oluşturma

Test ya da mevcut bir DNS bölgesini kullanabilirsiniz veya yeni bir bölge oluşturabilirsiniz. Oluşturmak ve azure'da yeni bir DNS bölgesi temsilci seçmek için bkz: [Öğreticisi: Etki alanınızı Azure DNS'de konak](dns-delegate-domain-azure-dns.md).

### <a name="add-the-alias-record-set"></a>Diğer kayıt kümesi Ekle

DNS bölgenizi hazır olduğunda bir diğer ad kaydı için bölge tepesinde ekleyebilirsiniz.

1. Kaynak grubunuzu açın ve DNS bölgesine tıklayın.
2. **Kayıt kümesi**’ne tıklayın.
3. Aşağıdaki tabloda kullanılarak ayarlanan kaydı ekleyin:

   |Ad  |Tür  |Diğer kayıt kümesi  |Diğer ad türü  |Azure kaynak|
   |---------|---------|---------|---------|-----|
   |@     |A|Evet|Azure kaynak|Traffic Manager - profilinizi|

## <a name="add-custom-hostnames"></a>Özel ana bilgisayar adlarını ekleme

Özel bir ana bilgisayar adı için hem web uygulamaları ekleyin.

1. Kaynak grubunuzu açın ve ilk web uygulamanıza tıklayın.
2. Sol sütunda tıklayın **özel etki alanları**.
3. Tıklayın **konak adı Ekle**.
4. Ana bilgisayar adı altında etki alanı adınızı yazın. Örneğin, contoso.com.

   Etki alanınız geçemediğinden ve yanındaki yeşil onay işaretleri Göster **ana bilgisayar adı kullanılabilirliği** ve **etki alanı sahipliğini**.
5. Tıklayın **konak adı Ekle**.
6. Yeni ana bilgisayar adı altında görmek için **ana bilgisayar adları atanan site**, tarayıcınızı yenileyin. Yenileme sayfasında her zaman değişiklikleri hemen göstermez.
7. İkinci web uygulamanız için bu yordamı yineleyin.

## <a name="test-your-web-apps"></a>Web uygulamalarınızı test edin

Artık web uygulamanızı size ulaşabildiğimizden emin olmak için test edebilirsiniz ve yük yüklenmekte olan Dengeli.

1. Bir web tarayıcısı açın ve etki alanınızı göz atın. Örneğin, contoso.com. Varsayılan web uygulaması sayfası görmeniz gerekir.
2. İlk web uygulamanızı durdurun.
3. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
4. Web tarayıcınızı başlatın ve etki alanınıza gidin. Yine de, varsayılan web uygulaması sayfası görmeniz gerekir.
5. İkinci web uygulamanızı durdurun.
6. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
7. Web tarayıcınızı başlatın ve etki alanınıza gidin. Web uygulaması durdurulduğunu belirten hata 403 görmeniz gerekir.
8. İkinci web uygulamanızı başlatın.
9. Web tarayıcınızı kapatın ve birkaç dakika bekleyin.
10. Web tarayıcınızı başlatın ve etki alanınıza gidin. Varsayılan web uygulaması sayfası yeniden görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Öğretici: Bir Azure genel IP adresini belirtmek için bir diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Bir diğer ad kaydı Apex etki alanı adları ile Traffic Manager'ı destekleyecek şekilde yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
