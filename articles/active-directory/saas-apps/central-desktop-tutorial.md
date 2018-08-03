---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Merkezi Masaüstü | Microsoft Docs'
description: Azure Active Directory ve Merkezi Masaüstü arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 82a6911c85dd1438aa8f60cb36194a2916bc91e7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429055"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Öğretici: Azure Active Directory Tümleştirme ile Merkezi Masaüstü

Bu öğreticide, Azure Active Directory (Azure AD) ile Merkezi Masaüstü tümleştirme konusunda bilgi edinin.

Merkezi Masaüstü Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Merkezi Masaüstü erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Merkezi Masaüstü için kendi Azure AD hesapları ile oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Merkezi Masaüstü ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Merkezi Masaüstü tek oturum üzerinde etkin olmayan abonelik

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamında kullanımı önerilmemektedir.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Bir Azure AD deneme ortamı yoksa [bir aylık ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Merkezi Masaüstü galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-central-desktop-from-the-gallery"></a>Merkezi Masaüstü Galeriden Ekle
Azure AD'de Merkezi Masaüstü tümleştirmesini yapılandırmak için Merkezi Masaüstü Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Merkezi Masaüstü Galeriden eklemek için aşağıdaki adımları uygulayın:**

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Git **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Merkezi Masaüstü**. Seçin **Merkezi Masaüstü** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Merkezi Masaüstü](./media/central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Merkezi "Britta Simon." adlı bir test kullanıcı tabanlı masaüstü ile test etme

Tek iş için oturum açma için Azure AD Merkezi Masaüstü karşılığı kullanıcının Azure AD'de bir kullanıcı için olan bilmesi gerekir. Diğer bir deyişle, Merkezi Masaüstü bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı kurmak gerekir.

Merkezi Masaüstü vermek **kullanıcıadı** aynı değer olarak **kullanıcı adı** Azure AD'de. Şimdi iki kullanıcı arasındaki bağlantı kurduğunuz.

Yapılandırma ve Azure AD çoklu oturum açma Merkezi Masaüstü ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
1. [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. [Merkezi Masaüstü test kullanıcısı oluşturma](#create-a-central-desktop-test-user) kullanıcı Azure AD gösterimini bağlı olduğu Merkezi Masaüstü bir karşılığı Britta simon'un sağlamak için.
1. [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
1. [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Merkezi Masaüstü uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Merkezi Masaüstü ile yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **Merkezi Masaüstü** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Çoklu oturum açma, etkinleştirmek için **çoklu oturum açma** iletişim kutusundaki **modu** aşağı açılan listesinden **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma iletişim kutusu](./media/central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

1. İçinde **Merkezi Masaüstü etki alanı ve URL'ler** bölümünde, aşağıdaki adımları uygulayın:

    ![Oturum açma bilgileri tek bir merkezi Masaüstü etki alanı ve URL'ler](./media/central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. İçinde **oturum açma URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<companyname>.centraldesktop.com`

    b. İçinde **tanımlayıcı** kutusuna aşağıdaki desene sahip bir URL yazın:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. İçinde **yanıt URL'si** kutusuna aşağıdaki desene sahip bir URL yazın: `https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısıyla güncelleştirme, yanıt URL'si ve oturum açma URL'si. İlgili kişi [Merkezi Masaüstü istemci Destek ekibine](https://imeetcentral.com/contact-us) bu değerleri almak için. 

1. İçinde **SAML imzalama sertifikası** bölümünden **sertifika**. Ardından, bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

1. **Kaydet** düğmesini seçin.

    ![Çoklu oturum açma düğmesi kaydetme yapılandırın](./media/central-desktop-tutorial/tutorial_general_400.png)
    
1. İçinde **Merkezi Masaüstü yapılandırmasını** bölümünden **yapılandırma Merkezi Masaüstü** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru** bölümü.

    ![Merkezi masaüstü yapılandırması](./media/central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

1. Oturum açın, **Merkezi Masaüstü** Kiracı.

1. Git **ayarları**. Seçin **Gelişmiş**ve ardından **çoklu oturum açma**.

    ![Kurulum - Gelişmiş](./media/central-desktop-tutorial/ic769563.png "Kurulum - Gelişmiş")

1. Üzerinde **tek oturum açma ayarları** sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma ayarları](./media/central-desktop-tutorial/ic769564.png "tek oturum açma ayarları")
    
    a. Seçin **etkinleştir SAML v2 çoklu oturum açma**.
    
    b. İçinde **SSO URL** kutusu, yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.
    
    c. İçinde **SSO oturum açma URL'si** kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.
    
    d. İçinde **SSO oturum kapatma URL'si** kutusu, yapıştırma **oturum kapatma URL'si** Azure portaldan kopyaladığınız değeri.

1. İçinde **ileti imzası doğrulama yöntemi** bölümünde, aşağıdaki adımları uygulayın:

    ![İleti imzası doğrulama yöntemi](./media/central-desktop-tutorial/ic769565.png "ileti imzası doğrulama yöntemi") bir. **Sertifika**’yı seçin.
    
    b. İçinde **SSO sertifikası** listesinden **RSH SHA256**.
    
    c. İndirilen sertifikanızı Not Defteri'nde açın. Ardından, sertifika içeriğini kopyalayın ve yapıştırın **SSO sertifikası** alan.
        
    d. Seçin **SAMLv2 oturum açma sayfanız bir bağlantı görüntüler**.
    
    e. Seçin **güncelleştirme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com) uygulamasını ayarladığınız sırada. Bu uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden **çoklu oturum açma** sekmesini ve sonra katıştırılmış erişin belgelerin **yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, sol bölmede seçin **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/central-desktop-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**. Ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/central-desktop-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusunda **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/central-desktop-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları uygulayın:

    ![Kullanıcı iletişim kutusu](./media/central-desktop-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-central-desktop-test-user"></a>Merkezi Masaüstü test kullanıcısı oluşturma

Merkezi masaüstü uygulamasında Azure AD kullanıcılarının oturum açabilmesi bunlar sağlanmalıdır. Bu bölümde merkezi Desktop uygulamasında Azure AD kullanıcı hesapları oluşturma işlemini açıklar.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir merkezi Masaüstü kullanıcı hesabı oluşturma araçları veya Merkezi Masaüstü tarafından sağlanan API'leri kullanabilirsiniz.

**Merkezi Masaüstü kullanıcı hesapları sağlamak için:**

1. Merkezi Masaüstü kiracınızda oturum açın.

1. Git **kişiler** > **iç üyeleri**.

1. Seçin **iç üyeleri ekleme**.

    ![Kişiler](./media/central-desktop-tutorial/ic781051.png "kişiler")
    
1. İçinde **yeni üyeler e-posta adresi** sağlayın ve ardından istediğiniz bir Azure AD hesabı yazın **sonraki**.

    ![E-posta adreslerini yeni üyelerin](./media/central-desktop-tutorial/ic781052.png "e-posta adreslerini yeni üye")

1. Seçin **Ekle iç üyeleri**.

    ![İç Üye Ekle](./media/central-desktop-tutorial/ic781053.png "iç Üye Ekle")
   
   >[!NOTE]
   >Eklediğiniz kullanıcı hesaplarını etkinleştirmek için onay bağlantısını içeren bir e-posta alırsınız.
   
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı erişimi Merkezi Masaüstü vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon merkezi masaüstüne atamak için aşağıdaki adımları uygulayın:**

1. Azure portalında uygulama görünümünü açın. Dizin görünümüne gidin ve ardından Git **kurumsal uygulamalar**.

1. Seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Merkezi Masaüstü**.

    ![Uygulamalar listesinde Merkezi Masaüstü Bağlantısı](./media/central-desktop-tutorial/tutorial_centraldesktop_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Seçin **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama Ekle bölmesi][203]

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listesi.

1. İçinde **kullanıcılar ve gruplar** iletişim kutusu, tıklayın **seçin** düğmesi.

1. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı erişim panelini kullanarak test edin.

Erişim Paneli'nde Merkezi Masaüstü kutucuğu seçtiğinizde, otomatik olarak Merkezi Masaüstü uygulamanızın oturum açmış.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/central-desktop-tutorial/tutorial_general_203.png

