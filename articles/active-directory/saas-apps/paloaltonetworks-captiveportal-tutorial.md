---
title: 'Öğretici: Palo Alto Networks - Captive portalı ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Palo Alto Networks - Captive portalı ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: fa47eaea590ecb84386a6e0ce4eff0a6933be554
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444210"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---captive-portal"></a>Öğretici: Palo Alto Networks - Captive portalı ile Azure Active Directory Tümleştirme

Bu öğreticide, Palo Alto Networks - Captive portalı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Palo Alto Networks tarafından sağlanan-Captive portalı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Palo Alto Networks - Captive Portal erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Palo Alto Networks - Captive portalıyla Azure AD hesaplarına (çoklu oturum açma) için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - Captive portalı ile Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Palo Alto Networks - Captive portalı çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Palo Alto Networks tarafından sağlanan - Captive portalından galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-palo-alto-networks---captive-portal-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - Captive portalından galeri ekleme
Palo Alto Networks - Azure AD'ye Captive portalı tümleştirmesini yapılandırmak için Galeri Captive portalından yönetilen SaaS uygulamaları listenize Palo Alto Networks - eklemeniz gerekir.

**Palo Alto Networks - galeri Captive portalından eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Palo Alto Networks - Captive portalı**seçin **Palo Alto Networks - Captive portalı** sonucu panelinden ardından **Ekle** uygulama eklemek için .

    ![Palo Alto Networks tarafından sağlanan-sonuç listesinde Captive portalı](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks - Captive "Britta Simon" adlı bir test kullanıcı tabanlı Portal ile test edin.

Tek iş için oturum açma için Azure AD ne Palo Alto Networks - Captive portalı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Palo Alto Networks - Captive Portal arasında bir bağlantı ilişki kurulması gerekir.

Palo Alto ağlarda - Captive portalı değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile-test etmek için Captive portalı, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Palo Alto Networks - Captive portalı test kullanıcısı oluşturma](#create-a-palo-alto-networks---captive-portal-test-user)**  - Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı Captive portalı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Palo Alto Networks - Captive Portalı Uygulama yapılandırın.

**Palo Alto Networks - Captive portalı ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Palo Alto Networks - Captive portalı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_samlbase.png)

1. Üzerinde **Palo Alto Networks - Captive portalı etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Palo Alto ağları - Captive portalı etki alanı ve URL'ler tek bilgi'oturum açma](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<Customer Firewall Hostname>/SAML20/SP`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<Customer Firewall Hostname>/SAML20/SP/ACS`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Palo Alto Networks - Captive Portal Destek ekibine](https://support.paloaltonetworks.com/support) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_400.png)

1. Palo Alto site başka bir tarayıcı penceresinde bir yönetici olarak açın.

1. Tıklayarak **cihaz**.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

1. Seçin **SAML kimlik sağlayıcısı** sol gezinti bölmesinden çubuk ve meta veri dosyası içeri aktarmak için "Al" seçeneğini tıklatın.

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

1. Aşağıdaki içeri aktarma penceresini Eylemler gerçekleştirin

    ![Palo Alto çoklu oturum açmayı yapılandırın](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. İçinde **profil adı** metin kutusuna bir ad ör. Azure AD Yöneticisi kullanıcı Arabirimi sağlar.
    
    b. İçinde **kimlik sağlayıcısı meta verileri**, tıklayın **Gözat** ve Azure portalından indirdiğiniz metadata.xml dosyası seçin
    
    c. **Tamam**’a tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/paloaltonetworks-captiveportal-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/paloaltonetworks-captiveportal-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/paloaltonetworks-captiveportal-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/paloaltonetworks-captiveportal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-palo-alto-networks---captive-portal-test-user"></a>Palo Alto Networks - Captive portalı test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Palo Alto Networks - Captive portalı adlı bir kullanıcı oluşturmaktır. Palo Alto Networks tarafından sağlanan - Captive portalı tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı Palo Alto Networks - Captive henüz mevcut değilse portalına erişme denemesi sırasında oluşturulur. 

> [!NOTE]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Palo Alto Networks - Captive Portal Destek ekibine](https://support.paloaltonetworks.com/support).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Palo Alto Networks - Captive Portal erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Palo Alto Networks - Captive portalı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Palo Alto Networks - Captive portalı**.

    ![Palo Alto ağları - uygulamalar listesinde Captive Portal bağlantısı](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltocaptiveportal_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Windows VM Güvenlik duvarının arkasında captive portalında yapılandırılır.  Captive portalında, RDP kullanarak Windows VM'de oturum açma çoklu oturum açmayı test etmek için. Gelen RDP oturumu içinde herhangi bir web sitesi için bir tarayıcı açın, otomatik olarak SSO URL'yi ve kimlik doğrulama istemi açmanız gerekir. Challenge tamamlandıktan sonra web sitelerine navgiate yapabileceksiniz. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_01.png
[2]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_02.png
[3]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_03.png
[4]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_04.png

[100]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_100.png

[200]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_200.png
[201]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_201.png
[202]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_202.png
[203]: ./media/paloaltonetworks-captiveportal-tutorial/tutorial_general_203.png

