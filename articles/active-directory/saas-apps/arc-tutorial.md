---
title: 'Öğretici: Azure Active Directory Tümleştirme ile yayımlama yay - SSO | Microsoft Docs'
description: Azure Active Directory ve yayımlama yay - SSO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: c1bf5ba68d8242a0ef0831987ac6fd041c62ace9
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969376"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Öğretici: Azure Active Directory Tümleştirme ile yayımlama yay - SSO

Bu öğreticide, yay yayımlama - SSO Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Yay yayımlama - SSO Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Yayımlama yay - SSO erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için yayımlama yay - SSO (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Yay yayımlama ile - SSO, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Yayı yayımlama - abonelik etkin SSO çoklu oturum açma

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Yay yayımlama - galerisinden SSO ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Yay yayımlama - galerisinden SSO ekleme
Yayımlama yay - Azure AD'ye SSO tümleştirmesini yapılandırmak için yayımlama - yay listenizi yönetilen SaaS uygulamaları için SSO galerisinden eklemeniz gerekir.

**Galeriden SSO yay yayımlama - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **yayımlama yay - SSO**seçin **yayımlama yay - SSO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Yayımlama - SSO sonuç listesinde yay](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma yay yayımlama ile test etme - SSO "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak, oturum için Azure AD yay Publishing'de karşılığı kullanıcının bilmesi gerekir - SSO, Azure AD'de bir kullanıcı için olan. Diğer bir deyişle, SSO bir Azure AD kullanıcısı ile ilgili kullanıcı yay yayımlama - arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma yay yayımlama ile-test etmek için SSO, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Yay yayımlama'bir Oluştur - SSO kullanıcı test](#create-an-arc-publishing---sso-test-user)**  - yayımlama yay - kullanıcı Azure AD gösterimini bağlı SSO Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, yay yayımlama içinde - SSO uygulama yapılandırın.

**Azure AD çoklu oturum açma yay yayımlama ile - SSO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **yayımlama yay - SSO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/arc-tutorial/tutorial_arc_samlbase.png)

1. Üzerinde **yayımlama yay - SSO etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Yay yayımlama - SSO etki alanı ve URL'ler tek bilgi'oturum açma](./media/arc-tutorial/tutorial_arc_url.png)

    1. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    1. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Yay yayımlama - SSO etki alanı ve URL'ler tek bilgi'oturum açma](./media/arc-tutorial/tutorial_arc_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [yayımlama yay - SSO istemci Destek ekibine](mailto:inf@washpost.com) bu değerleri almak için. 

1. Yayımlama yay - SSO uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/arc-tutorial/tutorial_arc_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | FirstName | User.givenName |
    | Soyadı | User.surname |
    | e-posta | User.Mail |
    | gruplar | User.assignedroles |

    1. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

     ![Çoklu oturum açmayı yapılandırın](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Çoklu oturum açmayı yapılandırın](./media/arc-tutorial/tutorial_attribute_05.png)
    
    1. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    1. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    1. Bırakın **Namespace** boş.
    
    1. Tıklayın **Tamam**

    > [!NOTE]
    > Burada **grupları** özniteliği ile eşlendi **user.assignedroles**. Bu grup adlarını uygulama yeniden eşlemek için Azure AD'de oluşturulan özel rolleridir. Daha fazla rehberliğe bulabilirsiniz [burada](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de özel roller oluşturma. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/arc-tutorial/tutorial_arc_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/arc-tutorial/tutorial_general_400.png)
    
1. Üzerinde **yayımlama yay - SSO yapılandırma** bölümünde **yapılandırma yay yayımlama - SSO** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Yay yayımlama - SSO yapılandırma](./media/arc-tutorial/tutorial_arc_configure.png) 

1. Çoklu oturum açmayı yapılandırma **yayımlama yay - SSO** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64), oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** için [yay Yayımlama - SSO Destek ekibine](mailto:inf@washpost.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/arc-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/arc-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/arc-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/arc-tutorial/create_aaduser_04.png)

    1. İçinde **adı** kutusuna **BrittaSimon**.

    1. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    1. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    1. **Oluştur**’a tıklayın.
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Bir Yayı yayımlama - SSO test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon yay yayımlama içinde - SSO adlı bir kullanıcı oluşturmaktır. Yayımlama yay - SSO tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, yayımlama yay - SSO henüz mevcut değilse erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [yayımlama yay - SSO Destek ekibine](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma yay yayımlama için - SSO erişimi vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon yay yayımlama için - SSO, atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **yayımlama yay - SSO**.

    ![Yay yayımlama - uygulamalar listesinde SSO bağlantı](./media/arc-tutorial/tutorial_arc_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Yay yayımlama - SSO kutucuk erişim Paneli'nde tıkladığınızda, otomatik olarak imzalanmış, yay yayımlama için - SSO uygulama açma.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

