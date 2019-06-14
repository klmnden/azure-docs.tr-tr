---
title: 'Öğretici: Facebook ile çalışma alanına Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma | Microsoft Docs'
description: Azure Active Directory ve Facebook ile iş yeri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72c2e23b0d60ca242549ebf2c058ea8f44f2b1c8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60520151"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Öğretici: Facebook ile çalışma için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı, otomatik olarak sağlama ve sağlamasını Facebook tarafından çalışma alanı için Azure AD'den kullanıcı hesapları için Facebook ve Azure AD çalışma alanına gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi iş yeri tarafından Facebook ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Facebook çoklu oturum açma etkin abonelik tarafından bir çalışma alanı

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-workplace-by-facebook"></a>Facebook ile çalışma alanına kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Facebook uygulama tarafından çalışma alanınızı erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra bu kullanıcılar çalışma alanınıza Facebook uygulama tarafından Buradaki yönergeleri izleyerek atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a>Facebook ile çalışma alanına kullanıcı atama önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı atandığı çalışma alanına Facebook tarafından sağlama yapılandırmasını test etmek için. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Facebook ile çalışma alanına kullanıcı atama, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD çalışma alanına sağlama API'si, Facebook'ın kullanıcı hesabı tarafından bağlama size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve çalışma alanına kullanıcı ve Grup dayanan Facebook tarafından atanan kullanıcı hesaplarını devre dışı bırak Azure AD'de atama.

>[!Tip]
>Uygulamayı da seçebilirsiniz Facebook tarafından çalışma alanı için SAML tabanlı çoklu oturum açmayı etkinleştirmek, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Hesabı için çalışma alanına Facebook tarafından Azure AD'de kullanıcı sağlamayı yapılandırmak için:

Bu bölümün amacı, Facebook tarafından çalışma alanı için Active Directory kullanıcı hesaplarının sağlamayı etkinleştirme adımlarını özetler sağlamaktır.

Azure AD'ye otomatik olarak atanan kullanıcılar için çalışma alanına göre Facebook hesabı ayrıntılarını eşitleme özelliğini destekler. Bu otomatik eşitleme, bunları ilk kez oturum açmayı denemeden önce erişim için Kullanıcıları yetkilendirmek için ihtiyacı olan verileri almak için Facebook tarafından çalışma alanı sağlar. Azure AD'de erişimi iptal edilmiş olduğunda, ayrıca kullanıcıların Facebook ile çalışma alanına geri sağlamaları sildiğinde.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tümuygulamalar** bölümü.

2. Çoklu oturum açma için Facebook ile çalışma alanına zaten yapılandırdıysanız, örneğiniz tarafından arama alanını kullanarak Facebook çalışma alanına arayın. Aksi takdirde seçin **Ekle** araması **Facebook ile çalışma alanına** uygulama galerisinde. Arama sonuçlarından Facebook tarafından çalışma alanı seçin ve uygulamalar listesine ekleyin.

3. Örneğinizi Facebook tarafından çalışma alanı seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**. 

    ![Sağlama](./media/workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde çalışma alanınızı Facebook Yöneticisi tarafından erişim belirteci girin ve Kiracı URL'si değerine `https://www.facebook.com/scim/v1/` . Bu [yönergeleri](https://developers.facebook.com/docs/workplace/integrations/custom-integrations/apps) çalışma alanı için bir erişim belirteci oluşturma. 

6. Azure portalında **Test Bağlantısı** Azure emin olmak için AD alanınıza Facebook uygulama tarafından bağlanabilir. Bağlantı başarısız olursa, çalışma alanınızı Facebook hesabı ve takım Yöneticisi izinlerine sahip olun.

7. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

8. Tıklayın **kaydedin.**

9. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için Facebook ile çalışma.**

10. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD çalışma alanına Facebook ile eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri kullanılır çalışma alanına kullanıcı hesaplarında eşleştirmek için Facebook ile güncelleştirme işlemleri için. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

11. Azure AD sağlama hizmeti için çalışma alanına Facebook ile etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

12. Tıklayın **kaydedin.**

Otomatik sağlama yapılandırma hakkında daha fazla bilgi için bkz. [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

Artık bir test hesabı oluşturabilirsiniz. Çalışma alanına Facebook ile eşitlenmiş hesabı doğrulamak için 20 dakika kadar bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](workplacebyfacebook-tutorial.md)
