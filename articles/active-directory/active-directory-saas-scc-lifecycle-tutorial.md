---
title: "Öğretici: Azure Active Directory Tümleştirme SCC yaşam | Microsoft Docs"
description: "Çoklu oturum açma, otomatik sağlama ve daha fazla etkinleştirmek için Azure Active Directory ile SCC yaşam döngüsü kullanmayı öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a30bcca720ff135d0180d73f46e78403e9bca43
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Öğretici: Azure Active Directory Tümleştirme ile SCC yaşam döngüsü
Bu öğreticinin amacı, Azure ve SCC yaşam döngüsü tümleştirmesini göstermektir.  

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Bir SCC yaşam döngüsü çoklu oturum açma (SSO) abonelik etkin

Bu öğreticiyi tamamladıktan sonra SCC yaşam döngüsü için atanmış Azure AD kullanıcıları çoklu oturum açma, SCC yaşam döngüsü şirket sitenizdeki (servis sağlayıcı tarafından başlatılan oturum açma) uygulamaya veya kullanarak okuyamayacak [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

1. Uygulama tümleştirmesi SCC yaşam döngüsü için etkinleştirme
2. Çoklu oturum açma (SSO) yapılandırma
3. Kullanıcı hazırlama işleminin yapılandırılması
4. Kullanıcılar atama

![Senaryo](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "senaryosu")

## <a name="enable-the-application-integration-for-scc-lifecycle"></a>SCC yaşam döngüsü için uygulama tümleştirmeyi etkinleştir
Bu bölümün amacı SCC yaşam döngüsü için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

**SCC yaşam döngüsü için uygulama tümleştirmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamaları](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "uygulamalar")
4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulama ekleme](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "uygulama ekleme")
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Galeriden bir uygulama eklemek](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Galeriden bir uygulama ekleme")
6. İçinde **arama kutusu**, türü **SCC yaşam döngüsü**.
   
    ![Uygulama Galerisi](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "uygulama Galerisi")
7. Sonuçlar bölmesinde seçin **SCC yaşam döngüsü**ve ardından **tam** uygulama eklemek için.
   
    ![SCC yaşam döngüsü](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC yaşam döngüsü")
   
## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın

Bu bölümün amacı kullanıcıların SCC yaşam döngüsü için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **SCC yaşam döngüsü** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için ** tek oturum yapılandırma üzerinde Aktar ** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "çoklu oturum açmayı yapılandırın")
2. Üzerinde **SCC yaşam döngüsü için oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "çoklu oturum açmayı yapılandırın")
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında **oturum üzerinde URL'si** metin kutusuna, türü URL kullanıcılarınız tarafından şu biçimi kullanarak SCC yaşam döngüsü uygulamanıza oturum açma için kullanılan "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" ve ardından **sonraki**.
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "uygulama URL'sini yapılandırın")
4. Üzerinde **çoklu oturum açma SCC yaşam döngüsü sırasında yapılandırma** sayfasında, **karşıdan meta veri**ve meta veri dosyası, bilgisayarınıza yerel olarak kaydedin.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "çoklu oturum açmayı yapılandırın")
5. Bu meta veri dosyası SCC yaşam döngüsü Destek ekibine iletin.
   
   >[!NOTE]
   >Çoklu oturum açma SCC yaşam döngüsü destek ekibi tarafından etkinleştirilmesi gerekir.
   > 
   > 

6. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "çoklu oturum açmayı yapılandırın")
   
## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır

Azure AD kullanıcıların SCC yaşam döngüsü oturum etkinleştirmek için bunların SCC yaşam döngüsü sağlanmalıdır. Kullanıcı SCC yaşam döngüsü için sağlama yapılandırmanız için eylem öğe yok.

Atanmış bir kullanıcı SCC yaşam döngüsü oturum açmaya çalıştığında SCC yaşam döngüsü hesap gerekirse, otomatik olarak oluşturulur.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına SCC yaşam döngüsü tarafından sağlanan veya herhangi diğer SCC yaşam döngüsü kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 
> 

## <a name="assign-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**SCC yaşam döngüsü için kullanıcılara atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde ** SCC yaşam döngüsü ** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
    ![Kullanıcılar atama](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "kullanıcı atama")
3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
    ![Evet](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Evet")

SSO ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

