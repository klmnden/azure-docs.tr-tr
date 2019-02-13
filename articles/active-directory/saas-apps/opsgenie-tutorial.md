---
title: 'Öğretici: OpsGenie ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: OpsGenie ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9858fc38698ae2c5bd272a3494bcf02bce2d8e9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194606"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Öğretici: OpsGenie ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile OpsGenie tümleştirme konusunda bilgi edinin.

OpsGenie Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- OpsGenie erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan (çoklu oturum açma) için OpsGenie ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi OpsGenie ile yapılandırma için aşağıdakiler gerekir:

- Azure AD aboneliği
- OpsGenie çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. OpsGenie galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie galeri ekleme
Azure AD'de OpsGenie tümleştirmesini yapılandırmak için OpsGenie Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden OpsGenie eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **OpsGenie**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/opsgenie-tutorial/tutorial_opsgenie_search.png)

1. Sonuçlar panelinde seçin **OpsGenie**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı OpsGenie ile test edin.

Tek iş için oturum açma için Azure AD ne OpsGenie karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının OpsGenie ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

OpsGenie içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma OpsGenie ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[OpsGenie test kullanıcısı oluşturma](#creating-a-opsgenie-test-user)**  - kullanıcı Azure AD gösterimini bağlı OpsGenie Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve OpsGenie uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma OpsGenie ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **OpsGenie** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

1. Üzerinde **OpsGenie etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/opsgenie-tutorial/tutorial_opsgenie_url.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://app.opsgenie.com/auth/login`

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/opsgenie-tutorial/tutorial_opsgenie_certificate.png)

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/opsgenie-tutorial/tutorial_general_400.png)

1. Üzerinde **OpsGenie yapılandırma** bölümünde **yapılandırma OpsGenie** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** hızlı başvuru bölümünden.

    ![Çoklu oturum açmayı yapılandırın](./media/opsgenie-tutorial/tutorial_opsgenie_configure.png)

1. Başka bir tarayıcı örneğinde açın ve ardından günlük OpsGenie yönetici olarak açın.

1. Tıklayın **ayarları**ve ardından **çoklu oturum açma** sekmesi.
   
    ![OpsGenie çoklu oturum açma](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

1. SSO'yu etkinleştirmek üzere işaretleyin **etkin**.
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

1. İçinde **sağlayıcısı** bölümünde **Azure Active Directory** sekmesi.
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

1. Azure Active Directory sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. İçinde **SAML 2.0 uç noktası** metin kutusu, yapıştırma **tek oturum üzerinde hizmet URL'si**Azure portaldan kopyaladığınız değeri.
    
    b. İçinde **meta veri URL'si:** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portaldan kopyaladığınız değeri.
    
    c. Tıklayın **değişiklikleri kaydetmek**.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/opsgenie-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/opsgenie-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/opsgenie-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/opsgenie-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-opsgenie-test-user"></a>OpsGenie test kullanıcısı oluşturma

Amacı, bu bölümün OpsGenie içinde Britta Simon adlı bir kullanıcı oluşturmaktır. 

1. Bir web tarayıcısı penceresinde OpsGenie Kiracı yönetici olarak oturum açın.

1. Tıklayarak kullanıcıların listesine gidin **kullanıcı** sol panelde.
   
   ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

1. Tıklayın **kullanıcı ekleme**.

1. Üzerinde **Kullanıcı Ekle** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   ![OpsGenie Settings](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   a. İçinde **e-posta** metin BrittaSimon türü e-posta adresi, Azure Active Directory'de ele.
   
   b. İçinde **tam adı** metin kutusuna **Britta Simon**.
   
   c. **Kaydet**’e tıklayın. 

>[!NOTE]
>Britta kendi profilinizi ayarlama yönergeleri içeren bir e-posta alır.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümün OpsGenie için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon OpsGenie için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **OpsGenie**.

    ![Çoklu oturum açmayı yapılandırın](./media/opsgenie-tutorial/tutorial_opsgenie_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde OpsGenie kutucuğa tıkladığınızda, otomatik olarak OpsGenie uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/opsgenie-tutorial/tutorial_general_203.png

