---
title: 'Öğretici: Azure Active Directory Tümleştirme Infogix Data3Sixty yöneten ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory arasındaki Infogix Data3Sixty yöneten yapılandırma konusunda bilgi edinin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: 99b55311293c04905d005604ce20d6816919b31c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Öğretici: Azure Active Directory Tümleştirme Infogix Data3Sixty yöneten ile

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Infogix Data3Sixty yöneten öğrenin.

Azure AD ile tümleştirme Infogix Data3Sixty yöneten ile aşağıdaki avantajları sağlar:

- Infogix Data3Sixty yöneten erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Infogix Data3Sixty yönetmek için (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Infogix Data3Sixty yöneten ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Infogix Data3Sixty yöneten çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Infogix Data3Sixty yöneten ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Galeriden Infogix Data3Sixty yöneten ekleme
Infogix Data3Sixty yöneten tümleştirilmesi Azure AD'ye yapılandırmak için Infogix Data3Sixty yöneten Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Infogix Data3Sixty yöneten Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Infogix Data3Sixty yöneten**seçin **Infogix Data3Sixty yöneten** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Infogix Data3Sixty yöneten sonuçlar listesinde](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Data3Sixty Infogix yöneten ile test etme.

Tekli çalışmaya oturum için Azure AD Infogix Data3Sixty yöneten karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ilgili Infogix Data3Sixty yöneten kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Infogix Data3Sixty yöneten ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Infogix Data3Sixty yöneten test kullanıcısı oluşturma](#create-an-infogix-data3sixty-govern-test-user)**  - Infogix Data3Sixty kullanıcı Azure AD gösterimini bağlantılı yöneten Britta Simon, karşılık gelen sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Infogix Data3Sixty yöneten uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Infogix Data3Sixty yöneten ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Infogix Data3Sixty yöneten** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_samlbase.png)

3. Üzerinde **Infogix Data3Sixty yöneten etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Infogix Data3Sixty yöneten etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, bir URL yazın: `https://data3sixty.com/ui`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.data3sixty.com/sso/acs`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Infogix Data3Sixty yöneten etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Infogix Data3Sixty yöneten istemci destek ekibi](mailto:data3sixtysupport@infogix.com) bu değerleri almak için.

5. Uygulama Infogix Data3Sixty yöneten SAML onaylar belirli bir biçimde bekler. Bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirmesi sayfasında bölüm. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir.
    
    ![Çoklu oturum açma attb yapılandırın](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_attribute.png)
    
6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | FirstName           | User.givenName |
    | Soyadı        | User.surname |
    | kullanıcı adı       | User.Mail    |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açma yapılandırma ekleme](./media/active-directory-saas-infogix-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/active-directory-saas-infogix-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

7. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (ham)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_certificate.png)

8. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-infogix-tutorial/tutorial_general_400.png)
    
9. Üzerinde **Infogix Data3Sixty yöneten yapılandırma** 'yi tıklatın **Infogix Data3Sixty yöneten yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Infogix Data3Sixty yöneten yapılandırma](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_configure.png) 

10. Çoklu oturum açma yapılandırmak için **Infogix Data3Sixty yöneten** yan, indirilen göndermek için ihtiyacınız **sertifika (ham), Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** için [ Destek ekibi Infogix Data3Sixty yöneten](mailto:data3sixtysupport@infogix.com). Bunlar, her iki tarafta da ayarlamanızı SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-infogix-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-infogix-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-infogix-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-infogix-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Bir Infogix Data3Sixty yöneten test kullanıcısı oluşturma


Bu bölümün amacı Infogix Data3Sixty yöneten içinde Britta Simon adlı bir kullanıcı oluşturmaktır. Infogix Data3Sixty yöneten yalnızca zaman sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, eylem öğe yok. Yeni bir kullanıcı henüz yoksa Infogix Data3Sixty yöneten erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Infogix Data3Sixty yöneten destek ekibi](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Infogix Data3Sixty yönetmek için erişim izni verme, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Infogix Data3Sixty yönetmek için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Infogix Data3Sixty yöneten**.

    ![Uygulamalar listesinde Infogix Data3Sixty yöneten bağlantı](./media/active-directory-saas-infogix-tutorial/tutorial_infogix_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Infogix Data3Sixty yöneten parçasında tıklattığınızda, otomatik olarak Infogix Data3Sixty yöneten uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-infogix-tutorial/tutorial_general_203.png

