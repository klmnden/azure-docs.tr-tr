---
title: "Öğretici: Azure Active Directory Tümleştirme ile Coupa | Microsoft Docs"
description: "Çoklu oturum açma, otomatik sağlama ve daha fazla etkinleştirmek için Azure Active Directory ile Coupa kullanmayı öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: c952975919cfd948f1b9ea93ff2ac2641a53f923
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Öğretici: Azure Active Directory Tümleştirme Coupa ile
Bu öğreticinin amacı, Azure ve Coupa tümleştirmesini göstermektir.  
Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Bir Coupa çoklu oturum açma (SSO) abonelik etkin

Bu öğreticiyi tamamladıktan sonra Coupa için atanmış Azure AD kullanıcılarının uygulama kullanarak çoklu oturum açabilirsiniz [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

* Uygulama tümleştirmesi Coupa için etkinleştirme
* Çoklu oturum açmayı yapılandırma
* Kullanıcı hazırlama işleminin yapılandırılması
* Kullanıcılar atama

![Senaryo](./media/active-directory-saas-coupa-tutorial/IC791897.png "senaryosu")

## <a name="enable-the-application-integration-for-coupa"></a>Coupa uygulama tümleştirmeyi etkinleştir
Bu bölümün amacı Coupa için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

**Coupa uygulama tümleştirmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
   ![Uygulamaları](./media/active-directory-saas-coupa-tutorial/IC700994.png "uygulamalar")
4. Tıklatın **Ekle** sayfanın sonundaki.
   
   ![Uygulama ekleme](./media/active-directory-saas-coupa-tutorial/IC749321.png "uygulama ekleme")
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
   ![Galeriden bir uygulama eklemek](./media/active-directory-saas-coupa-tutorial/IC749322.png "Galeriden bir uygulama ekleme")
6. İçinde **arama kutusu**, türü **Coupa**.
   
   ![Uygulama Galerisi](./media/active-directory-saas-coupa-tutorial/IC791898.png "uygulama Galerisi")
7. Sonuçlar bölmesinde seçin **Coupa**ve ardından **tam** uygulama eklemek için.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın

Bu bölümün amacı kullanıcıların Coupa için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.  

Coupa için çoklu oturum açmayı yapılandırma parmak izi değerinin bir sertifika almanızı gerektirir. Bu yordama bilmiyorsanız bkz [bir sertifikanın parmak izi değerini almak nasıl](http://youtu.be/YKQF266SAxI).

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Coupa şirket sitenize yönetici olarak oturum açma.
2. Git **Kurulum \> güvenlik denetimi**.
   
   ![Güvenlik denetimleri](./media/active-directory-saas-coupa-tutorial/IC791900.png "güvenlik denetimleri")
3. Bilgisayarınıza Coupa meta veri dosyası indirmek için tıklayın **karşıdan yükleme ve SP meta verileri içeri aktarma**.
   
   ![Coupa SP meta veri](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP meta verileri")
4. Farklı tarayıcı penceresinde, Klasik Azure portalında oturum açma.
5. Üzerinde **Coupa** uygulama tümleştirme sayfası, tıklatın **çoklu oturum açma yapılandırma** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-coupa-tutorial/IC791902.png "çoklu oturum açmayı yapılandırın")
6. Üzerinde **Coupa için oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-coupa-tutorial/IC791903.png "çoklu oturum açmayı yapılandırın")
7. Üzerinde **uygulama URL'sini Yapılandır** sayfasında, aşağıdaki adımları gerçekleştirin:
   
   ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-coupa-tutorial/IC791904.png "uygulama URL'sini yapılandırın")   
   1. İçinde **oturum üzerinde URL'si** metin kutusuna, Coupa uygulamanıza oturum açmaya kullanıcılarınız tarafından kullanılan türü URL'si (örn: "*http://company.Coupa.com*").
   2. İndirilen Coupa meta veri dosyanızı açın ve ardından kopyalama **AssertionConsumerService dizin/URL**.
   3. İçinde **Coupa yanıt URL'si** metin kutusuna, Yapıştır **AssertionConsumerService dizin/URL** değeri.
   4. **İleri**’ye tıklayın.
8. Üzerinde **çoklu oturum açma sırasında Coupa yapılandırmak** , meta veriler indirilemedi sayfasını tıklatın **karşıdan meta veri**ve ardından dosyayı bilgisayarınıza yerel olarak kaydedin.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-coupa-tutorial/IC791905.png "çoklu oturum açmayı yapılandırın")
9. Coupa şirket sitesinde Git **Kurulum \> güvenlik denetimi**.
   
   ![Güvenlik denetimleri](./media/active-directory-saas-coupa-tutorial/IC791900.png "güvenlik denetimleri")
10. İçinde **Coupa kimlik bilgilerini kullanarak oturum** bölümünde, aşağıdaki adımları gerçekleştirin:  

   ![Coupa kimlik bilgilerini kullanarak oturum](./media/active-directory-saas-coupa-tutorial/IC791906.png "Coupa kimlik bilgilerini kullanarak oturum açın") 
   1. Seçin **SAML kullanarak oturum**.
   2. Tıklatın **Gözat** indirilen Azure Active meta veri dosyanızı karşıya yüklemek için.
   3. **Kaydet** düğmesine tıklayın.
11. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
    
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-coupa-tutorial/IC791907.png "çoklu oturum açmayı yapılandırın")
    
## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır

Azure AD kullanıcıların Coupa oturum etkinleştirmek için bunların Coupa sağlanmalıdır.  

* Coupa söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Coupa** yönetici olarak şirket site.
2. Üstteki menüde tıklatın **Kurulum**ve ardından **kullanıcılar**.
   
   ![Kullanıcıların](./media/active-directory-saas-coupa-tutorial/IC791908.png "kullanıcılar")
3. **Oluştur**'a tıklayın.
   
   ![Kullanıcılar oluşturma](./media/active-directory-saas-coupa-tutorial/IC791909.png "kullanıcıları oluşturun")
4. İçinde **kullanıcı oluşturma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı ayrıntılarını](./media/active-directory-saas-coupa-tutorial/IC791910.png "kullanıcı ayrıntıları")
   
   1. Tür **oturum açma**, **ad**, **Soyadı**, **tek oturum açma kimliği**, **e-posta** özniteliklerini bir Geçerli bir Azure Active Directory hesabı ilgili metin kutularına içine sağlamak istiyorsunuz.
   2. **Oluştur**'a tıklayın.   
   >[!NOTE]
   >Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız. 
   > 

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Coupa tarafından sağlanan veya herhangi diğer Coupa kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 
> 

## <a name="assign-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**Kullanıcılar için Coupa atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde ** Coupa ** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
   ![Kullanıcılar atama](./media/active-directory-saas-coupa-tutorial/IC791911.png "kullanıcı atama")
3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
   ![Evet](./media/active-directory-saas-coupa-tutorial/IC767830.png "Evet")

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

