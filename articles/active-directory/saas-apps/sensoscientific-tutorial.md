---
title: 'Öğretici: Azure Active Directory SensoScientific kablosuz sıcaklık sistem izleme ile tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve SensoScientific kablosuz sıcaklık izleme sistemi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33a625e82f41bee1b8e3980192076d24a7471953
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60340782"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Öğretici: Azure Active Directory SensoScientific kablosuz sıcaklık sistem izleme ile tümleştirmesi

Bu öğreticide, Azure Active Directory (Azure AD) ile SensoScientific kablosuz sıcaklık izleme sistemi tümleştirme konusunda bilgi edinin.

Azure AD ile SensoScientific kablosuz sıcaklık izleme sistemi tümleştirme ile aşağıdaki avantajları sağlar:

- SensoScientific kablosuz sıcaklık izleme sistemi erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan SensoScientific kablosuz sıcaklık izleme sistemine (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi SensoScientific kablosuz sıcaklık izleme sistemi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir SensoScientific kablosuz sıcaklık izleme sistemi çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SensoScientific kablosuz sıcaklık sistem izleme ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Galeriden SensoScientific kablosuz sıcaklık sistem izleme ekleme
Azure AD'de SensoScientific kablosuz sıcaklık sistem izleme tümleştirmesini yapılandırmak için SensoScientific kablosuz sıcaklık izleme sistemi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SensoScientific kablosuz sıcaklık sistem izleme eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **SensoScientific kablosuz sıcaklık izleme sistemi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_search.png)

1. Sonuçlar panelinde seçin **SensoScientific kablosuz sıcaklık izleme sistemi**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SensoScientific kablosuz Sıcaklık "Britta Simon." adlı bir test kullanıcı tabanlı sistem izleme ile test etme

Tek iş için oturum açma için Azure AD ne SensoScientific kablosuz Sıcaklık İzleme sisteminde karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı SensoScientific kablosuz sıcaklık izleme sistemi arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** SensoScientific kablosuz sıcaklık sistem izleme.

Yapılandırma ve Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık izleme sistemi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SensoScientific kablosuz Sıcaklık İzleme sistem test kullanıcısı oluşturma](#creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user)**  - bir karşılığı Britta simon'un SensoScientific kablosuz sıcaklık kullanıcı Azure AD gösterimini bağlı sistem izleme sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SensoScientific kablosuz sıcaklık izleme sistemi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma SensoScientific kablosuz sıcaklık izleme sistemi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SensoScientific kablosuz sıcaklık izleme sistemi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_samlbase.png)

1. Üzerinde **SensoScientific kablosuz sıcaklık izleme sistemi etki alanı ve URL'ler** bölümü gerekmez, uygulama zaten Azure ile önceden tümleştirilmiş olduğu gibi tüm adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_url.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_general_400.png)

1. Üzerinde **SensoScientific kablosuz sıcaklık izleme sistemi yapılandırmasını** bölümünde **SensoScientific kablosuz sıcaklık izleme sistemi yapılandırma** açmak için **Yapılandır oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği** ve **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_configure.png) 

1. SensoScientific kablosuz sıcaklık izleme sistemi uygulamanıza yönetici olarak oturum açın.

1. Üst gezinti menüsünde **yapılandırma** ve Git **yapılandırma** altında **çoklu oturum açma** üzerinde tek oturum ayarlarını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png) 

1. İçinde **tek oturum açma ayarları** form aşağıdaki adımları gerçekleştirin:
 
    a. Seçin **verenin adı** olarak Azure AD.
    
    b. Yapıştırma **SAML varlık kimliği** veren URL'si metin kutusuna Azure portalından kopyalanan.
    
    c. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , çoklu oturum açma hizmeti URL'si metin kutusuna Azure portaldan kopyaladığınız.

    d. Yapıştırma **oturum kapatma URL'si** , çoklu oturum kapatma hizmeti URL'si metin kutusuna Azure portaldan kopyaladığınız.

    e. Azure Portalı'ndan yüklemiş ve buraya yükleyin sertifika göz atın.
    
    f. **Kaydet**’e tıklayın.
  
> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sensoscientific-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>SensoScientific kablosuz Sıcaklık İzleme sistem test kullanıcısı oluşturma

Azure AD kullanıcılarının SensoScientific kablosuz sıcaklık izleme sistemi için oturum açmayı etkinleştirmek için bunlar SensoScientific kablosuz sıcaklık izleme sistemine sağlanması gerekir. Çalışmak [SensoScientific kablosuz sıcaklık izleme sistemi, Destek ekibine](https://www.sensoscientific.com/contact-us/) SensoScientific kablosuz sıcaklık izleme sistemi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, SensoScientific kablosuz sıcaklık izleme sistemine erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon SensoScientific kablosuz sıcaklık izleme sistemine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **SensoScientific kablosuz sıcaklık izleme sistemi**.

    ![Çoklu oturum açmayı yapılandırın](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin. Erişim panelinde SensoScientific kablosuz sıcaklık izleme sistemi kutucuğuna tıklayın, otomatik olarak imzalanmış SensoScientific kablosuz sıcaklık izleme sistemi uygulamanıza açma. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sensoscientific-tutorial/tutorial_general_01.png
[2]: ./media/sensoscientific-tutorial/tutorial_general_02.png
[3]: ./media/sensoscientific-tutorial/tutorial_general_03.png
[4]: ./media/sensoscientific-tutorial/tutorial_general_04.png

[100]: ./media/sensoscientific-tutorial/tutorial_general_100.png

[200]: ./media/sensoscientific-tutorial/tutorial_general_200.png
[201]: ./media/sensoscientific-tutorial/tutorial_general_201.png
[202]: ./media/sensoscientific-tutorial/tutorial_general_202.png
[203]: ./media/sensoscientific-tutorial/tutorial_general_203.png

