---
title: 'Öğretici: New Relic ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: New Relic ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: 6aade578aef40b6f009ed42b4798bc9cefce5793
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54828341"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Öğretici: New Relic ile Azure Active Directory Tümleştirme

Bu öğreticide, New Relic, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

New Relic, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- New Relic erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için New Relic açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

New Relic ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- New Relic çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. New Relic galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-new-relic-from-the-gallery"></a>New Relic galeri ekleme
New Relic tümleştirmesi Azure AD'de yapılandırmak için New Relic Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**New Relic Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **New Relic**seçin **New Relic** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde yeni Relic](./media/new-relic-tutorial/tutorial_new-relic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı New Relic ile test edin.

Tek iş için oturum açma için Azure AD ne New Relic karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve New Relic ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

New Relic, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma New Relic ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[New Relic test kullanıcısı oluşturma](#create-a-new-relic-test-user)**  - kullanıcı Azure AD gösterimini bağlı New Relic Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve New Relic uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma New Relic ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **New Relic** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/new-relic-tutorial/tutorial_new-relic_samlbase.png)

1. Üzerinde **yeni Relic etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri yeni Relic etki alanı ve URL'ler](./media/new-relic-tutorial/tutorial_new-relic_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` -kendi yeni Relic hesabı kimliği değiştirdiğinizden emin olun

    b. İçinde **tanımlayıcı** metin değeri yazın: `rpm.newrelic.com`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/new-relic-tutorial/tutorial_new-relic_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/new-relic-tutorial/tutorial_general_400.png)

1. Üzerinde **yeni Relic yapılandırma** bölümünde **yapılandırma New Relic** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Yeni Relic yapılandırma](./media/new-relic-tutorial/tutorial_new-relic_configure.png) 

1. Farklı bir web tarayıcı penceresinde oturum açın, **New Relic** şirketinizin sitesi yöneticisi olarak.

1. Üstteki menüden **hesap ayarları**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797036.png "hesap ayarları")

1. Tıklayın **güvenlik ve kimlik doğrulama** sekmesine ve ardından **tekli oturum** sekmesi.
   
    ![Çoklu oturum açma](./media/new-relic-tutorial/ic797037.png "çoklu oturum açma")

1. SAML iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
   a. Tıklayın **Dosya Seç** indirilen Azure Active Directory sertifikanızı karşıya yüklemek için.

   b. İçinde **uzaktan oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
   
   c. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

   d. Tıklayın **Değişikliklerimi Kaydet**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/new-relic-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/new-relic-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/new-relic-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/new-relic-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-new-relic-test-user"></a>New Relic test kullanıcısı oluşturma

Azure Active Directory Kullanıcıları için New Relic oturum açmayı etkinleştirmek için bunlar New Relic ile sağlanması gerekir. New Relic söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı için New Relic sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **New Relic** şirketinizin sitesi yöneticisi olarak.

1. Üstteki menüden **hesap ayarları**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797040.png "hesap ayarları")

1. İçinde **hesabı** bölmesi sol tarafta **özeti**ve ardından **Kullanıcı Ekle**.
   
    ![Hesap ayarları](./media/new-relic-tutorial/ic797041.png "hesap ayarları")

1. Üzerinde **etkin kullanıcılar** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Etkin kullanıcılar](./media/new-relic-tutorial/ic797042.png "etkin kullanıcılar")
   
    a. İçinde **e-posta** metin sağlamak istediğiniz geçerli bir Azure Active Directory kullanıcı e-posta adresini yazın.

    b. Olarak **rol** seçin **kullanıcı**.

    c. Tıklayın **bu kullanıcıyı eklemek**.

>[!NOTE]
>API'leri, AAD kullanıcı hesapları sağlamak için New Relic tarafından sağlanan ya da tüm diğer New Relic kullanıcı hesabı oluşturma araçları kullanabilirsiniz.
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açmayı kullanmak için New Relic erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon için New Relic atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **New Relic**.

    ![New Relic bağlantıya uygulamalar listesi](./media/new-relic-tutorial/tutorial_new-relic_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde New Relic kutucuğa tıkladığınızda, otomatik olarak New Relic uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/new-relic-tutorial/tutorial_general_01.png
[2]: ./media/new-relic-tutorial/tutorial_general_02.png
[3]: ./media/new-relic-tutorial/tutorial_general_03.png
[4]: ./media/new-relic-tutorial/tutorial_general_04.png

[100]: ./media/new-relic-tutorial/tutorial_general_100.png

[200]: ./media/new-relic-tutorial/tutorial_general_200.png
[201]: ./media/new-relic-tutorial/tutorial_general_201.png
[202]: ./media/new-relic-tutorial/tutorial_general_202.png
[203]: ./media/new-relic-tutorial/tutorial_general_203.png

