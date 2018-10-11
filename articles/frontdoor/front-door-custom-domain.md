---
title: Öğretici - Azure Front Door yapılandırmanıza özel etki alanı ekleme | Microsoft Docs
description: Bu öğreticide Azure Front Door'a özel etki alanı eklemeyi öğreneceksiniz.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 8106c68397dea8d52c6d2daa2d09dfbc72c2a4c8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995068"
---
# <a name="tutorial-add-a-custom-domain-to-your-front-door"></a>Öğretici: Front Door örneğinize özel etki alanı ekleme
Bu öğreticide Front Door'a özel etki alanı ekleme adımları gösterilmektedir. Uygulama teslimi için Azure Front Door Service'i kullandığınızda, son kullanıcı isteğinde kendi etki alanı adınızın görünmesini istiyorsanız özel bir etki alanı kullanmanız gerekir. Görünür bir etki alanınızın olması, müşterileriniz için kolaylık sağlar ve markalama için faydalıdır.

Bir Front Door oluşturduğunuzda `azurefd.net` alt etki alanı olan varsayılan ön uç ana bilgisayar adı varsayılan olarak arka ucunuzdan Front Door içeriği teslimi için URL'ye eklenir (örneğin, https:\//contoso.azurefd.net/activeusers.htm). Size kolaylık olması için Azure Front Door, varsayılan ana bilgisayar adı özel etki alanı ile ilişkilendirme seçeneği sunar. Bu seçeneği kullanarak URL’nizde Front Door'a ait olan etki alanı adı yerine özel etki alanı ile içerik sunabilirsiniz (örneğin, https:\//www.contoso.com/photo.png). 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - CNAME DNS kaydı oluşturun.
> - Özel etki alanını Front Door'unuzla ilişkilendirin.
> - Özel etki alanını doğrulayın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımları tamamlayabilmeniz için öncelikle bir Front Door oluşturmanız gerekir. Daha fazla bilgi için bkz. [Hızlı başlangıç: Front Door oluşturma](quickstart-create-front-door.md).

Henüz özel bir etki alanınız yoksa ilk olarak bir etki alanı sağlayıcısından satın almanız gerekir. Örneğin bkz. [Özel etki alanı adı satın alma](https://docs.microsoft.com/azure/app-service/custom-dns-web-site-buydomains-web-app).

[DNS etki alanlarınızı](https://docs.microsoft.com/azure/dns/dns-overview) barındırmak için Azure kullanıyorsanız, etki alanı sağlayıcısının etki alanı adı sistemini (DNS) bir Azure DNS’e devretmeniz gerekir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns). Aksi takdirde, DNS etki alanınızı işlemek için bir etki alanı sağlayıcısı kullanıyorsanız [CNAME DNS kaydı oluşturma](#create-a-cname-dns-record) bölümüne geçin.


## <a name="create-a-cname-dns-record"></a>CNAME DNS kaydı oluşturma

Bir özel etki alanını Front Door'unuzla birlikte kullanabilmeniz için öncelikle etki alanı sağlayıcınızla Front Door'unuzun varsayılan ön uç ana bilgisayar adını (contoso.azurefd.net gibi) işaret eden bir kurallı ad (CNAME) kaydı oluşturmanız gerekir. CNAME kaydı, bir kaynak etki alanı adını hedef etki alanı adına eşleyen bir DNS kaydı türüdür. Azure Front Door Service için kaynak etki alanı adı, özel etki alanı adınızdır; hedef etki alanı adı ise Front Door varsayılan ana bilgisayar adınızdır. Front Door oluşturduğunuz CNAME kaydını doğruladıktan sonra kaynak özel etki alanına (www.contoso.com gibi) adreslenen trafik, belirtilen hedef Front Door varsayılan ön uç ana bilgisayar adına (contoso.azurefd.net gibi) yönlendirilir. 

Özel etki alanı ve alt etki alanı aynı anda yalnızca tek bir Front Door ile ilişkilendirilebilir. Ancak, birden fazla CNAME kaydı kullanarak farklı Front Door'lar için aynı özel etki alanından farklı alt etki alanları kullanabilirsiniz. Farklı alt etki alanlarına sahip özel bir etki alanını aynı Front Door'a da eşleyebilirsiniz.


## <a name="map-the-temporary-afdverify-sub-domain"></a>Geçici afdverify alt etki alanını eşleme

Üretim aşamasındaki var olan bir etki alanını eşlerken göz önünde bulundurmanız gereken özel durumlar vardır. Özel etki alanınızı Azure portalına kaydederken etki alanı için kısa bir kapalı kalma süresi yaşanabilir. Web trafiğinin kesintiye uğramaması için, ilk olarak özel etki alanınızı Azure afdverify alt etki alanı ile Front Door varsayılan ön uç ana bilgisayar adına eşleyerek geçici bir CNAME eşlemesi oluşturun. Bu yöntemle kullanıcılar, DNS eşlemesi gerçekleşirken kesinti olmadan etki alanınıza erişebilir.

Aksi takdirde, özel etki alanınızı ilk kez kullanıyorsanız ve üzerinde devam eden bir üretim trafiği yoksa, özel etki alanınızı doğrudan Front Door'unuza eşleyebilirsiniz. [Kalıcı özel etki alanını eşleme](#map-the-permanent-custom-domain) bölümüne geçin.

afdverify alt etki alanı ile bir CNAME kaydı oluşturmak için:

1. Özel etki alanınızın etki alanı sağlayıcısına ait web sitesinde oturum açın.

2. Sağlayıcının belgelerine başvurarak veya web sitesinin **Etki Alanı Adı**, **DNS** ya da **Ad sunucusu yönetimi** etiketli alanlarını arayarak DNS kayıtlarını yönetmeye ilişkin sayfayı bulun. 

3. Özel etki alanınız için bir CNAME kaydı girişi oluşturun ve alanları (alan adları değişebilir) aşağıdaki tabloda gösterildiği gibi tamamlayın:

    | Kaynak                    | Tür  | Hedef                     |
    |---------------------------|-------|---------------------------------|
    | afdverify.www.contoso.com | CNAME | afdverify.contoso.azurefd.net |

    - Kaynak: Özel etki alanı adınızı, afdverify alt etki alanıyla birlikte şu biçimde girin: afdverify._&lt;özel etki alanı adı&gt;_. Örneğin, afdverify.www.contoso.com.

    - Tür: *CNAME* yazın.

    - Hedef: Şu biçimde varsayılan Front Door ön uç ana bilgisayar adınızı girin ve afdverify alt etki alanını dahil edin: afdverify._&lt;uç nokta adı&gt;_.azurefd.net. Örneğin, afdverify.contoso.azurefd.net.

4. Yaptığınız değişiklikleri kaydedin.

Örneğin, GoDaddy etki alanı kayıt şirketi için yordam şu şekildedir:

1. Oturum açın ve kullanmak istediğiniz özel etki alanını seçin.

2. Etki Alanları bölümünde **Tümünü Yönet** seçeneğini belirleyip **DNS** | **Bölgeleri Yönet**’i seçin.

3. **Etki Alanı Adı** alanına özel etki alanınızı girin, ardından **Ara**’yı seçin.

4. **DNS Yönetimi** sayfasından **Ekle**’yi ve sonra **Tür** listesinden **CNAME** öğesini seçin.

5. CNAME girişi için aşağıdaki alanları doldurun:

    - Tür: *CNAME* seçeneğini işaretli bırakın.

    - Ana bilgisayar adı: afdverify alt etki alanı adı ile birlikte kullanmak istediğiniz özel etki alanınızın alt etki alanını girin. Örneğin, afdverify.www.

    - İşaret ettiği yer: Varsayılan Front Door ön uç ana bilgisayarınızın adını girin ve afdverify alt etki alanı adını da dahil edin. Örneğin, afdverify.contoso.azurefd.net. 

    - TTL: *1 Saat* seçeneğini işaretli bırakın.

6. **Kaydet**’i seçin.
 
    CNAME girişi DNS kayıtları tablosuna eklenir.


## <a name="associate-the-custom-domain-with-your-front-door"></a>Özel etki alanını Front Door'unuzla ilişkilendirme

Özel etki alanınızı kaydettikten sonra Front Door'unuza ekleyebilirsiniz.

1. [Azure portalda](https://portal.azure.com/) oturum açın ve bir özel etki alanına eşlemek istediğiniz ön uç ana bilgisayar adını içeren Front Door'a göz atın.
    
2. **Front Door tasarımcısı** sayfasında özel etki alanı eklemek için '+' işaretine tıklayın.
    
3. **Özel etki alanı**'nı seçin. 

4. **Ön uç ana bilgisayar adı** alanında CNAME kaydınızın hedef etki alanı olarak kullanılacak ön uç ana bilgisayar adı önceden doldurulmuş ve Front Door'unuzdan alınmıştır: *&lt;varsayılan ana bilgisayar adı&gt;*.azurefd.net. Bu değer değiştirilemez.

5. **Özel ana bilgisayar adı** için, CNAME kaydınızın kaynak etki alanı olarak kullanılacak alt etki alanı dahil özel etki alanınızı girin. Örneğin, www.contoso.com veya cdn.contoso.com. afdverify alt etki alanı adını kullanmayın.

6. **Add (Ekle)** seçeneğini belirleyin.

   Azure, girdiğiniz özel etki alanı adı için CNAME kaydının bulunduğunu doğrular. CNAME doğruysa, özel etki alanınız doğrulanır.

>[!WARNING]
> Front Door'unuzdaki her ön uç ana bilgisayar adı (özel etki alanları dahil olmak üzere) ile ilişkilendirilmiş varsayılan yola ('/\*') sahip yönlendirme kuralı olduğundan **emin olmanız** gerekir. Başka bir deyişle tüm yönlendirme kurallarınızın arasında ön uç ana bilgisayar adlarınızın her biri için varsayılan yolda ('/\*') tanımlanmış en az bir yönlendirme kuralı olmalıdır. Aksi takdirde son kullanıcı trafiği doğru şekilde yönlendirilmeyebilir.

## <a name="verify-the-custom-domain"></a>Özel etki alanını doğrulama

Özel etki alanınızın kaydını tamamladıktan sonra özel etki alanının varsayılan Front Door ön uç ana bilgisayar adınıza başvurduğunu doğrulayın.
 
Tarayıcınızda, özel etki alanını kullanarak dosyanın adresine gidin. Örneğin, özel etki alanınız robotics.contoso.com ise önbelleğe alınan dosyanın URL’si şu URL’ye benzer olmalıdır: http:\//robotics.contoso.com/my-public-container/my-file.jpg. Sonucun *&lt;Front Door ana bilgisayar adı&gt;*.azurefd.net adresinden Front Door'a doğrudan eriştiğinizde de aynı olduğundan emin olun.


## <a name="map-the-permanent-custom-domain"></a>Kalıcı özel etki alanını eşleme

afdverify alt etki alanının Front Door'unuza başarıyla eşlendiğini doğruladıysanız (veya üretim aşamasında olmayan yeni bir özel etki alanı kullanıyorsanız), özel etki alanını doğrudan varsayılan Front Door ön uç ana bilgisayar adına eşleyebilirsiniz.

Özel etki alanınıza yönelik bir CNAME kaydı oluşturmak için:

1. Özel etki alanınızın etki alanı sağlayıcısına ait web sitesinde oturum açın.

2. Sağlayıcının belgelerine başvurarak veya web sitesinin **Etki Alanı Adı**, **DNS** ya da **Ad Sunucusu Yönetimi** etiketli alanlarını arayarak DNS kayıtlarını yönetmeye ilişkin sayfayı bulun. 

3. Özel etki alanınız için bir CNAME kaydı girişi oluşturun ve alanları (alan adları değişebilir) aşağıdaki tabloda gösterildiği gibi tamamlayın:

    | Kaynak          | Tür  | Hedef           |
    |-----------------|-------|-----------------------|
    | www.contoso.com | CNAME | contoso.azurefd.net |

    - Kaynak: Özel etki alanınızın adını (örneğin, www.contoso.com) girin.

    - Tür: *CNAME* yazın.

    - Hedef: Varsayılan Front Door ön uç ana bilgisayar adını girin. Şu biçimde olmalıdır: _&lt;ana bilgisayar adı&gt;_.azurefd.net. Örneğin, contoso.azurefd.net.

4. Yaptığınız değişiklikleri kaydedin.

5. Daha önce geçici bir afdverify alt etki alanı CNAME kaydı oluşturduysanız silin. 

6. Bu özel etki alanını üretimde ilk kez kullanıyorsanız, [Özel etki alanını Front Door'unuzla ilişkilendirme](#associate-the-custom-domain-with-your-front-door) ve [Özel etki alanını doğrulama](#verify-the-custom-domain) adımlarını izleyin.

Örneğin, GoDaddy etki alanı kayıt şirketi için yordam şu şekildedir:

1. Oturum açın ve kullanmak istediğiniz özel etki alanını seçin.

2. Etki Alanları bölümünde **Tümünü Yönet** seçeneğini belirleyip **DNS** | **Bölgeleri Yönet**’i seçin.

3. **Etki Alanı Adı** alanına özel etki alanınızı girin, ardından **Ara**’yı seçin.

4. **DNS Yönetimi** sayfasından **Ekle**’yi ve sonra **Tür** listesinden **CNAME** öğesini seçin.

5. CNAME girişinin alanlarını doldurun:

    - Tür: *CNAME* seçeneğini işaretli bırakın.

    - Ana bilgisayar: Kullanılacak özel etki alanınızın alt etki alanını girin. Örneğin, www or profile.

    - Şuraya işaret eder: Front Door'unuzun varsayılan ana bilgisayar adını girin. Örneğin, contoso.azurefd.net. 

    - TTL: *1 Saat* seçeneğini işaretli bırakın.

6. **Kaydet**’i seçin.
 
    CNAME girişi DNS kayıtları tablosuna eklenir.

7. Bir afdverify CNAME kaydınız varsa yanındaki kalem simgesini ve sonra çöp kutusu simgesini seçin.

8. CNAME kaydını silmek için **Sil**’i seçin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Yukarıdaki adımlarda bir özel etki alanını bir Front Door'a eklediniz. Front Door'unuzu artık özel bir etki alanı ile ilişkilendirmek istemiyorsanız, aşağıdaki adımları uygulayarak özel etki alanını kaldırabilirsiniz:
 
1. Front Door tasarımcısında kaldırmak istediğiniz özel etki alanını seçin.

2. Özel etki alanı bağlam menüsünde Sil'e tıklayın.  

   Özel etki alanının uç noktanızla ilişkisi silinir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> - CNAME DNS kaydı oluşturun.
> - Özel etki alanını Front Door'unuzla ilişkilendirin.
> - Özel etki alanını doğrulayın.