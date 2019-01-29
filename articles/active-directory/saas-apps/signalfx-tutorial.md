---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile SignalFx | Microsoft Docs'
description: Azure Active Directory ve SignalFx arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 6f2d869f345aeb8f50d42de6b1533b849ffb2182
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197577"
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Öğretici: SignalFx ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SignalFx tümleştirme konusunda bilgi edinin.

Azure AD ile SignalFx tümleştirme ile aşağıdaki avantajları sağlar:

- SignalFx erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için SignalFx (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SignalFx yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik SignalFx çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SignalFx ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-signalfx-from-the-gallery"></a>Galeriden SignalFx ekleme
Azure AD'de SignalFx tümleştirmesini yapılandırmak için SignalFx Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SignalFx eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **SignalFx**seçin **SignalFx** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde SignalFx](./media/signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı SignalFx sınayın.

Tek iş için oturum açma için Azure AD ne SignalFx karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SignalFx ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma SignalFx ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SignalFx test kullanıcısı oluşturma](#create-a-signalfx-test-user)**  - kullanıcı Azure AD gösterimini bağlı SignalFx Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SignalFx uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SignalFx yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SignalFx** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/signalfx-tutorial/tutorial_signalfx_samlbase.png)

1. Üzerinde **SignalFx etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SignalFx etki alanı ve URL'ler tek oturum açma bilgileri](./media/signalfx-tutorial/tutorial_signalfx_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `https://api.signalfx.com/v1/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > Önceki değerin gerçek bir değer değil. Bu öğreticinin ilerleyen bölümlerinde açıklanan gerçek yanıt URL'si ile değeri güncelleştirin.

1. SignalFx uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.   

    ![Çoklu oturum açmayı yapılandırın](./media/signalfx-tutorial/tutorial_signalfx_attribute.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | User.FirstName          | User.givenName |
    | User.email          | User.Mail |
    | PersonImmutableID       | User.userPrincipalName    |
    | User.LastName       | User.surname    |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırma Ekle](./media/signalfx-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açma Addattb yapılandırın](./media/signalfx-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde, aşağıdaki adımları gerçekleştirin: 

    ![Sertifika indirme bağlantısı](./media/signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    b. Tıklayın **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/signalfx-tutorial/tutorial_general_400.png)

1. Üzerinde **SignalFx yapılandırma** bölümünde **yapılandırma SignalFx** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![SignalFx yapılandırma](./media/signalfx-tutorial/tutorial_signalfx_configure.png) 

1. SignalFx şirketinizin sitesi için yönetici olarak oturum.

1. Üzerinde üst öğesini SignalFx içinde **tümleştirmeler** tümleştirmeler sayfasını açın.

    ![SignalFx tümleştirme](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

1. Tıklayarak **Azure Active Directory** altında kutucuğuna **oturum açma hizmetleri** bölümü.
 
    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

1. Tıklayarak **yeni tümleştirme** altında **yükleme** sekmesinde aşağıdaki adımları gerçekleştirin:
 
    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. İçinde **adı** metin kutusuna, yeni bir tümleştirme adı ister **OurOrgName SAML SSO**.

    b. Kopyalama **tümleştirme kimliği** değeri ve WITH append **yanıt URL'si** gibi `https://api.signalfx.com/v1/saml/acs/<integration ID>` içinde **yanıt URL'si** textbox'ın **SignalFx etki alanı ve URL'ler** bölümü Azure Portalı'nda.

    c. Tıklayarak **dosyasını karşıya yükle** karşıya yüklemek için **Base64 olarak kodlanmış sertifika** Azure portalından indirilen **sertifika** metin.

    d. İçinde **veren URL'si** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız.

    e. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** hangi Azure portaldan kopyaladığınız.

    f. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/signalfx-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/signalfx-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/signalfx-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/signalfx-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-signalfx-test-user"></a>SignalFx test kullanıcısı oluşturma

Bu bölümün amacı SignalFx Britta Simon adlı bir kullanıcı oluşturmaktır. SignalFx tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa SignalFx erişme denemesi sırasında oluşturulur.

Bir kullanıcı için SignalFx SAML SSO ilk kez oturum açtığında [SignalFx Destek ekibine](mailto:kmazzola@signalfx.com) bunları kimliğini doğrulamak için bunlar üzerinden tıklatmalısınız bağlantısı içeren bir e-posta gönderir. Bu, yalnızca ilk kez oturum açtığında gerçekleşir; sonraki oturum açma denemesi e-posta doğrulama gerektirmez.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [SignalFx destek ekibi](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için SignalFx erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon SignalFx için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SignalFx**.

    ![Uygulamalar listesinde SignalFx bağlantı](./media/signalfx-tutorial/tutorial_signalfx_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde SignalFx kutucuğa tıkladığınızda, otomatik olarak SignalFx uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/signalfx-tutorial/tutorial_general_01.png
[2]: ./media/signalfx-tutorial/tutorial_general_02.png
[3]: ./media/signalfx-tutorial/tutorial_general_03.png
[4]: ./media/signalfx-tutorial/tutorial_general_04.png

[100]: ./media/signalfx-tutorial/tutorial_general_100.png

[200]: ./media/signalfx-tutorial/tutorial_general_200.png
[201]: ./media/signalfx-tutorial/tutorial_general_201.png
[202]: ./media/signalfx-tutorial/tutorial_general_202.png
[203]: ./media/signalfx-tutorial/tutorial_general_203.png

