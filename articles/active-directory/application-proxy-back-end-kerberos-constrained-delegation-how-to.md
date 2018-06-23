---
title: Uygulama proxy'si için kısıtlı Kerberos temsilcisi yapılandırma sorunlarını gidermek | Microsoft Docs
description: Uygulama proxy'si için Kerberos Kısıtlı temsilci yapılandırma sorunlarını gidermek
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: f318c53de073c8f1fa6c3ae11cb335a4a91e137d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334970"
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Uygulama proxy'si için kısıtlı Kerberos temsilcisi yapılandırma sorunlarını gidermek

SSO yayımlanan uygulamalara elde etmek için kullanılabilen yöntemler bir uygulamadan diğerine farklılık gösterebilir. Varsayılan olarak Azure Active Directory (Azure AD) uygulama proxy'si sunan bir Kerberos Kısıtlı temsilci (KCD) seçenektir. Kısıtlı Kerberos kimlik doğrulaması için arka uç uygulamaları çalıştırmak için kullanıcılarınızın bir bağlayıcı yapılandırabilirsiniz.

KCD etkinleştirme yordamı basittir. SSO destekleyen en çok çeşitli bileşenler ve kimlik doğrulama akışı genel anlaşılmasını gerektirir. Ancak bazı durumlarda, KDC SSO beklendiği gibi işlev değil. Bu senaryolar sorun giderme bilgilerinin iyi kaynakları gerekir.

Bu makalede tek bir sorun giderme ve bazı yaygın sorunları otomatik olarak düzeltmek yardımcı başvuru noktası sağlar. Daha karmaşık uygulama sorunlarını tanılama ele alınmaktadır.

Bu makalede aşağıdaki varsayımlar yapar:

-   Azure AD uygulaması Proxy dağıtımını [uygulama proxy'si ile çalışmaya başlama](manage-apps/application-proxy-enable.md) ve KCD olmayan uygulamalar için genel erişim beklendiği gibi çalışmayabilir.

-   Yayımlanan hedef uygulama Internet Information Services (IIS) ve Kerberos Microsoft uyarlamasını dayanır.

-   Sunucu ve uygulama ana bilgisayar tek bir Azure Active Directory etki alanında yer alması. Etki alanları arası ve orman senaryoları hakkında ayrıntılı bilgi için bkz: [KCD incelemeyi](https://aka.ms/KCDPaper).

-   Bir Azure konu olan uygulamanın yayımlanan ön kimlik doğrulaması etkin olan Kiracı. Kullanıcılar için Azure form tabanlı kimlik doğrulaması kimlik doğrulaması beklenir. Zengin istemci kimlik doğrulama senaryoları, bu makalenin ele değil. Bunlar belirli bir noktada gelecekte eklenmiş olabilir.

## <a name="prerequisites"></a>Önkoşullar

Azure AD uygulama proxy'si türlerde altyapıları veya ortamlar içinde dağıtılabilir. Mimariler kuruluştan kuruluşa farklılık gösterir. KCD ilgili sorunların en yaygın nedenlerini ortamlar değil. Basit yapılandırma hataları veya genel hataları çoğu sorunlara neden.

Bu nedenle, tüm Önkoşullar karşılanır emin olmak en iyisidir [KCD SSO'su kullanarak uygulama proxy'si](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md) sorunlarını gidermeye başlamadan önce. 2012R2 üzerinde kısıtlı Kerberos temsilcisi yapılandırma bölümü unutmayın. Bu işlem, Windows'un önceki sürümlerinde KCD yapılandırmak için farklı bir yaklaşım uygular. Ayrıca, bu hususlara dikkatli olun:

-   Belirli bir etki alanı denetleyicisi (DC) bir güvenli kanal iletişim kutusunu açmak bir etki alanı üye sunucusu seyrek değil. Ardından sunucuyu başka bir iletişim kutusu için herhangi bir zamanda taşımak. Bu nedenle bağlayıcı konakları yalnızca belirli yerel site DC'leri ile iletişim için sınırlı değildir.

-   Etki alanları arası senaryolar yerel ağ çevre dışında olabilir DC'leri bağlayıcı konağa doğrudan başvuruları kullanır. Bu durumlarda, ayrıca ileriye doğru trafiği ilgili diğer etki alanlarındaki temsil eden DC'lere göndermek eşit derecede önemlidir. Aksi durumda, temsilci başarısız olur.

-   Mümkünse, tüm etkin IP'leri ya da kimlik cihazlar bağlayıcı konakları ve DC'leri arasında yerleştirmez. Bu cihazları bazen overintrusive ve çekirdek RPC trafiğine engel.

Temsilci basit senaryolarda sınayın. Daha fazla değişken tanıtmak, daha fazla ile yüklüyorsa gerekebilir. Zaman kazanmak için tek bir bağlayıcıyı test sınırlayın. Sorun çözüldükten sonra ek bağlayıcı ekleyin.

Bazı çevresel etmenler bir sorun da katkıda bulunabilir. Bu etkenler önlemek için mümkün olduğunca test sırasında mimarisi en aza indirin. Örneğin, yanlış yapılandırılmış iç güvenlik duvarı ACL'ler ortak. Mümkünse, tüm trafik bir bağlayıcısından doğrudan DC'leri ve arka uç uygulaması gönderin.

Bağlayıcılar konumlandırmak için en iyi hedeflerine mümkün olduğunca yakın yerdir. Satır içi sınarken bulunur bir güvenlik duvarı, araştırmalar uzatmak ve gereksiz karmaşıklığı ekler.

Ne KCD sorunu gösterir. KCD SSO başarısız birkaç ortak göstergeleri vardır. Bir sorunun ilk işaretlerini tarayıcıda görünür.

   ![Yanlış KCD yapılandırma hatası](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Yetkilendirme eksik izinler nedeniyle başarısız oldu](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

Bu görüntüler her ikisi de aynı belirti göster: SSO hatası. Uygulama için kullanıcı erişimi reddedildi.

## <a name="troubleshooting"></a>Sorun giderme

Nasıl sorun giderme, sorun ve, gözlemlemek Belirtiler bağlıdır. Herhangi bir uzağa geçmeden önce aşağıdaki makalelerde keşfedin. Bunlar yararlı sorun giderme bilgileri sağlar:

-   [Uygulama proxy'si sorunları ve hata iletileri sorunlarını giderme](manage-apps/application-proxy-troubleshoot.md)

-   [Kerberos hataları ve belirtileri](manage-apps/application-proxy-troubleshoot.md#kerberos-errors)

-   [SSO ile çalışan, şirket içi ve bulut kimlikleri aynı değil](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities)

Bu noktada geldiyseniz, ana sorunu bulunmaktadır. Başlatmak için giderebileceğinizi aşağıdaki üç aşamanın akışına ayırın.

### <a name="client-pre-authentication"></a>İstemcisi ön kimlik doğrulaması 
Dış kullanıcı için Azure bir tarayıcı üzerinden kimlik doğrulaması. Azure için önceden kimlik doğrulama yeteneği, işleve KCD SSO için gereklidir. Test ve herhangi bir sorun varsa bu yeteneği adresi. Ön kimlik doğrulama aşama KCD veya yayımlanan uygulama ile ilişkili değil. Azure'da konu hesabı mevcut sağlamlık denetleyerek uyumsuzlukları düzeltmek kolaydır. Ayrıca olup olmadığını denetleyin veya devre dışı engellendi. Tarayıcıda hata yanıtı nedenini açıklamak için tanımlayıcı. Belirsiz doğrulamak için diğer Microsoft sorun giderme makaleleri kontrol edin.

### <a name="delegation-service"></a>Temsilci hizmeti 
Bir Kerberos Anahtar Dağıtım Merkezi (KDC) gelen kullanıcılar için bir Kerberos hizmet anahtarı alır Azure Ara sunucusu Bağlayıcısı.

Dış iletişimleri Azure ön uç ile istemci arasında şifrelemeyle KCD üzerinde sahip. Bu iletişimler yalnızca KCD çalıştığından emin olun. Azure Proxy Hizmeti bir Kerberos anahtarı almak için kullanılan geçerli bir kullanıcı kimliği sağlanır. Bu Kodsuz KCD mümkün değildir ve başarısız olur.

Daha önce belirtildiği gibi tarayıcı hata iletileri neden şeyler başarısız hakkında bazı iyi ipuçları sağlar. Etkinlik Kimliği ve zaman damgası yanıt aşağı Not emin olun. Bu bilgiler davranış Azure Proxy olay günlüğünde gerçek olayları ilişkilendirmenize yardımcı olur.

   ![Yanlış KCD yapılandırma hatası](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

Olay Günlüğü'nde görülen karşılık gelen girdilere 13019 veya 12027 olayları göster. Bağlayıcı olay günlüklerinde Bul **uygulama ve hizmet günlükleri** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt;  **Bağlayıcı** &gt; **yönetici**.

   ![Uygulama proxy'si olay günlüğündeki olay 13019](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Uygulama proxy'si olay günlüğündeki olay 12027](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

1.   Kullanım bir **A** uygulamanın adresi için iç DNS kaydı olmayan bir **CName**.

2.   Bağlayıcı ana bilgisayar için belirtilen hedef hesabının SPN temsilci hakkı verilmiş yeniden onayladıktan. Yeniden onayladıktan **herhangi bir kimlik doğrulama protokolünü kullan** seçilir. Daha fazla bilgi için bkz: [SSO yapılandırma makale](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md).

3.   SPN varlığı Azure AD'de, yalnızca bir örneği olduğunu doğrulayın. Sorunu `setspn -x` tüm etki alanı üye ana bilgisayarda bir komut isteminden.

4.   Bir etki alanı ilkesi sınırlayan zorlanır denetleyin [verilen Kerberos belirteçleri en büyük boyutunu](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/). Bu ilke, aşırı olmasını bulunursa, bir belirteç alınırken gelen bağlayıcı durdurur.

Bağlayıcı ana bilgisayar ve etki alanı KDC arasında alışverişleri yakalayan bir ağ izlemesi sorunları hakkında daha fazla alt düzey ayrıntılar elde etmek için İleri en iyi adımdır. Daha fazla bilgi için bkz: [derinlemesine sorun giderme kağıt](https://aka.ms/proxytshootpaper).

Raporlama iyi görünüyorsa, bir olay uygulama bir 401 döndürdüğünden kimlik doğrulamasının başarısız belirten günlüklerine bakın. Bu olay, hedef uygulama, biletini reddetti gösterir. Sonraki adıma gidin.

### <a name="target-application"></a>Hedef uygulama 
Bağlayıcı tarafından sağlanan Kerberos bileti tüketici. Bu aşamada, bir Kerberos Hizmet anahtarının arka ucuna göndermiş bağlayıcıyı bekler. İlk uygulama istek üstbilgisinde fırsattır.

1.   Uygulamanın İç URL Portalı'nda tanımlanan kullanarak, uygulama bağlayıcı ana bilgisayarda doğrudan tarayıcıdan erişilebilir olduğunu doğrulayın. Ardından, başarılı bir şekilde oturum açabilir. Ayrıntılar bağlayıcı bulunabilir **sorun giderme** sayfası.

2.   Tarayıcı ve uygulama arasındaki kimlik doğrulaması Kerberos kullanır hala bağlayıcı konakta onaylayın. Aşağıdaki eylemlerden birini gerçekleştirin:

3.  DevTools çalıştırın (**F12**) Internet Explorer ya da kullanım [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) bağlayıcı ana bilgisayardan. Uygulamaya dahili URL'yi kullanarak gidin. Döndürülen sunulan WWW yetkilendirme üstbilgileri incelemek ya da uygulamadan emin olmak için yanıtta anlaşmasına veya Kerberos varsa. 

    a. Uygulama tarayıcıdan yanıtta döndürülen sonraki Kerberos blob ile başlayan **YII**. Bu harfler Kerberos çalıştığını söyleyin. Microsoft NT LAN Yöneticisi (NTLM), diğer yandan, her zaman ile başlayan **TlRMTVNTUAAB**, NTLM Güvenlik Desteği Sağlayıcısı (Base64 kodlaması kodunu çözdü zaman NTLMSSP) okur. Görürseniz **TlRMTVNTUAAB** blob başlangıcında Kerberos kullanılabilir değil. Görmüyorsanız, **TlRMTVNTUAAB**, Kerberos büyük olasılıkla kullanılabilir.

       > [!NOTE]
       > Fiddler'ı kullanırsanız, bu yöntem, geçici olarak IIS uygulama yapılandırmasında genişletilmiş korumayı devre dışı bırak gerektirir.

       ![Ağ İnceleme tarayıcının](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    b. Bu şekilde blob ile başlamıyor **TIRMTVNTUAAB**. Bu örnekte, Kerberos kullanılabilir ve Kerberos blob ile başlamıyor **YII**.

4.  Geçici olarak NTLM IIS sitesinde sağlayıcıları listesinden kaldırın. Uygulama bağlayıcısı ana bilgisayarda Internet Explorer'dan doğrudan erişin. NTLM artık sağlayıcılar listesinde değil. Uygulama, yalnızca Kerberos kullanarak erişebilirsiniz. Erişim başarısız olursa, uygulamanın yapılandırma ile ilgili bir sorun olabilir. Kerberos kimlik doğrulaması çalışmıyor.

    a. Kerberos kullanılabilir değilse, IIS'de uygulamanın kimlik doğrulama ayarlarını denetleyin. Emin olun **anlaş** hemen altındaki NTLM ile üst listelenir. Görürseniz **anlaşma**, **Kerberos veya anlaşma**, veya **pku2u ile**, yalnızca Kerberos işlevsel ise devam edin.

       ![Windows kimlik doğrulama sağlayıcıları](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
    b. Geçici olarak Kerberos ve NTLM yerinde, portal uygulamasında için ön kimlik doğrulamasını devre dışı bırakın. Dış URL'yi kullanarak internet'ten erişmek deneyin. Kimlik doğrulaması yapmak istenir. Önceki adımda kullanılan aynı hesap ile bunu yapabilirsiniz. Aksi durumda, arka uç uygulaması, değil KCD ile ilgili bir sorun yoktur.

    c. Ön kimlik doğrulama Portalı'nda yeniden etkinleştirin. Azure üzerinden dış URL'sini aracılığıyla uygulamaya bağlanmaya tarafından kimlik doğrulaması. SSO başarısız olursa, tarayıcı ve 13022 olay günlüğünde olay Yasak hata iletisini görürsünüz:

       *Microsoft AAD Application Proxy Connector, Kerberos kimlik doğrulama girişimlerini bir HTTP 401 hata ile arka uç sunucusuna yanıtlaması çünkü kullanıcının kimliğini doğrulayamıyor.*

       ![HTTTP 401 Yasak hata alır](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

    d. IIS uygulama denetleyin. Azure AD'de aynı hesabı kullanmak üzere yapılandırılmış uygulama havuzu ve SPN yapılandırılmış olduğundan emin olun. IIS'de aşağıdaki çizimde gösterildiği gibi gidin:

       ![IIS uygulama yapılandırma penceresi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

       Kimlik öğrendikten sonra bu hesabı söz konusu SPN ile yapılandırılmış olduğundan emin olun. `setspn –q http/spn.wacketywack.com` bunun bir örneğidir. Bir komut isteminde aşağıdaki metni girin:

       ![SetSPN komut penceresi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

    e. Portal uygulama ayarlarında karşı tanımlanmış SPN denetleyin. Azure AD hesabının hedefe karşı yapılandırılmış aynı SPN uygulamanın uygulama havuzu tarafından kullanıldığından emin olun.

       ![Azure portalında SPN yapılandırma](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
    f. IIS ve select INTO gidin **yapılandırma Düzenleyicisi** uygulama için seçeneği. Gidin **system.webServer/security/authentication/windowsAuthentication**. Değer emin olun **UseAppPoolCredentials** olan **doğru**.

       ![IIS yapılandırması uygulama havuzları seçeneği kimlik bilgisi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

       Bu değeri değiştirmek **doğru**. Tüm önbelleğe alınan Kerberos biletleri, aşağıdaki komutu çalıştırarak arka uç sunucusundan kaldırın:

       ```powershell
       Get-WmiObject Win32_LogonSession | Where-Object {$_.AuthenticationPackage -ne 'NTLM'} | ForEach-Object {klist.exe purge -li ([Convert]::ToString($_.LogonId, 16))}
       ``` 

Daha fazla bilgi için bkz: [tüm oturumları için Kerberos istemci bilet önbelleğini temizlemek](https://gallery.technet.microsoft.com/scriptcenter/Purge-the-Kerberos-client-b56987bf).



Çekirdek modu etkin bırakırsanız, Kerberos işlemlerinin performansını artırır. Ancak aynı zamanda makine hesabını kullanarak şifresinin çözülmesini istenen hizmet bileti neden olur. Bu hesap, yerel sistem olarak da adlandırılır. Bu değer ayarlanırsa **True** uygulaması bir grupta birden fazla sunucu arasında barındırıldığında KCD ayırmak için.

-   Ek bir onay devre dışı **Genişletilmiş** koruma çok. Bazı senaryolarda **Genişletilmiş** koruma ihlal KCD belirli yapılandırmalarında etkinleştirildiğinde. Bu durumlarda uygulamanın varsayılan Web sitesinin bir alt klasörü olarak yayımlanmıştır. Bu uygulama yalnızca Anonim kimlik doğrulama için yapılandırılır. Tüm iletişim kutuları, alt nesnelerin herhangi etkin ayarları devralmak olmayacaktır öneren gri görünür. Test, ancak bu değere geri yüklemek unutmayın öneririz **etkin**, mümkün olduğunda.

    Bu ek denetimi yayımlanan uygulamanızın kullanmak üzere, izlemek, koyar. Ayrıca temsilci seçmek için yapılandırılmış olan ek bağlayıcılar döndür. Daha fazla bilgi için daha ayrıntılı teknik gözden geçirme okuma [Azure AD uygulama proxy'si sorun giderme](https://aka.ms/proxytshootpaper).

İlerleme hala yapamıyorsanız, Microsoft desteği size yardımcı olabilir. Doğrudan portal içinde bir destek bileti oluşturun. Mühendisin, sizinle iletişim kuracaktır.

## <a name="other-scenarios"></a>Diğer senaryolar

- Azure uygulama proxy'si, bir uygulamaya isteğini göndermeden önce bir Kerberos anahtarı ister. Bazı üçüncü taraf uygulamalar Tableau sunucu gibi kimlik doğrulama yöntemi istemeyiz. Bu uygulamalar gerçekleşmesi daha geleneksel anlaşmaları bekler. İlk istek uygulama bir 401 destekleyen kimlik doğrulama türü ile yanıt veren, anonimdir.

- Çoklu atlamalı kimlik doğrulaması yaygın senaryolarda olduğu bir uygulama, bir arka plan ve ön uç, katmanlı burada hem de SQL Server Reporting Services gibi bir kimlik doğrulaması gerektiren kullanılır. Çoklu atlama senaryoyu yapılandırmak için destek makalesine bakın [Kerberos Kısıtlı temsilci olabilir gerektiren protokol geçişi çoklu atlamalı senaryolarda](https://support.microsoft.com/help/2005838/kerberos-constrained-delegation-may-require-protocol-transition-in-mul).

## <a name="next-steps"></a>Sonraki adımlar
[Yönetilen bir etki alanında KCD yapılandırma](../active-directory-domain-services/active-directory-ds-enable-kcd.md).
