---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zoho | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Zoho arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: d332dd65d660106a9f296ea57f89379f07ced027
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34353866"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Öğretici: Azure Active Directory Tümleştirme Zoho ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Zoho tümleştirmek öğrenin.

Zoho Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Zoho erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Zoho (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Zoho ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zoho çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zoho ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zoho-from-the-gallery"></a>Galeriden Zoho ekleme
Azure AD Zoho tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zoho eklemeniz gerekir.

**Galeriden Zoho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Zoho**seçin **Zoho** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Zoho](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Zoho sınayın.

Tekli çalışmaya oturum için Azure AD Zoho karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Zoho ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Zoho içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zoho ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zoho test kullanıcısı oluşturma](#create-a-zoho-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Zoho sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zoho uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Zoho yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zoho** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. Üzerinde **Zoho etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zoho etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<company name>.zohomail.com`

    > [!NOTE] 
    > Bu değer gerçek değil. Bu değer gerçek oturum açma URL'si ile güncelleştirin. Kişi [Zoho istemci destek ekibi](https://www.zoho.com/mail/contact.html) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. Üzerinde **Zoho yapılandırma** 'yi tıklatın **yapılandırma Zoho** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, değişiklik parola URL'si ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Zoho yapılandırma](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. Farklı web tarayıcısı penceresinde Zoho posta şirket sitenize yönetici olarak oturum açın.

8. Git **Denetim Masası**.
   
    ![Denetim Masası](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Denetim Masası")

9. Tıklatın **SAML kimlik doğrulaması** sekmesi.
   
    ![SAML kimlik doğrulaması](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML kimlik doğrulaması")

10. İçinde **SAML kimlik doğrulama ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kimlik doğrulama ayrıntıları](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML kimlik doğrulama ayrıntıları")
   
    a. İçinde **oturum açma URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    b. İçinde **oturum kapatma URL'si** metin kutusuna, Yapıştır **Sign-Out URL** Azure portalından kopyalanan.
   
    c. İçinde **değişiklik parola URL'si** metin kutusuna, Yapıştır **değişiklik parola URL'si** Azure portalından kopyalanan.
       
    d. Not Defteri'nde Azure portalından indirdiğiniz, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **PublicKey** metin kutusu.
   
    e. Olarak **algoritması**seçin **RSA**.
   
    f. **Tamam**’a tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-zoho-test-user"></a>Zoho test kullanıcısı oluşturma

Azure AD kullanıcıların Zoho posta oturum etkinleştirmek için bunlar Zoho postaya sağlanmalıdır. Zoho söz konusu olduğunda, sağlama el ile bir görev olarak Mail'dir.

> [!NOTE]
> API Zoho postayla sağlama AAD kullanıcı hesaplarına sağlanan veya herhangi diğer Zoho posta kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum, **Zoho posta** yönetici olarak şirket site.

2. Git **Denetim Masası \> posta & belgeleri**.

3. Git **kullanıcı ayrıntıları \> kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "kullanıcı ekleme")

4. Üzerinde **kullanıcıları eklemek** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "kullanıcı ekleme")
   
    a. İçinde **ad** kullanıcı türü adı metin kutusuna, ister **Britta**.

    b. İçinde **Soyadı** metin kutusuna, kullanıcının soyadını türü ister **Simon**.

    c. İçinde **e-posta kimliği** metin kutusuna, kullanıcının e-posta kimliği türü ister **brittasimon@contoso.com**.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını girin.
   
    e. **Tamam**’a tıklayın.  
      
    > [!NOTE]
    > Azure Active Directory hesap sahibi etkin duruma gelmesi hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Zoho için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Zoho için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zoho**.

    ![Uygulamalar listesinde Zoho bağlantı](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zoho parçasında tıklattığınızda, otomatik olarak Zoho uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

