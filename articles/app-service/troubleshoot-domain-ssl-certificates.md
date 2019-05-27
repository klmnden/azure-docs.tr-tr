---
title: Etki alanı ve SSL sertifikaları - Azure App Service'e giderme | Microsoft Docs
description: Etki alanı ve Azure App Service SSL sertifikası sorunlarını giderme
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: c0584a69349c2785b5b6bce1d17c023c95b36151
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136189"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-app-service"></a>Etki alanı ve Azure App Service SSL sertifikası sorunlarını giderme

Bu makalede, Azure App Service'te web uygulamalarınız için bir etki alanı veya SSL sertifikası yapılandırdığınızda karşılaşabileceğiniz genel sorunları listeler. Ayrıca, bu sorunlar için olası nedenler ve çözümler açıklanmaktadır.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **destek al**.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="certificate-problems"></a>Sertifika sorunları

### <a name="you-cant-add-an-ssl-certificate-binding-to-an-app"></a>Bir uygulama için bir SSL sertifikası bağlaması eklenemiyor 

#### <a name="symptom"></a>Belirti

SSL bağlaması eklediğinizde, aşağıdaki hata iletisini alıyorsunuz:

"SSL bağlaması eklenemedi. Bu sertifikayı başka bir VIP kullandığından sertifika mevcut VIP için ayarlanamıyor."

#### <a name="cause"></a>Nedeni

Birden fazla uygulama arasında birden çok IP tabanlı SSL bağlamaları aynı IP adresi için varsa, bu sorun oluşabilir. Örneğin, bir uygulama eski bir sertifika ile bir IP tabanlı SSL vardır. Uygulama B aynı IP adresi için yeni bir sertifika ile bir IP tabanlı SSL sahiptir. Uygulama SSL bağlaması ile yeni sertifika güncelleştirdiğinizde, aynı IP adresi başka bir uygulama için kullanıldığından şu hatayla başarısız olur. 

#### <a name="solution"></a>Çözüm 

Bu sorunu gidermek için aşağıdaki yöntemlerden birini kullanın:

- IP tabanlı SSL bağlaması eski sertifikayı kullanan bir uygulamada silin. 
- Yeni sertifikayı kullanan yeni bir IP tabanlı SSL bağlaması oluşturun.

### <a name="you-cant-delete-a-certificate"></a>Sertifika silinemiyor 

#### <a name="symptom"></a>Belirti

Bir sertifika silmeye çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

"Şu anda bir SSL bağlamasında kullanıldığından sertifikayı silmek yüklenemiyor. Sertifikayı silmeden önce SSL bağlaması kaldırılmalıdır."

#### <a name="cause"></a>Nedeni

Sertifika başka bir uygulama kullanıyorsa, bu sorun oluşabilir.

#### <a name="solution"></a>Çözüm

Bu sertifika için SSL bağlaması uygulamalardan kaldırın. Sonra sertifikayı silmeyi deneyin. Sertifika hala silemiyorsanız, internet tarayıcınızın önbelleğini temizleyin ve Azure portalında yeni bir tarayıcı penceresi açın. Sonra sertifikayı silmeyi deneyin.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Bir App Service sertifikası satın alma 

#### <a name="symptom"></a>Belirti
Satın alamazsınız bir [Azure App Service sertifikası](./web-sites-purchase-ssl-web-site.md) Azure portalından.

#### <a name="cause-and-solution"></a>Nedeni ve çözümü
Bu sorun, aşağıdaki nedenlerle oluşabilir:

- App Service planı, ücretsiz veya paylaşılan ' dir. Bu fiyatlandırma katmanları SSL desteklemez. 

    **Çözüm**: Uygulama için App Service planı standart sürümüne yükseltebilir.

- Abonelik geçerli bir kredi kartı yok.

    **Çözüm**: Aboneliğiniz için geçerli bir kredi kartı ekleyin. 

- Abonelik teklifi, Microsoft Student gibi bir App Service sertifikası satın almayı desteklemiyor.  

    **Çözüm**: Aboneliğinizi yükseltin. 

- Abonelik satın alma işlemleri bir abonelikte izin verilen sınırına ulaştınız.

    **Çözüm**: App Service sertifikaları, Kullandıkça Öde ve EA abonelik türleri için 10 sertifika satın bir sınırı vardır. Diğer abonelik türleri için sınır 3'tür. Sınırı artırmak için kişi [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- App Service sertifikası sahtekarlık işaretlendi. Aşağıdaki hata iletisini aldığınız: "Sertifikanız olası dolandırıcılık için işaretlendi. İstek şu anda incelenmektedir. Azure desteğine başvurun. sertifika 24 saat içinde kullanılabilir hale gelmezse "

    **Çözüm**: Sertifika dolandırıcılık işaretlenir ve 24 saat sonra çözülmüş değildir, bu adımları izleyin:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Git **App Service sertifikaları**ve sertifikayı seçin.
    3. Seçin **sertifika yapılandırma** > **2. adım: Doğrulama** > **etki alanı doğrulaması**. Bu adım, sorunu çözmek için Azure sertifika sağlayıcısı bir e-posta bildirimi gönderir.

## <a name="custom-domain-problems"></a>Özel etki alanı sorunları

### <a name="a-custom-domain-returns-a-404-error"></a>Özel bir etki alanı bir 404 hatası döndürür. 

#### <a name="symptom"></a>Belirti

Özel etki alanı adını kullanarak siteye göz attığınızda, aşağıdaki hata iletisini alıyorsunuz:

"Hata 404-Web uygulaması bulunamadı."

#### <a name="cause-and-solution"></a>Nedeni ve çözümü

**Neden 1** 

Yapılandırdığınız özel bir etki alanı CNAME veya A kaydı eksik. 

**Çözüm nedeni 1 için**

- Bir A kaydı eklediyseniz, bir TXT kaydı da eklendiğinden emin olun. Daha fazla bilgi için [A kaydını oluşturun](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Uygulamanız için kök etki alanını kullanmak yoksa, bir A kaydı yerine CNAME kaydı kullanmanızı öneririz.
- Hem bir CNAME kaydı hem de bir A kaydı için aynı etki alanında kullanmayın. Bu sorun, çakışmaya neden ve çözülmüş etki alanı engelle. 

**Neden 2** 

Internet tarayıcısı hala etki alanınızın eski IP adresini önbelleğe alma. 

**Neden 2 çözümü**

Tarayıcı temizleyin. Windows cihazlar için komutu çalıştırabilirsiniz `ipconfig /flushdns`. Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınız için uygulamanın IP adresini işaret ettiğini doğrulayın. 

### <a name="you-cant-add-a-subdomain"></a>Bir alt etki alanı ekleyemezsiniz. 

#### <a name="symptom"></a>Belirti

Yeni bir ana bilgisayar adı bir alt etki alanı atamak için bir uygulamaya ekleyemezsiniz.

#### <a name="solution"></a>Çözüm

- Uygulamaya bir ana bilgisayar adı eklemek için izinlere sahip olduğunuzdan emin olmak için abonelik yöneticisine başvurun.
- Daha fazla alt etki alanlarını gerekiyorsa, Azure etki alanı adı hizmeti (DNS) etki alanı barındırma değiştirmenizi öneririz. Azure DNS kullanarak uygulamanıza 500 konak adları ekleyebilirsiniz. Daha fazla bilgi için [bir alt etki alanı ekleme](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).

### <a name="dns-cant-be-resolved"></a>DNS çözümlenemedi.

#### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini aldığınız:

"DNS kaydı bulunamadı."

#### <a name="cause"></a>Nedeni
Bu sorun, aşağıdaki nedenlerden biri için oluşur:

- Canlı (TTL) dönemi süresi doldu değil. TTL değeri belirleyin ve sonra geçmesi gereken süresi için bekleyin etki alanınız için DNS yapılandırmasını denetleyin.
- DNS yapılandırması doğru değil.

#### <a name="solution"></a>Çözüm
- Bu sorun kendiliğinden 48 saat bekleyin.
- DNS yapılandırmanızda TTL ayarını değiştirebilirsiniz, bu sorunu çözer olup olmadığını görmek için 5 dakika değerini değiştirin.
- Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınız için uygulamanın IP adresini işaret ettiğini doğrulayın. Seçili değilse uygulamanın doğru IP adresi için A kaydını yapılandırın.

### <a name="you-need-to-restore-a-deleted-domain"></a>Silinen bir etki alanı geri yüklemeniz gereken 

#### <a name="symptom"></a>Belirti
Etki alanınız artık Azure Portalı'nda görülebilir.

#### <a name="cause"></a>Nedeni 
Aboneliğin sahibi etki alanı yanlışlıkla silinmiş olabilir.

#### <a name="solution"></a>Çözüm
Etki alanınızı yedi günden kısa süre önce sildiyseniz etki alanını silme işlemi henüz başlatılmadı. Bu durumda, aynı etki alanında aynı abonelik altında Azure Portal'da yeniden satın alabilirsiniz. (Tam etki alanı adı arama kutusuna emin olun.) Bu etki alanı için yeniden ücret ödemezsiniz. Etki alanı yedi günden daha önce silinmişse, kişi [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) etki alanı geri yükleme konusunda Yardım.

## <a name="domain-problems"></a>Etki alanı sorunları

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>Yanlış etki alanı için SSL sertifikası satın

#### <a name="symptom"></a>Belirti

Yanlış etki alanı için bir App Service sertifikası satın. Doğru etki alanını kullanmak için sertifika güncelleştirilemiyor.

#### <a name="solution"></a>Çözüm

Bu sertifikayı silin ve ardından yeni bir sertifika satın alın.

Yanlış etki alanını kullanan geçerli sertifika "Verilen" durumda ise, bu sertifika için ayrıca faturalandırılırsınız. App Service sertifikaları edilemez değildir, ancak sizinle iletişime [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) diğer seçenekleri olup olmadığını görmek için. 

### <a name="an-app-service-certificate-was-renewed-but-the-app-shows-the-old-certificate"></a>Bir App Service sertifikası yenilendi, ancak uygulamanın eski sertifika gösterir 

#### <a name="symptom"></a>Belirti

App Service sertifikası yenilendi ancak App Service sertifikası kullanan bir uygulamayı hala eski sertifika kullanıyor. Ayrıca, HTTPS protokolünü gerekli olduğunu belirten bir uyarı alındı.

#### <a name="cause"></a>Nedeni 
Azure App Service, her sekiz saatte bir arka plan işi çalışır ve herhangi bir değişiklik varsa, sertifika kaynak eşitler. Bazen döndürme veya bir sertifikayı güncelleştirmek, uygulama hala eski sertifika ve yeni güncelleştirilmiş sertifika alıyor. Sertifika kaynak eşitleme işi henüz çalışmamışsa nedenidir. 
 
#### <a name="solution"></a>Çözüm

Sertifikanın bir eşitlemeye zorlayabilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **App Service sertifikaları**ve ardından bir sertifikayı seçin.
2. Seçin **yeniden anahtarla ve Eşitle**ve ardından **eşitleme**. Eşitleme tamamlanması biraz zaman alabilir. 
3. Eşitleme tamamlandığında, aşağıdaki bildirim görürsünüz: "Başarıyla tüm kaynaklar en son sertifika ile güncelleştirildi."

### <a name="domain-verification-is-not-working"></a>Etki alanı doğrulaması çalışmıyor 

#### <a name="symptom"></a>Belirti 
App Service sertifikası, sertifika kullanıma hazır hale gelmeden önce etki alanı doğrulaması gerektirir. Seçtiğinizde, **doğrulama**, işlem başarısız olur.

#### <a name="solution"></a>Çözüm
El ile bir TXT kaydı ekleyerek etki alanınızı doğrulayın:
 
1.  Etki alanı adınızı barındıran etki alanı adı hizmeti (DNS) sağlayıcısına gidin.
2.  Azure portalında gösterilen etki alanı belirteç değerini kullanan etki alanınız için bir TXT kaydı ekleyin. 

Çalıştırın ve ardından seçmek DNS yayma işlemi birkaç dakika bekleyin **Yenile** ve doğrulamayı tetiklemek için düğme. 

Alternatif olarak, el ile etki alanını doğrulamak için HTML Web sayfası yöntemi kullanabilirsiniz. Bu yöntem, sertifikanın verildiği etki alanının etki alanı sahipliğini doğrulamak sertifika yetkilisi sağlar.

1.  {Etki alanı doğrulama belirteci} .html adlı bir HTML dosyası oluşturun. Bu dosyanın içeriği, etki alanı Doğrulama belirtecinin değeri olmalıdır.
3.  Etki alanınızı barındıran web sunucusunun köküne bu dosyayı karşıya yükleyin.
4.  Seçin **Yenile** sertifika durumunu denetlemek için. Bu, doğrulama tamamlanması için birkaç dakika sürebilir.

Etki alanı doğrulama belirteci 1234abcd ile azure.com için standart bir sertifika satın alma, örneğin, bir web isteği yapılan https://azure.com/1234abcd.html 1234abcd döndürmelidir. 

> [!IMPORTANT]
> Bir sertifika siparişinin etki alanı doğrulama işlemini tamamlamak için yalnızca 15 gün vardır. 15 gün sonra sertifika yetkilisi, sertifika vermez ve sertifika için ücretlendirilmez. Bu durumda, bu sertifikayı silin ve yeniden deneyin.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Bir etki alanı satın alamazsınız

#### <a name="symptom"></a>Belirti
Azure portalında bir App Service etki alanı satın alamazsınız.

#### <a name="cause-and-solution"></a>Nedeni ve çözümü

Bu sorun, aşağıdaki nedenlerden biri için oluşur:

- Azure aboneliğinde herhangi bir kredi kartı yok veya kredi kartı geçersiz.

    **Çözüm**: Aboneliğiniz için geçerli bir kredi kartı ekleyin.

- Abonelik sahibi olmadığınız, böylece bir etki alanı satın almak için izniniz yok.

    **Çözüm**: [Sahip rolü atamak](../role-based-access-control/role-assignments-portal.md) hesabınıza. Veya bir etki alanı satın almak için izin almak için abonelik yöneticisine başvurun.
- Aboneliğinizde etki alanları satın almak için sınırına ulaştınız. Geçerli sınır 20'dir.

    **Çözüm**: Sınır için bir artış istemek için başvurun [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Azure abonelik türü, bir App Service etki alanı satın desteklemez.

    **Çözüm**: Bir Kullandıkça Öde aboneliği gibi başka bir abonelik türü için Azure aboneliğinizi yükseltin.

### <a name="you-cant-add-a-host-name-to-an-app"></a>Bir konak adı bir uygulamaya ekleyemezsiniz 

#### <a name="symptom"></a>Belirti

Bir ana bilgisayar adı eklediğinizde, doğrulamak ve etki alanını doğrulamak işlem başarısız olur.

#### <a name="cause"></a>Nedeni 

Bu sorun, aşağıdaki nedenlerden biri için oluşur:

- Bir ana bilgisayar adı eklemek için izniniz yok.

    **Çözüm**: Bir konak adı ekleme izni vermek için abonelik yöneticisine başvurun.
- Etki alanı sahipliğiniz doğrulanamadı.

    **Çözüm**: CNAME veya bir kayıt doğru şekilde yapılandırıldığını doğrulayın. Bir uygulamaya özel bir etki alanını eşlemek için bir CNAME kaydı ya da bir A kaydı oluşturun. Bir kök etki alanı kullanmak istiyorsanız, A ve TXT kayıtlarını kullanmanız gerekir:

    |Kayıt türü|Konak|Üzerine gelin|
    |------|------|-----|
    |A|@|Bir uygulama için IP adresi|
    |TXT|@|`<app-name>.azurewebsites.net`|
    |CNAME|www|`<app-name>.azurewebsites.net`|

## <a name="faq"></a>SSS

**My Web Sitem için'özel bir etki alanı satın almam sonra yapılandırmak zorunda mıyım?**

Azure portalından bir etki alanı satın aldığınızda, bu özel etki alanınızı kullanmak için App Service uygulama otomatik olarak yapılandırılır. Herhangi bir ek adımlar gerekmez. Daha fazla bilgi için izleme [Azure App Service kendi kendine yardım: Bir özel etki alanı adı ekleme](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name) channel9.

**Bunun yerine bir Azure VM'ye işaret edecek şekilde Azure Portalı'nda satın alınan bir etki alanı kullanabilir miyim?**

Evet, bir VM'ye etki alanını işaret edebilir. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](../dns/dns-custom-domain.md).

**Etki alanım GoDaddy veya Azure DNS tarafından barındırılıyor?**

App Service etki alanları, GoDaddy etki alanı kaydı ve Azure DNS etki alanlarınızı için kullanır. 

**Sahibim otomatik yenileme etkin ancak yine de bir e-posta yoluyla etki alanım için yenileme bildirimi alındı. Ne yapmalıyım?**

Otomatik yenileme varsa etkinleştirildiğinde, herhangi bir eylemde bulunmanız gerekmez. Bildirim e-posta, etki alanı dolmak ve, el ile yenilemek için otomatik olarak yenilenecek bildirmek için etkin sağlanmaz.

**Azure DNS için etki alanım barındırma ücret öder miyim?**

İlk etki alanı satın alma maliyetini yalnızca etki alanı kaydı için geçerlidir. Kayıt maliyete ek olarak, Azure DNS, kullanımınıza göre herhangi bir ücreti yoktur. Daha fazla bilgi için [Azure DNS fiyatlandırma](https://azure.microsoft.com/pricing/details/dns/) daha fazla ayrıntı için.

**Ben etki alanım daha önce Azure Portalı'ndan satın alınan ve Azure DNS barındırma GoDaddy barındırma geçmek istiyorsunuz. Bunu nasıl yapabilirim?**

Azure DNS barındırma geçirmek için zorunlu değildir. Azure DNS, Azure portalında etki alanı yönetim deneyimi hakkında geçirmek istiyorsanız bilgi taşımak Azure DNS gerekli adımları sağlar. Etki alanı App Service satın alınmış, görece sorunsuz yordamı GoDaddy barındırma için Azure DNS'yi geçişi olur.

**My etki alanından App Service etki alanı satın almak istiyorum ancak benim Azure DNS yerine GoDaddy etki alanında barındırmak?**

App Service etki alanları satın portalda 24 Temmuz 2017 tarihinden itibaren Azure DNS üzerinde barındırılır. Farklı bir barındırma sağlayıcı kullanmayı tercih ederseniz, bir etki alanı barındırma çözümü elde etmek için kendi Web sitesine gitmeniz gerekir.

**Etki alanım için gizlilik koruması için ödeme yapmam gerekir mi?**

Azure portalı üzerinden bir etki alanı satın aldığınızda, ek ücret ödemeden gizlilik eklemek seçebilirsiniz. Bu, Azure App Service aracılığıyla etki alanınız satın biridir.

**Ben artık etki alanım istemiyorum karar verirseniz, my para geri alabilirim?**

Bir etki alanı satın aldığınızda, bu süre içerisinde zaman etki alanı istemediğiniz karar verebilirsiniz beş gün boyunca ücretlendirilmez. Bu beş günlük süre içinde etki alanı istemediğinize karar verirseniz, ücretlendirilmez. (Bu konuda bir özel .uk etki alanlarıdır. ".Uk etki alanı satın alırsanız, hemen ücretlendirilir ve iade edilemez.)

**Aboneliğimde etki alanındaki başka bir Azure App Service uygulaması kullanabilir miyim?**

Evet. Özel etki alanları ve SSL dikey penceresinde Azure portalında erişim, satın aldığınız etki alanlarını görürsünüz. Bu etki alanları birini kullanacak şekilde yapılandırabilirsiniz.

**Başka bir abonelik için bir etki alanı bir abonelikten aktarabilirim?**

Başka bir abonelik/kaynak grubu için kullanılacak bir etki alanı taşıyabilirsiniz [taşıma AzResource](https://docs.microsoft.com/powershell/module/az.Resources/Move-azResource) PowerShell cmdlet'i.

**Şu anda bir Azure App Service uygulamasına sahip değilseniz özel etki alanım nasıl yönetebilirim?**

Bir App Service Web uygulaması olmasa bile, etki alanınız yönetebilirsiniz. Etki alanı, sanal makine, depolama vb. gibi Azure Hizmetleri için kullanılabilir. App Service Web Apps için etki alanı kullanmak istiyorsanız, etki alanı, web uygulamanıza bağlamak amacıyla ücretsiz App Service planı üzerinde değil bir Web uygulamasını dahil etmek gerekir.

**Özel bir etki alanı ile bir web uygulaması başka bir aboneliğe veya App Service ortamı v1'den V2'ye taşıyabilirim?**

Evet, web uygulamanızı abonelikler arasında taşıyabilirsiniz. Sunulan yönergeleri [kaynaklar Azure'da nasıl taşırım](../azure-resource-manager/resource-group-move-resources.md). Web uygulaması taşırken bazı sınırlamalar vardır. Daha fazla bilgi için [App Service kaynaklarını taşımak için sınırlamalar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations
).

Web uygulaması taşıdıktan sonra konak adı bağlamaları ayarlama özel etki alanları içindeki etki alanlarının aynı kalmalıdır. Konak adı bağlamaları yapılandırmak için ek adımlar gerekir.
