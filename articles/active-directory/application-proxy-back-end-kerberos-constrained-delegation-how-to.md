---
title: "Sorun giderme Kerberos Kısıtlı temsilci yapılandırmaları uygulama proxy'si için | Microsoft Docs"
description: "Sorun giderme Kerberos Kısıtlı temsilci yapılandırmaları için uygulama proxy'si."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 7b31f53e14e3f9a175e5dda95a18eb89dbca99dc
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Sorun giderme Kerberos Kısıtlı temsilci yapılandırmaları için uygulama proxy'si

SSO yayımlanan uygulamalara elde etmek için kullanılabilen yöntemler biraz bir uygulamadan diğerine farklılık gösterebilir. Varsayılan olarak, Azure uygulama proxy'si sağlayan seçeneklerden biri Kerberos Kısıtlı temsilci (KCD). Kullanıcılar adına arka uç uygulamaları için kısıtlı Kerberos kimlik doğrulaması gerçekleştirmek için bir bağlayıcı yapılandırabilirsiniz.

KCD etkinleştirme gerçek yordamı oldukça basittir ve genellikle en çok çeşitli bileşenler ve SSO kolaylaştıran kimlik doğrulaması akışı genel anlaşılmasını gerektirir. Burada KCD SSO beklendiği gibi işlev olmayan iyi bilgi kaynaklarıyla ilgili senaryoları gidermenize yardımcı olması için bulma olabilir seyrek.

Bu nedenle, bu makalede tek bir sorun giderme ve bazı görülen en yaygın sorunları otomatik olarak düzeltmek yardımcı olması başvuru noktası sağlamaya çalışır. Aynı anda daha karmaşık ve uygulama troubled tanılamaya yönelik ek yönergeler sunar.

Bu makalede aşağıdaki varsayımlar yapar:

-   Azure uygulaması Proxy dağıtımını [belgelerine](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) ve genel olmayan KCD uygulamalara erişim beklendiği gibi çalıştığını.

-   Yayımlanan hedef uygulama IIS ve Microsoft'un Kerberos üzerinde temel alır.

-   Sunucu ve uygulama ana bilgisayar tek bir Active Directory etki alanında bulunur. Etki alanı ve orman senaryoları çapraz ilgili ayrıntılı bilgiler bulunabilir [KCD Teknik İnceleme](http://aka.ms/KCDPaper).

-   Konu olan uygulamanın yayımlanan bir Azure Kiracı ile ön kimlik doğrulama etkinleştirildi ve kullanıcılar için Azure form tabanlı kimlik doğrulaması kimlik doğrulaması beklenir. Zengin istemci kimlik doğrulama senaryoları bu makalenin kapsamında değildir, ancak belirli bir noktada gelecekte eklenmesi.

## <a name="prerequisites"></a>Önkoşullar

Azure uygulama proxy'si altyapılarının birçok türlerine dağıtılabilir veya ortamları ve mimarileri kuruluştan kuruluşa Şüphesiz farklılık gösterir. KCD ilgili sorunların en yaygın nedenlerinden biri olmayan ortamlarda kendilerini, ancak yerine basit yanlış yapılandırmaları veya genel gözetim.

Bu nedenle, her zaman içinde düzenlendiği tüm ön koşul karşılanır sağlayarak başlatmak en iyisidir [KCD SSO'su kullanarak uygulama proxy'si makaleyi](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) önce başlatma sorunlarını giderme.

Bu önceki sürümlerinde Windows, aynı zamanda bazı başka noktalar oluşturduğunu sırasında KCD yapılandırma temelde farklı bir yaklaşım uygular gibi KCD 2012R2 üzerinde yapılandırma konusunda özellikle bölümü:

-   Belirli bir etki alanı denetleyicisi ile bir güvenli kanal iletişim kutusunu açmak bir etki alanı üye sunucusu seyrek değil. Bağlayıcı konakları yalnızca belirli yerel site ile DC'leri iletişim geliştirebilmek için sınırlandırılmalıdır olmayan şekilde başka bir iletişim kutusu için belirli bir zamanda taşıyın.

-   Yerel ağ çevre dışında yer alabileceği DC'leri bağlayıcı konağa doğrudan başvuruları senaryoları Bel etki alanları arası önceki noktasına benzer. Bu senaryoda, sonraki sürümlerde ilgili diğer etki alanlarındaki temsil DC'leri trafiği da olanak tanıyan veya başka temsilci başarısız emin olmak eşit oranda önemli olan.

-   Bunlar bazen zorlayıcı ve çekirdek RPC trafiğine engel olarak mümkün olduğunda, tüm etkin IP'leri/Kimlikler cihazlar bağlayıcı konakları ve DC'leri arasında yerleştirmekten kaçınmalısınız

Temsilci en basit senaryoyu test etmek için önerilir. Daha fazla değişken tanıtmak, daha fazla ile yüklüyorsa gerekebilir. Örneğin, tek bir bağlayıcıyı test sınırlama değerli zamandan tasarruf edebilirsiniz ve sorunları giderildikten sonra ek bağlayıcı eklenebilir.

Bazı çevresel etmenler de soruna katkıda bulunan. Test sırasında bu çevresel etmenler önlemek için tam minimum mimarisi en aza indirin. Örneğin, yanlış yapılandırılmış iç güvenlik duvarı ACL'ler seyrek, böylece mümkünse düz aracılığıyla DC'leri ve arka uç uygulaması izin verilen bir bağlayıcı gelen tüm trafiği vardır. 

Gerçekte bağlayıcılar konumlandırmak için mutlak en iyi olabileceğinden hedeflerine gibi yakın yerdir. Bir Güvenlik Duvarı'nı sat satır içi sahip yalnızca sınama adımında gereksiz karmaşıklık ekler ve yalnızca, araştırmalar uzatmak.

Bu nedenle, nelerin KCD sorun yine de oluşturduğunu? KCD başarısız SSO birkaç Klasik göstergeleri vardır ve tarayıcıda, kendilerini genellikle bir sorunun ilk işaretlerini bildirimi.

   ![Yanlış KCD yapılandırma hatası](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Yetkilendirme eksik izinleri nedeniyle başarısız oldu](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

tümü aynı belirtisi SSO gerçekleştirmek başarısız olan ve sonuç olarak uygulamaya kullanıcı erişimi reddetme, size aittir.

## <a name="troubleshooting"></a>Sorun giderme

Ardından sorun giderme nasıl sorun ve gözlemlenen Belirtiler bağlıdır. Yararlı bilgiler, henüz gelmiş olabilir üzerinden değil içermesi gibi daha fazla işlenmesini önce aşağıdaki bağlantılardan keşfedin:

-   [Uygulama proxy'si sorunları ve hata iletileri sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)

-   [Kerberos hataları ve belirtileri](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#kerberos-errors)

-   [SSO ile çalışan, şirket içi ve bulut kimlikleri aynı değildir](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd#working-with-sso-when-on-premises-and-cloud-identities-are-not-identical)

Ana sorunu kesinlikle şimdiye kadar bu ardından ekranınız varsa bulunmaktadır. Sorun giderme üç ayrı aşamaları akışına ayırarak başlatın.

**İstemcisi ön kimlik doğrulaması** - bir tarayıcı üzerinden Azure için kimlik doğrulama dış kullanıcı.

Azure için ön kimlik doğrulaması için olan işleve KCD SSO için zorunludur. Bu test edilip herhangi bir sorun varsa, ilk olarak ele. Ön kimlik doğrulama aşama ilişki KCD veya yayımlanan uygulama yok. Oldukça kolay sağlamlık ile uyumsuzlukları düzeltmek konu hesap denetimi Azure'da var ve onu devre dışı bırakılmış/engellenmez emin olmalıdır. Tarayıcıda hata yanıtı nedenini anlamak için genellikle açıklayıcı yeterli. Ayrıca, diğer bizim sorunlarını giderme belgeleri emin değilseniz, doğrulamak için kontrol edebilirsiniz.

**Temsilci hizmet** - kullanıcılar adına bir KDC (Kerberos Dağıtım Merkezi), bir Kerberos hizmet anahtarı edinme Azure Ara sunucusu Bağlayıcısı.

Dış iletişimleri Azure ön uç ile istemci arasında şifrelemeyle KCD üzerinde doğabilecek, diğerinden sağlama çalıştığını olması gerekir. Azure Proxy Hizmeti bir Kerberos anahtarı edinmek için kullanılan geçerli bir kullanıcı kimliği ile sağlanabilir şekilde budur. Bu, olmadan KCD mümkün değildir ve başarısız olmasına neden olabilir.

Daha önce belirtildiği gibi tarayıcı hata iletileri şey neden başarısız iyi bazı ipuçları genellikle sağlar. Bu davranış Azure Proxy olay günlüğünde gerçek olayları ilişkilendirmenize olanak tanır şekilde etkinlik kimliği ve zaman damgası yanıt aşağı Not emin olun.

   ![Yanlış KCD yapılandırma hatası](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

Ve olay günlüğüne olayları 13019 veya 12027 görülen karşılık gelen girdilere görülür. Bağlayıcı olay günlüklerinde bulabilirsiniz **uygulama ve hizmet günlükleri** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **bağlayıcı**&gt;**yönetici**.

   ![Uygulama proxy'si olay günlüğündeki olay 13019](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Uygulama proxy'si olay günlüğündeki olay 12027](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Bir A kaydı iç DNS sunucunuzun, uygulamanın adresi ve bir CName için kullanın.

-   Bağlayıcı ana hedef hesabının SPN ve belirlenen temsilci hakları verilmiş yeniden onayladıktan **herhangi bir kimlik doğrulama protokolünü kullan** seçilir. Bu konu hakkında daha fazla bilgi için bkz: [SSO yapılandırma makale](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)

-   Olduğunu SPN varlığı ad, tek bir örneğini vererek doğrulayın bir `setspn -x` herhangi bir etki alanı üye ana bir komut isteminden

-   Sınırlamak için bir etki alanı ilkesi uygulanıp uygulanmadığını denetleyin [en büyük boyut verilen Kerberos belirteçlerin](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/)bu bağlayıcı bir belirteç IF almasını engeller gibi aşırı olmasını bulundu

Bağlayıcı ana bilgisayar ve etki alanı KDC arasında alışverişleri yakalama bir ağ izlemesi sonra konuları hakkında daha fazla alt düzey ayrıntı alma sonraki en iyi adım olacaktır. Daha fazla ayrıntı görmek için [derinlemesine sorun giderme kağıt](https://aka.ms/proxytshootpaper).

Raporlama iyi görünüyorsa, bir olay nedeniyle bir 401 döndürme uygulaması kimlik doğrulamasının başarısız belirten günlüklerine görmeniz gerekir. Bu genellikle, bilet reddetme hedef uygulama gösterir, böylece ile aşağıdaki sonraki adıma devam.

**Hedef uygulama** -bağlayıcı tarafından sağlanan Kerberos bileti tüketici

Bağlayıcı bir Kerberos göndermiş beklenir bu aşamada bilet arka uç için ilk uygulama isteği içinde bir başlık olarak hizmet.

-   Uygulamanın İç URL Portalı'nda tanımlanan kullanarak, uygulama bağlayıcı ana bilgisayarda doğrudan tarayıcıdan erişilebilir olduğunu doğrulayın. Ardından, başarılı bir şekilde oturum açabilir. Bunun yapılması ile ilgili ayrıntılar connector sorunlarını giderme sayfasında bulunabilir.

-   Hala bağlayıcı konakta tarayıcı ve uygulama arasındaki kimlik doğrulaması Kerberos, aşağıdakilerden birini yaparak kullandığını doğrulayın:

1.  Geliştirme araçları çalıştırma (**F12**) Internet Explorer ya da kullanım [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) bağlayıcı ana bilgisayardan. İç URL'yi kullanarak uygulamasına gidin ve sunulan incelemek WWW yetkilendirme üstbilgileri döndürülen yanıt uygulamadan emin olmak için bu ya da negotiate veya Kerberos varsa. Döndürülen sonraki Kerberos blob uygulama tarayıcıdan yanıta genellikle başlayarak **YII**bu göstergesidir oynama olan Kerberos gelir. NTLM diğer yandan her zaman ile başlayan **TlRMTVNTUAAB**, Base64 kodlaması kodunu çözdü zaman NTLMSSP okur. Görürseniz **TlRMTVNTUAAB** blob başlangıcında, bu Kerberos anlamına gelir **değil** kullanılabilir. Bu görmüyorsanız, Kerberos olasıdır kullanılabilir.
    > [!NOTE]
    > Fiddler kullanarak, bu yöntem geçici olarak IIS'de uygulamanın config genişletilmiş korumayı devre dışı bırakılması gerekir.

     ![Ağ İnceleme tarayıcının](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Şekil:* TIRMTVNTUAAB ile başlamıyor olduğundan, bu Kerberos kullanılabilir bir örnektir. Bu, YII ile başlamıyor Kerberos Blob örneğidir.

2.  Geçici olarak NTLM IIS site ve erişim uygulamasından doğrudan bağlayıcı konaktaki IE üzerinde sağlayıcıları listesinden kaldırın. Artık sağlayıcılar listesinde NTLM ile yalnızca Kerberos kullanarak uygulamayı erişebilir olması gerekir. Bu başarısız olursa, daha sonra uygulamanın yapılandırma ile ilgili bir sorun yoktur ve Kerberos kimlik doğrulaması çalışmıyor önerir.

Kerberos kullanılabilir durumda değilse, uygulamanın kimlik doğrulaması emin olmak için IIS ayarlarında anlaşma onay üstteki hemen altındaki NTLM ile listelenir. (Anlaşma: kerberos veya anlaşma: pku2u ile). Yalnızca Kerberos işlevsel ise devam edin.

   ![Windows kimlik doğrulama sağlayıcıları](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   Kerberos ve NTLM yerinde, ön kimlik doğrulama Portalı'nda uygulama için şimdi geçici olarak devre dışı olanak sağlar. Dış URL'yi kullanarak internet'ten erişebildiğinizi varsa bkz. Kimlik doğrulaması yapmak istenir ve önceki adımda kullanılan aynı hesap ile bunu yapabiliyor olmanız gerekir. Aksi durumda, bu hiç değil KCD ve arka uç uygulaması ile ilgili bir sorun gösterir.

-   Şimdi ön kimlik doğrulama Portalı'nda yeniden etkinleştirin ve dış URL'sini aracılığıyla uygulamaya bağlanmak deneyerek Azure üzerinden kimlik doğrulaması. SSO başarısız olursa, tarayıcı yanı sıra 13022 olay günlüğünde olay Yasak hata iletisini görmeniz gerekir:

    *Microsoft AAD Application Proxy Connector, Kerberos kimlik doğrulama girişimlerini bir HTTP 401 hata ile arka uç sunucusuna yanıtlaması çünkü kullanıcının kimliğini doğrulayamıyor.*

    ![HTTTP 401 Yasak hata alır](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   SPN karşı AD içinde aşağıdaki çizimde olduğu gibi IIS'de giderek yapılandırıldı aynı hesabı kullanmak üzere yapılandırılmış uygulama havuzunda yapılandırılmış sağlamak için IIS uygulama denetleyin

    ![IIS uygulama yapılandırma penceresi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    Kimlik öğrendikten sonra bu hesabı kesinlikle ilgili SPN ile yapılandırıldığından emin olmak için bir komut isteminde aşağıdakileri gönderin. Örneğin,`setspn –q http/spn.wacketywack.com`

    ![SetSPN komut penceresi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Uygulamanın uygulama havuzunun kullandığı hedef AD hesabı karşı yapılandırılmış aynı SPN portalındaki uygulamanın ayarları karşı SPN tanımlanan denetleyin.

   ![Azure Portalı'nda SPN yapılandırma](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   IIS ve select INTO gidin **yapılandırma Düzenleyicisi** seçeneği için uygulama ve gidin **system.webServer/security/authentication/windowsAuthentication** değerieminolmakiçin**UseAppPoolCredentials** olan **True**

   ![IIS yapılandırması uygulama havuzları seçeneği kimlik bilgisi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Çekirdek modu da etkin bırakarak Kerberos işlemlerinin performansını geliştirmeye yararlı olan makine hesabı kullanarak şifresinin çözülmesini istenen hizmet bileti neden yapılırken. Bu nedenle bu uygulama bir grupta birden fazla sunucuda barındırılıyorsa true sonu KCD ayarlamak sahip yerel sistem olarak da adlandırılır.

-   Ek bir onay, aynı zamanda devre dışı bırakmak isteyebilirsiniz **Genişletilmiş** koruma çok. Burada bu uygulamanın varsayılan Web sitesi bir alt klasörü burada yayımlanan belirli yapılandırmalarında etkinleştirildiğinde KCD bölüneceği oluyor uygulamasına yol açıyordu karşılaşılan senaryoları olmuştur. Bu kendisine bağımlı nesneler herhangi bir etkin ayarı devralarak öneren çıkışı gri tüm iletişim kutularını bırakarak anonim kimlik doğrulaması için yalnızca yapılandırılır. Ancak burada olası her zaman bu etkinleştirildiğinde sahip öneririz böylece her şekilde test, ancak bu etkin geri unutmayın.

Bu ek denetimler yayımlanan uygulamanızı kullanmaya başlamak için izlemek, yerleştirdiğiniz. Devam edin ve döndürme temsilci seçmek için yapılandırılan ek bağlayıcılar, ancak sonra şeyleri daha fazla olup olmadığını biz bizim daha ayrıntılı teknik kılavuz okuma önerebileceğiniz [sorun giderme Azure AD uygulama proxy'si için tam Kılavuzu](https://aka.ms/proxytshootpaper)

Hala sorununuzu ilerleme yapamıyorsanız, destek ve buradan devam yardımcı olmak üzere birden çok memnun olacaktır. Doğrudan (mühendisin size ulaşmak) portal dahilinde bir destek bileti oluşturun.

## <a name="other-scenarios"></a>Diğer senaryolar

-   Azure uygulama proxy'si, bir uygulamaya isteğini göndermeden önce bir Kerberos anahtarı ister. Tableau Server gibi bazı üçüncü taraf uygulamalar kimlik doğrulama yöntemi gibi değil ve bunun yerine gerçekleşmesi daha geleneksel anlaşmaları bekliyor. İlk istek anonim kimlik doğrulama türü ile bir 401 destekler yanıtlamak uygulama izin verme.

-   Burada bir uygulama, bir arka uç hem ön uç SQL Reporting Services gibi gerektiren her iki kimlik doğrulama ile katmanlı senaryolarda yaygın olarak kullanılan çift atlama kimlik doğrulaması -.

## <a name="next-steps"></a>Sonraki adımlar
[Yönetilen bir etki alanında kerberos Kısıtlı temsilci (KCD) yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-enable-kcd)
