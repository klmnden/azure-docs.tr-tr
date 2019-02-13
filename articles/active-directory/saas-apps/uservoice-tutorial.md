---
title: 'Öğretici: UserVoice ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve UserVoice arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94e077e07796fc111c35b6571459a5e316096ebc
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206643"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Öğretici: UserVoice ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile UserVoice tümleştirme konusunda bilgi edinin.

UserVoice Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- UserVoice erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Uservoice'a (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

UserVoice ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir UserVoice çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. UserVoice galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-uservoice-from-the-gallery"></a>UserVoice galeri ekleme
Azure AD'de UserVoice tümleştirmesini yapılandırmak için UserVoice Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden UserVoice eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **UserVoice**seçin **UserVoice** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde UserVoice](./media/uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı UserVoice ile test edin.

Tek iş için oturum açma için Azure AD ne UserVoice karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve UserVoice ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Uservoice'da, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma UserVoice ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[UserVoice test kullanıcısı oluşturma](#create-a-uservoice-test-user)**  - kullanıcı Azure AD gösterimini bağlı UserVoice Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve UserVoice uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma UserVoice ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **UserVoice** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/uservoice-tutorial/tutorial_uservoice_samlbase.png)

1. Üzerinde **UserVoice etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![UserVoice etki alanı ve URL'ler tek oturum açma bilgileri](./media/uservoice-tutorial/tutorial_uservoice_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenantname>.UserVoice.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [UserVoice istemci Destek ekibine](https://www.uservoice.com/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifika değeri.

    ![Sertifika indirme bağlantısı](./media/uservoice-tutorial/tutorial_uservoice_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/uservoice-tutorial/tutorial_general_400.png)

1. Üzerinde **UserVoice yapılandırma** bölümünde **yapılandırma UserVoice** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![UserVoice yapılandırma](./media/uservoice-tutorial/tutorial_uservoice_configure.png) 

1. Farklı bir web tarayıcı penceresinde sitenize UserVoice şirket yönetici olarak oturum açın.

1. Üst araç çubuğunda tıklatın **ayarları**ve ardından **Web portalı** menüsünde.
   
    ![Uygulama tarafında ayarları bölümündeki](./media/uservoice-tutorial/ic777519.png "ayarları")

1. Üzerinde **Web portalı** sekmesinde **kullanıcı kimlik doğrulaması** bölümünde **Düzenle** açmak için **kullanıcı kimlik doğrulamasını Düzenle** iletişim sayfası.
   
    ![Web portalı sekmesini](./media/uservoice-tutorial/ic777520.png "Web portalı")

1. Üzerinde **kullanıcı kimlik doğrulamasını Düzenle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı kimlik doğrulamasını Düzenle](./media/uservoice-tutorial/ic777521.png "düzenleme kullanıcı kimlik doğrulaması")
   
    a. Tıklayın **çoklu oturum açma (SSO)**.
 
    b. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **SSO uzaktan oturum açma** metin.

    c. Yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri **SSO uzak oturum kapatma textbox**.
 
    d. Yapıştırma **parmak izi** Azure portaldan kopyaladığınız değeri **geçerli sertifikası SHA1 parmak izi** metin.
    
    e. Tıklayın **kimlik doğrulama ayarlarını kaydetme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/uservoice-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/uservoice-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/uservoice-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/uservoice-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-uservoice-test-user"></a>UserVoice test kullanıcısı oluşturma

Oturum açmak için UserVoice Azure AD kullanıcılarının etkinleştirmek için bunların UserVoice sağlanması gerekir. UserVoice söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:
1. Oturum açın, **UserVoice** Kiracı.

1. Git **ayarları**.
   
    ![Ayarları](./media/uservoice-tutorial/ic777811.png "ayarları")

1. Tıklayın **genel**.

1. Tıklayın **aracıları ve izinleri**.
   
    ![Aracıları ve izinleri](./media/uservoice-tutorial/ic777812.png "aracıları ve izinleri")

1. Tıklayın **yöneticileri ekleyin**.
   
    ![Yönetici eklemek](./media/uservoice-tutorial/ic777813.png "yöneticileri ekleyin")

1. Üzerinde **davet yöneticileri** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Yöneticiler davet](./media/uservoice-tutorial/ic777814.png "davet yöneticileri")
   
    a. E-postaları metin kutusunda sağlamak istediğiniz ve ardından hesabın e-posta adresini yazın **Ekle**.
   
    b. Tıklayın **davet**.

> [!NOTE]
> API'leri, AAD kullanıcı hesapları sağlamak için UserVoice tarafından sağlanan ya da tüm diğer UserVoice kullanıcı hesabı oluşturma araçları kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için UserVoice erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Uservoice'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **UserVoice**.

    ![Uygulamalar listesini UserVoice bağlantıdaki](./media/uservoice-tutorial/tutorial_uservoice_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde UserVoice kutucuğa tıkladığınızda, otomatik olarak UserVoice uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/uservoice-tutorial/tutorial_general_01.png
[2]: ./media/uservoice-tutorial/tutorial_general_02.png
[3]: ./media/uservoice-tutorial/tutorial_general_03.png
[4]: ./media/uservoice-tutorial/tutorial_general_04.png

[100]: ./media/uservoice-tutorial/tutorial_general_100.png

[200]: ./media/uservoice-tutorial/tutorial_general_200.png
[201]: ./media/uservoice-tutorial/tutorial_general_201.png
[202]: ./media/uservoice-tutorial/tutorial_general_202.png
[203]: ./media/uservoice-tutorial/tutorial_general_203.png

