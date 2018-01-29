---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Facebook ile çalışma alanına yapılandırma | Microsoft Docs"
description: "Otomatik olarak sağlamak ve Azure AD kullanıcı hesaplarından çalışma Facebook tarafından sağlanmasını öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 70686a48585d83ca5de78fdded99ae46e90cc20c
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlamayı için Facebook ile çalışma alanına yapılandırın

Bu öğreticide adımları Azure Active Directory (Azure AD) için çalışma alanına Facebook tarafından otomatik olarak sağlama ve devre dışı bırakma sağlama kullanıcı hesapları için gerekli da gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Facebook ile çalışma alanına ile Azure AD tümleştirme yapılandırmak için aşağıdakiler gerekir:

- Bir Azure AD aboneliği
- Abonelik çalışma alanına Facebook çoklu oturum açma (SSO) tarafından etkin

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, alabileceğiniz bir [bir aylık deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-workplace-by-facebook"></a>Facebook ile çalışma alanına kullanıcılar atayın

Azure AD "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların Azure AD'de bir uygulamaya atanan eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcıların ve grupların Azure AD'de Facebook uygulama tarafından işyeriniz erişmesi gereken kullanıcıları temsil karar verin. Daha sonra bu kullanıcılar çalışma alanınıza tarafından Facebook uygulaması'ndaki yönergeleri izleyerek atayabilirsiniz [bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   Tek bir atayarak sağlama yapılandırmayı test etme için çalışma alanına Facebook ile Azure AD kullanıcı. Ek kullanıcılar ve gruplar daha sonra atayın.
>*   Facebook ile çalışma alanına bir kullanıcıya atadığınızda, geçerli bir kullanıcı rolünün seçmeniz gerekir. Varsayılan erişim rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, çalışma alanına API Facebook tarafından sağlama kullanıcı hesabı için Azure AD konusunda size rehberlik eder. Ayrıca oluşturmak, güncelleştirmek ve çalışma Facebook tarafından atanan kullanıcı hesapları devre dışı bırakmak için sağlama hizmeti yapılandırmayı öğrenin. Bu kullanıcı ve grup atama Azure AD'de dayanır.

>[!Tip]
>Ayrıca etkin SAML tabanlı SSO tarafından Facebook, çalışma alanı için tarafından sağlanan yönergeleri izleyerek seçebileceğiniz [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayan olsa SSO otomatik sağlamayı bağımsız olarak yapılandırılabilir.

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Kullanıcı hesabı için çalışma alanına Facebook tarafından Azure AD'de sağlama yapılandırın

Azure AD çalışma alanına Facebook tarafından atanan kullanıcı hesabı ayrıntıları otomatik olarak eşitleme özelliği destekler. Bu otomatik eşitleme ilk kez oturum açmaya önlerinde erişim için Kullanıcıları yetkilendirmek için gereken verileri almak için Facebook ile çalışma alanına sağlar. Erişim Azure AD'de iptal edilmiş durumda olduğunda, ayrıca çalışma alanına Facebook tarafından kullanıcılardan XML'deki sağlamasını yapar.

1. İçinde [Azure portal](https://portal.azure.com)seçin **Azure Active Directory** > **Kurumsal uygulamaları** > **tüm uygulamaları**.

2. SSO için Facebook ile çalışma alanına zaten yapılandırdıysanız, çalışma alanına Facebook tarafından Örneğiniz için arama alanı kullanarak arayın. Aksi takdirde seçin **Ekle** arayın ve **Facebook ile çalışma alanına** uygulama galerisinde. Seçin **Facebook ile çalışma alanına** Arama sonuçlarından ve uygulamaları listenize ekleyin.

3. Facebook ile çalışma alanına örneğiniz seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![Çalışma alanına ekran görüntüsü tarafından Facebook sağlama seçenekleri](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, girin **gizli belirteci** ve **Kiracı URL** Facebook yönetici tarafından çalışma alanı.

6. Azure portalında seçin **Bağlantıyı Sına** Azure emin olmak için AD çalışma alanına Facebook uygulaması tarafından bağlanabilirler. Bağlantı başarısız olursa işyeriniz Facebook hesabına göre takım yönetici izinleri olduğundan emin olun.

7. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

8. **Kaydet**’i seçin.

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için çalışma alanına Facebook tarafından**.

10. İçinde **öznitelik eşlemelerini** bölümünde, Facebook ile eşitlenmiş Azure AD çalışma alanına kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikler kullanılan çalışma alanına kullanıcı hesaplarında eşleştirmek için Facebook tarafından güncelleştirme işlemleri için. Tüm değişiklikleri yürütmek için seçin **kaydetmek**.

11. Azure AD çalışma alanına Facebook tarafından hizmet sağlamayı etkinleştirmek için **ayarları** bölümünde **sağlama durumu** için **üzerinde**.

12. **Kaydet**’i seçin.

Otomatik sağlama yapılandırma hakkında daha fazla bilgi için bkz: [Facebook belgelerine](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Şimdi sınama hesabı oluşturabilirsiniz. Hesap çalışma alanına Facebook ile eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-facebook-at-work-tutorial.md)

