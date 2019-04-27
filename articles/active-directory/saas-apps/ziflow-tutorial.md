---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Ziflow | Microsoft Docs'
description: Azure Active Directory ve Ziflow arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 175e678365016bafd3d18f590a5434c32ac9fadd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60798014"
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Öğretici: Ziflow ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Ziflow tümleştirme konusunda bilgi edinin.

Azure AD ile Ziflow tümleştirme ile aşağıdaki avantajları sağlar:

- Ziflow erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Ziflow (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Ziflow yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Ziflow çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Ziflow ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ziflow-from-the-gallery"></a>Galeriden Ziflow ekleme
Azure AD'de Ziflow tümleştirmesini yapılandırmak için Ziflow Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Ziflow eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Ziflow**seçin **Ziflow** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Ziflow](./media/ziflow-tutorial/tutorial_ziflow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Ziflow sınayın.

Tek iş için oturum açma için Azure AD ne Ziflow karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Ziflow ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Ziflow ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Ziflow test kullanıcısı oluşturma](#create-a-ziflow-test-user)**  - kullanıcı Azure AD gösterimini bağlı Ziflow Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Ziflow uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Ziflow yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Ziflow** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/ziflow-tutorial/tutorial_ziflow_samlbase.png)

3. Üzerinde **Ziflow etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Ziflow etki alanı ve URL'ler tek oturum açma bilgileri](./media/ziflow-tutorial/tutorial_ziflow_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://ziflow-production.auth0.com/login/callback?connection=<UniqueID>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `urn:auth0:ziflow-production:<UniqueID>`

    > [!NOTE]
    > Yukarıdaki değerleri gerçek değildir. Benzersiz kimlik değerini tanımlayıcısı ve oturum açma URL'si, öğreticinin ilerleyen bölümlerinde açıklanan gerçek değeri ile güncelleştirir.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/ziflow-tutorial/tutorial_ziflow_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/ziflow-tutorial/tutorial_general_400.png)

6. Üzerinde **Ziflow yapılandırma** bölümünde **yapılandırma Ziflow** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Ziflow yapılandırma](./media/ziflow-tutorial/tutorial_ziflow_configure.png) 

7. Bir başka web tarayıcı penceresinde Ziflow bir güvenlik yöneticisi olarak oturum açın.

8. Sağ üst köşedeki Avatar üzerinde tıklayın ve ardından **hesabını yönetme**.

    ![Ziflow yapılandırmasını yönetme](./media/ziflow-tutorial/tutorial_ziflow_manage.png)

9. Sol üst köşedeki, tıklayın **çoklu oturum açma**.

    ![Ziflow yapılandırma oturum](./media/ziflow-tutorial/tutorial_ziflow_signon.png)

10. Üzerinde **çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Ziflow yapılandırma tek](./media/ziflow-tutorial/tutorial_ziflow_page.png)

    a. Seçin **türü** olarak **SAML2.0**.

    b. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız.

    c. Azure portalından içine yüklediğiniz base-64 kodlanmış sertifikasını karşıya yükle **imzalama sertifikası X509**.

    d. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    e. Gelen **kimlik sağlayıcınız için yapılandırma ayarlarını** bölümünde vurgulanan benzersiz kimlik değerini kopyalayın ve tanımlayıcı ve oturum açma URL'si ekleme **Ziflow etki alanı ve URL'ler bölüm** üzerinde Azure Portalı'nı tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ziflow-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/ziflow-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ziflow-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ziflow-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-ziflow-test-user"></a>Ziflow test kullanıcısı oluşturma

Ziflow için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Ziflow sağlanması gerekir. Ziflow içinde sağlama bir el ile gerçekleştirilen bir görevdir.

Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. İçin Ziflow bir güvenlik yöneticisi olarak oturum açın.

2. Gidin **kişiler** üstte.

    ![Ziflow yapılandırma kişiler](./media/ziflow-tutorial/tutorial_ziflow_people.png)

3. Tıklayın **Ekle** ve ardından **Kullanıcı Ekle**.

    ![Kullanıcı ekleniyor Ziflow yapılandırma](./media/ziflow-tutorial/tutorial_ziflow_add.png)

4. Üzerinde **kullanıcı ekleme** açılan, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleniyor Ziflow yapılandırma](./media/ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin brittasimon@contoso.com.

    b. İçinde **ad** metin kutusunda, Britta gibi kullanıcı adını girin.

    c. İçinde **Soyadı** metin kutusunda, son Simon gibi kullanıcı adını girin.

    d. Ziflow rolünüzü seçin.

    e. Tıklayın **ekleyin 1 kullanıcı**.

    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Ziflow erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Ziflow için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Ziflow**.

    ![Uygulamalar listesinde Ziflow bağlantı](./media/ziflow-tutorial/tutorial_ziflow_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Ziflow kutucuğa tıkladığınızda, otomatik olarak Ziflow uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ziflow-tutorial/tutorial_general_01.png
[2]: ./media/ziflow-tutorial/tutorial_general_02.png
[3]: ./media/ziflow-tutorial/tutorial_general_03.png
[4]: ./media/ziflow-tutorial/tutorial_general_04.png

[100]: ./media/ziflow-tutorial/tutorial_general_100.png

[200]: ./media/ziflow-tutorial/tutorial_general_200.png
[201]: ./media/ziflow-tutorial/tutorial_general_201.png
[202]: ./media/ziflow-tutorial/tutorial_general_202.png
[203]: ./media/ziflow-tutorial/tutorial_general_203.png

