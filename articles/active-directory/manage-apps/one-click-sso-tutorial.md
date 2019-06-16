---
title: Bir tıklayın SSO Azure AD uygulama galerisinde uygulamanızı yapılandırma | Microsoft Docs
description: Bir tıklayın SSO Azure AD uygulama galerisinde Uygulamanızı yapılandırmak için adımlar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e0416991-4b5d-4b18-89bb-91b6070ed3ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 358240823da469551e254356fc0613bea20d78c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057851"
---
# <a name="one-click-sso-feature-for-azure-ad-gallery-applications"></a>Azure AD galeri uygulamaları için bir tıklayın SSO özelliği

 Bu öğreticide, bir tıklayın SSO için SSO Yapılandırması kullanıcı Arabirimi sağlayan tüm SAML uygulamaları için gerçekleştirme konusunda bilgi edinin.

## <a name="introduction-to-one-click-sso"></a>Tek bir tıklamayla SSO giriş

Çoklu oturum açma SAML protokolü desteği, Azure AD galeri uygulamaları için yapılandırmak için bir SSO tıklayın özellik kullanıma sunulmuştur. Azure AD SSO yapılandırma sayfasında, müşterilerimizin Azure AD'ye meta veriler uygulama tarafta otomatik olarak yapılandırmak için bu seçeneği sağladık. Hedefi, müşterilerin SSO'yu hızlı bir şekilde el ile çok az çabayla ayarlama yardımcı olmaktır. 

## <a name="advantages-of-the-one-click-sso"></a>SSO bir avantajları tıklayın

- Galeri uygulamaları müşterilerin uygulama tarafında el ile Kurulum yapmak için gereken yere hızlı SSO yapılandırması.
- Daha verimli ve doğru şekilde yapılandırma.
- İş ortağı iletişim veya uygulama kurulumu için gereken destek için SAML yapılandırma kullanıcı Arabirimi sağlar.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği uygulamanın OneClick SSO ile yapılandırmak istediğiniz yönetici kimlik bilgilerine sahip.
- **My Apps güvenli oturum açma tarayıcı uzantısı** tarayıcıda yüklü Microsoft gelen. Bu uzantı hakkında daha fazla bilgi edinmek istiyorsanız, şuna başvurun [bağlantı](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="one-click-sso-feature-step-by-step-details"></a>Bir SSO tıklayın özellik adım adım ayrıntıları

1. Uygulamayı Azure AD uygulama galerisinden ekleyin.

2. Çoklu oturum açmayı üzerinde tıklayın.

3. Üzerinde etkinleştirme çoklu oturum açmayı tıklayın.

4. Temel bir SAML yapılandırma bölümünde zorunlu yapılandırma değerlerini doldurun.

    > [!NOTE] 
    > Lütfen uygulama yapılandırmasını özel talepler gerekiyorsa OneClick SSO gerçekleştirmeden önce yapılandırılacakları.

5. Bir tıklayın SSO özelliği için herhangi bir galeri uygulama uygulanmışsa, ekranı görürsünüz. Varsa **My Apps güvenli oturum açma tarayıcı uzantısı** olduğunu zaten yüklü, hakkında'ye tıklamanız **uzantıyı yükleme** seçeneği.

    ![My Apps güvenli oturum açma tarayıcı uzantısı yükleme](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Uzantı tarayıcıya ekledikten sonra tıklayarak **Kurulum uygulama adı** hangi yönlendirilirsiniz uygulama yönetim portalına. Yönetici olarak uygulamaya oturum açmanız gerekir.

    ![Kurulum uygulama adı](./media/one-click-sso-tutorial/setup-sso.png)

7. Tarayıcı uzantısı şimdi uygulamayı sizin için otomatik olarak yapılandırır. Öncelikle, onay ister devam etmek istiyorsanız. **Evet**'e tıklayın.

    ![Verileri otomatik doldurulur](./media/one-click-sso-tutorial/save-autopopulate.png)

    > [!NOTE]
    > Herhangi bir uygulama ek nagivation veya adımları gerekiyorsa, bu adımları gerçekleştirmek için doğru iletileri isteyen görmeniz gerekir. 

8. Yapılandırmasını yaptıktan sonra tıklayın **Tamam** değişiklikleri kaydedin.

    ![Otomatik doldurulan veri kaydetme](./media/one-click-sso-tutorial/save-data.png)

9. Onay başarılı bir açılır ileti görüntülenir ve SSO ayarlarınız başarıyla yapılandırıldı. Ardından, uygulamayı test edebilirsiniz.

    ![Yapılandırılmış SSO](./media/one-click-sso-tutorial/sso-configured.png)

10. Yapılandırma başarıyla tamamlandıktan sonra uygulama oturumu kapatılır ve Azure portalına geri dönersiniz.

11. Çoklu oturum açmayı test etmek için Test düğmesine tıklayabilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [My Apps güvenli oturum açma tarayıcı uzantısı nedir](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 