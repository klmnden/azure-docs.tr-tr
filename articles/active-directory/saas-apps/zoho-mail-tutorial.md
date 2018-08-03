---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Zoho | Microsoft Docs'
description: Azure Active Directory ve Zoho arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 03950d983f6ed119ae6cf7a7391418804bb20c76
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446288"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Öğretici: Azure Active Directory Zoho ile tümleştirme

Bu öğreticide, Zoho, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Azure AD ile Zoho tümleştirme ile aşağıdaki avantajları sağlar:

- Zoho erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Zoho (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Zoho yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Zoho çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Zoho ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-zoho-from-the-gallery"></a>Galeriden Zoho ekleme
Azure AD'de Zoho tümleştirmesini yapılandırmak için Zoho Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Zoho eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Zoho**seçin **Zoho** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Zoho](./media/zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Zoho sınayın.

Tek çalışmak için oturum açma için Azure AD ne Zoho karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Zoho ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Zoho, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Zoho ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Zoho test kullanıcısı oluşturma](#create-a-zoho-test-user)**  - kullanıcı Azure AD gösterimini bağlı Zoho Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Zoho uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Zoho yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Zoho** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zoho-mail-tutorial/tutorial_zoho_samlbase.png)

1. Üzerinde **Zoho etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Zoho etki alanı ve URL'ler tek oturum açma bilgileri](./media/zoho-mail-tutorial/tutorial_zoho_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.zohomail.com`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Zoho istemci Destek ekibine](https://www.zoho.com/mail/contact.html) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/zoho-mail-tutorial/tutorial_zoho_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/zoho-mail-tutorial/tutorial_general_400.png)

1. Üzerinde **Zoho yapılandırma** bölümünde **yapılandırma Zoho** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, parola URL'yi Değiştir ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Zoho yapılandırma](./media/zoho-mail-tutorial/tutorial_zoho_configure.png) 

1. Farklı bir web tarayıcı penceresinde Zoho posta şirket sitenize yönetici olarak oturum.

1. Git **Denetim Masası**.
   
    ![Denetim Masası](./media/zoho-mail-tutorial/ic789607.png "Denetim Masası")

1. Tıklayın **SAML kimlik doğrulaması** sekmesi.
   
    ![SAML kimlik doğrulaması](./media/zoho-mail-tutorial/ic789608.png "SAML kimlik doğrulaması")

1. İçinde **SAML kimlik doğrulama ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kimlik doğrulama ayrıntıları](./media/zoho-mail-tutorial/ic789609.png "SAML kimlik doğrulaması ayrıntıları")
   
    a. İçinde **oturum açma URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    b. İçinde **oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
    c. İçinde **parola URL'yi Değiştir** metin kutusu, yapıştırma **parola URL'yi Değiştir** , Azure Portalı'ndan kopyaladığınız.
       
    d. Not Defteri'nde Azure portalından indirilen, base-64 kodlanmış sertifika açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **PublicKey** metin.
   
    e. Olarak **algoritması**seçin **RSA**.
   
    f. **Tamam** düğmesine tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zoho-mail-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/zoho-mail-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zoho-mail-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zoho-mail-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-zoho-test-user"></a>Zoho test kullanıcısı oluşturma

Azure AD kullanıcılarının Zoho posta oturum etkinleştirmek için bunlar Zoho posta sağlanması gerekir. Zoho posta söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

> [!NOTE]
> Herhangi diğer Zoho posta kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Zoho e-posta yoluyla sağlanan.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum açın, **Zoho posta** yönetici olarak şirketin site.

1. Git **Denetim Masası \> posta ve belgeler**.

1. Git **kullanıcı ayrıntıları \> kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/zoho-mail-tutorial/ic789611.png "kullanıcı ekleme")

1. Üzerinde **kullanıcı ekleme** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı ekleme](./media/zoho-mail-tutorial/ic789612.png "kullanıcı ekleme")
   
    a. İçinde **ad** metin kutusu, kullanıcının ilk adını yazın ister **Britta**.

    b. İçinde **Soyadı** metin kullanıcı adının türünü ister **Simon**.

    c. İçinde **e-posta kimliği** metin kutusuna kullanıcı e-posta kimliği türünü ister **brittasimon@contoso.com**.

    d. İçinde **parola** metin kutusu, kullanıcının parolasını girin.
   
    e. **Tamam** düğmesine tıklayın.  
      
    > [!NOTE]
    > Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alırsınız.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Zoho için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Zoho için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Zoho**.

    ![Uygulamalar listesini Zoho bağlantıdaki](./media/zoho-mail-tutorial/tutorial_zoho_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zoho kutucuğa tıkladığınızda, otomatik olarak Zoho uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/zoho-mail-tutorial/tutorial_general_203.png

