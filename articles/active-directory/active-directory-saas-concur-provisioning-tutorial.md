---
title: "Öğretici: Azure Active Directory Tümleştirme ile Concur | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile Concur arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: cd35b6e2dc3171e9cffdb820bbc5b0d45ff58e07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a>Öğretici: Yapılandırma Concur kullanıcı sağlamak için

Bu öğreticinin amacı Concur ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den Concur sağlanmasını gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir Concur çoklu oturum açma abonelik etkin.
*   Concur takım yönetici izinlerine sahip bir kullanıcı hesabının.

## <a name="assigning-users-to-concur"></a>Kullanıcılar için Concur atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları Concur uygulamanıza erişimi olması gereken kullanıcılar temsil eden karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek, bu kullanıcılar Concur uygulamanıza atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a>Kullanıcılar için Concur atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının sağlama yapılandırmayı test etmek için Concur atanabilir. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Concur atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD Concur'ın kullanıcı hesabına API sağlama konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama göre Concur atanan kullanıcı hesaplarında devre dışı bırakın.

> [!Tip] 
> Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma için Concur, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Concur sağlama etkinleştirme anahat sağlamaktır.

Var. harcama hizmet uygulamalarda etkinleştirmek için uygun Kurulum ve kullanım Web Hizmeti Yönetim profilinin olması gerekir. WS yönetici rolünü, T & E yönetim işlevleri için kullandığınız varolan yönetici profilinizin eklemeyin.

Danışmanlar concur veya istemci Yöneticisi ayrı bir Web hizmeti yönetici profili oluşturmanız gerekir ve istemci Yöneticisi bu profil için Web Services Yöneticisi işlevleri (örneğin, etkinleştirme uygulamalar) kullanmanız gerekir. Bu profiller (T & E yönetim profili atanan WSAdmin rolü olmamalıdır) istemci yöneticinin günlük T & E yönetici profilinden ayrı tutulmalıdır.

Uygulama etkinleştirmek için kullanılacak profili oluşturduğunuzda, istemci yöneticinin adı kullanıcı profili alanlarına girin. Bu profile sahipliği atar. Bir veya daha fazla profil oluşturulur sonra istemci tıklatın için bu profili ile oturum açmanız gerekir "*etkinleştirmek*" düğmesi için bir iş ortağı uygulama Web Hizmetleri menü içinde.

Aşağıdaki nedenlerle Bu eylem normal T & E Yönetim için kullandıkları profille yapılmalıdır değil.

* İstemci tıklar biri olması gerekir "*Evet*" uygulama etkinleştirildikten sonra görüntülenen iletişim penceresinde. Bu, istemci, veya iş ortağı bu Evet düğmesini tıklatın edilemez şekilde kendi verilere erişmek iş ortağı uygulamasını için istekli olup bildirir.

* Bir uygulama etkinleştirilmiş bir istemci yönetici T & E yönetici profili (devre profilinde kaynaklanan) şirket olan, uygulama ile başka bir etkin WS yönetim profili etkinleştirilene kadar profili çalışmaz etkinleştirilmiş uygulamalardan bırakır. Ayrı WS yönetim profilleri oluşturmak için beklenen nedeni budur.

* Bir yönetici şirketten ayrılması durumunda WS yönetim profiline ilişkili adı etkilemeden bu profili gerek yoktur çünkü etkin uygulama devre isterseniz değiştirme yönetici değiştirilebilir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Concur** Kiracı.

2. Gelen **Yönetim** menüsünde, select **Web Hizmetleri**.
   
    ![Concur kiracısı](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur kiracısı")

3. Sol taraftaki gelen **Web Hizmetleri** bölmesinde, **iş ortağı uygulamasını etkinleştir**.
   
    ![İş ortağı uygulamasını etkinleştir](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "iş ortağı uygulamasını etkinleştir")

4. Gelen **etkinleştirmek uygulama** listesinde **Azure Active Directory**ve ardından **etkinleştirmek**.
   
    ![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. Tıklatın **Evet** kapatmak için **eylemi onaylayın** iletişim.
   
    ![Eylemi onaylamak](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "eylemi onaylayın")

6. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

7. Çoklu oturum açma için Concur zaten yapılandırdıysanız arama alanı kullanarak Concur Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** arayın ve **Concur** uygulama galerisinde. Arama sonuçlarından Concur seçin ve uygulamaları listenize ekleyin.

8. Concur örneğiniz seçin ve ardından **sağlama** sekmesi.

9. Ayarlama **sağlama modunda** için **otomatik**. 
 
    ![Sağlama](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. Altında **yönetici kimlik bilgileri** bölümünde, girin **kullanıcı adı** ve **parola** Concur yöneticinizin.

11. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD Concur uygulamanıza bağlanabilir. Bağlantı başarısız olursa Concur hesabınızın Team yönetici izinleri olduğundan emin olun.

12. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

13. Tıklatın **kaydedin.**

14. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Concur.**

15. İçinde **öznitelik eşlemelerini** bölümünde, Concur için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri Concur kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

16. Azure AD hizmeti Concur için sağlama etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

17. Tıklatın **kaydedin.**

Şimdi sınama hesabı oluşturabilirsiniz. Hesap için Concur eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-concur-tutorial.md)

