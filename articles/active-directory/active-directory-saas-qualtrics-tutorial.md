---
title: "Öğretici: Azure Active Directory Tümleştirme ile Qualtrics | Microsoft Docs"
description: "Çoklu oturum açma, otomatik sağlama ve daha fazla etkinleştirmek için Azure Active Directory ile Qualtrics kullanmayı öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2fcde595a40dafda7549f5bccb582b57585b314e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Öğretici: Azure Active Directory Tümleştirme Qualtrics ile
Bu öğreticinin amacı, Azure ve Qualtrics tümleştirmesini göstermektir.  

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Bir Qualtrics çoklu oturum açma (SSO) abonelik etkin

Bu öğreticiyi tamamladıktan sonra Qualtrics için atanmış Azure AD kullanıcıları çoklu oturum açma Qualtrics şirket sitenizdeki (servis sağlayıcı tarafından başlatılan oturum açma) uygulamaya veya kullanarak okuyamayacak [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

1. Uygulama tümleştirmesi Qualtrics için etkinleştirme
2. Çoklu oturum açma (SSO) yapılandırma
3. Kullanıcı hazırlama işleminin yapılandırılması
4. Kullanıcılar atama

![Senaryo](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "senaryosu")

## <a name="enabling-the-application-integration-for-qualtrics"></a>Uygulama tümleştirmesi Qualtrics için etkinleştirme
Bu bölümün amacı Qualtrics için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

**Qualtrics uygulama tümleştirmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
   ![Uygulamaları](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "uygulamalar")
4. Tıklatın **Ekle** sayfanın sonundaki.
   
   ![Uygulama ekleme](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "uygulama ekleme")
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
   ![Galeriden bir uygulama eklemek](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Galeriden bir uygulama ekleme")
6. İçinde **arama kutusu**, türü **Qualtrics**.
   
   ![Uygulama Galerisi](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "uygulama Galerisi")
7. Sonuçlar bölmesinde seçin **Qualtrics**ve ardından **tam** uygulama eklemek için.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın

Bu bölümün amacı kullanıcıların Qualtrics için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **Qualtrics** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "çoklu oturum açmayı yapılandırın")
2. Üzerinde **Qualtrics için oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "çoklu oturum açmayı yapılandırın")
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında **üzerinde Qualtrics oturum URL'si** metin kutusuna, URL'nizi yazın (örneğin: "*https://ssotest2ut1.qualtrics.com*") ve ardından **sonraki**.
   
   ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "uygulama URL'sini yapılandırın")
4. Üzerinde **çoklu oturum açma sırasında Qualtrics yapılandırma** sayfasında, **karşıdan meta veri**ve meta veri dosyası, bilgisayarınıza kaydedin.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "çoklu oturum açmayı yapılandırın")
5. Meta veri dosyası Qualtrics Destek ekibine gönderin.
   
   >[!NOTE]
   >SSO yapılandırma Qualtrics destek ekibi tarafından gerçekleştirilmesi gerekir. Yapılandırma tamamlandıktan hemen sonra bir bildirim alırsınız.
   > 
   > 
6. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "çoklu oturum açmayı yapılandırın")
   
## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır

Kullanıcı için Qualtrics hazırlama yapılandırmanız için eylem öğe yok. Atanmış bir kullanıcı erişim paneli kullanılarak Qualtrics oturum açmaya çalıştığında Qualtrics kullanıcının var olup olmadığını denetler.  

Hiçbir kullanıcı hesabı varsa kullanılabilir henüz, Qualtrics tarafından otomatik olarak oluşturulur.

## <a name="assign-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**Kullanıcılar için Qualtrics atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde **Qualtrics** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
   ![Kullanıcılar atama](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "kullanıcı atama")
3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
   ![Evet](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Evet")

SSO ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

