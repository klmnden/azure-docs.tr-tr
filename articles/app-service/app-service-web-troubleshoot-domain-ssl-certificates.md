---
title: Etki alanı ve Azure web uygulamalarında SSL sertifikası sorunlarını giderme | Microsoft Docs
description: Etki alanı ve Azure web uygulamalarında SSL sertifikası sorunlarını giderme
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
ms.date: 04/18/2018
ms.author: genli
ms.openlocfilehash: 59a9011edef49494288716ab16f30e28e440293b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Etki alanı ve Azure web uygulamalarında SSL sertifikası sorunlarını giderme

Bu makalede, Azure web uygulamaları için bir etki alanı veya SSL sertifikası yapılandırdığınızda karşılaşabileceğiniz ortak sorunlar listelenmiştir. Ayrıca bu sorunlar için olası nedenler ve çözümler açıklanmaktadır.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **destek alın**.

## <a name="certificate-problems"></a>Sertifika sorunları

### <a name="you-cant-add-an-ssl-certificate-binding-to-a-web-app"></a>Bir SSL sertifikası bağlaması bir web uygulaması'na eklenemiyor 

#### <a name="symptom"></a>Belirti

SSL bağlaması eklediğinizde, aşağıdaki hata iletisini alıyorsunuz:

"SSL bağlaması eklenemedi. Bu sertifikayı başka bir VIP tarafından kullanıldığından sertifika mevcut VIP için ayarlanamıyor."

#### <a name="cause"></a>Nedeni

Birden çok web uygulama arasında birden çok IP tabanlı SSL bağlamaları aynı IP adresi için varsa, bu sorun oluşabilir. Örneğin, bir IP tabanlı SSL eski bir sertifika ile web uygulaması A sahiptir. Web uygulaması B aynı IP adresi için yeni bir sertifika ile bir IP tabanlı SSL sahiptir. Web uygulaması SSL bağlaması ile yeni sertifika güncelleştirdiğinizde, aynı IP adresi başka bir uygulama için kullanıldığından bu hata ile başarısız olur. 

#### <a name="solution"></a>Çözüm 

Bu sorunu gidermek için aşağıdaki yöntemlerden birini kullanın:

- Eski sertifikayı kullanan web uygulaması üzerinde IP temelli SSL bağlama silin. 
- Yeni sertifika kullanan yeni bir IP tabanlı SSL bağlaması oluşturun.

### <a name="you-cant-delete-a-certificate"></a>Sertifika silinemiyor 

#### <a name="symptom"></a>Belirti

Bir sertifika silmeye çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

"Şu anda bir SSL bağlaması kullanıldığından sertifikayı silmek oluşturulamıyor. Sertifika silmeden önce SSL bağlaması kaldırılması gerekir."

#### <a name="cause"></a>Nedeni

Başka bir web uygulamasının sertifikası kullanıyorsa, bu sorun ortaya çıkabilir.

#### <a name="solution"></a>Çözüm

Bu sertifika için SSL bağlama web uygulamaları kaldırın. Sonra sertifikayı silmeyi deneyin. Sertifika hala silemiyorsanız Internet tarayıcısı önbelleğini temizlemek ve yeni bir tarayıcı penceresi Azure portalında yeniden açın. Sonra sertifikayı silmeyi deneyin.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Bir uygulama hizmeti sertifika satın alın 

#### <a name="symptom"></a>Belirti
Satın alamazsınız bir [Azure uygulama hizmeti sertifika](./web-sites-purchase-ssl-web-site.md) Azure portalından.

#### <a name="cause-and-solution"></a>Nedeni ve çözümü
Bu sorun aşağıdaki nedenlerden biriyle oluşabilir:

- Uygulama hizmeti planı ücretsiz veya paylaşılan ' dir. Bu fiyatlandırma katmanlarına SSL desteklemez. 

    **Çözüm**: standart web uygulaması için uygulama hizmeti planını yükseltin.

- Abonelik geçerli bir kredi kartı sahip değil.

    **Çözüm**: aboneliğiniz için geçerli bir kredi kartı ekleyin. 

- Abonelik teklif, Microsoft Student gibi bir uygulama hizmeti sertifikası satın desteklemiyor.  

    **Çözüm**: aboneliğinizi yükseltme. 

- Abonelik bir abonelikte izin verilen satınalma sınırına ulaşıldı.

    **Çözüm**: uygulama hizmeti sertifikaları Kullandıkça Öde ve EA abonelik türleri için 10 sertifika satın almalara sınırlaması vardır. Diğer abonelik türleri için izin verilen maksimum 3'tür. Sınırını artırmak için başvurun [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Uygulama hizmeti sertifika sahtekarlık işaretlendi. Aşağıdaki hata iletisini aldığınız: "sertifikanızı olası sahtekarlık için işaretlendi. İstek şu anda incelenmektedir. Sertifika 24 saat içinde kullanılabilir olmaz, lütfen Azure desteğine başvurun."

    **Çözüm**: sertifika sahtekarlık işaretlenir ve 24 saat sonra çözülmüş değil, şu adımları izleyin:

    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Git **uygulama hizmeti sertifikaları**ve sertifikayı seçin.
    3. Seçin **sertifika yapılandırma** > **2. adım: doğrulama** > **etki alanı doğrulama**. Bu adım sorunu çözmek için Azure sertifika sağlayıcısı için bir e-posta bildirimi gönderir.

## <a name="domain-problems"></a>Etki alanı sorunları

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>Yanlış etki alanı için bir SSL sertifikası satın

#### <a name="symptom"></a>Belirti

Yanlış etki alanı için bir uygulama hizmet sertifikası satın. Doğru etki alanını kullanmak üzere sertifika güncelleştirilemiyor.

#### <a name="solution"></a>Çözüm

Bu sertifikayı silin ve yeni bir sertifika satın alın.

Yanlış etki alanı kullanan geçerli sertifika "Verilen" durumda ise, bu sertifika için de faturalandırılırsınız. Uygulama Hizmeti sertifikaları iade değildir, ancak ile bağlantı kurabildiğini [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) diğer seçenekleri olup olmadığını görmek için. 

### <a name="an-app-service-certificate-was-renewed-but-the-web-app-shows-the-old-certificate"></a>Bir uygulama hizmeti sertifikası yenilendi, ancak eski sertifikayı web uygulaması gösterir 

#### <a name="symptom"></a>Belirti

Uygulama hizmeti sertifikası yenilendi, ancak uygulama hizmeti sertifikayı kullanan web uygulaması hala eski sertifika kullanıyor. Ayrıca, HTTPS protokolünü gerekli olduğunu belirten bir uyarı alındı.

#### <a name="cause"></a>Nedeni 
Azure App Service Web Apps özelliğini her sekiz saatte bir arka plan işi çalışır ve herhangi bir değişiklik olursa sertifika kaynak eşitlenir. Bazen döndürün veya bir sertifikayı güncelleştirmek, uygulama hala eski sertifika ve yeni güncelleştirilmiş sertifika alıyor. Sertifika kaynak eşitleme işi henüz Çalıştır kurmadı nedenidir. 
 
#### <a name="solution"></a>Çözüm

Sertifikanın bir eşitleme uygulayabilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **uygulama hizmeti sertifikaları**ve ardından sertifikayı seçin.
2. Seçin **anahtar yenileme ve eşitleme**ve ardından **eşitleme**. Eşitleme tamamlanması biraz zaman alır. 
3. Eşitleme tamamlandığında, aşağıdaki bildirim görürsünüz: "Başarıyla son sertifikayla tüm kaynakları güncelleştirildi."

### <a name="domain-verification-is-not-working"></a>Etki alanı doğrulama çalışmıyor 

#### <a name="symptom"></a>Belirti 
Sertifika kullanıma hazır hale gelmeden önce etki alanı doğrulama uygulama hizmeti sertifikası gerektirir. Seçtiğinizde, **doğrula**, işlem başarısız olur.

#### <a name="solution"></a>Çözüm
El ile bir TXT kaydı ekleyerek etki alanınızı doğrulayın:
 
1.  Etki alanı adınızı barındıran etki alanı adı hizmeti (DNS) Sağlayıcısı'na gidin.
2.  Azure portalında gösterilen etki alanı belirtecin değerini kullanır, etki alanı için bir TXT kaydı ekleyin. 

Çalıştırmak ve daha sonra seçmek DNS yayma için birkaç dakika bekleyin **yenileme** doğrulama tetiklemek için düğmesi. 

Alternatif olarak, etki alanınızın el ile doğrulamak için HTML Web sayfası yöntemi kullanabilirsiniz. Bu yöntem için sertifika etki alanının etki alanı sahipliğini doğrulamanız sertifika yetkilisi sağlar.

1.  {Etki alanı doğrulama belirteci} .html adlı bir HTML dosyası oluşturun. Bu dosyanın içeriğini etki alanı doğrulama belirteci değeri olmalıdır.
3.  Etki alanınızı barındıran web sunucusunun kökünde bu dosyayı karşıya yükleyin.
4.  Seçin **yenileme** sertifika durumunu denetlemek için. Doğrulama tamamlanması için birkaç dakika sürebilir.

Etki alanı doğrulama belirteci 1234abcd ile azure.com için standart bir sertifika satın alma, örneğin, bir web isteği yapılan http://azure.com/1234abcd.html 1234abcd döndürmelidir. 

> [!IMPORTANT]
> Etki alanı doğrulama işlemini tamamlamak için yalnızca 15 gün bir sertifika gerekir. 15 gün sonra sertifikanın sertifika yetkilisi engeller ve sertifika için sizden ücret istenmese. Bu durumda, bu sertifikayı silin ve yeniden deneyin.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Bir etki alanı satın alamazsınız

#### <a name="symptom"></a>Belirti
Azure portalında Web uygulamaları veya uygulama hizmeti etki alanındaki bir etki alanı satın alamazsınız.

#### <a name="cause-and-solution"></a>Nedeni ve çözümü

Bu sorun aşağıdaki nedenlerden birinden dolayı oluşur:

- Azure abonelikte kredi kartı yoktur veya kredi kartı geçersiz.

    **Çözüm**: aboneliğiniz için geçerli bir kredi kartı ekleyin.

- Abonelik sahibi değilseniz, böylece bir etki alanı satın almak için izniniz yok.

    **Çözüm**: [sahip rolünü eklemek](../billing/billing-add-change-azure-subscription-administrator.md) hesabınıza. Veya bir etki alanı satın izni almak için aboneliği yöneticisine başvurun.
- Aboneliğinizi etki alanlarında satın sınırına ulaştınız. Geçerli sınırı 20'dir.

    **Çözüm**: sınırı artırma isteğinde bulunmak için başvurun [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Azure Aboneliğinize bir uygulama hizmeti etki alanı satın desteklemez.

    **Çözüm**: Azure aboneliğinizi Kullandıkça Öde aboneliğine gibi başka bir abonelik türü yükseltin.

### <a name="you-cant-add-a-host-name-to-a-web-app"></a>Bir web uygulaması için bir ana bilgisayar adı eklenemez 

#### <a name="symptom"></a>Belirti

Bir ana bilgisayar adı eklediğinizde, doğrulamak ve etki alanını doğrulayın ve işlemi başarısız.

#### <a name="cause"></a>Nedeni 

Bu sorun aşağıdaki nedenlerden birinden dolayı oluşur:

- Bir ana bilgisayar adını eklemek için izniniz yok.

    **Çözüm**: ana bilgisayar adını eklemek için izin vermek için Abonelik Yöneticisi isteyin.
- Etki alanı sahipliğini doğrulanamadı.

    **Çözüm**:, CNAME veya bir kayıt doğru şekilde yapılandırıldığını doğrulayın. Web uygulaması için özel bir etki alanı eşlemek için bir CNAME kaydı veya bir A kaydını oluşturun. Bir kök etki alanı kullanmak isterseniz, A ve TXT kayıtlarını kullanmanız gerekir:

    |Kayıt türü|Konak|İşaret|
    |------|------|-----|
    |A|@|Bir web uygulaması için IP adresi|
    |TXT|@|< Uygulama adı >. azurewebsites.net|
    |CNAME|www|< Uygulama adı >. azurewebsites.net|

### <a name="dns-cant-be-resolved"></a>DNS çözümlenemedi.

#### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini aldığınız:

"DNS kaydı bulunamadı."

#### <a name="cause"></a>Nedeni
Bu sorun aşağıdaki nedenlerden birinden dolayı oluşur:

- Canlı (TTL) süresi süresi doldu değil. TTL değeri belirleyin ve ardından süresi dolmak üzere beklemek, etki alanınız için DNS yapılandırmasını denetleyin.
- DNS yapılandırması doğru değil.

#### <a name="solution"></a>Çözüm
- Bu sorun kendiliğinden için 48 saat bekleyin.
- Değer, DNS yapılandırmanızda TTL ayarı değiştirebilirsiniz varsa, bu sorunu çözer olup olmadığını görmek için 5 dakika olarak değiştirin.
- Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınızın web uygulamanızın IP adresine işaret doğrulanamadı. Seçili değilse, A kaydını web uygulamasının doğru IP adresi için yapılandırın.

### <a name="you-need-to-restore-a-deleted-domain"></a>Silinen bir etki alanı geri yüklemeniz gerekir 

#### <a name="symptom"></a>Belirti
Etki alanınızı artık Azure portalında görünür olur.

#### <a name="cause"></a>Nedeni 
Aboneliğin sahibi etki alanı yanlışlıkla silinmiş olabilir.

#### <a name="solution"></a>Çözüm
Etki alanınızı yedi günden önce silinmişse, etki alanı silme işlemi henüz başlatılmadı. Bu durumda, aynı etki alanında aynı abonelik altında Azure Portal'daki yeniden satın alabilirsiniz. (Arama kutusuna tam etki alanı adı yazdığınızdan emin olun.) Bu etki alanı için yeniden ücret olmaz. Etki alanı en az yedi gün önce silinmişse, kişi [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) etki alanı geri yüklemek Yardım.

### <a name="a-custom-domain-returns-a-404-error"></a>Özel bir etki alanı 404 hatası döndürür 

#### <a name="symptom"></a>Belirti

Özel etki alanı adını kullanarak siteye göz attığınızda, aşağıdaki hata iletisini alıyorsunuz:

"Hata 404 Web uygulaması bulunamadı."


#### <a name="cause-and-solution"></a>Nedeni ve çözümü

**Neden 1** 

Yapılandırdığınız özel etki alanı CNAME veya A kaydı eksik. 

**Neden 1 için çözüm**

- Bir A kaydı eklediyseniz, TXT kaydı ayrıca eklendiğinden emin olun. Daha fazla bilgi için bkz: [A kaydını oluşturun](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Web uygulamanız için kök etki alanı kullanmayı yoksa, bir A kaydı yerine bir CNAME kaydı kullanmanızı öneririz.
- Aynı etki alanı için bir CNAME kaydı ve bir A kaydı kullanmayın. Bu çakışmaya neden ve çözülmüş etki alanı engeller. 

**Neden 2** 

Internet tarayıcısı hala eski IP adresi etki alanınız için önbelleğe alma. 

**Neden 2 için çözüm**

Tarayıcı temizleyin. Windows cihazlar için komutu çalıştırabilirsiniz `ipconfig /flushdns`. Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınızın web uygulamanızın IP adresine işaret doğrulanamadı. 

### <a name="you-cant-add-a-subdomain"></a>Bir alt etki alanı eklenemez 

#### <a name="symptom"></a>Belirti

Yeni bir ana bilgisayar adı bir alt etki alanı atamak için bir web uygulaması ekleyemezsiniz.

#### <a name="solution"></a>Çözüm

- Web uygulaması için bir ana bilgisayar adını eklemek için izinlere sahip olduğunuzdan emin olmak için Abonelik Yöneticisi ile denetleyin.
- Daha fazla alt etki alanları ihtiyacınız varsa, etki alanını Azure DNS'ye barındıran değiştirmenizi öneririz. Azure DNS kullanarak, web uygulamanıza 500 ana bilgisayar adlarını ekleyebilirsiniz. Daha fazla bilgi için bkz: [bir alt etki alanı ekleme](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















