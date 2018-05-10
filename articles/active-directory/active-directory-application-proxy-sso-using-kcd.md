---
title: Uygulama proxy'si ile çoklu oturum açma | Microsoft Docs
description: Çoklu oturum açma Azure AD uygulama proxy'si kullanma özelliğini sağlamak nasıl ele alınmaktadır.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: aee1c1ad44cada857ca0fc8fc42565448b5bfa46
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-to-your-apps-with-application-proxy"></a>Kerberos Kısıtlanmış temsilci seçimi için çoklu oturum açma uygulamalarınızı uygulama proxy'si ile uygulama

Şirket içi için çoklu oturum açma sağlayabilir yayımlanan uygulamaları aracılığıyla uygulama proxy'si tümleşik Windows kimlik doğrulaması ile güvenli hale getirilir. Bu uygulama için erişim bir Kerberos anahtarı gerektirir. Uygulama proxy'si, bu uygulamaları desteklemek için Kerberos Kısıtlı temsilci (KCD) kullanır. 

Çoklu oturum açma kullanıcının kimliğine bürünmek için Active Directory Uygulama proxy'si bağlayıcılar izin vererek tümleşik Windows kimlik doğrulaması (IWA) kullanarak uygulamalarınızı etkinleştirebilirsiniz. Bağlayıcılar bu izni göndermek ve şirket adına belirteçleri almak için kullanın.

## <a name="how-single-sign-on-with-kcd-works"></a>KCD works ile nasıl çoklu oturum açma
Bir kullanıcı IWA kullanan şirket içi uygulamaya erişmeyi denediğinde Bu diyagramda akışını açıklar.

![Microsoft AAD kimlik doğrulaması akışı diyagramı](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Kullanıcı uygulama proxy'si aracılığıyla şirket içi uygulamaya erişmek için URL'yi girer.
2. Uygulama proxy'si istek preauthenticate için Azure AD kimlik doğrulama hizmetleri yönlendirir. Bu noktada, tüm geçerli kimlik doğrulama ve yetkilendirme ilkeleri, çok faktörlü kimlik doğrulaması gibi Azure AD geçerlidir. Kullanıcı doğrulanırsa, Azure AD bir belirteç oluşturur ve kullanıcıya gönderir.
3. Kullanıcı belirteci için uygulama proxy'si geçirir.
4. Uygulama proxy'si belirteci doğrular ve kullanıcı asıl adı (UPN) alır ve ardından istek, UPN ve hizmet asıl adı (SPN) bağlayıcı dually kimliği doğrulanmış olarak güvenli bir kanal üzerinden gönderir.
5. Kerberos Kısıtlı temsilci (KCD) anlaşması ile şirket içi bağlayıcı gerçekleştiren AD, uygulama için bir Kerberos belirteci almak için kullanıcının kimliğine bürünmek.
6. Active Directory Uygulama bağlayıcısı için Kerberos belirteci gönderir.
7. Bağlayıcı ilk istek AD'den alınan Kerberos belirteci kullanarak uygulama sunucusuna gönderir.
8. Uygulama için uygulama proxy'si hizmeti daha sonra döndürülen bağlayıcı yanıta gönderir ve son kullanıcıya.

## <a name="prerequisites"></a>Önkoşullar
IWA uygulamalar için çoklu oturum açmayı ile çalışmaya başlamadan önce ortamınızın aşağıdaki ayarları ve yapılandırmaları hazır olduğundan emin olun:

* SharePoint Web uygulamaları gibi uygulamalar, tümleşik Windows kimlik doğrulaması kullanacak şekilde ayarlanır. Daha fazla bilgi için bkz: [Kerberos kimlik doğrulaması için desteği etkinleştirme](https://technet.microsoft.com/library/dd759186.aspx), veya SharePoint bakın [Plan SharePoint 2013'te Kerberos kimlik doğrulaması için](https://technet.microsoft.com/library/ee806870.aspx).
* Tüm uygulamaların [hizmet asıl adları](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).
* Bağlayıcısı çalıştıran sunucu ve uygulama çalıştıran sunucunun etki alanına katılmış ve aynı etki alanında veya güvenilen etki alanları parçası olan. Etki alanına katılma ile ilgili daha fazla bilgi için bkz: [bir bilgisayarı bir etki alanına](https://technet.microsoft.com/library/dd807102.aspx).
* Bağlayıcısı çalıştıran sunucunun TokenGroupsGlobalAndUniversal özniteliğine kullanıcılar için okuma erişimi vardır. Bu varsayılan ayarı tarafından güvenlik ortamı artırma etkilenmiş.

### <a name="configure-active-directory"></a>Active Directory'yi yapılandırma
Active Directory yapılandırması, uygulama proxy'si Bağlayıcınızı ve uygulama sunucusu aynı etki alanında veya değil olmanıza bağlı olarak değişir.

#### <a name="connector-and-application-server-in-the-same-domain"></a>Bağlayıcı ve uygulama sunucusu aynı etki alanında
1. Active Directory'de Git **Araçları** > **kullanıcıları ve Bilgisayarları**.
2. Bağlayıcısı çalıştıran sunucuyu seçin.
3. Sağ tıklatıp **özellikleri** > **temsilci**.
4. Seçin **bu bilgisayara yalnızca belirtilen hizmetlere temsilci seçmek için güven**. 
5. Altında **için bu hesabı atanmış kimlik bilgileri sunabileceği Hizmetler** uygulama sunucusunun SPN kimlik değeri ekleyin. Bu listede tanımlanan uygulamaları karşı AD'de kullanıcının kimliğine bürünmek uygulama ara sunucusu Bağlayıcısı sağlar.

   ![Bağlayıcı SVR özellikleri penceresinin ekran görüntüsü](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Bağlayıcı ve uygulama sunucusu farklı etki alanlarında
1. KDC ile çalışma alanlarında önkoşullarının listesi için bkz: [alanlarında Kerberos Kısıtlı temsilci](https://technet.microsoft.com/library/hh831477.aspx).
2. Kullanım `principalsallowedtodelegateto` bağlayıcı sunucusu için temsilci seçme uygulama proxy'si etkinleştirmek için bağlayıcı sunucudaki özelliği. Uygulama sunucusu `sharepointserviceaccount` ve temsilci sunucusu `connectormachineaccount`. Windows 2012 R2 için bir örnek olarak bu kodu kullanabilirsiniz:

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount

Sharepointserviceaccount SP'ler makine hesabı veya SP uygulama havuzunun altında çalıştığı bir hizmet hesabı olabilir.

## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın 
1. Uygulamanızı bölümünde açıklanan yönergeleri göre yayımlamak [uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md). Seçtiğinizden emin olun **Azure Active Directory** olarak **ön kimlik doğrulama yöntemi**.
2. Uygulamanızı kurumsal uygulamalar listesinde göründükten sonra onu seçin ve tıklatın **çoklu oturum açma**.
3. Çoklu oturum açma modu ayarlamak **tümleşik Windows kimlik doğrulaması**.  
4. Girin **iç uygulama SPN** uygulama sunucusunun. Bu örnekte, yayımlanan uygulamamız için SPN http/www.contoso.com ' dir. Bu SPN bağlayıcı atanmış kimlik bilgileri sunabilir Hizmetler listesinde olması gerekir. 
5. Seçin **oturum açma kimlik temsilcisi** kullanıcılarınızın adına kullanmak bağlayıcı. Daha fazla bilgi için bkz: [farklı şirket içi ve bulut kimlikleri ile çalışma](#Working-with-different-on-premises-and-cloud-identities)

   ![Gelişmiş uygulama yapılandırması](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Windows olmayan uygulamalar için SSO
Azure AD bulut kullanıcı kimliği doğruladığında Azure AD uygulama proxy'si Kerberos temsilci akışında başlatır. Şirket içi istek geldikten sonra Azure AD uygulama ara sunucusu Bağlayıcısı'nı yerel Active Directory ile etkileşim kurarak kullanıcı adına bir Kerberos anahtarı verir. Bu işlem, Kerberos Kısıtlı temsilci (KCD) olarak adlandırılır. Sonraki aşamasında, bir isteği arka uç uygulama bu Kerberos anahtarı ile gönderilir. Bu tür istekleri göndermek nasıl tanımlayan çeşitli protokoller vardır. Çoğu Windows olmayan sunucuları üzerinde Azure AD uygulama proxy'si artık desteklenmektedir Negotiate/SPNego bekler.

Kerberos hakkında daha fazla bilgi için bkz: [tüm Kerberos Kısıtlı temsilci (KCD) hakkında bilmek istediğiniz](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

Windows olmayan uygulamalar genellikle kullanıcı kullanıcı adlarını veya etki alanı yerine SAM hesap adlarını e-posta adresleri. Bu durum, uygulamalarınız için geçerliyse, bulut kimliklerinizi uygulama kimliklerinizi bağlanmak için yetkilendirilmiş oturum açma kimlik alanı yapılandırmanız gerekir. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Şirket içi ve bulut kimlikleri ile çalışma
Uygulama proxy'si, kullanıcıların bulutta ve şirket içi tam olarak aynı kimliğe sahip olduğunu varsayar. Bu durumda değilse, çoklu oturum açma için hala KCD kullanabilirsiniz. Yapılandırma bir **oturum açma kimlik temsilcisi** hangi kimlik, çoklu oturum açma gerçekleştirirken kullanılması gerektiğini belirlemek her bir uygulama için.  

Bu özellik şirket içi ve bulut kimlikleri SSO buluttan farklı kullanıcı adları ve parolalar girmesini gerek kalmadan şirket içi uygulamalara sahip olan birçok kuruluş sağlar. Bu kuruluşların içerir:

* Dahili olarak birden çok etki alanınız (joe@us.contoso.com, joe@eu.contoso.com) ve tek bir etki alanı bulutta (joe@contoso.com).
* Dahili yönlendirilebilir olmayan etki alanı adına sahip (joe@contoso.usa) ve yasal bir bulutta.
* Etki alanı adları dahili olarak kullanmamanız (CAN)
* İçi farklı diğer adlar kullanın ve bulutta. Örneğin, joe-johns@contoso.com vs. joej@contoso.com  

Uygulama proxy'si ile hangi kimlik Kerberos anahtarını almak için kullanmayı seçebilirsiniz. Bu ayarı bir uygulamadır. Bu seçeneklerden bazısı e-posta adresi biçimi kabul ediyor musunuz sistemler için uygun olan, başkalarının alternatif oturum açma için tasarlanmıştır.

![Temsilci oturum açma kimlik parametresi ekran görüntüsü](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Temsilci oturum açma kimliği kullanılırsa, değer tüm etki alanları veya ormanlar, kuruluşunuzda arasında benzersiz olmayabilir. Bu sorunu önlemek için iki kez iki farklı bağlayıcı gruplarını kullanarak bu uygulamaları yayımlayarak. Her uygulama farklı kullanıcı İzleyici olduğundan, kendi bağlayıcılarını farklı bir etki alanına katılmasını sağlayabilir.

### <a name="configure-sso-for-different-identities"></a>SSO için farklı kimlik Yapılandır
1. E-posta adresi (posta) ana kimliktir şekilde Azure AD Connect ayarları yapılandırın. Bu özelleştirme işleminin bir parçası değiştirerek yapılır **kullanıcı asıl adı** alanındaki eşitleme ayarları. Bu ayarlar, ayrıca kullanıcıların Office365, Windows10 aygıtları nasıl oturum belirler ve kendi kimlik deposu olarak Azure AD kullanan diğer uygulamalar.  
   ![Kullanıcıların ekran görüntüsü - kullanıcı asıl adı açılan tanımlama](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Değiştirmek istediğiniz uygulama için uygulama yapılandırma ayarlarında seçin **oturum açma kimlik temsilcisi** kullanılacak:

   * Kullanıcı asıl adı (örneğin, joe@contoso.com)
   * Diğer kullanıcı asıl adı (örneğin, joed@contoso.local)
   * Kullanıcı asıl adı (örneğin, Can) kullanıcıadı parçası
   * Kullanıcı adı bölümü diğer kullanıcı asıl adı (örneğin, joed)
   * Şirket içi SAM hesap adı (etki alanı denetleyicisi yapılandırmasına bağlıdır)

### <a name="troubleshooting-sso-for-different-identities"></a>SSO için farklı kimlik sorunlarını giderme
SSO işleminde hata oluşursa, bağlayıcı makine olay günlüğü'nde açıklandığı gibi göründüğü [sorun giderme](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).
Ancak, bu uygulama diğer HTTP yanıtlarını çeşitli yanıtlar sırada bazı durumlarda, istek başarıyla arka uç uygulamaya gönderilir. Bu gibi durumlarda sorun giderme uygulaması proxy'si oturum olay günlüğünde bağlayıcı makinede olay numarası 24029 inceleyerek başlamalısınız. Olay Ayrıntıları içinde "kullanıcı" alanında temsilci seçmek için kullanılan kullanıcı kimliği görüntülenir. Oturum açma etkinleştirmek için seçin **Göster Analitik ve hata ayıklama günlüklerini** Olay Görüntüleyicisi'ni Görünüm menüsünde.

## <a name="next-steps"></a>Sonraki adımlar

* [Kerberos kısıtlanmış temsili kullanmak üzere bir uygulama proxy'si uygulama yapılandırma](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Uygulama Ara sunucusu ile ilgili sorunları giderme](active-directory-application-proxy-troubleshoot.md)


En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın

