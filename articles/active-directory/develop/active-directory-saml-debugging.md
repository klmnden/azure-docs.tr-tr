---
title: SAML tabanlı çoklu oturum açma Azure Active Directory'de uygulamalar için hata ayıklama | Microsoft Docs
description: "SAML tabanlı çoklu oturum açma Azure Active Directory'de uygulamalar için hata ayıklama öğrenin "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 55ff6b7a70bcdcceacb1484f9969337f9853ce50
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>SAML tabanlı çoklu oturum açma Azure Active Directory uygulamalarında hata ayıklama

SAML tabanlı uygulama tümleştirmesi hata ayıklarken, genellikle gibi bir araç kullanmanız yararlı olur [Fiddler](http://www.telerik.com/fiddler) SAML isteğinde ve uygulamaya verilmiş SAML belirteci içeren SAML yanıtını görmek için. 

![Fiddler][1]

Sık sorunları Azure Active Directory veya uygulama tarafında bir yanlış yapılandırma ilgilidir. Hatayı görmek yere bağlı olarak, SAML istek veya yanıtın gözden vardır:

- My şirket oturum açma sayfası [Bağlantılar bölümüne] içinde bir hata görüyorum
- [Bağlantılar bölümüne] imzasını sonra uygulamanın sayfasında görüyorum hata

## <a name="i-see-an-error-in-my-company-sign-in-page"></a>Oturum açma şirket sayfamda hata görüyorum

Hata kodu ve çözüm adımları arasında bire bir eşleme bulabilirsiniz [Federasyon için yapılandırılan bir galeri uygulamaya oturumu açmada sorun çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML).

Oturum açma şirket sayfanızda bir hata görürseniz, hata iletisi ve sorunu çözmek için SAML isteğinde gerekir.

### <a name="how-can-i-get-the-error--message"></a>Hata iletisi nasıl alabilirim?

Hata iletisini almak için sayfanın sağ alt köşesinde bakın. İçeren bir hata görürsünüz:

- Bir correlationıd değeri
- Bir zaman damgası
- Bir ileti

![Hata][2] 

 
Bağıntı kimliği ve zaman damgası oturum açma hatası ile ilişkili arka uç günlüklerini bulmak için kullanabileceğiniz benzersiz bir tanımlayıcı oluşturur. Sorununuz için daha hızlı bir çözüm sağlamak için mühendisleri Yardım için Microsoft ile bir destek servis talebi oluşturduğunuzda, bu değerleri önemlidir.

İleti oturum açma sorunun temel nedeni oluşturur. 


### <a name="how-can-i-review-the-saml-request"></a>SAML isteğinde nasıl gözden geçirebilir miyim?

Uygulama Azure Active Directory tarafından gönderilen SAML genellikle son HTTP 200 yanıtı isteğidir [ https://login.microsoftonline.com ](https://login.microsoftonline.com).
 
Fiddler kullanarak, SAML isteğinde bu satırı seçip, ardından seçerek görüntüleyebileceğiniz **denetçiler > WebForms** sağ panelde sekmesindedir. Sağ **SAMLRequest** değer ve ardından **TextWizard için Gönder**. Ardından **gelen Base64** gelen **dönüştürme** belirteç kodunu çözer ve içeriğini görmek için menüsü. 

Ayrıca, ayrıca SAMLrequest değerini kopyalayın ve başka bir Base64 kod çözücü kullanın.

    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    Destination=https://login.microsoftonline.com/<Tenant-ID>/saml2
    AssertionConsumerServiceURL="https://contoso.applicationname.com/acs"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>

SAML isteğinde kodunu çözdü aşağıdakileri gözden geçirin:

1. **Hedef** SAML isteğinde SAML çoklu oturum açma hizmeti Azure Active Directory'den alınan URL'si karşılık gelir.
 
2. **Veren** aynı isteği SAML olduğundan **tanımlayıcısı** Azure Active Directory uygulaması için yapılandırılmış. Azure AD veren dizininizde bir uygulamayı bulmak için kullanır.

3. **AssertionConsumerServiceURL** burada Azure Active Directory'den SAML belirteci almak uygulama bekler değil. Bu değer Azure Active Directory'de yapılandırabilirsiniz ancak SAML isteğinde parçası ise bu zorunlu değildir.


## <a name="i-see-an-error-on-the-applications-page-after-signing-in"></a>Bir hata uygulamanın sayfasında oturum açtıktan sonra görüyorum

Bu senaryoda, uygulama Azure AD tarafından verilen yanıt kabul etmiyor. Eksik veya yanlış ne olduğunu bilmeniz uygulama satıcısı ile SAML belirteci içeren Azure AD'den yanıt doğrulamak önemlidir. 

### <a name="how-can-i-get-and-review-the-saml-response"></a>Nasıl almak ve SAML yanıtını gözden?

SAML belirteci içeren Azure AD'den yanıt genellikle bir HTTP 302 yeniden yönlendirme sonra oluşan biridir https://login.microsoftonline.com ve gönderilmesi için yapılandırılmış **yanıt URL'si** uygulamanın. 

SAML belirteci bu satırı seçip, ardından seçerek görüntüleyebilirsiniz **denetçiler > WebForms** sağ panelde sekmesindedir. Buradan, sağ **SAMLResponse** değer ve seçin **TextWizard için Gönder**. Ardından **gelen Base64** gelen **dönüştürme** belirteç kodunu çözer ve başka bir Base64 kod çözücü kullanma içeriğini görmek için menüsü. 

Ayrıca değer'de kopyalayabilirsiniz **SAMLrequest** ve başka bir Base64 kod çözücü kullanın.

Ziyaret [oturum açtıktan sonra uygulamanın sayfada hata](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML) sorun giderme kılavuzluğu hakkında daha fazla bilgi ne SAML yanıt yanlış veya eksik olabilir.

Yanıt SAML gözden geçirme hakkında bilgi için makalesini ziyaret edin [tek oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML#response).


## <a name="related-articles"></a>İlgili Makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Önceden tümleştirilen uygulamalar için SAML belirtecinde verilen talepler özelleştirme](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png
[2]: ../media/active-directory-saml-debugging/error.png
