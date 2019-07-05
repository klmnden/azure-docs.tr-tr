---
title: SAML tabanlı çoklu oturum açma - Azure Active Directory hata ayıklama | Microsoft Docs
description: Azure Active Directory'de uygulamalar için hata ayıklama SAML tabanlı çoklu oturum açma.
services: active-directory
author: rwike77
documentationcenter: na
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/18/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: luleon, hirsin, smalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4331acf639af90448b5508e3487f4979e9b82c45
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482720"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Azure Active Directory'de uygulamalar için SAML tabanlı çoklu oturum açma hata ayıklama

Bulma ve düzeltme hakkında bilgi edinin [çoklu oturum açma](../manage-apps/what-is-single-sign-on.md) destekleyen uygulamaları Azure Active Directory'de (Azure AD) için sorunları [güvenlik onaylama işlemi biçimlendirme dili (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Başlamadan önce

Yüklemenizi öneririz [My Apps güvenli oturum açma uzantısı](../user-help/my-apps-portal-end-user-troubleshoot.md#im-having-trouble-installing-the-my-apps-secure-sign-in-extension). Bu tarayıcı uzantısı ile çoklu oturum açma sorunları çözmek için gereken SAML yanıtını bilgileri ve SAML isteğini toplamak kolay hale getirir. Uzantı yükleyemezsiniz durumunda, bu makalede hem ile hem de yüklü uzantısı olmadan sorunların nasıl giderileceğini gösterir.

My Apps güvenli oturum açma uzantısı yükleyip için aşağıdaki bağlantılardan birini kullanın.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

## <a name="test-saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açma testi

SAML tabanlı çoklu oturum açma Azure AD arasında test etmek için ve bir hedef uygulama:

1. Oturum [Azure portalında](https://portal.azure.com) genel yönetici veya uygulamaları Yönetme yetkisine başka bir yönetici olarak.
1. Sol dikey penceresinden seçin **Azure Active Directory**ve ardından **kurumsal uygulamalar**. 
1. Kurumsal uygulamalar listesinden uygulamayı çoklu oturum açmayı test etmek istediğiniz ve ardından sol SELECT seçeneklerinden seçin **çoklu oturum açma**.
1. SAML tabanlı çoklu oturum açmayı test deneyimini açmak için Git **Test çoklu oturum açma** (5. adım). Varsa **Test** düğmesi gri, kullanıma ve gerekli öznitelikleri ilk doldurmak gereken **temel SAML yapılandırma** bölümü.
1. İçinde **Test çoklu oturum açma** dikey penceresinde, hedef uygulama için oturum açmak için Kurumsal kimlik bilgilerinizi kullanın. Geçerli kullanıcının veya farklı bir kullanıcı olarak oturum açabilir. Farklı bir kullanıcı olarak oturum açın, bir komut istemi kimlik doğrulamasından geçmesini isteyin.

    ![Test SAML SSO sayfasını gösteren ekran görüntüsü](./media/howto-v1-debug-saml-sso-issues/test-single-sign-on.png)

Oturumunuz başarıyla açıldıktan, test geçti. Bu durumda, Azure AD, bir SAML yanıtını belirteç uygulamaya verilen. Uygulama SAML belirteci başarıyla oturum açmak için kullanılır.

Uygulama sayfası veya oturum açma şirket sayfasındaki bir hata varsa, hatayı gidermek için sonraki bölümlerde birini kullanın.

## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Şirket oturum açma sayfanız bir oturum açma hatası çözümleyin

Oturum açmaya çalıştığında, aşağıdaki örneğe benzer, şirket oturum açma sayfasında bir hata görebilirsiniz.

![Şirket oturum açma sayfasında bir hata olduğunu gösteren örnek](./media/howto-v1-debug-saml-sso-issues/error.png)

Bu hata ayıklama için hata iletisini ve SAML isteğini gerekir. My Apps güvenli oturum açma uzantısı otomatik olarak bu bilgileri toplar ve Azure AD çözümlemesi rehberlik görüntüler.

### <a name="to-resolve-the-sign-in-error-with-the-my-apps-secure-sign-in-extension-installed"></a>My Apps güvenli oturum açma uzantısı ile oturum açma hatayı gidermek için yüklü

1. Uzantı bir hata oluştuğunda, Azure AD'ye yönlendiren **Test çoklu oturum açma** dikey penceresi.
1. Üzerinde **Test çoklu oturum açma** dikey penceresinde **SAML isteğini indir**.
1. Belirli bir çözümleme hatası ve SAML isteğindeki değerleri Kılavuzu göre görmeniz gerekir.
1. Göreceğiniz bir **düzelt** düğmesi yapılandırma sorunu çözmek için Azure AD'de otomatik olarak güncelleştirilecek. Bu düğmeyi görmüyorsanız, ardından oturum açma sorunu Azure AD'de bir yanlış yapılandırma nedeniyle değil.

Herhangi bir çözüm için oturum açma hatası sağlanırsa, bize bildirmek için geri bildirim textbox kullanmak öneririz.

### <a name="to-resolve-the-error-without-installing-the-my-apps-secure-sign-in-extension"></a>My Apps güvenli oturum açma uzantısı yüklemeden hatayı gidermek için

1. Hata iletisi, sayfanın sağ alt köşesindeki kopyalayın. Hata iletisi içerir:
    - Bağıntı kimliği ve zaman damgası. Bu değerler, çünkü bunlar, sorunu tanımlamak ve sorununuz için doğru bir çözüm sağlamak için mühendislerin yardımcı Microsoft ile bir destek olayı oluştururken önemlidir.
    - Sorunun kök nedenini tanımlayan bir ifade.
1. Azure AD'ye geri dönün ve bulma **Test çoklu oturum açma** dikey penceresi.
1. Yukarıdaki metin kutusuna **çözümleme Hadoop'u**, hata iletisi yapıştırın.
1. Tıklayın **çözümleme Hadoop'u** adımlar bu sorunu çözmek için görüntülenecek. Kılavuz, SAML isteğini veya SAML yanıtını bilgileri gerektirebilir. My Apps güvenli oturum açma uzantı kullanmıyorsanız, bir aracı aşağıdaki gibi ihtiyacınız olabilecek [Fiddler](https://www.telerik.com/fiddler) SAML isteği ve yanıt almanızı sağlar.
1. SAML isteğindeki hedef SAML çoklu oturum açma hizmeti Azure AD'den elde edilen URL'si karşılık geldiğini doğrulayın.
1. Azure AD'de uygulama için yapılandırdığınız aynı tanımlayıcıyı SAML isteğindeki veren olduğunu doğrulayın. Azure AD veren dizininizde uygulamayı bulmak için kullanır.
1. Burada Azure AD'den SAML belirteci almak uygulamanın beklediği AssertionConsumerServiceURL olduğunu doğrulayın. Bu değeri Azure AD'de yapılandırabilirsiniz, ancak SAML isteğinde bir parçasıysa, zorunlu değildir.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Uygulama sayfasını bir oturum açma hatası çözümleyin

Başarıyla oturum açın ve ardından uygulamanın sayfasında bir hata görebilirsiniz. Bu, Azure AD uygulamaya belirteç, ancak uygulamanın bir yanıtı kabul ortaya çıkar.

Hatayı gidermek için aşağıdaki adımları izleyin:

1. Uygulama Azure AD galerisindeki ise, uygulamanın Azure AD ile tümleştirmeye yönelik tüm adımları izlediğinizden doğrulayın. Uygulamanız için tümleştirme yönergeleri bulmak için bkz: [SaaS uygulama tümleştirmesi öğreticileri listesi](../saas-apps/tutorial-list.md).
1. SAML yanıtını alın.
    - My Apps güvenli oturum açma uzantısı yüklü değilse, gelen **Test çoklu oturum açma** dikey penceresinde tıklayın **SAML yanıtını indirme**.
    - Uzantı yüklü değilse gibi bir araç kullanın [Fiddler](https://www.telerik.com/fiddler) SAML yanıtını almak için.
1. Bu öğeleri yanıt SAML belirtecindeki dikkat edin:
   - Kullanıcının benzersiz tanımlayıcısı Nameıd değeri ve biçimi
   - Belirtecinde verilen talepleri
   - Belirteç imzalamak için kullanılan sertifika.

     SAML yanıtını hakkında daha fazla bilgi için bkz. [tek oturum açma SAML Protokolü](single-sign-on-saml-protocol.md).

1. SAML yanıtını gözden geçirdikten sonra bkz. [oturum açtıktan sonra bir uygulamanın sayfada hata](../manage-apps/application-sign-in-problem-application-error.md) sorunun nasıl giderileceği hakkında yönergeler için. 
1. Başarıyla oturum açmak hala kaldıramıyorsanız, SAML yanıttan eksik uygulamanın satıcısına isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Çoklu oturum açma uygulamanıza çalıştığından, yapabilirsiniz [otomatik kullanıcı hazırlama ve SaaS uygulamalarına sağlamayı](../manage-apps/user-provisioning.md) veya [koşullu erişim ile çalışmaya başlama](../conditional-access/app-based-conditional-access.md).
