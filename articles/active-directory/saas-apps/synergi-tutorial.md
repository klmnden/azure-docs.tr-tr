---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Synergi | Microsoft Docs'
description: Azure Active Directory ve Synergi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 73c970e1-f1ba-420b-b225-414fdf93b140
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 416c5669424e7052459d94a6c487ea81aee5532d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156063"
---
# <a name="tutorial-azure-active-directory-integration-with-synergi"></a>Öğretici: Synergi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Synergi tümleştirme konusunda bilgi edinin.

Azure AD ile Synergi tümleştirme ile aşağıdaki avantajları sağlar:

- Synergi erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Synergi (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Synergi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik Synergi çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Synergi ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-synergi-from-the-gallery"></a>Galeriden Synergi ekleme
Azure AD'de Synergi tümleştirmesini yapılandırmak için Synergi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Synergi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Synergi**seçin **Synergi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Synergi](./media/synergi-tutorial/tutorial_synergi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Synergi sınayın.

Tek iş için oturum açma için Azure AD ne Synergi karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Synergi ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Synergi içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Synergi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Synergi test kullanıcısı oluşturma](#create-a-synergi-test-user)**  - kullanıcı Azure AD gösterimini bağlı Synergi Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Synergi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Synergi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Synergi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/synergi-tutorial/tutorial_synergi_samlbase.png)

1. Üzerinde **Synergi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Synergi etki alanı ve URL'ler tek oturum açma bilgileri](./media/synergi-tutorial/tutorial_synergi_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.irmsecurity.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.irmsecurity.com/sso/<organization id>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Synergi Destek ekibine](https://www.irmsecurity.com/contact/) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/synergi-tutorial/tutorial_synergi_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/synergi-tutorial/tutorial_general_400.png)

1. Üzerinde **Synergi yapılandırma** bölümünde **yapılandırma Synergi** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![Synergi yapılandırma](./media/synergi-tutorial/tutorial_synergi_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Synergi** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64), oturum kapatma URL'si ve SAML varlık kimliği** için [Synergi Destek ekibine](https://www.irmsecurity.com/contact/).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/synergi-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/synergi-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/synergi-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/synergi-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-synergi-test-user"></a>Synergi test kullanıcısı oluşturma

Bu bölümde, Britta Simon Synergi içinde adlı bir kullanıcı oluşturun. Çalışmak [Synergi Destek ekibine](https://www.irmsecurity.com/contact/) Synergi platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Synergi erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Synergi için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Synergi**.

    ![Uygulamalar listesinde Synergi bağlantı](./media/synergi-tutorial/tutorial_synergi_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Synergi kutucuğa tıkladığınızda, otomatik olarak Synergi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/synergi-tutorial/tutorial_general_01.png
[2]: ./media/synergi-tutorial/tutorial_general_02.png
[3]: ./media/synergi-tutorial/tutorial_general_03.png
[4]: ./media/synergi-tutorial/tutorial_general_04.png

[100]: ./media/synergi-tutorial/tutorial_general_100.png

[200]: ./media/synergi-tutorial/tutorial_general_200.png
[201]: ./media/synergi-tutorial/tutorial_general_201.png
[202]: ./media/synergi-tutorial/tutorial_general_202.png
[203]: ./media/synergi-tutorial/tutorial_general_203.png

