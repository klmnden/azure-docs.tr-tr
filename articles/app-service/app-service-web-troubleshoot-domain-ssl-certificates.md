---
title: Etki alanı ve Azure web apps'te SSL sertifikası sorunlarını giderme | Microsoft Docs
description: Etki alanı ve Azure web apps'te SSL sertifikası sorunları giderme
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
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 5c5bdb8fad60a2e4196c2c9f74764e27cec5ba62
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52970782"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Etki alanı ve Azure web apps'te SSL sertifikası sorunları giderme

Bu makalede, Azure web uygulamalarınız için bir etki alanı veya SSL sertifikası yapılandırdığınızda karşılaşabileceğiniz genel sorunları listeler. Ayrıca, bu sorunlar için olası nedenler ve çözümler açıklanmaktadır.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **destek al**.

## <a name="certificate-problems"></a>Sertifika sorunları

### <a name="you-cant-add-an-ssl-certificate-binding-to-a-web-app"></a>Bir SSL sertifikası bağlaması bir web uygulamasına eklenemiyor 

#### <a name="symptom"></a>Belirti

SSL bağlaması eklediğinizde, aşağıdaki hata iletisini alıyorsunuz:

"SSL bağlaması eklenemedi. Bu sertifikayı başka bir VIP kullandığından sertifika mevcut VIP için ayarlanamıyor."

#### <a name="cause"></a>Nedeni

Birden fazla web uygulama arasında birden çok IP tabanlı SSL bağlamaları aynı IP adresi için varsa, bu sorun oluşabilir. Örneğin, bir IP tabanlı SSL eski bir sertifika ile bir web uygulaması vardır. Web uygulaması B aynı IP adresi için yeni bir sertifika ile bir IP tabanlı SSL sahiptir. Web uygulaması SSL bağlaması ile yeni sertifika güncelleştirdiğinizde, aynı IP adresi başka bir uygulama için kullanıldığından şu hatayla başarısız olur. 

#### <a name="solution"></a>Çözüm 

Bu sorunu gidermek için aşağıdaki yöntemlerden birini kullanın:

- Eski sertifikayı kullanan web uygulaması üzerinde IP tabanlı SSL bağlaması silin. 
- Yeni sertifikayı kullanan yeni bir IP tabanlı SSL bağlaması oluşturun.

### <a name="you-cant-delete-a-certificate"></a>Sertifika silinemiyor 

#### <a name="symptom"></a>Belirti

Bir sertifika silmeye çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

"Şu anda bir SSL bağlamasında kullanıldığından sertifikayı silmek yüklenemiyor. Sertifikayı silmeden önce SSL bağlaması kaldırılmalıdır."

#### <a name="cause"></a>Nedeni

Başka bir web uygulaması sertifikası kullanması durumunda bu sorun oluşabilir.

#### <a name="solution"></a>Çözüm

SSL bağlaması için bu sertifika, web uygulamaları kaldırın. Sonra sertifikayı silmeyi deneyin. Sertifika hala silemiyorsanız, internet tarayıcınızın önbelleğini temizleyin ve Azure portalında yeni bir tarayıcı penceresi açın. Sonra sertifikayı silmeyi deneyin.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Bir App Service sertifikası satın alma 

#### <a name="symptom"></a>Belirti
Satın alamazsınız bir [Azure App Service sertifikası](./web-sites-purchase-ssl-web-site.md) Azure portalından.

#### <a name="cause-and-solution"></a>Nedeni ve çözümü
Bu sorun, aşağıdaki nedenlerle oluşabilir:

- App Service planı, ücretsiz veya paylaşılan ' dir. Bu fiyatlandırma katmanları SSL desteklemez. 

    **Çözüm**: web uygulaması için App Service planı standart sürümüne yükseltebilir.

- Abonelik geçerli bir kredi kartı yok.

    **Çözüm**: aboneliğiniz için geçerli bir kredi kartı ekleyin. 

- Abonelik teklifi, Microsoft Student gibi bir App Service sertifikası satın almayı desteklemiyor.  

    **Çözüm**: aboneliğinizi yükseltin. 

- Abonelik satın alma işlemleri bir abonelikte izin verilen sınırına ulaştınız.

    **Çözüm**: App Service sertifikaları, Kullandıkça Öde ve EA abonelik türleri için 10 sertifika satın sınırlaması vardır. Diğer abonelik türleri için sınır 3'tür. Sınırı artırmak için kişi [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- App Service sertifikası sahtekarlık işaretlendi. Aşağıdaki hata iletisi alındı: "sertifikanız olası dolandırıcılık için işaretlendi. İstek şu anda incelenmektedir. Sertifika 24 saat içinde kullanılabilir hale gelmezse, lütfen Azure desteğine başvurun."

    **Çözüm**: sertifika dolandırıcılık işaretlenir ve 24 saat sonra çözülmüş değildir, bu adımları izleyin:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Git **App Service sertifikaları**ve sertifikayı seçin.
    3. Seçin **sertifika yapılandırma** > **2. adım: doğrulama** > **etki alanı doğrulaması**. Bu adım, sorunu çözmek için Azure sertifika sağlayıcısı bir e-posta bildirimi gönderir.

## <a name="domain-problems"></a>Etki alanı sorunları

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>Yanlış etki alanı için SSL sertifikası satın

#### <a name="symptom"></a>Belirti

Yanlış etki alanı için bir App Service sertifikası satın. Doğru etki alanını kullanmak için sertifika güncelleştirilemiyor.

#### <a name="solution"></a>Çözüm

Bu sertifikayı silin ve ardından yeni bir sertifika satın alın.

Yanlış etki alanını kullanan geçerli sertifika "Verilen" durumda ise, bu sertifika için ayrıca faturalandırılırsınız. App Service sertifikaları edilemez değildir, ancak sizinle iletişime [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) diğer seçenekleri olup olmadığını görmek için. 

### <a name="an-app-service-certificate-was-renewed-but-the-web-app-shows-the-old-certificate"></a>Bir App Service sertifikası yenilendi, ancak eski sertifikayı web uygulamasını gösterir 

#### <a name="symptom"></a>Belirti

App Service sertifikası yenilendi ancak App Service sertifikasını kullanan web uygulaması hala eski sertifika kullanıyor. Ayrıca, HTTPS protokolünü gerekli olduğunu belirten bir uyarı alındı.

#### <a name="cause"></a>Nedeni 
Azure App Service'in Web Apps özelliği, bir arka plan işi her sekiz saatte bir çalışır ve herhangi bir değişiklik varsa, sertifika kaynak eşitler. Bazen döndürme veya bir sertifikayı güncelleştirmek, uygulama hala eski sertifika ve yeni güncelleştirilmiş sertifika alıyor. Sertifika kaynak eşitleme işi henüz çalışmamışsa nedenidir. 
 
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
Azure portalında Web Apps ve App Service etki alanındaki bir etki alanı satın alamazsınız.

#### <a name="cause-and-solution"></a>Nedeni ve çözümü

Bu sorun, aşağıdaki nedenlerden biri için oluşur:

- Azure aboneliğinde herhangi bir kredi kartı yok veya kredi kartı geçersiz.

    **Çözüm**: aboneliğiniz için geçerli bir kredi kartı ekleyin.

- Abonelik sahibi olmadığınız, böylece bir etki alanı satın almak için izniniz yok.

    **Çözüm**: [sahip rolü atamak](../role-based-access-control/role-assignments-portal.md) hesabınıza. Veya bir etki alanı satın almak için izin almak için abonelik yöneticisine başvurun.
- Aboneliğinizde etki alanları satın almak için sınırına ulaştınız. Geçerli sınır 20'dir.

    **Çözüm**: sınır için bir artış istemek için başvurun [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Azure abonelik türü, bir App Service etki alanı satın desteklemez.

    **Çözüm**: Azure aboneliğinizi Kullandıkça Öde aboneliği gibi başka bir abonelik türü için yükseltin.

### <a name="you-cant-add-a-host-name-to-a-web-app"></a>Bir web uygulaması için bir ana bilgisayar adı eklenemiyor 

#### <a name="symptom"></a>Belirti

Bir ana bilgisayar adı eklediğinizde, doğrulamak ve etki alanını doğrulamak işlem başarısız olur.

#### <a name="cause"></a>Nedeni 

Bu sorun, aşağıdaki nedenlerden biri için oluşur:

- Bir ana bilgisayar adı eklemek için izniniz yok.

    **Çözüm**: konak adı ekleme izni vermek için abonelik yöneticisine başvurun.
- Etki alanı sahipliğiniz doğrulanamadı.

    **Çözüm**:, CNAME veya bir kayıt doğru şekilde yapılandırıldığını doğrulayın. Web uygulamasına özel bir etki alanını eşlemek için bir CNAME kaydı ya da bir A kaydı oluşturun. Bir kök etki alanı kullanmak istiyorsanız, A ve TXT kayıtlarını kullanmanız gerekir:

    |Kayıt türü|Host|Üzerine gelin|
    |------|------|-----|
    |A|@|Bir web uygulaması için IP adresi|
    |TXT|@|< Uygulama adı >. azurewebsites.net|
    |CNAME|www|< Uygulama adı >. azurewebsites.net|

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
- Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınızda web uygulamanızın IP adresine işaret doğrulayın. Bu gereksinimleri karşılamıyorsa A kaydını web uygulamasının doğru IP adresi için yapılandırın.

### <a name="you-need-to-restore-a-deleted-domain"></a>Silinen bir etki alanı geri yüklemeniz gereken 

#### <a name="symptom"></a>Belirti
Etki alanınız artık Azure Portalı'nda görülebilir.

#### <a name="cause"></a>Nedeni 
Aboneliğin sahibi etki alanı yanlışlıkla silinmiş olabilir.

#### <a name="solution"></a>Çözüm
Etki alanınızı yedi günden kısa süre önce sildiyseniz etki alanını silme işlemi henüz başlatılmadı. Bu durumda, aynı etki alanında aynı abonelik altında Azure Portal'da yeniden satın alabilirsiniz. (Tam etki alanı adı arama kutusuna emin olun.) Bu etki alanı için yeniden ücret ödemezsiniz. Etki alanı yedi günden daha önce silinmişse, kişi [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) etki alanı geri yüklemek Yardım.

### <a name="a-custom-domain-returns-a-404-error"></a>Özel bir etki alanı bir 404 hatası döndürür. 

#### <a name="symptom"></a>Belirti

Özel etki alanı adını kullanarak siteye göz attığınızda, aşağıdaki hata iletisini alıyorsunuz:

"Hata 404-Web uygulaması bulunamadı."


#### <a name="cause-and-solution"></a>Nedeni ve çözümü

**Neden 1** 

Yapılandırdığınız özel bir etki alanı CNAME veya A kaydı eksik. 

**Çözüm nedeni 1 için**

- Bir A kaydı eklediyseniz, bir TXT kaydı da eklendiğinden emin olun. Daha fazla bilgi için [A kaydını oluşturun](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Web uygulamanız için kök etki alanını kullanmak yoksa, bir A kaydı yerine CNAME kaydı kullanmanızı öneririz.
- Hem bir CNAME kaydı hem de bir A kaydı için aynı etki alanında kullanmayın. Bu, çakışmaya neden ve çözülmüş etki alanı engelle. 

**Neden 2** 

Internet tarayıcısı hala etki alanınızın eski IP adresini önbelleğe alma. 

**Neden 2 çözümü**

Tarayıcı temizleyin. Windows cihazlar için komutu çalıştırabilirsiniz `ipconfig /flushdns`. Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınızda web uygulamanızın IP adresine işaret doğrulayın. 

### <a name="you-cant-add-a-subdomain"></a>Bir alt etki alanı ekleyemezsiniz. 

#### <a name="symptom"></a>Belirti

Bir alt etki alanı atamak için bir web uygulaması için yeni bir ana bilgisayar adı eklenemiyor.

#### <a name="solution"></a>Çözüm

- Bir ana bilgisayar adı web uygulamasına eklemek için izinlere sahip olduğunuzdan emin olmak için abonelik yöneticisine başvurun.
- Daha fazla alt etki alanlarını gerekiyorsa, etki alanı barındırma için Azure DNS'yi değiştirmenizi öneririz. Azure DNS kullanarak web uygulamanıza 500 konak adları ekleyebilirsiniz. Daha fazla bilgi için [bir alt etki alanı ekleme](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















