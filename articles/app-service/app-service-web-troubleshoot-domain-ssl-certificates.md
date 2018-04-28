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
ms.openlocfilehash: ba21475b771f1688c0a3f2f34c8d961fba5cd182
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Etki alanı ve Azure web uygulamalarında SSL sertifikası sorunlarını giderme

Bu makalede, Azure web uygulamaları için etki alanı veya SSL sertifikası yapılandırdığınızda karşılaşabileceğiniz ortak sorunlar listelenmiştir. Olası nedenler ve çözümler bu sorunları için de açıklanmaktadır.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve tıklayın **destek alın**.

## <a name="certificate-problems"></a>Sertifika sorunları

### <a name="unable-to-add-bind-ssl-certificate-to-a-web-app"></a>Bir Web uygulamasına bağlama SSL sertifika eklenemiyor 

### <a name="symptom"></a>Belirti

SSL bağlaması eklediğinizde, aşağıdaki hata iletisini alıyorsunuz:

**SSL bağlaması eklemek başarısız oldu. Bu sertifika zaten başka bir VIP tarafından kullanıldığından mevcut VIP için sertifika ayarlanamıyor.**

### <a name="cause"></a>Nedeni

Birden çok web uygulama arasında birden çok IP tabanlı SSL bağlamaları aynı IP adresi için varsa, bu sorun oluşabilir. Örneğin, web uygulaması bir IP tabanlı SSL eski sertifika ile sahiptir. IP tabanlı SSL ile aynı IP adresi için yeni sertifika ile Web uygulaması B. Web uygulaması SSL bağlaması ile yeni sertifika güncelleştirdiğinizde, başka bir uygulama için aynı IP adresi kullanıldığından bu hata ile başarısız olur. 

### <a name="solution"></a>Çözüm 

Bu sorunu gidermek için aşağıdaki yöntemlerden birini kullanın:

- Eski sertifikayı kullanan web uygulaması üzerinde IP temelli SSL bağlama silin. 
- Yeni sertifika kullanan yeni bir IP tabanlı SSL bağlaması oluşturun.

### <a name="unable-to-delete-a-certificate"></a>Sertifika silinemedi 

### <a name="symptom"></a>Belirti

Bir sertifika silmeye çalıştığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Sertifika şu anda bir SSL bağlaması kullanıldığından silinemedi. Sertifika silmeden önce SSL bağlaması kaldırılmalıdır.**

### <a name="cause"></a>Nedeni

Başka bir web uygulaması tarafından kullanılan sertifikanın, bu sorun ortaya çıkabilir.

### <a name="solution"></a>Çözüm

Bu sertifika için SSL bağlama web uygulamaları kaldırın. Sonra sertifikayı silmeyi deneyin. Sertifika hala silemiyorsanız Internet tarayıcısı önbelleğini temizlemek için Azure portalında yeni bir tarayıcı penceresi açın. Ve ardından sertifika silmeyi deneyin.

### <a name="unable-to-purchase-an-app-service-certificate"></a>Bir uygulama hizmeti sertifikası satın kurulamıyor 

### <a name="symptom"></a>Belirti
Satın alamazsınız bir [uygulama hizmeti sertifika](./web-sites-purchase-ssl-web-site.md) Azure portalından.

### <a name="cause-and-solution"></a>Nedeni ve çözümü
Bu sorun aşağıdaki nedenlerden biriyle oluşabilir:

- Uygulama hizmeti planı "Ücretsiz" veya "Paylaşılan" dir. SSL için bu fiyatlandırma katmanlarına desteklemiyoruz. 

    **Çözüm**: "Standart" web uygulaması için uygulama hizmeti planını yükseltin.

- Abonelik geçerli bir kredi kartı yok.

    **Çözüm**: aboneliğiniz için geçerli bir kredi kartı ekleyin. 

- Abonelik teklif, Microsoft Student gibi bir uygulama hizmeti sertifikası satın desteklemez.  

    **Çözüm**: aboneliğinizi yükseltme. 

- Abonelik satın alma işlemleri bir abonelikte izin verilen maksimum sınırına ulaştı.

    **Çözüm**: uygulama hizmeti sertifikaları Öde olarak ve EA abonelikleri türleri için 10 sertifika satın almalara sınırlaması vardır. Diğer abonelik türleri için izin verilen maksimum 3'tür. Sınırını artırmak için başvurun [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Uygulama hizmeti sertifika sahtekarlık işaretlendi. Aşağıdaki hata iletisini alıyorsunuz: "sertifikanızı olası sahtekarlık için işaretlendi. İstek şu anda incelenmektedir. Sertifika 24 saat içinde kullanılabilir olmaz;"

    **Çözüm**: sertifika sahtekarlık işaretlenir ve 24 saat sonra çözümlenmemiş, şu adımları izleyin:

    1. [Azure Portal](https://portal.azure.com)’da oturum açın.
    2. Git **uygulama hizmeti sertifikaları**, sertifikayı seçin.
    3. Seçin **sertifika yapılandırma** > **2. adım: doğrulama** > **etki alanı doğrulama**. Bu sorunu çözmek için Azure sertifika sağlayıcısı için bir e-posta bildirimi gönderir.

## <a name="domain-problems"></a>Etki alanı sorunları

### <a name="purchased-ssl-certificate-for-wrong-domain"></a>Yanlış etki alanı için SSL sertifikası satın

### <a name="symptom"></a>Belirti

Yanlış etki alanı için bir uygulama hizmet sertifikası satın ve doğru etki alanını kullanmak üzere sertifika güncelleştirilemiyor.

### <a name="solution"></a>Çözüm

- Bu sertifikayı silin ve yeni bir sertifika satın alın.
- Yanlış etki alanı kullanan geçerli sertifika "Verilen" durumdaysa sonra aynı zamanda bu sertifika için Fatura edilecek. Uygulama Hizmeti sertifikaları iade değildir, ancak ile bağlantı kurabildiğini [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) diğer seçenekleri olup olmadığını görmek için. 

### <a name="app-service-certificate-was-renewed-but-still-shows-the-old-certificate"></a>Uygulama hizmeti sertifikası yenilendi ancak hala eski sertifika gösterir 

### <a name="symptom"></a>Belirti

Uygulama hizmeti sertifikası yenilendi ancak uygulama hizmeti sertifikayı kullanan web uygulaması hala eski sertifika kullanıyor. Ayrıca, HTTPS protokolünü gerekli olduğunu belirten bir uyarı alındı.

### <a name="cause"></a>Nedeni 
Web uygulama hizmeti her sekiz saatte bir arka plan işi çalışır ve herhangi bir değişiklik olursa sertifika kaynak eşitlenir. Döndürme veya bir sertifikayı güncelleştirmek, bu nedenle, bazen uygulama hala eski sertifika ve yeni güncelleştirilmiş sertifika alıyor. Sertifika kaynak eşitleme işi henüz çalıştırılmadı olmasıdır. 
 
### <a name="solution"></a>Çözüm

Sertifikanın bir eşitleme uygulayabilirsiniz:

1. [Azure Portal](https://portal.azure.com)’da oturum açın. Seçin **uygulama hizmeti sertifikaları**ve ardından sertifikayı seçin.
2. Tıklatın **anahtar yenileme ve eşitleme**ve ardından **eşitleme**. Tamamlanması biraz zaman alır. 
3. Eşitleme tamamlandığında, aşağıdaki bildirim görürsünüz: "Tüm kaynaklar en son sertifikası ile başarıyla güncelleştirildi".

### <a name="domain-verification-is-not-working"></a>Etki alanı doğrulama çalışmıyor 

### <a name="symptom"></a>Belirti 
Sertifika kullanıma hazır hale gelmeden önce etki alanı doğrulama uygulama hizmeti sertifikası gerektirir. Tıkladığınızda **doğrula**, işlem başarısız olur.

### <a name="solution"></a>Çözüm
El ile bir TXT kaydı ekleyerek etki alanınızı doğrulayın:
 
1.  Etki alanı adınızı barındıran etki alanı adı hizmeti (DNS) Sağlayıcısı'na gidin.
2.  Azure portalında gösterilen etki alanı belirtecin değerini kullanır, etki alanı için bir TXT kaydı ekleyin. 

Çalıştırın ve ardından DNS'ye yayma işlemi birkaç dakika bekleyin **yenileme** doğrulama tetiklemek için düğmesi. 

El ile doğrulamak için alternatif yöntemi "için sertifika etki alanının etki alanı sahipliğini doğrulamanız sertifika yetkilisi izin vermek için kullanılan HTML Web sayfası" yöntemidir.

1.  {Etki alanı doğrulama belirteci} .html adlı bir HTML dosyası oluşturun. 
2.  Bu dosya etki alanı doğrulama belirteci değeri içerik olması gerekir.
3.  Etki alanınızı barındıran web sunucusunun kökünde bu dosyayı karşıya yükleme
4.  Tıklatın **yenileme** sertifika durumunu denetlemek için. Doğrulama tamamlanması için birkaç dakika sürebilir.

Etki alanı doğrulama belirteci '1234abcd' ile azure.com için standart bir sertifika satın aldıkları, örneğin, ardından web isteği yapılan http://azure.com/1234abcd.html 1234abcd döndürmelidir. 

> [!IMPORTANT]
> Etki alanı doğrulama işlemini tamamlamak için yalnızca 15 gün bir sertifika gerekir. 15 gün sonra sertifikayı sertifika yetkilisi tarafından reddedilir ve sertifika için sizden ücret değil. Bu durumda, bu sertifikayı silin ve yeniden deneyin.
>
> 

### <a name="unable-to-purchase-a-domain"></a>Bir etki alanı satın almak için

### <a name="symptom"></a>Belirti
Bir etki alanı Web uygulaması veya uygulama hizmeti etki alanından Azure Portalı'nda satın aldığınız olamaz.

### <a name="cause-and-solution"></a>Nedeni ve çözümü

Bu sorun aşağıdaki nedenlerden birinden dolayı oluşur:

- Kredi kartı geçersiz kredi kartı veya Azure aboneliği.

    **Çözüm**:, yoksa, aboneliğiniz için geçerli bir kredi kartı ekleyin.

- Abonelik sahibi değilseniz, etki alanı satın alma izni olmayabilir.

    **Çözüm**: [sahip rolünü eklemek](../billing/billing-add-change-azure-subscription-administrator.md) hesabı veya bir etki alanı satın almak için izin almak için aboneliği yöneticisine başvurun.
- Aboneliğinizi etki alanlarında satın sınırına ulaştınız. Geçerli sınırı 20'dir.

    **Çözüm**: İstek artış sınırı, kişi [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Azure aboneliğinize uygulama hizmeti etki alanı satın desteklemez.

    **Çözüm**: Azure aboneliğinizi Kullandıkça Öde aboneliğine gibi diğer abonelik türleri yükseltin.

### <a name="unable-to-add-a-hostname-to-web-app"></a>Bir ana bilgisayar adı Web uygulamasına eklenemedi 

### <a name="symptom"></a>Belirti

Bir ana bilgisayar adı eklediğinizde, doğrulamak ve etki alanını doğrulayın ve işlemi başarısız.

### <a name="cause"></a>Nedeni 

Bu sorun aşağıdaki nedenlerden birinden dolayı oluşur:

- Bir ana bilgisayar adı eklemek için izniniz yok.

    **Çözüm**: Abonelik yöneticisine bir ana bilgisayar adı ekleme iznine sahip olduğunuzdan emin olun.
- Etki alanı sahipliğini doğrulanamadı.

    **Çözüm**:, CNAME veya bir kayıt varsa doğru bir şekilde yapılandırıldığını doğrulayın. Web uygulaması için özel etki alanını eşlemek için bir CNAME veya A kaydını oluşturun. Kök etki alanı kullanmak isterseniz, A ve TXT kayıtlarını kullanmanız gerekir:

    |Kayıt türü|Host|İşaret|
    |------|------|-----|
    |A|@|Web uygulaması için IP adresi|
    |TXT|@|< Uygulama adı >. azurewebsites.net|
    |CNAME|www|< Uygulama adı >. azurewebsites.net|

### <a name="dns-cannot-be-resolved"></a>DNS çözümlenemiyor

### <a name="symptom"></a>Belirti

Bir "DNS kaydı bulunamadı" hata iletisi alırsınız.

### <a name="cause"></a>Nedeni
Bu sorun aşağıdaki nedenlerden birinden dolayı oluşur:

- Yaşam süresi (TTL) süresi süresi doldu değil. TTL değeri belirleyin ve ardından süresi dolmak üzere beklemek, etki alanınız için DNS yapılandırmasını denetleyin.
- DNS yapılandırması doğru değil.

### <a name="solution"></a>Çözüm
- Bu sorun kendiliğinden için 48 saat bekleyin.
- Değer, DNS yapılandırmanızda TTL ayarı değiştirebilirsiniz varsa, bu sorunu çözer olup olmadığını görmek için 5 dakika olarak değiştirin.
- Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınızın web uygulama IP adresine işaret doğrulanamadı. Yoksa, A kaydını web uygulamasının doğru IP adresi için yapılandırın.

### <a name="restore-a-deleted-domain"></a>Silinen bir etki alanı geri yükleme 

### <a name="symptom"></a>Belirti
Etki alanınızı artık Azure portalında görünür olur.

### <a name="cause"></a>Nedeni 
Etki alanı yanlışlıkla abonelik sahibi tarafından silinmiş olabilir.

### <a name="solution"></a>Çözüm
Etki alanınızı yedi günden önce silinmişse, etki alanı silme işlemi henüz başlatılmadı. Bu durumda, aynı etki alanında (arama kutusuna tam etki alanı adı olduğundan emin olun) aynı abonelik altında Azure Portal'daki yeniden satın alabilirsiniz. Yeniden bu etki alanı için ücret alınmaz. Etki alanı en az yedi gün önce silinmişse, temasa [Azure Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) etki alanı geri yüklemek Yardım.

### <a name="custom-domain-returns-404-or-site-inaccessible"></a>Özel etki alanı 404 veya site erişilemez döndürür 

### <a name="symptom"></a>Belirti

Özel etki alanı adını kullanarak siteye göz attığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Hata 404 Web uygulaması bulunamadı.**


### <a name="cause-and-solution"></a>Nedeni ve çözümü

**Neden 1** 

Yapılandırdığınız özel etki alanı CNAME veya A kaydı eksik. 

**Neden 1 için çözüm**

- Bir A kaydı eklediyseniz, TXT kaydı de eklendiğinden emin olun. Daha fazla bilgi için bkz: [Oluştur--a kaydı](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Web uygulamanız için kök etki alanı kullanmayı yoksa kaydı yerine bir CNAME kaydı kullanmak için önerilir.
- Aynı etki alanı için bir CNAME ve A kayıt kullanmayın. Bu, çakışmaya neden ve etki alanı çözmeleri engeller. 

**Neden 2** 

Internet tarayıcısı hala eski IP adresi etki alanınız için önbelleğe alma. 

**Neden 2 için çözüm**

Tarayıcı temizleyin. Windows cihazlar için komutu çalıştırabilirsiniz `ipconfig /flushdns`. Kullanım [WhatsmyDNS.net](https://www.whatsmydns.net/) etki alanınızın web uygulama IP adresine işaret doğrulanamadı. 

### <a name="unable-to-add-subdomain"></a>Alt etki alanı eklenemiyor 

### <a name="symptom"></a>Belirti

Yeni bir ana bilgisayar adı bir alt etki alanını atamak için bir web uygulaması ekleyemezsiniz.

### <a name="solution"></a>Çözüm

- Web uygulaması için bir ana bilgisayar adı eklemek için izinlere sahip olduğundan emin olmak için Abonelik Yöneticisi ile denetleyin.
- Daha fazla alt etki alanlarını ihtiyacınız varsa, etki alanını Azure DNS'ye barındıran değiştirmenizi öneririz. Azure DNS kullanarak, web uygulamanıza 500 ana bilgisayar adları ekleyebilirsiniz. Daha fazla bilgi için bkz: [bir alt etki alanı ekleme](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















