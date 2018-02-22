---
title: "Azure Web uygulamaları için özel etki alanı adı satın alma"
description: "Azure App Service'te bir web uygulaması ile özel etki alanı adı satın alma öğrenin."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: cephalin
ms.openlocfilehash: 152dbb6d47dfdf3bf5df945b823f64e58e7d91e2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Azure Web uygulamaları için özel etki alanı adı satın alma

Uygulama hizmeti etki alanları (Önizleme) doğrudan Azure içinde yönetilen en üst düzey etki alanlarıdır. Bunlar için özel etki alanlarını yönetmek kolay hale [Azure Web Apps](app-service-web-overview.md). Bu öğretici, bir uygulama hizmeti etki alanı satın alma ve Azure Web uygulamaları için DNS adları atamak nasıl gösterir.

Bu makalede Azure uygulama hizmeti (Web Apps, API Apps, Mobile Apps, Logic Apps) içindir. Azure VM veya Azure Storage için bkz: [atamak uygulama hizmeti etki alanı Azure VM veya Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Bulut Hizmetleri için bkz: [bir Azure bulut hizmeti için bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Bir App Service uygulaması oluşturun](/azure/app-service/) veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın.
* [Aboneliğiniz harcama sınırını kaldırmak](../billing/billing-spending-limit.md#remove). Uygulama hizmeti etki alanları ücretsiz abonelik KREDİLERİ ile satın alamıyor.

## <a name="prepare-the-app"></a>Uygulamayı hazırlama

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Azure Web uygulamaları, web uygulaması'nın özel etki alanlarında kullanılacak [uygulama hizmeti planı](https://azure.microsoft.com/pricing/details/app-service/) Ücretli katmanı olmalıdır (**paylaşılan**, **temel**, **standart**, veya **Premium**). Bu adımda, web uygulaması desteklenen fiyatlandırma katmanı olduğundan emin emin olun.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com)'ı açın ve Azure hesabınızla oturum açın.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Azure Portal'da uygulamaya gitme

Sol menüden **Uygulama Hizmetleri**'ni ve ardından uygulamanın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/select-app.png)

App Service uygulamasının yönetim sayfasını görüyorsunuz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Uygulama sayfasının sol gezintisini **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Uygulamanın geçerli katmanı mavi kenarlıkla vurgulanmıştır. Uygulamanın **Ücretsiz** katmanında olmadığından emin olun. **Ücretsiz** katmanında özel DNS desteklenmez. 

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Uygulama hizmeti planı değilse **serbest**, Kapat **fiyatlandırma katmanınızı seçin** sayfasında ve geçin [etki alanı satın alma](#buy-the-domain).

### <a name="scale-up-the-app-service-plan"></a>App Service planının ölçeğini artırma

Ücretsiz olmayan katmanlardan birini seçin (**Paylaşılan**, **Temel**, **Standart** veya **Premium**). 

**Seç**'e tıklayın.

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

![Ölçeklendirme işlemi onayı](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Etki alanı satın alma

### <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure Portal](https://portal.azure.com/)'ı açın ve Azure hesabınızla oturum açın.

### <a name="launch-buy-domains"></a>Satınalma etki alanları başlatma
İçinde **Web Apps** sekmesini seçin, web uygulamanızın adını tıklatın, **ayarları**ve ardından **özel etki alanları**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

İçinde **özel etki alanları** sayfasında, **etki alanı satın**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

> [!NOTE]
> Göremiyorsanız **uygulama hizmet alanları** bölümünde ihtiyacınız Azure hesabınızda harcama sınırını kaldırmak (bkz [Önkoşullar](#prerequisites)).
>
>

### <a name="configure-the-domain-purchase"></a>Etki alanı satın alma yapılandırın

İçinde **uygulama hizmeti etki alanı** sayfasında **etki alanı Ara** satın alın ve yazmak için istediğiniz etki alanı adını yazın `Enter`. Önerilen kullanılabilir etki alanlarının hemen altındaki metin kutusuna gösterilir. Satın almak istediğiniz bir veya daha fazla etki alanı seçin.
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

> [!NOTE]
> Aşağıdaki [üst düzey etki alanları](https://wikipedia.org/wiki/Top-level_domain) uygulama hizmeti etki alanı tarafından desteklenir: _com_, _net_, _co.uk_, _org_, _nl_, _içinde_, _biz_, _org.uk_, ve _co.in_.
>
>

Tıklatın **iletişim bilgileri** ve etki alanı bilgilerini formu doldurun. Tamamlandığında tıklatarak **Tamam** uygulama hizmeti etki alanı sayfaya geri dönün.
   
Mümkün olduğunca doğruluğu ile tüm gerekli alanları doldurun önemlidir. Hatalı veri iletişim bilgileri için etki alanı satın almak için başarısız olmasına neden olabilir. 

Ardından, etki alanınız için istediğiniz seçenekleri seçin. Açıklamalar için aşağıdaki tabloya bakın:

| Ayar | Önerilen Değer | Açıklama |
|-|-|-|
|Gizlilik koruması | Etkinleştirme | "Satın alma fiyatına dahil Gizlilik Koruması" kabul _ücretsiz_. Bazı üst düzey etki alanları gizlilik korumasını desteklemeyen kaydedicilerin tarafından yönetilir ve üzerinde listelenen **Gizlilik Koruması** sayfası. |
| Varsayılan konak adları atayın | **www** ve **@** | İstenen konak adı bağlamaları isterseniz seçin. Etki alanı satın alma işlemi tamamlandığında, web uygulamanızı sırasında seçilen ana bilgisayar adları erişilebilir. Web uygulamasının arkasında ise [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), kök etki alanının atama seçeneğiniz görmüyorum (@), çünkü destek A kayıtlarını trafik Yöneticisi yapar. Etki alanı satın alma işlemi tamamlandıktan sonra ana bilgisayar adı atamaları değişiklik yapabilirsiniz. |

### <a name="accept-terms-and-purchase"></a>Koşulları kabul etmek ve satın alma

Tıklatın **yasal koşulları** ve ücretleri koşullarını gözden geçirin ve ardından için **satın**.

> [!NOTE]
> Uygulama hizmeti etki alanı Azure DNS etki alanlarını barındırmak için kullanın. Etki alanı kayıt ücreti yanı sıra, kullanım ücretleri Azure DNS için geçerlidir. Bilgi için bkz: [Azure DNS fiyatlandırma](https://azure.microsoft.com/pricing/details/dns/).
>
>

Geri **uygulama hizmeti etki alanı** sayfasında, **Tamam**. İşlemi sürerken aşağıdaki bildirimler bakın:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>Ana bilgisayar adları test

Web uygulamanıza varsayılan ana bilgisayar adları atanmışsa seçilen her ana bilgisayar adı için bir başarı bildirimi de görebilirsiniz. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Seçilen konak adları Ayrıca bkz: **özel etki alanları** sayfasında **özel ana bilgisayar adları** bölümü. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

Ana bilgisayar adları sınamak için tarayıcıda listelenen ana bilgisayar adları gidin. Önceki ekran örnekte giderek deneyin _kontoso.net_ ve _www.kontoso.net_.

## <a name="assign-hostnames-to-web-app"></a>Web uygulaması için ana bilgisayar adları atama

Bir veya daha fazla varsayılan ana bilgisayar adları, web uygulamanızın satın alma işlemi sırasında atamama seçerseniz veya listede olmayan bir ana bilgisayar adı atamak gerekiyorsa, bir ana bilgisayar adı dilediğiniz zaman atayabilirsiniz.

Ayrıca, herhangi bir web uygulaması için uygulama hizmeti etki alanındaki ana bilgisayar adları atayabilirsiniz. Adımlar olup uygulama hizmeti etki alanı ve web uygulaması aynı aboneliğe ait üzerinde bağlıdır.

- Farklı bir abonelik: eşleme harici olarak satın alınan bir etki alanı gibi web uygulaması için uygulama hizmeti etki alanından özel DNS kaydı. Bir uygulama hizmeti etki alanına özel DNS adlarını ekleme hakkında daha fazla bilgi için bkz: [özel DNS kayıtlarını yönetme](#custom). Bir web uygulaması bir dış satın alınan etki alanını eşlemek için bkz: [Azure Web uygulamaları için var olan bir özel DNS ad eşleme](app-service-web-tutorial-custom-domain.md). 
- Aynı abonelik: aşağıdaki adımları kullanın.

### <a name="launch-add-hostname"></a>Başlatma ana bilgisayar adı ekleyin
İçinde **uygulama hizmetleri** sayfasında, web uygulamanızın seçmek için ana bilgisayar adları atamak istediğiniz adı seçin **ayarları**ve ardından **özel etki alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Satın alınan etki alanınızın içinde listelendiğinden emin olun **uygulama hizmet alanları** bölümünde, ancak bunu seçmeyin. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Aynı Abonelikteki tüm uygulama hizmeti etki alanları web uygulamanın gösterilen **özel etki alanları** sayfası. Web uygulamanızın abonelikte etki alanınızda olan ancak web uygulamanın göremiyorum **özel etki alanları** sayfasında, yeniden açmayı deneyin **özel etki alanları** sayfa veya Web sayfasını yenileyin. Ayrıca, Azure portalı üstündeki bildirim zil ilerleme veya oluşturma hatalarını denetleyin.
>
>

**Konak adı ekle**'yi seçin.

### <a name="configure-hostname"></a>Ana bilgisayar adı yapılandırma
İçinde **ana bilgisayar adını eklemek** iletişim kutusunda, uygulama hizmet etki alanınız veya herhangi bir alt etki alanı tam etki alanı adını yazın. Örneğin:

- kontoso.net
- www.kontoso.net
- abc.kontoso.net

Tamamlandığında, seçin **doğrulama**. Ana bilgisayar adı kayıt türü sizin için otomatik olarak seçilir.

**Konak adı ekle**'yi seçin.

İşlem tamamlandığında, atanan konak adı için bir başarı bildirim görür.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Ana bilgisayar adı Kapat ekleme
İçinde **ana bilgisayar adını eklemek** sayfasında, istediğiniz gibi web uygulamanız için başka bir konak adı atayın. Tamamlandığında, kapatmak **ana bilgisayar adını eklemek** sayfası.

Yeni atanan hostname(s), uygulamanızın içinde görmelisiniz **özel etki alanları** sayfası.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>Ana bilgisayar adları test

Tarayıcıda listelenen ana bilgisayar adları gidin. Önceki ekran örnekte giderek deneyin _abc.kontoso.net_.

## <a name="renew-the-domain"></a>Etki alanı yenileme

Satın aldığınız uygulama hizmeti etki alanı satın alma zamandan itibaren bir yıl için geçerlidir. Varsayılan olarak, etki alanı sonraki yıl için Ödeme yönteminizi şarj tarafından otomatik olarak yenilemek için yapılandırılır. Otomatik yenilemeyi devre dışı bırakmak isterseniz veya etki alanınızda el ile yenilemek istiyorsanız, burada adımları izleyin.

İçinde **Web Apps** sekmesini seçin, web uygulamanızın adını tıklatın, **ayarları**ve ardından **özel etki alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

İçinde **uygulama hizmet alanları** bölümünde, yapılandırmak istediğiniz etki alanını seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

Etki alanı sol gezinti bölmesinden seçin **etki alanı yenileme**. Etki alanınızı otomatik yenileme durdurmayı seçin **kapalı**ve ardından **kaydetmek**. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-autorenew.png)

Etki alanınızı el ile yenilemek için seçin **yenileme etki alanı**. Ancak, bu düğme etki alanının geçerliliği sona ermeden önce 90 güne kadar etkin değil.

<a name="custom"></a>

## <a name="manage-custom-dns-records"></a>Özel DNS kayıtlarını yönetme

Azure üzerinde bir uygulama hizmeti etki alanı için DNS kayıtlarını kullanılarak yönetilen [Azure DNS](https://azure.microsoft.com/services/dns/). Ekleyebilir, kaldırabilir, ve DNS kayıtlarını güncelleştirmek, harici olarak satın alınan bir etki alanı için olduğu gibi.

### <a name="open-app-service-domain"></a>Açık uygulama hizmeti etki alanı

Azure portalında, sol menüden seçin **tüm hizmetleri** > **uygulama hizmet alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Yönetmek için etki alanını seçin. 

### <a name="access-dns-zone"></a>Erişim DNS bölgesi

Etki alanının soldaki menüde seçin **DNS bölgesi**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Bu eylem açılır [DNS bölgesi](../dns/dns-zones-records.md) Azure DNS App Service etki alanınızda sayfası. DNS kayıtlarını düzenleme hakkında daha fazla bilgi için bkz: [Azure portalında DNS bölgelerini yönetme](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>İptal satın alma (delete etki alanı)

Uygulama hizmeti etki alanı satın aldıktan sonra tam iade için satın alma işleminizi iptal etmek için beş gün sahip. Beş gün sonra uygulama hizmeti etki silebilir ancak para iadesi alamazsınız.

### <a name="open-app-service-domain"></a>Açık uygulama hizmeti etki alanı

Azure portalında, sol menüden seçin **tüm hizmetleri** > **uygulama hizmet alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Etki alanına, iptal etmek veya silmek istediğiniz seçin. 

### <a name="delete-hostname-bindings"></a>Konak adı bağlamaları Sil

Etki alanının soldaki menüde seçin **konak adı bağlamaları**. Tüm Azure hizmetlerinden konak adı bağlamaları burada listelenir.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Tüm konak adı bağlamaları silinene kadar uygulama hizmeti etki alanı silinemiyor.

Her ana bilgisayar adı bağlama seçerek silme **...**   >  **Silmek**. Tüm bağlamaları silindikten sonra Seç **kaydetmek**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>İptal veya silme

Etki alanının soldaki menüde seçin **genel bakış**. 

Satın alınan etki alanında iptal süresi geçti değil, seçin **iptal satın alma**. Aksi takdirde, gördüğünüz bir **silmek** yerine düğme. Para iadesi olmadan etki alanına silmek için seçin **silmek**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

İşlemi onaylamak için seçin **Evet**.

İşlem tamamlandıktan sonra etki alanı aboneliğiniz yayımlanan herkes yeniden satın almak için kullanılabilir. 

## <a name="direct-default-url-to-a-custom-directory"></a>Varsayılan URL'yi özel bir dizine yönlendirme

Varsayılan olarak, App Service web isteklerini uygulama kodunuzun kök dizinine yönlendirir. Gibi bir alt dizine yönlendirmek için `public`, bkz: [doğrudan özel bir dizin için varsayılan URL](app-service-web-tutorial-custom-domain.md#virtualdir).

## <a name="more-resources"></a>Diğer kaynaklar

[Sık sorulan sorular: Uygulama hizmeti etki alanı (Önizleme) ve özel etki alanları](https://blogs.msdn.microsoft.com/appserviceteam/2017/08/08/faq-app-service-domain-preview-and-custom-domains/)