---
title: 'Öğretici: Azure Active Directory Tümleştirme Mimecast kişisel portalıyla | Microsoft Docs'
description: Azure Active Directory ve Mimecast kişisel Portal arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: jeedes
ms.openlocfilehash: f1415a1ddc49f10539915ccf0ce8f95ce7daf321
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051879"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Öğretici: Azure Active Directory Mimecast kişisel portalı ile tümleştirme

Bu öğreticide, Mimecast kişisel portalı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Mimecast kişisel portalı, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Mimecast kişisel Portal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Mimecast kişisel portalına (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Mimecast kişisel portalıyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Mimecast kişisel portalı çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Mimecast kişisel portalı ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>Galeriden Mimecast kişisel portalı ekleme
Azure AD'de Mimecast kişisel portalı tümleştirmesini yapılandırmak için Mimecast kişisel portalı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Mimecast kişisel portalı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Mimecast kişisel portalı**seçin **Mimecast kişisel portalı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Mimecast kişisel portalı](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Mimecast kişisel "Britta Simon" adlı bir test kullanıcı tabanlı Portal ile Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Mimecast kişisel portalında karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Mimecast kişisel portalında arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Mimecast kişisel portalı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Mimecast kişisel portalı test kullanıcısı oluşturma](#create-a-mimecast-personal-portal-test-user)**  - kullanıcı Azure AD gösterimini bağlı Mimecast kişisel portalında Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Mimecast kişisel portalı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Mimecast kişisel portalıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Mimecast kişisel portalı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. Üzerinde **Mimecast kişisel portalı etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Mimecast kişisel portalı etki alanı ve URL'ler tek oturum açma bilgileri](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL: 

    | Bölge  |  Değer | 
    | --------------- | --------------- | 
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:

    | Bölge  |  Değer | 
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | Güney Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Avustralya       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Yurtdışında        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. İçinde **yanıt URL'si** metin kutusuna bir URL: 

    | Bölge  |  Değer | 
    | --------------- | --------------- | 
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Amerika Birleşik Devletleri   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|
    
    > [!NOTE] 
    > Tanımlayıcı değerini gerçek değil. Değerini gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [Mimecast kişisel portalı istemcisi Destek ekibine](http://www.mimecast.com/customer-success/technical-support/) değeri alınamıyor. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. Üzerinde **Mimecast kişisel portalı Yapılandırması** bölümünde **Mimecast kişisel portalı yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Mimecast kişisel Portal Yapılandırması](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. Farklı bir web tarayıcı penceresinde, bir yönetici olarak, Mimecast kişisel portalında oturum açın.

8. Git **Hizmetleri \> uygulamaları**.
   
    ![Uygulamaları](./media/mimecast-personal-portal-tutorial/ic794998.png "uygulamaları")

9. Tıklayın **kimlik doğrulaması profilleri**.
   
    ![Kimlik doğrulaması profilleri](./media/mimecast-personal-portal-tutorial/ic794999.png "kimlik doğrulaması profilleri")

10. Tıklayın **yeni kimlik doğrulama profili**.
   
    ![Yeni kimlik doğrulama profili](./media/mimecast-personal-portal-tutorial/ic795000.png "yeni kimlik doğrulama profili")

11. İçinde **kimlik doğrulama profili** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik doğrulama profili](./media/mimecast-personal-portal-tutorial/ic795001.png "kimlik doğrulama profili")
   
    a. İçinde **açıklama** metin yapılandırmanız için bir ad yazın.
   
    b. Seçin **Mimecast kişisel portalı SAML kimlik doğrulaması zorunlu**.
   
    c. Olarak **sağlayıcısı**seçin **Azure Active Directory**.
   
    d. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.
   
    e. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
   
    f. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    g. Açık, **base-64** Defteri'nde kodlanmış sertifika Azure portalından indirilen, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası (meta veriler)** metin.

    h. Seçin **çoklu oturum açmaya izin ver**.
   
    i. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/mimecast-personal-portal-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/mimecast-personal-portal-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/mimecast-personal-portal-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/mimecast-personal-portal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-mimecast-personal-portal-test-user"></a>Mimecast kişisel portalı test kullanıcısı oluşturma

Mimecast kişisel portalda oturum açarken Azure AD kullanıcılarının etkinleştirmek için bunlar Mimecast kişisel portalına sağlanması gerekir. Mimecast kişisel söz konusu olduğunda, sağlama elle bir görevin portalıdır.

Kullanıcılar oluşturabilmeniz için önce bir etki alanı kayıt olmanız gerekir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Mimecast kişisel portalı** yönetici olarak.

2. Git **dizinleri \> iç**.
   
    ![Dizinleri](./media/mimecast-personal-portal-tutorial/ic795003.png "dizinleri")

3. Tıklayın **yeni etki alanı kayıt**.
   
    ![Yeni etki alanı kayıt](./media/mimecast-personal-portal-tutorial/ic795004.png "kaydetme yeni etki alanı")

4. Yeni etki alanınız oluşturulduktan sonra tıklayın **yeni adresi**.
   
    ![Yeni adresi](./media/mimecast-personal-portal-tutorial/ic795005.png "yeni adresi")

5. Yeni adresi iletişim kutusunda geçerli bir Azure aşağıdaki adımları sağlamak istediğiniz AD hesabı:
   
    ![Kaydet](./media/mimecast-personal-portal-tutorial/ic795006.png "Kaydet")
   
    a. İçinde **e-posta adresi** metin kutusuna **e-posta adresi** olarak kullanıcı **BrittaSimon@contoso.com**.
    
    b. İçinde **genel adı** metin kutusuna **kullanıcıadı** olarak **BrittaSimon**.

    c. İçinde **parola**, ve **parolayı onayla** metin kutuları, türü **parola** kullanıcının.
   
    b. **Kaydet**’e tıklayın.

>[!NOTE]
>Azure AD kullanıcı hesapları sağlamak için herhangi bir kişisel Mimecast portalı kullanıcı hesabı oluşturma araçları veya Mimecast kişisel portalı tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kişisel Mimecast Portalı'na erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Mimecast kişisel portalına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Mimecast kişisel portalı**.

    ![Uygulamalar listesinde Mimecast kişisel portalı bağlantı](./media/mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Mimecast kişisel portalı kutucuğa tıkladığınızda, otomatik olarak Mimecast kişisel portalı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-personal-portal-tutorial/tutorial_general_203.png

