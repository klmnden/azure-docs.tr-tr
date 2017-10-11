---
title: "Öğretici: Azure Active Directory Tümleştirme Merkezi Masaüstü ile | Microsoft Docs"
description: "Merkezi Masaüstü Azure Active Directory ile çoklu oturum açma, otomatik sağlama ve daha fazlasını sağlamak için nasıl kullanılacağını öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Öğretici: Azure Active Directory Tümleştirme ile Merkezi Masaüstü
Bu öğreticinin amacı, Azure ve Merkezi Masaüstü tümleştirmesini göstermektir. Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Merkezi bir masaüstü çoklu oturum açma etkin Abonelik / Merkezi Masaüstü Kiracı

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

* Uygulama tümleştirmesi Merkezi Masaüstü için etkinleştirme
* Çoklu oturum açma (SSO) yapılandırma
* Kullanıcı hazırlama işleminin yapılandırılması
* Kullanıcılar atama

![Senaryo](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "senaryosu")

## <a name="enable-the-application-integration-for-central-desktop"></a>Merkezi Masaüstü uygulama tümleştirmeyi etkinleştir
Bu bölümün amacı, uygulama tümleştirmesi Merkezi Masaüstü için etkinleştirmek nasıl anahat sağlamaktır.

**Uygulama tümleştirmesi Merkezi Masaüstü için etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
   ![Uygulamaları](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "uygulamalar")
4. Tıklatın **Ekle** sayfanın sonundaki.
   
   ![Uygulama ekleme](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "uygulama ekleme")
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
   ![Galeriden bir uygulama eklemek](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Galeriden bir uygulama ekleme")
6. İçinde **arama kutusu**, türü **Merkezi Masaüstü**.
   
   ![Uygulama Galerisi](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "uygulama Galerisi")
7. Sonuçlar bölmesinde seçin **Merkezi Masaüstü**ve ardından **tam** uygulama eklemek için.
   
   ![Merkezi Masaüstü](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Merkezi Masaüstü")
   
## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın

Bu bölümün amacı kullanıcıların Merkezi Masaüstü kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

Bu yordam bir parçası olarak, bir base-64 kodlanmış sertifika Merkezi Masaüstü kiracınız karşıya yüklemek için gerekli değildir.  
Bu yordama bilmiyorsanız bkz [ikili bir sertifika bir metin dosyasına dönüştürmek nasıl](http://youtu.be/PlgrzUZ-Y1o).

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **Merkezi Masaüstü** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için ** tek oturum yapılandırma üzerinde Aktar ** iletişim.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "çoklu oturum açmayı yapılandırın")
2. Üzerinde **Merkezi Masaüstü oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
   ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "çoklu oturum açmayı yapılandırın")
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında, aşağıdaki adımları uygulayın ve ardından **sonraki**: 
   
   1. İçinde **Merkezi Masaüstü Oturum açma URL'si** metin kutusuna, Merkezi Masaüstü Kiracı URL'sini yazın (örneğin: *http://contoso.centraldesktop.com*).
   2. Merkezi Masaüstü yanıt URL'si metin kutusuna Merkezi Masaüstü AssertionConsumerService URL'nizi yazın (örneğin: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Değer Merkezi Masaüstü meta verileri alabilir (örn: *http://contoso.centraldesktop.com*).
   >  
   
   ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "uygulama URL'sini yapılandırın")
4. Üzerinde **çoklu oturum açma sırasında Merkezi Masaüstü yapılandırma** , sertifika indirmek için sayfasını tıklatın **indirme sertifika**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.
   
  ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "çoklu oturum açmayı yapılandırın")
5. Oturum, **Merkezi Masaüstü** Kiracı.
6. Git **ayarları**, tıklatın **Gelişmiş**ve ardından **çoklu oturum açma**.
   
  ![Kurulum - Gelişmiş](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Kurulum - Gelişmiş")
7. Üzerinde **tek oturum açma ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:
   
  ![Çoklu oturum açma ayarları](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "tek oturum açma ayarları")
   
  1. Seçin **etkinleştir SAML v2 çoklu oturum açma**.
  2. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Merkezi Masaüstü yapılandırma** sayfasında, kopya **veren URL'si** değer ve ardından yapıştırın **SSO URL** metin kutusu.
  3. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Merkezi Masaüstü yapılandırma** sayfasında, kopya **uzaktan oturum açma URL'si** değer ve ardından yapıştırın **SSO oturum açma URL'si** metin kutusu .
  4. Azure Klasik portalında üzerinde **çoklu oturum açma sırasında Merkezi Masaüstü yapılandırma** sayfasında, kopya **tek Sign-Out hizmeti URL'si** değer ve ardından yapıştırın **SSO oturum kapatma URL'si**metin kutusu.
8. İçinde **ileti imzası doğrulama yöntemi** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![İleti imzası doğrulama yöntemi](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "ileti imzası doğrulama yöntemi")
   
  1. Seçin **sertifika**.
  2. Gelen **SSO sertifika** listesinde **RSH SHA256**.
  3. İndirilen sertifika bir metin dosyası oluşturun, metin dosyasının içeriğini kopyalayın ve ardından yapıştırın **SSO sertifika** alan.  
     >[!TIP]
     >Daha fazla ayrıntı için bkz: [ikili bir sertifika bir metin dosyasına dönüştürme](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Seçin **SAMLv2 oturum açma sayfanız bir bağlantı görüntüler**.
9. Tıklatın **güncelleştirme**.
10. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
    
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "çoklu oturum açmayı yapılandırın")
    
## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır

AAD kullanıcıların oturum açabilmesi için bunların Merkezi Masaüstü uygulamaya sağlanmalıdır. Bu bölümde, AAD kullanıcı hesaplarının Merkezi Masaüstü nasıl oluşturulacağı açıklanmaktadır.

**Merkezi Masaüstü kullanıcı hesaplarına sağlamak için:**
1. Merkezi Masaüstü kiracınız için oturum açın.
2. Git **kişiler \> iç üyeleri**.
3. Tıklatın **iç üye eklemek**.
   
  ![Kişiler](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "kişiler")
4. İçinde **yeni üyeler e-posta adresi** metin kutusuna, sağlamak istediğiniz ve ardından bir AAD hesabıyla yazın **sonraki**.
   
  ![E-posta adreslerini yeni üyeler](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "e-posta adreslerini yeni üyeler")
5. Tıklatın **Ekle iç üyeleri**.
   
  ![İç üye ekleme](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "iç üye ekleme")
   
   >[!NOTE]
   >Eklemiş olduğunuz kullanıcı hesabını etkinleştirmek için tıklatın için gereksinim duydukları onay bağlantısı içeren bir e-posta alırsınız.
   > 

>[!NOTE]
>Tüm diğer Merkezi Masaüstü kullanıcı hesabı oluşturma araçlarını kullanabilir veya API'ler sağlama AAD kullanıcı hesaplarına Merkezi Masaüstü tarafından sağlanan
>  

## <a name="assign-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**Kullanıcılar için merkezi masaüstü atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.
2. Üzerinde **Merkezi Masaüstü** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
   ![Kullanıcılar atama](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "kullanıcı atama")
3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
   ![Evet](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Evet")

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

