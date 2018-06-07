---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zoho bir | Microsoft Docs'
description: Çoklu oturum açma arasındaki Azure Active Directory Zoho bir yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 3ffabecad5cd82380d7f05d48761bd8f1e430b37
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655297"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Öğretici: Azure Active Directory Tümleştirme ile Zoho bir

Bu öğreticide, Zoho bir Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

Zoho bir Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zoho bir erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Zoho bir için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Zoho bir ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zoho bir çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zoho bir ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zoho-one-from-the-gallery"></a>Galeriden Zoho bir ekleme
Tümleştirmesini Zoho bir Azure AD'ye yapılandırmak için Zoho bir Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zoho bir eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Zoho bir**seçin **Zoho bir** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde bir Zoho](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zoho "Britta Simon" adlı bir test kullanıcı tabanlı bir test.

Tekli çalışmaya oturum için Azure AD Zoho bir karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Zoho bir arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile Zoho bir test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zoho bir test kullanıcısı oluşturma](#create-a-zoho-one-test-user)**  - Zoho kullanıcı Azure AD gösterimini bağlantılı bir Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zoho bir uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Zoho bir ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zoho bir** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_samlbase.png)

3. Üzerinde **Zoho bir etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Zoho bir etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_url.png)

    a. İçinde **tanımlayıcısı (varlık kimliği)** metin kutusuna, bir URL yazın: `one.zoho.com`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    c. Denetleme **Göster Gelişmiş URL ayarları**.

    d. İçinde **geçiş durumunu** metin kutusuna, bir URL yazın:`https://one.zoho.com`

4. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu aşağıdaki adımı gerçekleştirin:

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`
     
    > [!NOTE] 
    > Yukarıdaki **yanıt URL'si** ve **oturum açma URL'si** değeri gerçek değil. Değer, gerçek yanıt URL'si ve oturum açma, öğreticide daha sonra açıklanan URL'si ile güncelleştirir. 

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-zohoone-tutorial/tutorial_general_400.png)
    
7. Üzerinde **Zoho bir yapılandırma** 'yi tıklatın **Zoho bir yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Zoho bir yapılandırma](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_configure.png) 

8. Farklı web tarayıcısı penceresinde Zoho bir şirket sitenize yönetici olarak oturum açın.

9. Üzerinde **kuruluş** sekmesinde, tıklatın **Kurulum** altında **SAML kimlik doğrulaması**.

    ![Zoho bir kuruluş](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_setup.png)

10. Açılan sayfada aşağıdaki adımları gerçekleştirin:

    ![Zoho bir SIG](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_save.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    b. İçinde **Sign-out URL** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    c. Tıklatın **Gözat** karşıya yüklemek için **sertifika (Base64)** Azure Portalı'ndan indirilen.

    d. **Kaydet**’e tıklayın.

11. SAML kimlik doğrulaması kurulumu kaydedildikten sonra kopyalama **SAML Identfier** değeri ve bu değeri kullanın **yanıt URL'si** Azure portalında altında **Zoho bir etki alanı ve URL'leri** bölüm.

    ![Zoho bir saml](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_samlidenti.png)

12. Git **etki alanları** sekmesini ve sonra **etki alanı Ekle**.

    ![Zoho bir etki alanı](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_domain.png)

13. Üzerinde **etki alanı Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir etki alanı Ekle](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. İçinde **etki alanı adı** metin kutusuna, türü etki alanı gibi **contoso.com**.

    b. **Ekle**'ye tıklayın.

    >[!Note]
    >Etki alanı izleme ekledikten sonra [bu](https://www.zoho.com/one/help/admin-guide/domain-verification.html) adımları etki alanınızı doğrulayın. Etki alanı verfified eklendiğinde, etki alanı adınızı kullanmak **oturum açma URL'si** içinde **Zoho bir etki alanı ve URL'leri** Azure portalı bölümünde.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-zohoone-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-zohoone-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-zohoone-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-zohoone-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-zoho-one-test-user"></a>Zoho bir test kullanıcısı oluşturma

Azure AD kullanıcılarının Zoho bir oturum açmayı etkinleştirmek için bunlar Zoho bir sağlanmalıdır. Bir Zoho içinde sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. İçin Zoho tek bir güvenlik yöneticisi olarak oturum açın.

2. Üzerinde **kullanıcılar** sekmesi tıklatın üzerinde **kullanıcı logosu**.

    ![Zoho bir kullanıcı](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_users.png)

3. Üzerinde **Kullanıcı Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zoho bir kullanıcı ekleme](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. İçinde **adı** metin kutusuna, gibi kullanıcı adını girin **Britta simon**.
    
    b. İçinde **e-posta adresi** metin kutusunda, bir kullanıcı gibi e-posta girin **brittasimon@contoso.com**.

    >[!Note]
    >Doğrulanmış etki alanınız etki alanı listeden seçin.

    c. **Ekle**'ye tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Zoho bir erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Zoho bir atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zoho bir**.

    ![Uygulamalar listesinde Zoho bir bağlantı](./media/active-directory-saas-zohoone-tutorial/tutorial_zohoone_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zoho bir kutucuğa tıkladığınızda, otomatik olarak Zoho bir uygulamanız açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zohoone-tutorial/tutorial_general_203.png

