---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Netsuite | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Netsuite arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2018
ms.author: jeedes
ms.openlocfilehash: 2f2bd227a3d8c0b797f37026032938fbcfe3de9e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34351346"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Öğretici: Azure Active Directory Tümleştirme Netsuite ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Netsuite tümleştirmek öğrenin.

Netsuite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Netsuite erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Netsuite (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Netsuite ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Netsuite çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Netsuite ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-netsuite-from-the-gallery"></a>Galeriden Netsuite ekleme
Azure AD Netsuite tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Netsuite eklemeniz gerekir.

**Galeriden Netsuite eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Tıklatın **yeni uygulama** iletişim kutusunun üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Netsuite**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. Sonuçlar panelinde seçin **Netsuite**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Netsuite ile test etme

Tekli çalışmaya oturum için Azure AD Netsuite karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Netsuite ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Bu bağlantı değeri atayarak ilişkisi **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** Netsuite içinde.

Yapılandırma ve Azure AD çoklu oturum açma Netsuite ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Netsuite test kullanıcısı oluşturma](#creating-a-netsuite-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Netsuite sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Netsuite uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Netsuite yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Netsuite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. Üzerinde **Netsuite etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    > [!NOTE] 
    > Bunlar gerçek değerleri değildir. Bu değerleri gerçek yanıt URL'si ile güncelleştirin. Kişi [Netsuite destek ekibi](http://www.netsuite.com/portal/services/support.shtml) bu değerleri almak için.
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve XML dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. Üzerinde **Netsuite yapılandırma** 'yi tıklatın **yapılandırma Netsuite** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. Tarayıcınızda yeni bir sekme açın ve Netsuite şirket sitenizin yönetici olarak oturum açın.

8. Sayfanın üstündeki araç çubuğunda **Kurulum**, ardından **Kurulum Yöneticisi**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. Gelen **kurulum görevlerini** listesinde **tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. İçinde **yönetmek kimlik doğrulama** 'yi tıklatın **SAML çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. Üzerinde **SAML Kurulumu** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    a. Kopya **SAML çoklu oturum açma hizmet URL'si** değeri **hızlı başvuru** bölümünü **yapılandırma oturum açma** ve yapıştırın **kimlik sağlayıcısı oturum açma sayfası** Netsuite alanındaki.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    b. Netsuite içinde seçin **birincil kimlik doğrulama yöntemini**.

    c. Etiketli alan için **SAMLV2 kimlik sağlayıcısı meta verileri**seçin **IDP meta veri dosyasını karşıya yükle**. Ardından **Gözat** Azure portalından indirdiğiniz meta veri dosyasını karşıya yükleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    d. Tıklatın **gönderme**.

12. Azure AD'de tıklayın **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** onay kutusunu ve özniteliğini ekleyin.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. İçin **öznitelik adı** alan, yazın `account`. İçin **öznitelik değeri** alanında, Netsuite hesabı kimliğinizi yazın Bu, sabit ve hesap değişiklikle değerdir. Hesabı Kimliğiniz bulmak yönergeler aşağıda verilmiştir:

      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    a. Netsuite içinde tıklatın **Kurulum** üst gezinti menüsünde.

    b. Altında tıklatın **kurulum görevlerini** seçin sol gezinti menüsünün bölümünde **tümleştirme** bölümünde ve tıklatın **Web Hizmetleri tercihleri**.

    c. Netsuite hesabı Kimliğinizi kopyalayın ve yapıştırın **öznitelik değeri** Azure AD alanındaki.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. Kullanıcıların çoklu oturum açma Netsuite gerçekleştirmeden önce ilk Netsuite için uygun izinler atanmaları gerekir. Bu izinleri atamak için aşağıdaki yönergeleri izleyin.

    a. Üst gezinti menüsünde **Kurulum**, ardından **Kurulum Yöneticisi**.
      
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    b. Sol gezinti menüsünde seçin **kullanıcıları/rolleri**, ardından **Rolleri Yönet**.
      
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    c. Tıklatın **yeni rol**.

    d. Yazın bir **adı** yeni rolünüz için.
      
      ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    e. **Kaydet**’e tıklayın.

    f. Üstteki menüde tıklatın **izinleri**. Ardından **Kurulum**.
      
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    g. Seçin **ayarlamak yukarı SAML çoklu oturum açma**ve ardından **Ekle**.

    h. **Kaydet**’e tıklayın.

    i. Üst gezinti menüsünde **Kurulum**, ardından **Kurulum Yöneticisi**.
      
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    j. Sol gezinti menüsünde seçin **kullanıcıları/rolleri**, ardından **kullanıcıları yönetme**.
      
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    k. Bir test kullanıcı seçin. Ardından **Düzenle**.
      
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    l. Rolleri iletişim kutusunda, oluşturduğunuz ve'ı tıklatın rolü seçin **Ekle**.
      
       ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    m. **Kaydet**’e tıklayın.
    
### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 

### <a name="creating-a-netsuite-test-user"></a>Netsuite test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Netsuite içinde oluşturulur. Netsuite yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler.
Bu bölümde, eylem öğe yok. Bir kullanıcı Netsuite zaten yoksa, Netsuite erişmeyi denediğinde yeni bir tane oluşturulur.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Netsuite için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Netsuite için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Netsuite**.

    ![Çoklu oturum açmayı yapılandırın](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Çoklu oturum açma ayarlarınızı sınamak için adresinden erişim Paneli'nde açın [ https://myapps.microsoft.com ](https://myapps.microsoft.com/), oturum test dikkate ve tıklatın **Netsuite**.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı Yapılandır](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

