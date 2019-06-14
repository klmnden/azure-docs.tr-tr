---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Salesforce yapılandırma | Microsoft Docs'
description: Azure Active Directory ile Salesforce arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 712cc5ce62225987f8cc3ea13b5e4fd10a7d5eaf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515757"
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için Salesforce yapılandırın

Bu öğreticinin amacı otomatik olarak sağlamak için Salesforce ve Azure AD gerçekleştirme adımları gerekir ve kaldırabilir kullanıcının Azure AD'den Salesforce hesaplarını göstermektir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Bir Azure Active directory kiracısı
* Salesforce.com Kiracı

> [!IMPORTANT]
> Ardından bir Salesforce.com deneme hesabı kullanıyorsanız, otomatik kullanıcı sağlamayı yapılandırma mümkün olmayacaktır. Deneme hesapları satın alınan kadar etkin gerekli API erişimi yok. Ücretsiz'i kullanarak bu sınırlandırma çerçevesinde alabilirsiniz [Geliştirici hesabı](https://developer.salesforce.com/signup) Bu öğreticiyi tamamlamak için.

Salesforce korumalı ortam kullanıyorsanız, lütfen bkz [Salesforce korumalı alan tümleştirme Öğreticisi](https://go.microsoft.com/fwLink/?LinkID=521879).

## <a name="assigning-users-to-salesforce"></a>Salesforce kullanıcıları atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcıların veya grupların Azure AD'de Salesforce uygulamanızı erişmesi karar vermeniz gerekir. Bu karar yaptıktan sonra bu kullanıcıların Salesforce uygulamanızı yönergelerini takip ederek atayabilirsiniz [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Salesforce kullanıcıları atama önemli ipuçları

* Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Salesforce atanır. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcı için Salesforce atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmıyor

    > [!NOTE]
    > Bu uygulama profilleri, Salesforce müşteri kullanıcıların Azure AD'de atarken seçmek isteyebilirsiniz sağlama işleminin bir parçası olarak içeri aktarır. Salesforce'tan alınır profillerini Azure AD'de rolleri olarak göründüğünü unutmayın.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si Salesforce'nın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı salesforce'ta devre dışı bırak.

> [!Tip]
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için Salesforce, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı yapılandırma

Bu bölümün amacı, Active Directory kullanıcı hesaplarının salesforce'a kullanıcı sağlamayı etkinleştirin adımlarını özetler sağlamaktır.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için Salesforce zaten yapılandırdıysanız arama alanını kullanarak Salesforce Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Salesforce** uygulama galerisinde. Arama sonuçlarından Salesforce seçin ve uygulama listenize ekleyin.

3. Salesforce örneğinizi seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**.

    ![Sağlama](./media/salesforce-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde, aşağıdaki yapılandırma ayarları sağlayın:

    a. İçinde **yönetici kullanıcı adı** metin kutusuna bir Salesforce hesabı adı **Sistem Yöneticisi** atanan Salesforce.com profilinde.

    b. İçinde **yönetici parolası** metin kutusuna bu hesabın parolasını yazın.

6. Salesforce güvenlik belirtecini almak için aynı Salesforce yönetici hesabı yeni bir sekme ve oturum açın. Sayfanın sağ üst köşesindeki adınıza tıklayın ve ardından **ayarları**.

    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/salesforce-provisioning-tutorial/sf-my-settings.png "otomatik kullanıcı sağlamayı etkinleştirin")

7. Sol gezinti bölmesinde **kişisel Bilgilerim** ilgili bölümü genişletin ve ardından **sıfırlama My güvenlik belirteci**.
  
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/salesforce-provisioning-tutorial/sf-personal-reset.png "otomatik kullanıcı sağlamayı etkinleştirin")

8. Üzerinde **güvenlik belirteci sıfırlama** sayfasında **güvenlik belirteci sıfırlama** düğmesi.

    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/salesforce-provisioning-tutorial/sf-reset-token.png "otomatik kullanıcı sağlamayı etkinleştirin")

9. Bu yönetici hesabı ile ilişkili e-posta gelen kutusunu kontrol edin. Salesforce.com yeni güvenlik belirteci içeren bir e-posta arayın.

10. Belirteci kopyalayın, Azure AD pencerenize gidin ve yapıştırın **gizli belirteç** alan.

11. **Kiracı URL'si** Salesforce üzerinde Salesforce kamu Bulutu örneğiyse girilmesi gerekir. Aksi halde isteğe bağlıdır. Biçimi kullanarak Kiracı URL'si girin "https://\<örneği your\>. my.salesforce.com," değiştirerek \<örneği your\> Salesforce örneğinizin adı ile.

12. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, Salesforce uygulamanızı bağlanabilirsiniz.

13. İçinde **bildirim e-posta** alan, bir kişi veya grubun sağlama hata bildirimleri alabilir ve aşağıdaki onay kutusunu işaretleyin e-posta adresini girin.

14. Tıklayın **kaydedin.**  

15. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları için Salesforce.**

16. İçinde **öznitelik eşlemelerini** bölümünde, Salesforce Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler olarak Not **eşleşen** özellikleri güncelleştirme işlemleri için Salesforce kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

17. Azure AD sağlama hizmeti için Salesforce etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

18. Tıklayın **kaydedin.**

Bu, herhangi bir kullanıcı ve/veya salesforce'a kullanıcılar ve Gruplar bölümünde atanan gruplar ilk eşitlemeyi başlatır. İlk eşitleme hizmetini çalıştıran sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Salesforce uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
