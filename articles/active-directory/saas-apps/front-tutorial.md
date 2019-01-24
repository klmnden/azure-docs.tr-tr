---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile ön | Microsoft Docs'
description: Azure Active Directory ve ön arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: 72bbd6b39e6a2f59b718071f11882c37ef571cae
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826386"
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a>Öğretici: Ön ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ön tümleştirme konusunda bilgi edinin.

Azure AD ile ön tümleştirme ile aşağıdaki avantajları sağlar:

- Ön erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan öne (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ön yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir ön çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ön ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-front-from-the-gallery"></a>Galeriden ön ekleme
Azure AD'de öne tümleştirmesini yapılandırmak için ön Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ön eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **ön**seçin **ön** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ön](./media/front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ön test.

Tek çalışmak için oturum açma için Azure AD ne önde gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı önde arasında bir bağlantı ilişki kurulması gerekir.

Değerini önde, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ön ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Ön test kullanıcısı oluşturma](#create-a-front-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı bir karşılığı Britta simon'un önde sağlamak için.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ön uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ön yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ön** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/front-tutorial/tutorial_front_samlbase.png)

1. Üzerinde **ön etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/front-tutorial/tutorial_front_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.frontapp.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.frontapp.com/sso/saml/callback`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcı ve yanıt URL'si öğretici veya kişi daha sonra açıklanacak güncelleştirme [ön istemci Destek ekibine](mailto:support@frontapp.com) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/front-tutorial/tutorial_front_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/front-tutorial/tutorial_general_400.png)
    
1. Üzerinde **ön yapılandırma** bölümünde **yapılandırma ön** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/front-tutorial/tutorial_front_configure.png) 

1. Ön kiracınıza yönetici olarak oturum.

1. Git **ayarları (sol kenar alt kısmındaki dişli simgesi) > Tercihler**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/front-tutorial/tutorial_front_000.png)

1. Tıklayın **çoklu oturum açma** bağlantı.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/front-tutorial/tutorial_front_001.png)

1. Seçin **SAML** aşağı açılan listesinde **çoklu oturum açma**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/front-tutorial/tutorial_front_002.png)

1. İçinde **giriş noktası** metin değerini put **çoklu oturum açma hizmeti URL'si** Azure AD Uygulama Yapılandırma Sihirbazı'ndan.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/front-tutorial/tutorial_front_003.png)

1. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **imzalama sertifikası** metin.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/front-tutorial/tutorial_front_004.png)

1. Üzerinde **hizmet sağlayıcısı ayarları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/front-tutorial/tutorial_front_005.png)

    a. Değerini kopyalayın **varlık kimliği** yapıştırın **tanımlayıcı** metin kutusunda **ön etki alanı ve URL'ler** bölümü Azure Portalı'nda.

    b. Değerini kopyalayın **ACS URL** yapıştırın **yanıt URL'si** metin kutusunda **ön etki alanı ve URL'ler** bölümü Azure Portalı'nda.
    
1. Tıklayın **Kaydet** düğmesi.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/front-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/front-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/front-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/front-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-front-test-user"></a>Ön test kullanıcısı oluşturma

Bu bölümde, Britta Simon önde adlı bir kullanıcı oluşturun. Çalışmak [ön istemci Destek ekibine](mailto:support@frontapp.com) ön platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde öne erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon öne atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **ön**.

    ![Uygulamalar listesini ön bağlantıya](./media/front-tutorial/tutorial_front_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, Azure AD erişim panelini kullanarak SSOconfiguration test etmektir.

Erişim panelinde ön kutucuğa tıkladığınızda, otomatik olarak ön uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/front-tutorial/tutorial_general_01.png
[2]: ./media/front-tutorial/tutorial_general_02.png
[3]: ./media/front-tutorial/tutorial_general_03.png
[4]: ./media/front-tutorial/tutorial_general_04.png

[100]: ./media/front-tutorial/tutorial_general_100.png

[200]: ./media/front-tutorial/tutorial_general_200.png
[201]: ./media/front-tutorial/tutorial_general_201.png
[202]: ./media/front-tutorial/tutorial_general_202.png
[203]: ./media/front-tutorial/tutorial_general_203.png

