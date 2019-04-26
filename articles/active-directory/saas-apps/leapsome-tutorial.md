---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Leapsome | Microsoft Docs'
description: Azure Active Directory ve Leapsome arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37549cc76e1490b0758de8e296523b0e70c98dbf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60260623"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Öğretici: Leapsome ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Leapsome tümleştirme konusunda bilgi edinin.

Azure AD ile Leapsome tümleştirme ile aşağıdaki avantajları sağlar:

- Leapsome erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Leapsome (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Leapsome yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Leapsome çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Leapsome ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-leapsome-from-the-gallery"></a>Galeriden Leapsome ekleme
Azure AD'de Leapsome tümleştirmesini yapılandırmak için Leapsome Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Leapsome eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Leapsome**seçin **Leapsome** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Leapsome](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Leapsome sınayın.

Tek iş için oturum açma için Azure AD ne Leapsome karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Leapsome ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Leapsome ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Leapsome test kullanıcısı oluşturma](#create-a-leapsome-test-user)**  - kullanıcı Azure AD gösterimini bağlı Leapsome Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Leapsome uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Leapsome yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Leapsome** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

1. Üzerinde **Leapsome etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Leapsome etki alanı ve URL'ler tek oturum açma bilgileri](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://www.leapsome.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Leapsome etki alanı ve URL'ler tek oturum açma bilgileri](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > Önceki yanıt URL'si ve oturum açma URL değeri, gerçek değeri değil. Bunlar gerçek değerlerle öğreticinin ilerleyen bölümlerinde açıklanan güncelleştirir.

1. Leapsome uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki anlık görüntüde bir örnek gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri | Ad alanı |
    | ---------------| --------------- | --------- |   
    | firstName | User.givenName | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | Soyadı | User.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | başlık | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | resmi | Çalışanın resim URL'si | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > Resim özniteliğinin değerini gerçek değil. Bu değer, gerçek resim URL'si ile güncelleştirin. Bu değer kişi almak için [Leapsome istemci Destek ekibine](mailto:support@leapsome.com).
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin ilgili satır için ad alanı URI'si girin.
    
    e. Tıklayın **Tamam**

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/leapsome-tutorial/tutorial_general_400.png)
    
1. Üzerinde **Leapsome yapılandırma** bölümünde **yapılandırma Leapsome** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Leapsome yapılandırma](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

1. Farklı bir web tarayıcı penceresinde Leapsome için bir güvenlik yöneticisi olarak oturum açın.

1. Sağ üstte ayarları logolardan birine tıklayın ve ardından **yönetici ayarları**. 

    ![Leapsome Ayarla](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

1. Sol menü çubuğunda tıklatın **çoklu oturum açma (SSO)** ve **SAML tabanlı çoklu oturum açma (SSO)** sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a. Seçin **etkinleştirme SAML tabanlı çoklu oturum açma**.

    b. Kopyalama **oturum açma URL'si (oturum açma başlatmak için kullanıcılarınızın burada işaret)** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **Leapsome etki alanı ve URL'ler** bölümü Azure portalı.

    c. Kopyalama **yanıt URL'si (yanıtı alır, kimlik sağlayıcınızdan)** yapıştırın ve değer **yanıt URL'si** metin kutusunda **Leapsome etki alanı ve URL'ler** bölümü Azure portalı.

    d. İçinde **SSO oturum açma URL'si (Kimlik sağlayıcısı tarafından sağlanan)** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, Azure Portalı'ndan kopyaladığınız.

    e. --Sertifika başlar ve son sertifika--yorumlar olmadan Azure portalından indirilen ve yapıştırın sertifikayı kopyalamanız **(Kimlik sağlayıcısı tarafından sağlanan) sertifikası** metin.

    f. Tıklayın **güncelleştirme SSO ayarlarını**.
    
### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/leapsome-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/leapsome-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/leapsome-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/leapsome-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-leapsome-test-user"></a>Leapsome test kullanıcısı oluşturma

Bu bölümde, Britta Simon Leapsome içinde adlı bir kullanıcı oluşturun. Çalışmak [Leapsome istemci Destek ekibine](mailto:support@leapsome.com) kullanıcı veya Leapsome platformunda beyaz listeye alınması gereken etki alanı eklemek için. Etki alanı ekibi tarafından eklenirse, kullanıcıların otomatik olarak Leapsome platforma sağlanan. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Leapsome erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Leapsome için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Leapsome**.

    ![Uygulamalar listesinde Leapsome bağlantı](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Leapsome kutucuğa tıkladığınızda, otomatik olarak Leapsome uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

