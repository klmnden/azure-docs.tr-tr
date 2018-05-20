---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Ziflow | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Ziflow arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeedes
ms.openlocfilehash: 4f667edbf6b0fab8c8dac4442d50d14252c4c4cd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Öğretici: Azure Active Directory Tümleştirme Ziflow ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Ziflow tümleştirmek öğrenin.

Ziflow Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Ziflow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Ziflow (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Ziflow ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Ziflow çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Ziflow ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-ziflow-from-the-gallery"></a>Galeriden Ziflow ekleme
Azure AD Ziflow tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Ziflow eklemeniz gerekir.

**Galeriden Ziflow eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Ziflow**seçin **Ziflow** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Ziflow](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Ziflow sınayın.

Tekli çalışmaya oturum için Azure AD Ziflow karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Ziflow ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Ziflow ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Ziflow test kullanıcısı oluşturma](#create-a-ziflow-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Ziflow sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Ziflow uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Ziflow yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Ziflow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_samlbase.png)

3. Üzerinde **Ziflow etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Ziflow etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_url.png)

    a. İçinde **URL üzerinde oturum** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.ziflow.io/#/login-sso/<Unique ID>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `urn:auth0:ziflow-production:<Unique ID>`

    > [!NOTE] 
    > Yukarıdaki değerleri gerçek değildir. Öğreticide daha sonra açıklanan gerçek değer ile benzersiz kimliği değeri tanımlayıcısı ve oturum açma URL'si güncelleştirir. Kişi [Ziflow destek ekibi](mailto:support@ziflow.com) oturum açma URL'si alt etki alanı değeri için.
    
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-ziflow-tutorial/tutorial_general_400.png)

6. Üzerinde **Ziflow yapılandırma** 'yi tıklatın **yapılandırma Ziflow** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Ziflow yapılandırma](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_configure.png) 

7. Bir farklı web tarayıcısı penceresinde, Ziflow bir güvenlik yöneticisi olarak oturum açın.


8. Sağ üst köşeden üzerinde Avatar tıklayın ve ardından **hesabını yönetmek**.

    ![Ziflow yapılandırmasını Yönet](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_manage.png)

9. Üst sol, tıklatın **çoklu oturum açma**.

    ![Ziflow yapılandırma oturum](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_signon.png)

10. Üzerinde **çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Ziflow yapılandırma tek](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_page.png)

    a. Seçin **türü** olarak **SAML2.0**.

    b.In **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. Azure portalından içine yüklediğiniz base-64 kodlanmış sertifika karşıya **imzalama sertifikası X509**.

    d. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    e. Gelen **tanımlayıcısı sağlayıcınız için yapılandırma ayarlarını** bölümünde vurgulanan benzersiz kimliği değerini kopyalayın ve tanımlayıcı ve oturum açma URL'si içinde sona **Ziflow etki alanı ve URL'leri bölümüne** üzerinde Azure portalı.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-ziflow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-ziflow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-ziflow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-ziflow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-ziflow-test-user"></a>Ziflow test kullanıcısı oluşturma

Azure AD kullanıcıları için Ziflow oturum açmak etkinleştirmek için bunların Ziflow sağlanmalıdır. Ziflow içinde sağlama bir el ile bir görevdir.

Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. İçin Ziflow bir güvenlik yöneticisi olarak oturum açın.

2. Gidin **kişiler** üstte.

    ![Ziflow yapılandırma kişiler](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_people.png)

3. Tıklatın **Ekle** ve ardından **Kullanıcı Ekle**.

    ![Kullanıcı ekleniyor Ziflow yapılandırma](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_add.png)

4. Üzerinde **kullanıcı ekleme** açılan, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleniyor Ziflow yapılandırma](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. İçinde **e-posta** metin kutusunda, bir kullanıcı gibi e-posta girin brittasimon@contoso.com.

    b. İçinde **ad** metin kutusuna, Britta gibi kullanıcının ilk adını girin.

    c. İçinde **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadını girin.

    d. Ziflow rol seçin.

    e. Tıklatın **ekleyin 1 kullanıcı**.

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Ziflow için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Ziflow için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Ziflow**.

    ![Uygulamalar listesinde Ziflow bağlantı](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Ziflow parçasında tıklattığınızda, otomatik olarak Ziflow uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_203.png

