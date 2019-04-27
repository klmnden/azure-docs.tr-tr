---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Workteam | Microsoft Docs'
description: Azure Active Directory ve Workteam arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 41df17a1-ba69-414f-8ec3-11079b030df6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7cd986544dfb1472f5cc8a013fec951dca42a59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60523773"
---
# <a name="tutorial-azure-active-directory-integration-with-workteam"></a>Öğretici: Workteam ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Workteam tümleştirme konusunda bilgi edinin.

Azure AD ile Workteam tümleştirme ile aşağıdaki avantajları sağlar:

- Workteam erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Workteam (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Workteam yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Workteam çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Workteam ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-workteam-from-the-gallery"></a>Galeriden Workteam ekleme
Azure AD'de Workteam tümleştirmesini yapılandırmak için Workteam Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Workteam eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Workteam**seçin **Workteam** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Workteam](./media/workteam-tutorial/tutorial_workteam_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Workteam sınayın.

Tek iş için oturum açma için Azure AD ne Workteam karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Workteam ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Workteam ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Workteam test kullanıcısı oluşturma](#create-a-workteam-test-user)**  - kullanıcı Azure AD gösterimini bağlı Workteam Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Workteam uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Workteam yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Workteam** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/workteam-tutorial/tutorial_workteam_samlbase.png)

3. Üzerinde **Workteam etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Workteam etki alanı ve URL'ler tek oturum açma bilgileri](./media/workteam-tutorial/tutorial_workteam_url.png)

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Workteam etki alanı ve URL'ler tek oturum açma bilgileri](./media/workteam-tutorial/tutorial_workteam_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://app.workte.am`
     
5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/workteam-tutorial/tutorial_workteam_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/workteam-tutorial/tutorial_general_400.png)
    
7. Üzerinde **Workteam yapılandırma** bölümünde **yapılandırma Workteam** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Workteam yapılandırma](./media/workteam-tutorial/tutorial_workteam_configure.png) 

8. Farklı bir web tarayıcı penceresinde Workteam için bir güvenlik yöneticisi olarak oturum açın.

9. Sağ üst köşedeki tıklayarak **logosu profil** ve ardından **kuruluş ayarlarına**. 

    ![Workteam ayarları](./media/workteam-tutorial/tutorial_workteam_settings.png)

10. Altında **kimlik doğrulaması** bölümünde, tıklayarak **ayarları logosu**.

     ![Workteam azure](./media/workteam-tutorial/tutorial_workteam_azure.png)

11. Üzerinde **SAML ayarlarını** sayfasında, aşağıdaki adımları gerçekleştirin:

     ![Workteam saml](./media/workteam-tutorial/tutorial_workteam_saml.png)

    a. Seçin **SAML IDP** olarak **AD Azure**.

    b. İçinde **SAML çoklu oturum açma hizmeti URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız.

    c. İçinde **SAML varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız.

    d. Not Defteri'nde açın **base-64 kodlamalı sertifika** Azure portalından indirdiğiniz, içeriğini kopyalayın ve ardından yapıştırın **SAML imzalama sertifikası (Base64)** kutusu.

    e. **Tamam** düğmesine tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/workteam-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/workteam-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/workteam-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/workteam-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-workteam-test-user"></a>Workteam test kullanıcısı oluşturma

Azure AD kullanıcılarının oturum açma için Workteam etkinleştirmek için bunlar Workteam sağlanması gerekir. Workteam içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Workteam bir güvenlik yöneticisi olarak oturum açın.

2. Üst Orta kısmındaki üzerinde **kuruluş ayarlarına** sayfasında **kullanıcılar** ve ardından **yeni kullanıcı**.

    ![Workteam kullanıcı](./media/workteam-tutorial/tutorial_workteam_user.png)

3. Üzerinde **yeni çalışan** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Workteam newuser](./media/workteam-tutorial/tutorial_workteam_newuser.png)

    a. İçinde **adı** metin kutusunda, gibi kullanıcı adını girin **Brittasimon**.

    b. İçinde **e-posta** metin kutusuna, kullanıcının gibi e-posta girin **Brittasimon\@contoso.com**.

    c. **Tamam** düğmesine tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Workteam erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Workteam için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Workteam**.

    ![Uygulamalar listesinde Workteam bağlantı](./media/workteam-tutorial/tutorial_workteam_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Workteam kutucuğa tıkladığınızda, otomatik olarak Workteam uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/workteam-tutorial/tutorial_general_01.png
[2]: ./media/workteam-tutorial/tutorial_general_02.png
[3]: ./media/workteam-tutorial/tutorial_general_03.png
[4]: ./media/workteam-tutorial/tutorial_general_04.png

[100]: ./media/workteam-tutorial/tutorial_general_100.png

[200]: ./media/workteam-tutorial/tutorial_general_200.png
[201]: ./media/workteam-tutorial/tutorial_general_201.png
[202]: ./media/workteam-tutorial/tutorial_general_202.png
[203]: ./media/workteam-tutorial/tutorial_general_203.png

