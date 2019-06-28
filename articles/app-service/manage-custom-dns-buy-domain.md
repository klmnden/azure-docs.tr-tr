---
title: App Service - azure'da özel etki alanı adı satın alın
description: Azure App Service'te bir web uygulaması ile bir özel etki alanı adı satın alma hakkında bilgi edinin.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 6bba176a27cc70321915654e3e2e62320f22c16c
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310135"
---
# <a name="buy-a-custom-domain-name-for-azure-app-service"></a>Azure App Service için bir özel etki alanı adı satın alma

App Service etki alanları doğrudan Azure'da yönetilen üst düzey etki alanları bulunur. Bunlar için özel etki alanlarını yönet daha kolay hale [Azure App Service](overview.md). Bu öğreticide, bir App Service etki alanı satın alma ve Azure App Service'e DNS adları atama işlemini göstermektedir.

Azure VM veya Azure depolama için bkz: [atama App Service etki alanı için Azure VM veya Azure depolama](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Bulut Hizmetleri için bkz: [Azure bulut hizmeti için bir özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Bir App Service uygulaması oluşturun](/azure/app-service/) veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın.
* [Aboneliğinizdeki harcama limitini Kaldır](../billing/billing-spending-limit.md#remove). App Service etki alanları ücretsiz abonelik KREDİLERİ satın alamazsınız.

## <a name="prepare-the-app"></a>Uygulamayı hazırlama

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Azure App Service'in, uygulamanızın özel etki alanlarında kullanılacak [App Service planı](https://azure.microsoft.com/pricing/details/app-service/) Ücretli katmanı olmalıdır (**paylaşılan**, **temel**, **standart**, veya  **Premium**). Bu adımda, uygulamayı desteklenen bir fiyatlandırma katmanında olduğundan emin olun.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com)'ı açın ve Azure hesabınızla oturum açın.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Azure Portal'da uygulamaya gitme

Sol menüden **Uygulama Hizmetleri**'ni ve ardından uygulamanın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/select-app.png)

App Service uygulamasının yönetim sayfasını görüyorsunuz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Uygulama sayfasının sol gezintisini **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Uygulamanın geçerli katmanı mavi kenarlıkla vurgulanmıştır. Uygulamanın **F1** katmanında olmadığından emin olun. **F1** katmanında özel DNS desteklenmez. 

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

App Service planı içinde değilse **F1** kapatın, katmanı **ölçeği** sayfasında ve atlamak [etki alanı satın alma](#buy-the-domain).

### <a name="scale-up-the-app-service-plan"></a>App Service planının ölçeğini artırma

Ücretsiz olmayan katmanlardan birini seçin (**D1**, **B1**, **B2**, **B3** veya **Üretim** kategorisindeki herhangi bir katmanı). Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

**Uygula**'ya tıklayın.

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

![Ölçeklendirme işlemi onayı](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Etki alanı satın alma

### <a name="pricing-information"></a>Fiyatlandırma bilgileri
Azure App Service etki alanları hakkında bilgi fiyatlandırma sayfasını ziyaret edin [App Service Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/app-service/windows/) ve App Service etki alanı kaydırın.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure Portal](https://portal.azure.com/)'ı açın ve Azure hesabınızla oturum açın.

### <a name="launch-buy-domains"></a>Satın alma etki alanları'nı başlatın
İçinde **uygulama hizmetleri** sekmesinde, uygulamanızı, select adına **ayarları**ve ardından **özel etki alanları**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

İçinde **özel etki alanları** sayfasında **etki alanı satın**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

> [!NOTE]
> Göremiyorsanız **App Service etki alanları** bölümünde, gerek Azure hesabınızdaki harcama limitini kaldırmak (bkz [önkoşulları](#prerequisites)).
>
>

### <a name="configure-the-domain-purchase"></a>Etki alanı satın alma yapılandırın

İçinde **App Service etki alanı** sayfasında **etki alanı Ara** satın alın ve yazmak için istediğiniz etki alanı adı yazın `Enter`. Önerilen kullanılabilir etki alanları yalnızca metin kutusunun altında gösterilir. Satın almak istediğiniz bir veya daha fazla etki alanı seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

> [!NOTE]
> Aşağıdaki [üst düzey etki alanlarının](https://wikipedia.org/wiki/Top-level_domain) App Service etki alanları tarafından desteklenir: _com_, _net_, _co.uk_, _kuruluş_, _nl_, _içinde_, _biz_, _org.uk_, ve _co.in_.
>
>

Tıklayın **irtibat bilgileri** ve etki alanı bilgilerini formu doldurun. İşiniz bittiğinde tıklayın **Tamam** App Service etki alanı sayfaya geri dönün.

Mümkün olduğunca fazla doğruluk ile tüm gerekli alanları doldurun önemlidir. İletişim bilgileri için yanlış veriler hata etki alanları satın almak için neden olabilir.

Ardından, etki alanınız için istenen seçenekleri belirleyin. Açıklamalar için aşağıdaki tabloya bakın:

| Ayar | Önerilen Değer | Açıklama |
|-|-|-|
|Gizlilik Koruması | Etkinleştirme | "Satın alma fiyatına dahil edilen gizlilik korumasını" kabul etmek _ücretsiz_. Bazı üst düzey etki alanlarında gizlilik korumasını desteklemeyen kaydedicilerin tarafından yönetilir ve üzerinde listelenen **Gizlilik Koruması** sayfası. |
| Varsayılan konak adları atayın | **www** ve **\@** | İstenen konak adı bağlamaları isterseniz seçin. Etki alanı satın alma işlemi tamamlandığında, seçili ana bilgisayar adları, uygulamanızı erişilebilir. Uygulama arkasında ise [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), kök etki alanının atama seçeneğini görmüyorum. (@), Traffic Manager desteği A kayıtlarını yaptığından. Etki alanı satın alma işlemini tamamladıktan sonra ana bilgisayar adı atamaları değişiklik yapabilirsiniz. |

### <a name="accept-terms-and-purchase"></a>Koşulları kabul edin ve satın alın

Tıklayın **yasal koşulları** ücretleri ve koşullarını gözden geçirin ve ardından **satın**.

> [!NOTE]
> App Service etki alanları, GoDaddy etki alanı kaydı ve Azure DNS etki alanlarınızı için kullanır. Etki alanı kayıt ücretin yanı sıra, Azure DNS için kullanım ücretleri uygulanır. Bilgi için [Azure DNS fiyatlandırma](https://azure.microsoft.com/pricing/details/dns/).
>
>

Geri **App Service etki alanı** sayfasında **Tamam**. İşlem devam ederken, aşağıdaki bildirimler görürsünüz:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>Test ana bilgisayar adları

Uygulamanıza varsayılan konak adları atadıysanız, seçilen her bir ana bilgisayar için bir başarı bildirimi de görebilirsiniz.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Seçili konak adı aynı zamanda gördüğünüz **özel etki alanları** sayfasında **özel ana bilgisayar adları** bölümü.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

> [!NOTE]
> A **değil güvenli** özel etki alanınızı henüz bir SSL sertifikasına bağlıdır ve herhangi bir tarayıcıdan HTTPS isteğini özel etki alanınız için bir hata veya tarayıcıya bağlı olarak bir uyarı alırsınız anlamına gelir için etiket. SSL bağlaması yapılandırmak için bkz [satın alma ve Azure App Service için SSL sertifikası yapılandırma](web-sites-purchase-ssl-web-site.md).
>

Ana bilgisayar adları test etmek için tarayıcıda listelenen ana bilgisayar adları gidin. Önceki ekran görüntüsünde örnekte giderek deneyin _kontoso.net_ ve _www\.kontoso.net_.

## <a name="assign-hostnames-to-app"></a>Uygulama için konak adları atayın

Satın alma işlemi sırasında bir veya daha fazla varsayılan konak adları uygulamanıza atamamayı seçerseniz veya listede olmayan bir ana bilgisayar adı atamak istiyorsanız, bir ana bilgisayar adı dilediğiniz zaman atayabilirsiniz.

App Service etki alanı konak adı için herhangi bir uygulama da atayabilirsiniz. Adımlar olup App Service etki alanı ve uygulama aynı aboneliğe ait üzerinde bağlıdır.

- Farklı bir abonelik: Harici olarak satın alınan bir etki alanı eşlemesi özel DNS kayıtları App Service etki alanı App ister. Özel DNS adları bir App Service etki alanı ekleme hakkında daha fazla bilgi için bkz: [özel DNS kayıtlarını yönetme](#custom). Bir uygulamaya dış satın alınan etki alanına eşlemek için bkz [mevcut bir özel DNS adını Azure App Service'e eşlemek](app-service-web-tutorial-custom-domain.md). 
- Aynı abonelik: Aşağıdaki adımları kullanın.

### <a name="launch-add-hostname"></a>Başlatma, konak adı Ekle
İçinde **uygulama hizmetleri** sayfasında, seçmek için ana bilgisayar adları atamak istediğiniz uygulamanızın adını seçin **ayarları**ve ardından **özel etki alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Satın alınan etki alanınızı listelendiğinden emin olun **App Service etki alanları** bölümünde, ancak seçmeyin. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Aynı Abonelikteki tüm App Service etki alanları uygulamanın gösterilen **özel etki alanları** sayfası. Etki alanınızı uygulamanın abonelikte, ancak uygulamanın göremediğinizi **özel etki alanları** sayfasında, yeniden açmayı deneyin **özel etki alanları** sayfası veya Web sayfasını yenileyin. Ayrıca, Azure portalının üst kısmındaki bildirim zil ilerleme veya oluşturma hatası için denetleyin.
>
>

**Konak adı ekle**'yi seçin.

### <a name="configure-hostname"></a>Konak adını yapılandırma
İçinde **konak adı Ekle** iletişim kutusunda, App Service etki alanı veya tüm alt etki alanının tam etki alanı adını yazın. Örneğin:

- kontoso.net
- www\.kontoso.net
- abc.kontoso.net

İşiniz bittiğinde seçin **doğrulama**. Konak adı kayıt türü sizin için otomatik olarak seçilir.

**Konak adı ekle**'yi seçin.

İşlem tamamlandığında, atanan konak adı için başarılı bildirim görürsünüz.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Kapat konak adı Ekle
İçinde **konak adı Ekle** sayfasında, uygulamanızı istediğiniz gibi başka bir konak adı atayın. İşiniz bittiğinde kapatın **konak adı Ekle** sayfası.

Yeni atanan hostname(s) uygulamanızın içinde görmelisiniz **özel etki alanları** sayfası.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>Test ana bilgisayar adları

Listelenen ana bilgisayar adları tarayıcıda gidin. Önceki ekran görüntüsünde örnekte giderek deneyin _abc.kontoso.net_.

## <a name="renew-the-domain"></a>Etki alanını Yenile

Satın aldığınız App Service etki alanı, satın aldığınız tarihten itibaren bir yıl boyunca geçerlidir. Varsayılan olarak, etki alanı otomatik olarak sonraki yıl için Ödeme yönteminizi kurumuna yenilemek için yapılandırılır. Etki alanı adınızı el ile yenileyebilirsiniz.

Otomatik yenilemeyi devre dışı bırakmak isterseniz veya etki alanınızda el ile yenilemek istiyorsanız, buradaki adımları izleyin.

İçinde **uygulama hizmetleri** sekmesinde, uygulamanızı, select adına **ayarları**ve ardından **özel etki alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

İçinde **App Service etki alanları** bölümünde, yapılandırmak istediğiniz etki alanını seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

Etki alanının sol gezinti bölmesinden seçin **etki alanı yenileme**. Etki alanınız otomatik olarak yenileme durdurmayı seçin **kapalı**, ardından **Kaydet**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-autorenew.png)

Etki alanınızı el ile yenilemek için seçin **yenileme etki alanı**. Ancak, bu düğmeyi kadar etkin değil [etki alanının süresi dolmadan önce 90 gün](#when-domain-expires).

Etki alanı yenileme başarılı olursa, 24 saat içinde bir e-posta bildirimi alırsınız.

## <a name="when-domain-expires"></a>Etki alanı dolduğunda

Azure, süresi dolan ile ilgilenen veya App Service etki alanları gibi süresi:

* Otomatik yenilemeyi devre dışı bırakılırsa: 90 gün önce etki alanı zaman aşımı, yenileme bildirim e-posta gönderilir ve **yenileme etki alanı** portalda düğmesi etkinleştirilir.
* Otomatik yenileme etkin olduğunda: Gün, etki alanı sona erme tarihinden sonra Azure için etki alanı adı yenileme faturalandırmak çalışır.
* Otomatik yenileme sırasında bir hata oluşursa (örneğin, dosya kartınızda süresi doldu) veya Otomatik yenilemeyi devre dışı bırakıldı ve süresi dolacak şekilde etki alanı izin verirseniz, Azure, etki alanı zaman aşımı ve park etki alanı adınızı size bildirir. Yapabilecekleriniz [el ile yenileme](#renew-the-domain) etki alanınız.
* 4 ile 12 gün günü dolduktan sonra Azure ek bildirim e-postaları gönderir. Yapabilecekleriniz [el ile yenileme](#renew-the-domain) etki alanınız.
* 19 gün dolduktan sonra etki alanınız beklemeye kalır, ancak bir kullanım ücreti tabi olur. Herhangi bir geçerli yenileme ve kullanım ücretleri, bir etki alanı adı yenilemek için müşteri desteği çağırabilirsiniz.
* 25 gün dolduktan sonra etki alanınız için bir etki alanı adı sektör artırma hizmeti olan Azure koyar. Herhangi bir geçerli yenileme ve kullanım ücretleri, bir etki alanı adı yenilemek için müşteri desteği çağırabilirsiniz.
* 30 günü dolduktan sonra artık etki alanınızı kullanabilir.

<a name="custom"></a>

## <a name="manage-custom-dns-records"></a>Özel DNS kayıtlarını yönetme

Azure'da, bir App Service etki alanı için DNS kayıtlarını kullanılarak yönetilir [Azure DNS](https://azure.microsoft.com/services/dns/). Ekleyebilir, kaldırabilir, ve DNS kayıtlarını güncelleştirmek, harici olarak satın alınan bir etki alanı için olduğu gibi.

### <a name="open-app-service-domain"></a>Açık App Service etki alanı

Azure portalında, sol menüden seçin **tüm hizmetleri** > **App Service etki alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Yönetilecek etki alanını seçin. 

### <a name="access-dns-zone"></a>Erişim DNS bölgesi

Etki alanının sol menüde **DNS bölgesi**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Bu eylem açar [DNS bölgesi](../dns/dns-zones-records.md) Azure DNS'de, App Service etki alanı sayfası. DNS kayıtlarını düzenleme hakkında daha fazla bilgi için bkz: [Azure portalda DNS bölgelerini yönetme](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Satın alma (Sil etki alanı) iptal et

App Service etki alanı satın aldıktan sonra tam bir para iadesi için satın alma işleminizi iptal etmek için beş gün vardır. Beş gün sonra App Service etki alanı silebilirsiniz ancak bir para iadesi alamazsınız.

### <a name="open-app-service-domain"></a>Açık App Service etki alanı

Azure portalında, sol menüden seçin **tüm hizmetleri** > **App Service etki alanları**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Etki alanına iptal etmek veya silmek istediğiniz seçin. 

### <a name="delete-hostname-bindings"></a>Konak adı bağlamaları Sil

Etki alanının sol menüde **konak adı bağlamaları**. Tüm Azure hizmetlerinden gelen konak adı bağlamaları burada listelenir.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

App Service etki alanı, tüm ana bilgisayar adı bağlamaları silinmeden silinemiyor.

Her ana bilgisayar adı bağlaması seçerek silme **...**   >  **Sil**. Tüm bağlamaları silindikten sonra seçin **Kaydet**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>İptal etme veya silme

Etki alanının sol menüde **genel bakış**. 

Satın alınan etki alanında iptal süresi geçen değil, seçin **satın alma iptal**. Aksi takdirde, gördüğünüz bir **Sil** yerine düğme. Para iadesi olmadan etki alanını silmek için seçin **Sil**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

İşlemi doğrulamak için şunu seçin **Evet**.

İşlem tamamlandıktan sonra etki alanı aboneliğinizde yayımlanmış ve herkesin yeniden satın almanız için kullanılabilir. 

## <a name="direct-default-url-to-a-custom-directory"></a>Varsayılan URL'yi özel bir dizine yönlendirme

Varsayılan olarak, App Service web isteklerini uygulama kodunuzun kök dizinine yönlendirir. Gibi bir alt dizinine yönlendirmek için `public`, bkz: [doğrudan varsayılan URL'yi özel bir dizine](app-service-web-tutorial-custom-domain.md#virtualdir).
