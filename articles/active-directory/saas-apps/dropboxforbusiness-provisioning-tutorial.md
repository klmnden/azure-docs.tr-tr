---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Dropbox'ı yapılandırma | Microsoft Docs"
description: Azure Active Directory ve iş için Dropbox arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d53d10b036a37489be0b7aae6208880044b766a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60280006"
---
# <a name="tutorial-configure-dropbox-for-business-for-automatic-user-provisioning"></a>Öğretici: İş için Dropbox için otomatik kullanıcı hazırlama yapılandırın

Bu öğreticinin amacı, iş ve Azure AD'ye otomatik olarak sağlama ve sağlamasını dropbox'a iş için Azure AD'den kullanıcı hesapları için Dropbox gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Bir Dropbox iş çoklu oturum açma etkin aboneliği için.
*   İş için Dropbox takım Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-dropbox-for-business"></a>İş için Dropbox kullanıcıları atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları için iş kolu uygulaması, dropbox'a erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, dropbox'a iş uygulaması için bu kullanıcılara atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>İş için Dropbox kullanıcıları atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını test etmek iş için Dropbox atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   İş için Dropbox için kullanıcı atama, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz...

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si işletmenin kullanıcı hesabı için Dropbox'a bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları için kullanıcı ve Grup dayanan iş dropbox'ta devre dışı bırak Azure AD'de atama.

>[!Tip]
>Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için iş için Dropbox, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırmak için:

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için zaten iş için Dropbox yapılandırdıysanız, Örneğiniz için arama alanını kullanarak iş Dropbox arayın. Aksi takdirde seçin **Ekle** araması **iş için Dropbox** uygulama galerisinde. Arama sonuçlarından iş için Dropbox'ı seçin ve uygulama listenize ekleyin.

3. İş için Dropbox'ın örneğinizi seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**. 

    ![sağlama](./media/dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde **Authorize**. Bir Dropbox iş oturum açma iletişim kutusu için yeni bir tarayıcı penceresinde açar.

6. Üzerinde **oturum açma için Azure AD'ye bağlamak için Dropbox** iletişim kutusunda, Dropbox iş Kiracı için oturum açın.

     ![Kullanıcı sağlamayı](./media/dropboxforbusiness-provisioning-tutorial/ic769518.png "kullanıcı sağlama")

7. Dropbox iş Kiracı için değişiklik yapmak için Azure Active Directory izin vermek istediğinizi onaylayın. Tıklayın **izin**.
    
      ![Kullanıcı sağlamayı](./media/dropboxforbusiness-provisioning-tutorial/ic769519.png "kullanıcı sağlama")

8. Azure portalında **Test Bağlantısı** Azure emin olmak için AD iş uygulaması için Dropbox bağlanabilirsiniz. Bağlantı başarısız olursa, iş hesabı takım Yöneticisi izinleri için Dropbox'emin olun ve deneyin **"Yetkilendir"** adım yeniden uygulayın.

9. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

10. Tıklayın **kaydedin.**

11. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları dropbox'a işletmeler için.**

12. İçinde **öznitelik eşlemelerini** bölümünde, dropbox'a iş için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için iş için Dropbox kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

13. Azure AD sağlama hizmeti için iş için Dropbox'ı etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

14. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya Dropbox for Business kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve iş uygulaması için Dropbox üzerinde'sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](dropboxforbusiness-tutorial.md)
