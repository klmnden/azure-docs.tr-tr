---
title: Çoklu oturum açma, Azure Active Directory'de kurumsal uygulamalar için Yönetim | Microsoft Docs
description: Azure Active Directory Uygulama galerisinden kuruluşunuzdaki kurumsal uygulamalar için çoklu oturum açma ayarları yönetme
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: 81b959a08f55f13fd0bcadce32b65ba64f9bb270
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365500"
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Kurumsal uygulamalar için çoklu oturum açmayı yönetme

Bu makalede nasıl kullanılacağını [Azure portalında](https://portal.azure.com) kurumsal uygulamalar için çoklu oturum açma ayarları yönetmek için. Kurumsal uygulamalar, dağıtıldığı ve kuruluşunuzda kullanılan uygulamalardır. Bu makale özellikle gelen eklenmiş olan uygulamalar için geçerlidir [Azure Active Directory Uygulama galerisinde](what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-the-portal"></a>Portalda uygulamalarınızı bulma
Tüm kurumsal uygulamaları için çoklu oturum açmayı ayarlama görüntülenebilir ve Azure portalında yönetilir. Uygulamaları bulunabilir **tüm hizmetleri** &gt; **kurumsal uygulamalar** Portalı'nın bölümü. 

![Kurumsal uygulamalar dikey penceresi](./media/configure-single-sign-on-portal/enterprise-apps-blade.png)

Seçin **tüm uygulamaları** yapılandırılmış olan tüm uygulamaların bir listesini görüntülemek için. Bir uygulamayı seçtikten burada raporları bu uygulama için görüntülenebilir ve çeşitli ayarları yönetilen uygulamayı, ilgili kaynakları görüntüler.

Çoklu oturum açma ayarları yönetmek için seçin **çoklu oturum açma**.

![Uygulama kaynağı dikey penceresi](./media/configure-single-sign-on-portal/enterprise-apps-sso-blade.png)

## <a name="single-sign-on-modes"></a>Çoklu oturum açma modları
**Çoklu oturum açma** ile başlayan bir **modu** yapılandırılması için çoklu oturum açma modunu sağlayan menüsü. Kullanılabilir seçenekler şunlardır:

* **SAML tabanlı oturum açma** -bu seçenek Azure WS-Federation, SAML 2.0 protokolünü kullanarak Active Directory ile tam Federasyon çoklu oturum açma uygulamanın desteklediği veya Openıd connect protokolleri kullanılabilir.
* **Parola tabanlı oturum açma** -bu seçenek, Azure AD parola formu doldurarak bu uygulama için destekliyorsa kullanılabilir.
* **Bağlantılı oturum açma** -"Varolan çoklu oturum açma" multi-Device Yöneticiler, kullanıcının Azure AD erişim paneli veya Office 365 uygulama başlatıcısında bu uygulamanın bağlantısını yerleştirmek bu seçeneği sağlar.

Bu modları hakkında daha fazla bilgi için bkz. [nasıl çoklu oturum açma Azure Active Directory ile](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML tabanlı oturum açma
**SAML tabanlı oturum açma** seçeneği dört bölümlere ayrılmıştır:

### <a name="domains-and-urls"></a>Etki alanları ve URL'ler
Azure AD dizininizi uygulama etki alanı ve URL'ler hakkında tüm ayrıntıları burada eklenen budur. İsteğe bağlı tüm girişler seçerek görüntülenebilir ancak oturum tek iş uygulaması yapmak için gereken tüm giriş ekranında doğrudan görüntülenen **Gelişmiş URL ayarlarını göster** onay kutusu. Desteklenen giriş tam listesini içerir:

* **oturum açma URL'si** – burada kullanıcının uygulamada oturum gider. Uygulama, hizmet sağlayıcısı tarafından başlatılan çoklu oturum açma, bu URL'yi bir kullanıcı oturum açtığında gerçekleştirmek için yapılandırıldıysa, Azure ad kimlik doğrulaması ve kullanıcı oturum açmak için hizmet sağlayıcısı yönlendirir. 
  * Bu alan doldurulursa, Azure AD uygulama Office 365 ve Azure AD erişim paneli başlatmak için URL'yi kullanır.
  * Bu alan atlanırsa, ardından Azure AD bunun yerine kimlik sağlayıcısı tarafından başlatılan oturum açma, Azure AD erişim paneli, Office 365'ten uygulama başlatıldığında gerçekleştirir veya Azure AD çoklu oturum açma URL'si.
* **Tanımlayıcı** -bu URI için çoklu oturum açma yapılandırılmış uygulama benzersiz şekilde tanımlamak. Bu Azure AD uygulama geri SAML belirteci için hedef kitle parametre olarak gönderen değerdir ve doğrulamak için uygulamayı bekleniyor. Bu değer, uygulama tarafından sağlanan herhangi bir SAML meta verilerinde varlık kimliği olarak görünür.
* **Yanıt URL'si** -burada SAML belirteci almak uygulamanın beklediği yanıt URL'dir. Bu onaylama tüketici hizmeti (ACS) URL'si olarak da adlandırılır. Bunlar girildikten sonra sonraki ekrana devam etmek için İleri'ye tıklayın. Bu ekranda, Azure AD'de SAML belirteci kabul etmek üzere etkinleştirmek için uygulama tarafından yapılandırılması için gerekenler hakkında bilgi sağlar.
* **Geçiş durumu** -kimlik doğrulaması tamamlandıktan sonra kullanıcı yeniden yönlendirileceği uygulamaya bildirmek yardımcı olabilecek isteğe bağlı parametresi geçiş durumudur. Bazı uygulamalar farklı Bu alan kullanır ancak genellikle uygulama geçerli bir URL değeridir (Ayrıntılar için uygulamanın çoklu oturum açma belgelerine bakın). Geçiş durumunu ayarlama özelliği, yeni Azure portalına benzersiz olan yeni bir özelliktir.

### <a name="user-attributes"></a>Kullanıcı Öznitelikleri
Yöneticiler burada görüntüleyebilir ve kullanıcılar oturum gönderilen SAML belirtecindeki uygulamayı her Azure AD sorunlarını öznitelikleri Düzenle budur.

Desteklenen yalnızca düzenlenebilir özniteliği **kullanıcı tanımlayıcısı** özniteliği. Bu özniteliğin değeri Azure AD'de her kullanıcının uygulamadaki benzersiz olarak tanımlayan alandır. Uygulama kullanıcı adı ve benzersiz tanımlayıcı olarak "e-posta adresi" kullanarak dağıtıldıysa, örneğin, ardından değeri "user.mail" alanına Azure AD'de ayarlanır.

### <a name="saml-signing-certificate"></a>SAML İmzalama Sertifikası
Bu bölümde, Azure AD, her zaman kullanıcının kimliğini doğrular uygulamaya verilen SAML belirteçlerini imzalamak için kullandığı sertifikayı ayrıntılarını gösterir. Denetlenecek, geçerli bir sertifikanın özelliklerini, sona erme tarihi dahil olmak üzere sağlar.

### <a name="application-configuration"></a>Uygulama yapılandırması
Son bölüm, belgeleri ve/veya Azure Active Directory kimlik sağlayıcısı olarak kullanmak için uygulamayı yapılandırmak için gereken denetimleri sağlar.

**Uygulama Yapılandır** çıkış menü uygulamayı yapılandırmak için yeni kısa, katıştırılmış yönergeleri sağlar. Yeni Azure portalına benzersiz başka bir yeni özellik budur.

> [!NOTE]
> Salesforce.com uygulama embedded belgeleri tam bir örnek için bkz. Ek uygulamalar için belge sürekli olarak ekleniyor.
> 
> 

![Embedded belgeleri](./media/configure-single-sign-on-portal/enterprise-apps-blade-embedded-docs.png)

## <a name="password-based-sign-on"></a>Parola tabanlı oturum açma
Uygulama için destekleniyorsa, parola tabanlı SSO modu ve seçerek **Kaydet** anında parola tabanlı SSO yapmak için yapılandırır. Parola tabanlı SSO dağıtma hakkında daha fazla bilgi için bkz. [nasıl çoklu oturum açma Azure Active Directory ile](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

![Parola tabanlı oturum açma](./media/configure-single-sign-on-portal/enterprise-apps-blade-password-sso.png)

## <a name="linked-sign-on"></a>Bağlantılı oturum açma
Uygulama için destekleniyorsa, bağlantılı SSO modu seçmek istediğiniz Azure AD erişim paneli veya Office 365 kullanıcılar bu uygulamayı tıklattığında yönlendirileceği URL'yi girmenizi sağlar. Bağlantılı SSO (eski adıyla. "mevcut SSO") hakkında daha fazla bilgi için bkz: [nasıl çoklu oturum açma Azure Active Directory ile](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

![Bağlantılı oturum açma](./media/configure-single-sign-on-portal/enterprise-apps-blade-linked-sso.png)

## <a name="feedback"></a>Geri Bildirim

Geliştirilmiş kullanma gibi umuyoruz Azure AD deneyimi. Lütfen geri bildirim yakında tutun! Geri bildirim ve geliştirme için fikir gönderin **Yönetici portalı** bölümünü bizim [geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Size her gün harika yeni öğeler oluşturma hakkında çok heyecanlıyız ve kılavuzunuzu şekle kullanın ve sonraki geliştirmemiz ne tanımlayın.

