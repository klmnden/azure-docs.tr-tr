---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için yapılandırma kutusu | Microsoft Docs'
description: Azure Active Directory ve kutusu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd7826455624ca4a84d668455f522cbde411ac8b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60431765"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>Öğretici: Yapılandırma kutusu için otomatik kullanıcı hazırlama

Bu öğreticinin amacı kutusu ve Azure AD kutusuna Azure AD'den otomatik olarak sağlama ve devre dışı bırakma sağlama kullanıcı hesapları için gerçekleştirmeniz gereken adımlar göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi kutusuyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD kiracısı
- Kutusu iş planı ya da daha iyi

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bunu yapmanızı öneririz *değil* bir üretim ortamı kullanın.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assigning-users-to-box"></a>Box'a kullanıcı atama 

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Box uygulamanızı erişmesi gereken kullanıcıları temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar Box uygulamanızı atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Kullanıcıları ve grupları atama
**Kutusu > kullanıcıları ve grupları** Azure portalında sekme kutusuna hangi kullanıcıların ve grupların erişim verilmesi gerektiğini belirtmenize olanak sağlar. Bir kullanıcı veya grup atamasını gerçekleşmesi aşağıdakiler neden olur:

* Azure AD kutusuna kimlik doğrulaması için atanan kullanıcı (ya da doğrudan atama veya grup üyeliği) izin verir. Bir kullanıcı atanmamışsa, Azure AD kutusuna oturum açmak için bunları izin vermez ve Azure AD oturum açma sayfasında bir hata döndürür.
* Bir uygulama kutucuğunda kutusu için kullanıcının eklenen [uygulama Başlatıcısı](../manage-apps/end-user-experiences.md).
* Otomatik sağlama etkinleştirildiğinde otomatik olarak sağlanacak sağlama kuyruğa atanan kullanıcılar ve/veya grupları eklenir.
  
  * Yalnızca bir kullanıcı, nesneyi sağlanması için yapılandırılmış olan, ardından doğrudan atanmış tüm kullanıcılar sağlama kuyruğuna yerleştirilir ve atanan tüm grupların üyeleri olan tüm kullanıcılar sağlama kuyruğuna yerleştirilir. 
  * Grup nesnelerini sağlanması için yapılandırıldıysa, tüm atanmış Grup nesnelerini kutusu ve bu grupların üyeleri olan tüm kullanıcılar için sağlanır. Grup ve kullanıcı üyeliklerini kutusuna yazılan sırasında korunur.

Kullanabileceğiniz **öznitelikleri > çoklu oturum açma** hangi kullanıcı özniteliklerine (veya talep) kutusuna SAML tabanlı kimlik doğrulaması sırasında sunulan yapılandırmak için sekmesinde ve **öznitelikleri > sağlama** için sekmesinde nasıl kullanıcı ve grup öznitelikleri Azure AD'den kutusuna operations sağlama sırasında akışı yapılandırın.

### <a name="important-tips-for-assigning-users-to-box"></a>Box'a kullanıcı atama önemli ipuçları 

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmasını test etmek için kutusuna atanmış. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Box'a kullanıcı atama, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD sağlama API'si kutunun kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve sağlama hizmeti oluşturmak için yapılandırma güncelleştirmesi ve atanan kullanıcı hesapları Azure AD'de kullanıcı ve Grup atamasına dayalı kutusunda devre dışı bırak.

Otomatik sağlama etkinleştirildiğinde otomatik olarak sağlanacak sağlama kuyruğa atanan kullanıcılar ve/veya grupları eklenir.
    
 * Yalnızca kullanıcı nesneleri doğrudan atanan kullanıcılar sağlama kuyruğuna yerleştirilir ve atanan tüm grupların üyeleri olan tüm kullanıcılar sağlama kuyruğuna yerleştirilir sağlanacak yapılandırılır. 
    
 * Grup nesnelerini sağlanması için yapılandırıldıysa, tüm atanmış Grup nesnelerini kutusu ve bu grupların üyeleri olan tüm kullanıcılar için sağlanır. Grup ve kullanıcı üyeliklerini kutusuna yazılan sırasında korunur.

> [!TIP] 
> Uygulamayı da seçebilirsiniz SAML tabanlı çoklu oturum açma kutusu etkin olarak, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının kutusunu sağlamayı etkinleştirmek nasıl anahat sağlamaktır.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için kutusu zaten yapılandırdıysanız arama alanını kullanarak kutusunun Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **kutusu** uygulama galerisinde. Arama sonuçlarından kutusunu seçin ve uygulama listenize ekleyin.

3. Örneğiniz kutusunu seçin ve ardından **sağlama** sekmesi.

4. Ayarlama **hazırlama modu** için **otomatik**. 

    ![Sağlama](./media/box-userprovisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** bölümünde **Authorize** bir oturum açma iletişim kutusu yeni bir tarayıcı penceresinde açın.

6. Üzerinde **kutusuna erişim vermek için oturum açma** sayfasında, gerekli kimlik bilgilerini sağlayın ve ardından **Authorize**. 
   
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/box-userprovisioning-tutorial/IC769546.png "otomatik kullanıcı sağlamayı etkinleştirin")

7. Tıklayın **kutusuna erişim ver** bu işlem son noktanın yetkilendirilmesi için ve Azure portalına dönün. 
   
    ![Otomatik kullanıcı sağlamayı etkinleştirin](./media/box-userprovisioning-tutorial/IC769549.png "otomatik kullanıcı sağlamayı etkinleştirin")

8. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, Box uygulamanızı bağlanabilirsiniz. Bağlantı başarısız olursa, Box hesabınıza takım Yöneticisi izinlerine sahip olduğundan emin olun ve deneyin **"Yetkilendir"** adım yeniden uygulayın.

9. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

10. Tıklayın **kaydedin.**

11. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları kutusu.**

12. İçinde **öznitelik eşlemelerini** bölümünde, kutusuna Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri kutusunda kullanıcı hesapları için güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

13. Azure AD sağlama hizmeti kutusu etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** Ayarlar bölümünde

14. Tıklayın **kaydedin.**

Herhangi bir kullanıcı ve/veya Box'a kullanıcılar ve Gruplar bölümünde atanan gruplar ilk eşitleme başlar. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve Box uygulamanızı üzerindeki sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklayan etkinlik günlüklerini sağlama için bağlantıları izleyin.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

Eşitlenmiş kullanıcılar listelenir kutusuna kiracınızda **yönetilen kullanıcılara** içinde **Yönetici Konsolu**.

![Tümleştirme durumu](./media/box-userprovisioning-tutorial/IC769556.png "tümleştirme durumu")


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](box-tutorial.md)
