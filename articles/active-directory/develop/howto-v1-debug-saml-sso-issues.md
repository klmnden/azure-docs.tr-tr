---
title: SAML tabanlı çoklu oturum açma - Azure Active Directory hata ayıklama | Microsoft Docs
description: Azure Active Directory'de uygulamalar için hata ayıklama SAML tabanlı çoklu oturum açma.
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock, smalser
ms.openlocfilehash: 7f653eca0c768cc35df039cbd51153484710d80a
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422262"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Azure Active Directory'de uygulamalar için SAML tabanlı çoklu oturum açma hata ayıklama

Bulma ve düzeltme hakkında bilgi edinin [çoklu oturum açma](../manage-apps/what-is-single-sign-on.md) destekleyen uygulamaları Azure Active Directory'de (Azure AD) için sorunları [güvenlik onaylama işlemi biçimlendirme dili (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Başlamadan önce
Yüklemenizi öneririz [My Apps güvenli oturum açma uzantısı](../user-help/active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension). Bu tarayıcı uzantısı, SAML isteğini ve çoklu oturum açma ile ilgili sorunları çözmek için gereken SAML yanıtını bilgi toplamak kolaylaştırır. Uzantı yükleyemezsiniz durumunda, bu makalede hem ile hem de yüklü uzantısı olmadan sorunların nasıl giderileceğini gösterir.

My Apps güvenli oturum açma uzantısı yükleyip için aşağıdaki bağlantılardan birini kullanın.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açma testi

SAML tabanlı çoklu oturum açma AAD ve bir hedef uygulama arasında test etmek için:

1.  Oturum [Azure portalında](https://portal.azure.com) genel yönetici veya uygulamaları Yönetme yetkisine başka bir yönetici olarak.
2.  Sol dikey penceresinde **Azure Active Directory**ve ardından **kurumsal uygulamalar**. 
3.  Kurumsal uygulamalar listesinden uygulamayı çoklu oturum açmayı test etmek istediğiniz ve ardından sol'e tıklayın, seçeneklerden tıklatın **çoklu oturum açma**.
4.  SAML tabanlı çoklu oturum açmayı test deneyimini açmak için **etki alanı ve URL'ler** bölümünde **Test SAML ayarı**. Test SAML ayarı düğmesine bastığınızda, out ve gerekli öznitelikleri kaydetme önce doldurmanız gerekir.
5.  İçinde **Test çoklu oturum açma** dikey penceresinde, hedef uygulama için oturum açmak için Kurumsal kimlik bilgilerinizi kullanın. Geçerli kullanıcının veya farklı bir kullanıcı olarak oturum açabilir. Farklı bir kullanıcı olarak oturum açın, bir komut istemi kimlik doğrulamasından geçmesini isteyin.

    ![Test SAML sayfası](./media/howto-v1-debug-saml-sso-issues/testing.png)


Oturumunuz başarıyla açıldıktan, test geçti. Bu durumda, Azure AD, bir SAML yanıtını belirteç uygulamaya verilen. Uygulama SAML belirteci başarıyla oturum açmak için kullanılır.

Uygulama sayfası veya oturum açma şirket sayfasındaki bir hata varsa, hatayı gidermek için sonraki bölümlerde birini kullanın.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Şirket oturum açma sayfanız bir oturum açma hatası çözümleyin

Oturum açmayı deneyin, oturum açma şirket sayfanızda bir hata görebilirsiniz. 

![Oturum açma hatası](./media/howto-v1-debug-saml-sso-issues/error.png)

Bu hata ayıklama için hata iletisini ve SAML isteğini gerekir. My Apps güvenli oturum açma uzantısı otomatik olarak bu bilgileri toplar ve Azure AD çözümlemesi rehberlik görüntüler. 

Güvenli MyApps ile oturum açma hatayı gidermek için oturum açma uzantısı yüklü:

1.  Uzantı bir hata oluştuğunda, Azure AD'ye yönlendiren **Test çoklu oturum açma** dikey penceresi. 
2.  Üzerinde **Test çoklu oturum açma** dikey penceresinde tıklayın **SAML isteğini indir**. 
3.  Belirli bir çözümleme hatası ve SAML isteğindeki değerleri Kılavuzu göre görmeniz gerekir. Yönergeleri inceleyin.

MyApps güvenli oturum açma uzantısı yüklemeden hatayı gidermek için:

1. Hata iletisi, sayfanın sağ alt köşesindeki kopyalayın. Hata iletisi içerir:
    - Bağıntı kimliği ve zaman damgası. Bu değerler, çünkü bunlar, sorunu tanımlamak ve sorununuz için doğru bir çözüm sağlamak için mühendislerin yardımcı Microsoft ile bir destek olayı oluştururken önemlidir.
    - Sorunun kök nedenini tanımlayan bir ifade.
2.  Azure AD'ye geri dönün ve bulma **Test çoklu oturum açma** dikey penceresi.
3.  Yukarıdaki metin kutusuna **çözümleme Hadoop'u**, hata iletisi yapıştırın.
3.  Tıklayın **çözümleme Hadoop'u** adımlar bu sorunu çözmek için görüntülenecek. Kılavuz, SAML isteğini veya SAML yanıtını bilgileri gerektirebilir. MyApps güvenli oturum açma uzantı kullanmıyorsanız, bir aracı aşağıdaki gibi ihtiyacınız olabilecek [Fiddler](https://www.telerik.com/fiddler) SAML isteği ve yanıt almanızı sağlar.
4.  SAML çoklu oturum açma hizmeti Azure Active Directory'den alınan URL'si SAML isteğindeki hedef karşılık doğrulayın
5.  Azure Active Directory'de uygulama için yapılandırdığınız aynı tanımlayıcıyı SAML isteğindeki veren olduğunu doğrulayın. Azure AD veren dizininizde uygulamayı bulmak için kullanır.
6.  Burada Azure Active Directory'den SAML belirteci almak uygulamanın beklediği AssertionConsumerServiceURL olduğunu doğrulayın. Azure Active Directory'de, bu değeri yapılandırabilirsiniz, ancak SAML isteğinde bir parçasıysa, zorunlu değildir.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Uygulama sayfasını bir oturum açma hatası çözümleyin

Başarıyla oturum açın ve ardından uygulamanın sayfasında bir hata görebilirsiniz. Bu, Azure AD uygulamaya belirteç, ancak uygulamanın bir yanıtı kabul ortaya çıkar.   

Hatayı gidermek için:

1. Uygulama Azure AD galerisindeki ise, uygulamanın Azure AD ile tümleştirmeye yönelik tüm adımları uyguladıysanız doğrulayın. Uygulamanız için tümleştirme yönergeleri bulmak için bkz: [SaaS uygulama tümleştirmesi öğreticileri listesi](../saas-apps/tutorial-list.md).
2. SAML yanıtını alın.
    - My Apps güvenli oturum açma uzantısı yüklü değilse, gelen **Test çoklu oturum açma** dikey penceresinde tıklayın **SAML yanıtını indirme**.
    - Uzantı yüklü değilse gibi bir araç kullanın [Fiddler](https://www.telerik.com/fiddler) SAML yanıtını almak için. 
3. Bu öğeleri yanıt SAML belirtecindeki dikkat edin:
    - Kullanıcının benzersiz tanımlayıcısı Nameıd değeri ve biçimi
    - Belirtecinde verilen talepleri
    - Belirteç imzalamak için kullanılan sertifika. SAML yanıtını gözden geçirme hakkında daha fazla bilgi için bkz: [tek oturum açma SAML Protokolü](single-sign-on-saml-protocol.md).
4. SAML yanıtını hakkında daha fazla bilgi için bkz. [tek oturum açma SAML Protokolü](single-sign-on-saml-protocol.md).
5. SAML yanıtını gözden geçirdikten sonra bkz. [oturum açtıktan sonra bir uygulamanın sayfada hata](../manage-apps/application-sign-in-problem-application-error.md) sorunu çözme konusunda yönergeler için. 
6. Başarıyla oturum açmak hala kaldıramıyorsanız, SAML yanıttan eksik uygulamanın satıcısına isteyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Çoklu oturum açma uygulamanıza çalıştığından, yapabilirsiniz [otomatik kullanıcı hazırlama ve sağlamayı kaldırma SaaS uygulamalarına](../manage-apps/user-provisioning.md), veya [koşullu erişim ile çalışmaya başlama](../conditional-access/app-based-conditional-access.md).


