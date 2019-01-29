---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Elium | Microsoft Docs'
description: Azure Active Directory ve Elium arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: ac88d3accad8aa9ec523ecce5eb11b6c10eb913c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181495"
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Öğretici: Elium ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Elium tümleştirme konusunda bilgi edinin.

Azure AD ile Elium tümleştirme ile aşağıdaki avantajları sağlar:

- Elium erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Elium (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Elium yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Elium çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Elium ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-elium-from-the-gallery"></a>Galeriden Elium ekleme
Azure AD'de Elium tümleştirmesini yapılandırmak için Elium Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Elium eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Elium**seçin **Elium** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Elium](./media/elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Elium sınayın.

Tek iş için oturum açma için Azure AD ne Elium karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Elium ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Elium ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Elium test kullanıcısı oluşturma](#create-an-elium-test-user)**  - kullanıcı Azure AD gösterimini bağlı Elium Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Elium uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Elium yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Elium** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/elium-tutorial/tutorial_elium_samlbase.png)

1. Üzerinde **Elium etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Elium etki alanı ve URL'ler tek oturum açma bilgileri](./media/elium-tutorial/tutorial_elium_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<platform-domain>.elium.com/login/saml2/acs`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Elium etki alanı ve URL'ler tek oturum açma bilgileri](./media/elium-tutorial/tutorial_elium_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri erişmenizi sağlayacak **SP meta veri dosyası** , indirilebilir `https://<platform-domain>.elium.com/login/saml2/metadata`, bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

1. Elium uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/tutorial_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
           
    | Öznitelik Adı | Öznitelik Değeri |   
    | ---------------| ----------------|
    | e-posta   |User.Mail |
    | first_name| User.givenName |
    | Soyadı| User.surname|
    | job_title| user.jobtitle|
    | Şirket| User.CompanyName|
    
    > [!NOTE]
    > Bu varsayılan taleplerdir. **E-posta talebi yalnızca**. Yalnızca e-posta ayrıca sağlama JIT için talep zorunludur. Diğer özel talepler başka bir müşteri platformuna bir müşteri bir platformdan diğerine farklılık gösterebilir.

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/tutorial_attribute_04.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/tutorial_attribute_05.png)

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Ad alanı boş bırakın.
    
    e. **Tamam**’a tıklayın. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/elium-tutorial/tutorial_elium_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/elium-tutorial/tutorial_general_400.png)
    
1. Farklı bir web tarayıcı penceresinde Elium şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **kullanıcı profili** sağ üst köşedeki ve ardından **Yönetim**.

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/user1.png)

1. Seçin **güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/user2.png)

1. Ekranı aşağı kaydırarak **çoklu oturum açma (SSO)** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/user3.png)

    a. Değerini kopyalayın **söz konusu olduğu saml2 tabanlı kimlik doğrulamasını hesabınız için çalıştığını doğrulayın** yapıştırın **oturum açma URL'si** metin üzerinde **Elium etki alanı ve URL'ler** azure'da bölümü Portalı.

    > [!NOTE]
    > SSO yapılandırdıktan sonra her zaman varsayılan uzaktan oturum açma sayfası şu URL'den erişebilirsiniz: `https://<platform_domain>/login/regular/login` 

    b. Seçin **etkinleştirme SAML2 Federasyon** onay kutusu.

    c. Seçin **JIT sağlama** onay kutusu.

    d. Açık **SP meta verileri** tıklayarak **indirme** düğmesi.

    e. Arama **Entityıd** içinde **SP meta verileri** dosyası, kopya **Entityıd** değeri ve yapıştırın **tanımlayıcı** üzerindemetin **Elium etki alanı ve URL'ler** bölümünde Azure portalında. 

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/user4.png)

    f. Arama **AssertionConsumerService** içinde **SP meta verileri** dosyası, kopya **konumu** yapıştırın ve değer **yanıt URL'si** metin kutusu üzerinde **Elium etki alanı ve URL'ler** bölümünde Azure portalında.

    ![Çoklu oturum açmayı yapılandırın](./media/elium-tutorial/user5.png)

    g. Not Defteri'ne Azure portalından indirilen meta veri dosyası açın, içeriğini kopyalayın ve yapıştırın **IDP meta verileri** metin.

    h. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/elium-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/elium-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/elium-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/elium-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-elium-test-user"></a>Bir Elium test kullanıcısı oluşturma

Bu bölümün amacı Elium Britta Simon adlı bir kullanıcı oluşturmaktır. Elium tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Elium erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Elium Destek ekibine](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Elium erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Elium için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Elium**.

    ![Uygulamalar listesinde Elium bağlantı](./media/elium-tutorial/tutorial_elium_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Elium kutucuğa tıkladığınızda, otomatik olarak Elium uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/elium-tutorial/tutorial_general_01.png
[2]: ./media/elium-tutorial/tutorial_general_02.png
[3]: ./media/elium-tutorial/tutorial_general_03.png
[4]: ./media/elium-tutorial/tutorial_general_04.png

[100]: ./media/elium-tutorial/tutorial_general_100.png

[200]: ./media/elium-tutorial/tutorial_general_200.png
[201]: ./media/elium-tutorial/tutorial_general_201.png
[202]: ./media/elium-tutorial/tutorial_general_202.png
[203]: ./media/elium-tutorial/tutorial_general_203.png

