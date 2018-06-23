---
title: SAML tabanlı çoklu oturum açma - Azure Active Directory hata ayıklama | Microsoft Docs
description: Azure Active Directory uygulamalarında hata ayıklama SAML tabanlı çoklu oturum açma.
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
ms.openlocfilehash: d0c006b21e00693fe6c8b35237d4ce55f67c0f75
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320600"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>SAML tabanlı çoklu oturum açma Azure Active Directory uygulamalarında hata ayıklama

Bulma ve düzeltme hakkında bilgi edinin [çoklu oturum açma](../manage-apps/what-is-single-sign-on.md) destekleyen uygulamalar Azure Active Directory'de (Azure AD) sorunlarda [güvenlik onaylama işlemi biçimlendirme dili (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Başlamadan önce
Yüklemenizi öneririz [My uygulamaları güvenli oturum açma uzantısı](../active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension). Bu tarayıcı uzantısı SAML isteğinde ve çoklu oturum açma ile ilgili sorunları çözmek için gereksinim duyduğunuz SAML yanıt bilgilerini toplamak kolay hale getirir. Uzantı yükleyemezsiniz durumunda, bu makalede, hem ile hem de yüklü uzantısı olmadan sorunları gidermek nasıl gösterilmektedir.

Oturum açma My uygulamaları güvenli uzantısı yükleyip için aşağıdaki bağlantılardan birini kullanın.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açmayı test edin

SAML tabanlı çoklu oturum açma AAD ve bir hedef uygulama arasında test etmek için:

1.  Oturum [Azure portal](https://portal.azure.com) genel yönetici veya uygulamaları yönetmek için yetkili diğer yönetici olarak.
2.  Sol dikey penceresinde tıklayın **Azure Active Directory**ve ardından **kurumsal uygulamalar**. 
3.  Kuruluş uygulamaları listesinden çoklu oturum açmayı test etmek istediğiniz ve ardından sol tıklatıldığında seçeneklerinden uygulamayı tıklatın **çoklu oturum açma**.
4.  SAML tabanlı çoklu oturum açma test deneyimini açmak için **etki alanı ve URL'leri** bölümünde **Test SAML ayarı**. Test SAML ayarı düğmesi gri ise, out ve gerekli öznitelikler kaydetme ilk doldurmanız gerekir.
5.  İçinde **Test çoklu oturum açma** dikey penceresinde, hedef uygulama için oturum açmak için şirket kimlik bilgilerinizi kullanın. Geçerli kullanıcı veya farklı bir kullanıcı olarak oturum açabilir. Farklı bir kullanıcı olarak oturum açarsanız, bir istem kimlik doğrulaması yapmanızı ister.

Başarıyla imzalı olup olmadığını test geçti. Bu durumda, Azure AD bir SAML yanıtı uygulamaya belirteç. SAML belirteci başarıyla oturum açmak için kullanılan uygulama.

Şirket oturum açma veya uygulamanın sayfasını bir hata varsa, sonraki bölümlerde biri hatayı gidermek için kullanın.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Bir oturum açma hatası oturum açma şirket sayfanızda çözümleyin

Oturum açmaya çalıştığınızda, oturum açma şirket sayfanızda bir hata görebilirsiniz. 

![Oturum açma hatası](media/active-directory-saml-debugging/error.png)

Bu hata ayıklama için hata iletisi ve SAML isteğinde gerekir. My uygulamaları güvenli oturum açma uzantısı otomatik olarak bu bilgileri toplar ve üzerinde Azure AD çözümlemesi rehberlik görüntüler. 

Güvenli MyApps ile oturum açma sorunu çözümlemek için oturum açma uzantısı yüklü:

1.  Dahili bir hata oluştuğunda, Azure AD'ye yönlendirir **Test çoklu oturum açma** dikey. 
2.  Üzerinde **Test çoklu oturum açma** dikey penceresinde tıklatın **SAML isteğini indir**. 
3.  Hata ve değerlerin SAML isteğinde Kılavuzu temelinde belirli çözümleme görmeniz gerekir. Yönergeleri inceleyin.

Oturum açma uzantısı MyApps güvenli yüklemeden hatayı gidermek için:

1. Sayfanın sağ alt köşesindeki hata ileti kopyalayın. Hata iletisi içerir:
    - Correlationıd değeri ve zaman damgası. Sorunu belirlemek ve sorunu doğru bir çözüm sağlamak için mühendisleri Yardım için Microsoft ile bir destek servis talebi oluşturduğunuzda, bu değerleri önemlidir.
    - Sorunun kök nedenini tanımlayan ifade.
2.  Azure AD ile geri dönün ve Bul **Test çoklu oturum açma** dikey.
3.  Yukarıdaki metin kutusuna **alma çözüm Kılavuzu**, hata iletisi yapıştırın.
3.  Tıklatın **alma çözüm Kılavuzu** sorunu çözmek için adımları görüntülemek için. Kılavuzu SAML isteğinde veya SAML yanıtını bilgileri gerektirebilir. Oturum açma MyApps güvenli uzantı kullanmıyorsanız, bir aracı gibi gerekebilecek [Fiddler](http://www.telerik.com/fiddler) SAML istek ve yanıt almak için.
4.  SAML çoklu oturum açma hizmeti Azure Active Directory'den alınan URL'si SAML isteğinde hedef karşılık doğrulayın
5.  SAML isteğinde veren, Azure Active Directory'de uygulama için yapılandırdığınız aynı tanımlayıcısıdır doğrulayın. Azure AD veren dizininizde bir uygulamayı bulmak için kullanır.
6.  Burada Azure Active Directory'den SAML belirteci almak uygulama bekler AssertionConsumerServiceURL olduğunu doğrulayın. Bu değer Azure Active Directory'de yapılandırabilirsiniz, ancak SAML isteğinde parçası ise bu zorunlu değildir.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Bir oturum açma hatası uygulama sayfasında çözümleyin

Başarıyla oturum açın ve ardından uygulamanın sayfada bir hata görebilirsiniz. Bu, Azure AD uygulamaya belirteç, ancak uygulama yanıtı kabul etmediğinden ortaya çıkar.   

Hatayı gidermek için:

1. Uygulama Azure AD Galerisi'nde ise, uygulama Azure AD ile tümleştirme için tüm adımları izlediğinizden doğrulayın. Uygulamanız için tümleştirme yönergeleri bulmak için bkz: [SaaS uygulama tümleştirmesi öğreticiler listesi](../saas-apps/tutorial-list.md).
2. SAML yanıtını alır.
    - My uygulamaları güvenli oturum açma uzantısı yüklüyse, gelen **Test çoklu oturum açma** dikey penceresinde tıklatın **SAML yanıtını karşıdan**.
    - Uzantısı yüklü değilse, bir aracı gibi kullandığınız [Fiddler](http://www.telerik.com/fiddler) SAML yanıtını alınamadı. 
3. Bu öğeleri SAML yanıt belirteci dikkat edin:
    - Kullanıcının benzersiz tanıtıcısını NameID değeri ve biçimi
    - Belirtecinde verilen talepler
    - Belirteç imzalamak için kullanılan sertifika. SAML yanıtını gözden geçirme hakkında daha fazla bilgi için bkz: [tek oturum açma SAML Protokolü](active-directory-single-sign-on-protocol-reference.md).
4. SAML yanıtını hakkında daha fazla bilgi için bkz: [tek oturum açma SAML Protokolü](active-directory-single-sign-on-protocol-reference.md).
5. SAML yanıtını geçirdikten, bkz: [oturum açtıktan sonra bir uygulamanın sayfada hata](../application-sign-in-problem-application-error.md) sorun giderme kılavuzu. 
6. Başarıyla oturum açmak hala erişilemiyor varsa, uygulamanın satıcısına SAML yanıttan eksik sorabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Çoklu oturum açma uygulamanıza çalıştığından, size verebilir [otomatikleştirmek kullanıcı sağlama ve SaaS uygulamaları etkinleştirmektir](../active-directory-saas-app-provisioning.md), veya [koşullu erişimi kullanmaya başlama](../active-directory-conditional-access-azure-portal-get-started.md).


