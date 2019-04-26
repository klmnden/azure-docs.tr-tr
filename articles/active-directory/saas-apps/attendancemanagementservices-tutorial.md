---
title: 'Öğretici: Katılım Yönetim Hizmetleri ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve katılım Yönetim Hizmetleri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3642bea878ca4d1582319e5e1d964dfa43ff061
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60283714"
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Öğretici: Katılım Yönetim Hizmetleri ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile katılım Yönetim Hizmetleri Tümleştirme konusunda bilgi edinin.

Azure AD'ye katılım Yönetim Hizmetleri Tümleştirme ile aşağıdaki avantajları sağlar:

- Katılım Yönetim Hizmetleri erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan için katılım Yönetim Hizmetleri (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Katılım Yönetim Hizmetleri ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir katılımcı Yönetim Hizmetleri çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden katılım yönetim hizmet ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-attendance-management-services-from-the-gallery"></a>Galeriden katılım yönetim hizmet ekleme
Azure AD'ye katılım Management Services tümleştirmesini yapılandırmak için katılım Yönetim Hizmetleri Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden katılım Yönetim Hizmetleri eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **katılım Yönetim Hizmetleri**seçin **katılım Yönetim Hizmetleri** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde katılım Yönetim Hizmetleri](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve katılım yönetim "Britta Simon" adlı bir test kullanıcı tabanlı hizmetler ile Azure AD çoklu oturum açma testi.

Tek iş için oturum açma için Azure AD katılımını Yönetim Hizmetleri karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı katılımını yönetim Hizmetleri'nde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma katılım Yönetim Hizmetleri ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **Bir katılımcı Yönetim Hizmetleri test kullanıcısı oluşturma** - bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı katılımcı yönetim hizmetleri sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve katılım Yönetim Hizmetleri uygulamanızda çoklu oturum açma yapılandırın.

**Azure AD çoklu oturum açma katılım Yönetim Hizmetleri ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **katılım Yönetim Hizmetleri** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_samlbase.png)

1. Üzerinde **katılım Yönetim Hizmetleri etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Oturum açma bilgileri tek bir katılımcı Yönetim Hizmetleri etki alanı ve URL'ler](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://id.obc.jp/<tenant information >/`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [katılım Management Services İstemcisi Destek ekibine](https://www.obcnet.jp/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/attendancemanagementservices-tutorial/tutorial_general_400.png)

1. Üzerinde **katılım Yönetim Hizmetleri Yapılandırması** bölümünde **katılım yönetim hizmetlerini yapılandırın** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Katılım Yönetim Hizmetleri Yapılandırması](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_configure.png) 

1. Farklı bir tarayıcı penceresinde, katılım Yönetim Hizmetleri şirket sitenize yönetici olarak oturum.

1. Tıklayarak **SAML kimlik doğrulaması** altında **güvenlik yönetimi bölümünde**.

    ![Katılım Yönetim Hizmetleri Yapılandırması](./media/attendancemanagementservices-tutorial/user1.png)

1. Aşağıdaki adımları gerçekleştirin:

    ![Katılım Yönetim Hizmetleri Yapılandırması](./media/attendancemanagementservices-tutorial/user2.png)

    a. Seçin **kullanım SAML kimlik doğrulaması**.

    b. İçinde **tanımlayıcı** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız. 

    c. İçinde **kimlik doğrulama uç noktası URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. Tıklayın **bir dosya seçin** Azure AD'den yüklediğiniz sertifikayı karşıya yüklemek için.

    e. Seçin **parola ile kimlik doğrulaması devre dışı bırak**.

    f. Tıklayın **kayıt**

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada! Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/attendancemanagementservices-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/attendancemanagementservices-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/attendancemanagementservices-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/attendancemanagementservices-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-attendance-management-services-test-user"></a>Bir katılımcı Yönetim Hizmetleri test kullanıcısı oluşturma

Azure AD kullanıcıların katılımını Yönetim Hizmetleri için oturum etkinleştirmek için bunlar katılım Yönetim Hizmetleri içine sağlanması gerekir. Katılım Yönetim Hizmetleri söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Katılım Yönetim Hizmetleri şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **kullanıcı yönetimi** altında **güvenlik yönetimi bölümünde**.

    ![Çalışan Ekle](./media/attendancemanagementservices-tutorial/user5.png)

1. Tıklayın **yeni kurallar oturum açma**.

    ![Çalışan Ekle](./media/attendancemanagementservices-tutorial/user3.png)

1. İçinde **OBCiD bilgi** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/attendancemanagementservices-tutorial/user4.png)

    a. İçinde **OBCiD** metin kutusuna kullanıcı e-posta türünü ister **BrittaSimon\@contoso.com**.

    b. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    c. Tıklayın **kayıt**


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, katılım Yönetim hizmetlerine erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon katılım Yönetim hizmetlerine atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **katılım Yönetim Hizmetleri**.

    ![Uygulamalar listesinde katılım Yönetim Hizmetleri Bağla](./media/attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli katılım Yönetim Hizmetleri kutucuğa tıkladığınızda, size otomatik olarak katılım Yönetim Hizmetleri uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/attendancemanagementservices-tutorial/tutorial_general_01.png
[2]: ./media/attendancemanagementservices-tutorial/tutorial_general_02.png
[3]: ./media/attendancemanagementservices-tutorial/tutorial_general_03.png
[4]: ./media/attendancemanagementservices-tutorial/tutorial_general_04.png

[100]: ./media/attendancemanagementservices-tutorial/tutorial_general_100.png

[200]: ./media/attendancemanagementservices-tutorial/tutorial_general_200.png
[201]: ./media/attendancemanagementservices-tutorial/tutorial_general_201.png
[202]: ./media/attendancemanagementservices-tutorial/tutorial_general_202.png
[203]: ./media/attendancemanagementservices-tutorial/tutorial_general_203.png

