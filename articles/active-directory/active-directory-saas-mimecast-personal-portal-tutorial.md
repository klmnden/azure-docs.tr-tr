---
title: 'Öğretici: Azure Active Directory Tümleştirme Mimecast kişisel portalıyla | Microsoft Docs'
description: Çoklu oturum açma Mimecast kişisel Portal ile Azure Active Directory arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: jeedes
ms.openlocfilehash: 4b976481e60926e6ab95a679e6dcc8aa0818b48b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34346889"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Öğretici: Azure Active Directory Tümleştirme Mimecast kişisel portalı ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Mimecast kişisel Portal tümleştirmek öğrenin.

Mimecast kişisel Portal Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mimecast kişisel Portal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak Mimecast kişisel portalına (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Mimecast kişisel Portal ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Mimecast kişisel Portal çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Mimecast kişisel Portal ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Galeriden Mimecast kişisel Portal ekleme
Azure AD Mimecast kişisel Portal tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Mimecast kişisel Portal eklemeniz gerekir.

**Galeriden Mimecast kişisel Portal eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Mimecast kişisel Portal**seçin **Mimecast kişisel Portal** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Mimecast kişisel portalı](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Mimecast kişisel "Britta Simon" adlı bir test kullanıcı tabanlı Portal ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Mimecast kişisel Portalı'nda bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Mimecast kişisel portalında arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırmak ve Azure AD çoklu oturum açma Mimecast kişisel portalıyla sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Mimecast kişisel Portal test kullanıcısı oluşturma](#create-a-mimecast-personal-portal-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Mimecast kişisel Portal sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Mimecast kişisel portalı uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Mimecast kişisel Portal ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Mimecast kişisel Portal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. Üzerinde **Mimecast kişisel Portal etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mimecast kişisel Portal etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, bir URL yazın: 

    | Bölge  |  Değer | 
    | --------------- | --------------- | 
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Offshore        | `https://jer-api.mimecast.com/login/saml`|

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın:

    | Bölge  |  Değer | 
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Güney Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Avustralya       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Offshore        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. İçinde **yanıt URL'si** metin kutusuna, bir URL yazın: 

    | Bölge  |  Değer | 
    | --------------- | --------------- | 
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Offshore        | `https://jer-api.mimecast.com/login/saml`|
    
    > [!NOTE] 
    > Tanımlayıcı değeri gerçek değil. Değerin gerçek tanımlayıcısı ile güncelleştirin. Kişi [Mimecast kişisel Portal istemci destek ekibi](http://www.mimecast.com/customer-success/technical-support/) değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. Üzerinde **Mimecast kişisel Portal Yapılandırması** 'yi tıklatın **Mimecast kişisel Portal Yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Mimecast kişisel Portal Yapılandırması](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. Farklı web tarayıcısı penceresinde Mimecast kişisel portalınızı bir yönetici olarak oturum.

8. Git **Hizmetleri \> uygulamaları**.
   
    ![Uygulamaları](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "uygulamalar")

9. Tıklatın **kimlik doğrulaması profilleri**.
   
    ![Kimlik doğrulama profilleri](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "kimlik doğrulaması profilleri")

10. Tıklatın **yeni kimlik doğrulama profili**.
   
    ![Yeni kimlik doğrulama profili](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "yeni kimlik doğrulama profili")

11. İçinde **kimlik doğrulama profili** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama profili](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "kimlik doğrulama profili")
   
    a. İçinde **açıklama** metin kutusuna, yapılandırmanız için bir ad yazın.
   
    b. Seçin **Mimecast kişisel portalı SAML kimlik doğrulaması zorunlu**.
   
    c. Olarak **sağlayıcı**seçin **Azure Active Directory**.
   
    d. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği**, Azure portalından kopyalanan.
   
    e. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
   
    f. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    g. Açık, **base-64** Not Defteri'nde kodlanmış sertifika Azure Portalı'ndan indirilen, içeriğini, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısı sertifikası (meta verileri)** metin kutusu.

    h. Seçin **çoklu oturum açmaya izin verme**.
   
    i. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-mimecast-personal-portal-test-user"></a>Mimecast kişisel Portal test kullanıcısı oluşturma

Azure AD kullanıcılarının Mimecast kişisel portalda oturum açarken etkinleştirmek için bunlar Mimecast kişisel portalına sağlanmalıdır. Mimecast kişisel söz konusu olduğunda, sağlama el ile bir görev portalıdır.

Bir etki alanı kullanıcıları oluşturabilmeniz için önce kaydetmeniz gerekir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Mimecast kişisel Portal** yönetici olarak.

2. Git **dizinleri \> iç**.
   
    ![Dizinleri](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "dizinleri")

3. Tıklatın **kayıt yeni bir etki alanı**.
   
    ![Yeni etki alanı kayıt](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "kayıt yeni bir etki alanı")

4. Yeni etki alanınızın oluşturulduktan sonra tıklatın **yeni adresi**.
   
    ![Yeni bir adres](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "yeni adresi")

5. Yeni adres iletişim kutusunda geçerli bir Azure aşağıdaki adımları sağlamak istediğiniz AD hesabı:
   
    ![Kaydet](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Kaydet")
   
    a. İçinde **e-posta adresi** metin kutusuna, türü **e-posta adresi** kullanıcının **BrittaSimon@contoso.com**.
    
    b. İçinde **genel adı** metin kutusuna, türü **kullanıcıadı** olarak **BrittaSimon**.

    c. İçinde **parola**, ve **parolayı onayla** metin kutuları, türü **parola** kullanıcının.
   
    b. **Kaydet**’e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesaplarını sağlamak için herhangi bir Mimecast kişisel Portal kullanıcı hesabı oluşturma araçlarını veya Mimecast kişisel portalı tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Mimecast kişisel portalına erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Mimecast kişisel portalına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Mimecast kişisel Portal**.

    ![Uygulamalar listesinde Mimecast kişisel Portal bağlantısı](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Mimecast kişisel Portal parçasında tıklattığınızda, otomatik olarak Mimecast kişisel Portal uygulamanızın açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

