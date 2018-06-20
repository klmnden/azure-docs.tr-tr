---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlamayı Yapılandır kutusu | Microsoft Docs'
description: Çoklu oturum açma kutusunu ve Azure Active Directory arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: jeedes
ms.openlocfilehash: 2a2bb427e00e735e5f0dfaa0c85e60afbe890ea6
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36230429"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>Öğretici: Kutusunu otomatik kullanıcı sağlamayı yapılandırın

Bu öğreticinin amacı kutusunu ve Azure AD kutusuna Azure AD'den otomatik sağlama ve devre dışı bırakma sağlama kullanıcı hesaplarına gerçekleştirmeniz gereken adımlar göstermektir.

> [!NOTE]
> Bu öğretici Azure AD kullanıcı sağlama hizmeti üstünde oluşturulmuş bir bağlayıcı açıklar. Bu hizmet ne yaptığını, nasıl çalıştığı ve sık sorulan sorular önemli ayrıntılar için bkz: [otomatikleştirmek kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına etkinleştirmektir](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme kutusuyla yapılandırmak için aşağıdaki öğeleri gerekir:

- Azure AD kiracısı
- Kutusunu iş planı ya da daha iyi

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamında kullanın.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-box"></a>Kullanıcıları kutusuna atama 

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları kutusunu uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar kutusunu uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Kullanıcılar ve gruplar atayın
**Kutusunu > Kullanıcılar ve gruplar** sekme Azure portalında hangi kullanıcıların ve grupların kutusuna erişim verilmesi gerektiğini belirtmenize olanak sağlar. Bir kullanıcı veya grup ataması gerçekleşmesi aşağıdakiler neden olur:

* Azure AD kutusuna kimlik doğrulaması için atanan kullanıcı (ya da doğrudan atama veya grup üyeliği) izin verir. Bir kullanıcı atanmamışsa, Azure AD kutusuna oturum açmak için onları izin vermez ve Azure AD oturum açma sayfasında bir hata döndürür.
* Bir uygulama bölme kutusu için kullanıcının eklenir [uygulama Başlatıcısı](../manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users).
* Otomatik sağlama etkinleştirildiğinde otomatik olarak sağlanacak sağlama kuyruğuna atanan kullanıcılar ve/veya grupları eklenir.
  
  * Yalnızca kullanıcı nesneleri sağlanması için yapılandırılmış olan tüm doğrudan atanan kullanıcılar sağlama sıraya sonra yerleştirilir ve hiçbir atanan gruplarının üyeleri olan tüm kullanıcıları sağlama kuyruğuna yerleştirilir. 
  * Grup nesneleri sağlanacak yapılandırıldıysa, tüm atanmış olan Grup nesneleri kutusu ve bu grupların üyeleri olan tüm kullanıcıları için sağlanır. Grup ve kullanıcı üyeliklerini kutusuna yazılan üzerine korunur.

Kullanabileceğiniz **öznitelikleri > çoklu oturum açma** hangi kullanıcı öznitelikleri (veya talep) kutusuna SAML tabanlı kimlik doğrulaması sırasında sunulur yapılandırmak için sekmesini ve **öznitelikleri > sağlama** nasıl kullanıcı ve grup öznitelikleri Azure AD kutusuna operations sağlama sırasında akış yapılandırmak için sekmesini.

### <a name="important-tips-for-assigning-users-to-box"></a>Kullanıcıların kutusuna atamak için önemli ipuçları 

*   Önerilir tek bir Azure AD kullanıcısının kutusuna sağlama yapılandırmayı test atanmış. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı kutusuna atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD kutunun kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve grup atama göre kutusundaki devre dışı bırak.

Otomatik sağlama etkinleştirildiğinde otomatik olarak sağlanacak sağlama kuyruğuna atanan kullanıcılar ve/veya grupları eklenir.
    
 * Yalnızca kullanıcı nesneleri doğrudan atanan kullanıcılar sağlama kuyruğuna yerleştirilir ve hiçbir atanan gruplarının üyeleri olan tüm kullanıcıları sağlama kuyruğuna yerleştirilir sağlanması için yapılandırılır. 
    
 * Grup nesneleri sağlanacak yapılandırıldıysa, tüm atanmış olan Grup nesneleri kutusu ve bu grupların üyeleri olan tüm kullanıcıları için sağlanır. Grup ve kullanıcı üyeliklerini kutusuna yazılan üzerine korunur.

> [!TIP] 
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma kutusu, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik olarak bir kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının kutusu sağlama etkinleştirme anahat sağlamaktır.

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için kutusu zaten yapılandırdıysanız arama alanı kullanarak kutusunu Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **kutusunu** uygulama galerisinde. Arama sonuçlarından kutusunu işaretleyin ve uygulamaları listenize ekleyin.

3. Örneğiniz kutusunu seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![sağlama](./media/box-userprovisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** 'yi tıklatın **Authorize** kutusunun oturum açma iletişim yeni bir tarayıcı penceresi açın.

6. Üzerinde **kutusuna erişim vermek için oturum açma** sayfasında, gerekli kimlik bilgilerini sağlayın ve ardından **Authorize**. 
   
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/box-userprovisioning-tutorial/IC769546.png "otomatik kullanıcı sağlamayı etkinleştirin")

7. Tıklatın **kutusuna erişim izni verme** bu işlemi yetkilendirmek için ve Azure portalına dönün. 
   
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/box-userprovisioning-tutorial/IC769549.png "otomatik kullanıcı sağlamayı etkinleştirin")

8. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD kutusuna uygulamanıza bağlanabilir. Bağlantı başarısız olursa kutusunu hesabınızın Team yönetici izinlerine sahip olduğundan emin olun ve deneyin **"Yetkilendir"** adım yeniden uygulayın.

9. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

10. Tıklatın **kaydedin.**

11. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları kutusu.**

12. İçinde **öznitelik eşlemelerini** bölümünde, Azure AD'den kutusuna eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri güncelleştirme işlemleri için kullanıcı hesapları kutusunda eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

13. Hizmet kutusu sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

14. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya grupları kullanıcılar ve Gruplar bölümünde kutusunda atanan ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 40 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve kutusunu uygulamanızı sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik günlükleri sağlamak için bağlantıları izleyin.

Günlükleri sağlama Azure AD okuma hakkında daha fazla bilgi için bkz: [otomatik olarak bir kullanıcı hesabı sağlama raporlama](../active-directory-saas-provisioning-reporting.md).

Altında listelenen kutusunu kiracınızda eşitlenmiş kullanıcıları **yönetilen kullanıcılara** içinde **Yönetici Konsolu**.

![Tümleştirme durumu](./media/box-userprovisioning-tutorial/IC769556.png "tümleştirme durumu")


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](box-tutorial.md)
