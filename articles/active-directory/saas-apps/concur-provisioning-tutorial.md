---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için Concur yapılandırma | Microsoft Docs'
description: Azure Active Directory ve Concur arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 441aa9805f2a453e22f207238315125d2a281838
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60280474"
---
# <a name="tutorial-configure-concur-for-automatic-user-provisioning"></a>Öğretici: Concur otomatik kullanıcı hazırlama için yapılandırma

Bu öğreticinin amacı, Concur ve Azure AD sağlama ve sağlamasını Concur Azure AD'den kullanıcı hesaplarına otomatik olarak gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Azure Active directory kiracısı.
*   Concur çoklu oturum açma abonelik etkin.
*   Concur takım Yöneticisi izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-concur"></a>Concur için kullanıcı atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırma ve sağlama hizmetini etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD'de grupları Concur uygulamanıza erişmek isteyen kullanıcılar temsil karar vermeniz gerekir. Karar sonra buradaki yönergeleri izleyerek bu kullanıcılar, Concur uygulamanıza atayabilirsiniz:

[Kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a>Concur için kullanıcı atama önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcı sağlama yapılandırmayı test etmek için Concur atanabilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı için Concur atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirin

Bu bölümde, Azure AD sağlama API'si Concur'ın kullanıcı hesabına bağlanma aracılığıyla size yol gösterir ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirme ve Azure AD'de kullanıcı ve Grup atamasına dayalı Concur atanan kullanıcı hesaplarını devre dışı bırakın.

> [!Tip] 
> Ayrıca seçtiğiniz etkin SAML tabanlı çoklu oturum açma için Concur, yönergeleri izleyerek sağlanan [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarınızı Concur olarak hazırlanmasını etkinleştirmek nasıl anahat sağlamaktır.

Var. harcama hizmet uygulamalarda özelliğini etkinleştirmek için uygun Kurulum ve kullanım Web Hizmeti Yönetim profilinin olması gerekir. WS yönetici rolünü T & E yönetim işlevleri için kullandığınız mevcut yönetici profilinize eklemeyin.

Danışmanlar concur veya istemci Yöneticisi ayrı bir Web hizmeti yönetici profili oluşturmanız gerekir ve istemci yönetici bu profil için Web Hizmetleri Yöneticisi işlevleri (örneğin, etkinleştirme uygulamaları) kullanmanız gerekir. Bu profiller (T & yönetici profil atanan WSAdmin role sahip olmamalıdır) istemci yöneticinin günlük T & E yönetici profili ayrı olarak tutulmalıdır.

Uygulamayı etkinleştirmek için kullanılacak profilini oluşturduğunuzda, istemci yöneticinin adı kullanıcı profili alanlarına girin. Bu profile sahip olma atar. Bir veya daha fazla profil oluşturulduktan sonra istemci tıklayarak bu profili ile oturum oturum açmanız gerekir "*etkinleştirme*" bir iş ortağı uygulaması içinde Web Hizmetleri Menüsü düğmesi.

Aşağıdaki nedenlerle Bu eylem için normal T & E yönetim kullandıkları profiliyle gerçekleştirilmemelidir.

* Tıkladığında bir istemcinin gerekir "*Evet*" uygulama etkinleştirildikten sonra görüntülenen iletişim kutusu penceresinde. Bu istemci verilerini, siz veya iş ortağı, Evet düğmesi tıklayamazsınız şekilde erişmek iş ortağı uygulaması için istekli olup olmadığını bildirir.

* Uygulama etkin bir istemci yönetici T & E yönetici (devre dışı bırakıldı profilinde kaynaklanan) şirket, uygulama ile başka bir etkin WS yönetim profili etkinleştirilene kadar profil çalışmaz kullanarak etkin herhangi bir uygulama profili bırakır. Farklı WS yönetim profilleri oluşturmak için gereken nedeni budur.

* Yönetici şirketten ayrılması durumunda, WS yönetim profiline ilişkili ad etkilemeden bu profili gerek yoktur çünkü etkin uygulama devre dışı bırakıldı isterseniz değiştirme yönetici değiştirilebilir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Concur** Kiracı.

2. Gelen **Yönetim** menüsünde **Web Hizmetleri**.
   
    ![Concur kiracısı](./media/concur-provisioning-tutorial/IC721729.png "Concur kiracısı")

3. Sol taraftaki gelen **Web Hizmetleri** bölmesinde **iş ortağı uygulamasını etkinleştir**.
   
    ![İş ortağı uygulamasını etkinleştir](./media/concur-provisioning-tutorial/ic721730.png "iş ortağı uygulamasını etkinleştir")

4. Gelen **uygulama etkinleştir** listesinden **Azure Active Directory**ve ardından **etkinleştirme**.
   
    ![Microsoft Azure Active Directory](./media/concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. Tıklayın **Evet** kapatmak için **eylemi onaylayın** iletişim.
   
    ![Eylemi Onayla](./media/concur-provisioning-tutorial/ic721732.png "eylemi onaylayın")

6. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

7. Çoklu oturum açma için zaten Concur yapılandırdıysanız, Concur arama alanını kullanarak Örneğiniz için arama yapın. Aksi takdirde seçin **Ekle** araması **Concur** uygulama galerisinde. Arama sonuçlarından Concur seçin ve uygulama listenize ekleyin.

8. Concur örneğinizi seçin ve ardından **sağlama** sekmesi.

9. Ayarlama **hazırlama modu** için **otomatik**. 
 
    ![Sağlama](./media/concur-provisioning-tutorial/provisioning.png)

10. Altında **yönetici kimlik bilgileri** bölümünde, girin **kullanıcı adı** ve **parola** Concur yöneticinizin.

11. Azure portalında **Test Bağlantısı** Azure emin olmak için AD, Concur uygulamanıza bağlanabilirsiniz. Bağlantı başarısız olursa, Concur hesabınızın takım Yöneticisi izinleri olduğundan emin olun.

12. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

13. Tıklayın **kaydedin.**

14. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları Concur.**

15. İçinde **öznitelik eşlemelerini** bölümünde, Concur için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri, Concur kullanıcı hesaplarını güncelleştirme işlemleri eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

16. Azure AD sağlama hizmeti için Concur etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları** bölümü

17. Tıklayın **kaydedin.**

Artık bir test hesabı oluşturabilirsiniz. Hesap için Concur eşitlenmiş doğrulamak için 20 dakika kadar bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırın](concur-tutorial.md)

