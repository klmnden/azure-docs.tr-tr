---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle OpenAthens | Microsoft Docs'
description: Azure Active Directory ve OpenAthens arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.openlocfilehash: 269b216a94b1233c5f9f9a634fda3c05e46cac90
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435923"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Öğretici: Azure Active Directory OpenAthens ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile OpenAthens tümleştirme konusunda bilgi edinin.

Azure AD ile OpenAthens tümleştirme ile aşağıdaki avantajları sağlar:

- OpenAthens erişimi, Azure AD'de kontrol edebilirsiniz.
- Kullanıcılarınız için OpenAthens (çoklu oturum açma) ile Azure AD hesaplarına otomatik olarak oturum açabilmesi etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile OpenAthens yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir OpenAthens çoklu oturum açma etkin

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden OpenAthens ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-openathens-from-the-gallery"></a>Galeriden OpenAthens ekleme
Azure AD'de OpenAthens tümleştirmesini yapılandırmak için OpenAthens Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden OpenAthens eklemek için**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gözat **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi][2]
    
1. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **OpenAthens**seçin **OpenAthens** sonuçlar paneli ve ardından **Ekle** düğmesi.

    ![Sonuç listesinde OpenAthens](./media/openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı OpenAthens ile test etme

Tek iş için oturum açma için Azure AD ne OpenAthens karşılığı kullanıcı için kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki içinde OpenAthens kurmak gerekir.

OpenAthens içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma OpenAthens ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on), bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user), Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. [Bir OpenAthens test kullanıcısı oluşturma](#create-a-openathens-test-user), kullanıcının Azure AD gösterimini bağlı OpenAthens Britta simon'un bir karşılığı vardır.
1. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. [Çoklu oturum açmayı test](#test-single-sign-on), yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve OpenAthens uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile OpenAthens yapılandırmak için**

1. Azure portalında, üzerinde **OpenAthens** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusunda **SAML tabanlı oturum açma** olarak **modu**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/openathens-tutorial/tutorial_openathens_samlbase.png)

1. İçinde **OpenAthens etki alanı ve URL'ler** bölümünde, bir değer girin `https://login.openathens.net/saml/2/metadata-sp` içinde **tanımlayıcı** metin kutusu.

    ![Oturum açma bilgileri çoklu OpenAthens etki alanı ve URL'ler](./media/openathens-tutorial/tutorial_openathens_url.png)

1. İçinde **SAML imzalama sertifikası** bölümünden **meta veri XML**ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Bağlantı AMSL imzalama sertifikası yükle](./media/openathens-tutorial/tutorial_openathens_certificate.png) 

1. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi Kaydet](./media/openathens-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde OpenAthens şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Seçin **bağlantıları** altındaki listeden **Yönetim** sekmesi. 

    ![Çoklu oturum açmayı yapılandırın](./media/openathens-tutorial/tutorial_openathens_application1.png)

1. Seçin **SAML 1.1/2.0**ve ardından **yapılandırma** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
1. Yapılandırmasını eklemek için seçin **Gözat** Azure portalından indirdiğiniz meta veri .xml dosyasını karşıya yükleme düğmesini ve ardından **Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/openathens-tutorial/tutorial_openathens_application3.png)

1. Altında aşağıdaki adımları **ayrıntıları** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. İçinde **görünen adı eşlemesi**seçin **kullanım özniteliği**.

    b. İçinde **Display name özniteliği** metin kutusunda, değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. İçinde **benzersiz kullanıcı eşleme**seçin **kullanım özniteliği**.

    d. İçinde **benzersiz kullanıcı özniteliği** metin kutusunda, değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. İçinde **durumu**, üç onay kutusunu seçin.

    f. İçinde **yerel hesap**seçin **otomatik olarak**.

    g. Seçin **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekme ve katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Ekli belge özelliği hakkında daha fazla bilgi için bkz. [Azure AD belgeleri katıştırılmış](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında "Britta Simon." adlı bir test kullanıcısı oluşturma sağlamaktır

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturma**

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/openathens-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/openathens-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/openathens-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/openathens-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusunda, **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, Britta Simon için e-posta adresi yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** metin kutusu.

    d. **Oluştur**’u seçin.
  
### <a name="create-an-openathens-test-user"></a>Bir OpenAthens test kullanıcısı oluşturma

Tam zamanında sağlama OpenAthens destekler ve kullanıcılar, başarılı kimlik doğrulamasından sonra otomatik olarak oluşturulur. Bu bölümde herhangi bir eylem gerçekleştirmeniz gerekmez.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için OpenAthens erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon OpenAthens için atamak için**

1. Uygulamaları görüntüleyin, Azure portalında, açık dizin görünümüne gidin ve Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. İçinde **uygulamaları** listesinden **OpenAthens**.

    ![Uygulamalar listesinde OpenAthens bağlantı](./media/openathens-tutorial/tutorial_openathens_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **atama Ekle** bölmesi.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** listesinden **Britta Simon**.

1. Seçin **seçin** düğmesine **kullanıcılar ve gruplar** listesi.

1. Seçin **atama** düğmesine **atama Ekle** bölmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Seçtiğinizde, **OpenAthens** döşeme erişim panelinde OpenAthens uygulamanıza, otomatik olarak imzalanıp imzalanmayacağını.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi için bkz. [Azure AD ile kullanmak için SaaS uygulama tümleştirmesi öğreticileri](tutorial-list.md).
* Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma hakkında daha fazla bilgi için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

<!--Image references-->

[1]: ./media/openathens-tutorial/tutorial_general_01.png
[2]: ./media/openathens-tutorial/tutorial_general_02.png
[3]: ./media/openathens-tutorial/tutorial_general_03.png
[4]: ./media/openathens-tutorial/tutorial_general_04.png

[100]: ./media/openathens-tutorial/tutorial_general_100.png

[200]: ./media/openathens-tutorial/tutorial_general_200.png
[201]: ./media/openathens-tutorial/tutorial_general_201.png
[202]: ./media/openathens-tutorial/tutorial_general_202.png
[203]: ./media/openathens-tutorial/tutorial_general_203.png

