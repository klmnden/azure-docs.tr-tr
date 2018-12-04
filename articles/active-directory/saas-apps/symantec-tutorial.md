---
title: 'Öğretici: Azure Active Directory Tümleştirme Symantec Web güvenlik hizmetini (WSS) | Microsoft Docs'
description: Azure Active Directory ve Symantec Web güvenlik hizmetini (WSS) arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b933bc5f5ecb39c3462e4e9bd300f1e07fd718c0
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838784"
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Öğretici: Azure Active Directory Tümleştirme Symantec Web güvenlik hizmetini (WSS)

Bu öğreticide, böylece WSS SAML kimlik doğrulaması kullanarak Azure AD'de sağlanan bir son kullanıcı kimlik doğrulaması ve kullanıcı Symantec Web güvenlik hizmetini (WSS) hesabınızın Azure Active Directory (Azure AD) hesabınızla tümleştirmek hakkında bilgi edineceksiniz veya grubu düzeyi ilkesi kuralları.

Symantec Web güvenlik hizmetini (WSS) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tüm son kullanıcılar ve grupları Azure AD portalından WSS hesabınız tarafından kullanılan yönetin. 

- Son kullanıcıların, Azure AD kimlik bilgilerini kullanarak WSS içindeki doğrulatmak izin verin.

- Kullanıcının zorlamayı etkinleştir ve WSS hesabınızdaki tanımlanan düzeyi ilke kuralları gruplayın.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Symantec Web güvenlik hizmetini (WSS) ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Symantec Web güvenlik hizmetini (WSS) hesabı

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim amaçlı kullanılmakta olan WSS hesabıyla önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, bu test üretim amacıyla kullanılmakta olan WSS hesabınızı kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma için Azure AD hesabınızın içinde tanımlanan son kullanıcı kimlik bilgilerini kullanarak WSS etkinleştirmek için yapılandırır.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Symantec Web güvenlik hizmetini (WSS) uygulama ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Galeriden Symantec Web güvenlik hizmetini (WSS) ekleme
Azure AD'de Symantec Web güvenlik hizmetini (WSS) tümleştirmesini yapılandırmak için Symantec Web güvenlik hizmetini (WSS) Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Symantec Web güvenlik hizmetini (WSS) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Symantec Web güvenlik hizmetini (WSS)** seçin **Symantec Web güvenlik hizmetini (WSS)** sonucu panelinden ardından **Ekle** düğme eklemek için uygulama.

    ![Sonuç listesinde Symantec Web güvenlik hizmetini (WSS)](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Symantec Web güvenlik hizmeti ("Britta Simon" adlı bir test kullanıcı tabanlı WSS) test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Symantec Web güvenlik hizmetini (WSS) bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Symantec Web güvenlik hizmetini (WSS) arasında bir bağlantı ilişki kurulması gerekir.

Symantec Web güvenlik hizmetini (WSS) içindeki değeri atamak **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Symantec Web güvenlik hizmetini (WSS) test kullanıcısı oluşturma](#create-a-symantec-web-security-service-wss-test-user)**  - Symantec Web güvenlik hizmetini (WSS), kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Symantec Web güvenlik hizmetini (WSS) uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Symantec Web güvenlik hizmetini (WSS)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

1. Üzerinde **Symantec Web güvenlik hizmetini (WSS) etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Symantec Web güvenlik hizmetini (WSS) etki alanı ve URL'ler tek oturum açma bilgileri](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://saml.threatpulse.net:8443/saml/saml_realm`

    b. İçinde **yanıt URL'si** metin kutusuna URL'yi yazın: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Temasa [Symantec Web güvenlik hizmetini (WSS) istemci Destek ekibine](https://www.symantec.com/contact-us) varsa değerlerini **tanımlayıcı** ve **yanıt URL'si** herhangi bir nedenle çalışmıyor.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/symantec-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açma Symantec Web güvenlik hizmetini (WSS) tarafında yapılandırma WSS çevrimiçi belgelerine bakın. İndirilen **meta veri XML** dosya WSS Portalı'na aktarılması gerekir. İlgili kişi [Symantec Web güvenlik hizmetini (WSS) Destek ekibine](https://www.symantec.com/contact-us) WSS portalındaki yapılandırmayla ilgili yardıma ihtiyacınız varsa.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/symantec-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/symantec-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/symantec-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/symantec-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Symantec Web güvenlik hizmetini (WSS) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Symantec Web güvenlik hizmetini (WSS) adlı bir kullanıcı oluşturun. Karşılık gelen uç kullanıcı adı WSS Portalı'nda el ile oluşturulabilir veya birkaç dakika sonra (yaklaşık 15 dakika) WSS portalına eşitlenecek Azure AD'de sağlanan kullanıcılar/gruplar bekleyebilirsiniz. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. Web siteleri göz atmak için kullanılan son kullanıcı makinenin genel IP adresini de Symantec Web güvenlik hizmetini (WSS) portalında sağlanması gerekir.

> [!NOTE]
> Lütfen [Buraya](https://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) makinenizin genel IP adresi günceller.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Symantec Web güvenlik hizmetini (WSS) erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Symantec Web güvenlik hizmetini (WSS) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Symantec Web güvenlik hizmetini (WSS)**.

    ![Uygulamalar listesinde Symantec Web güvenlik hizmetini (WSS) bağlantısı](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

WSS hesabınızı Azure AD'nize SAML kimlik doğrulaması için kullanmak üzere yapılandırdığınıza göre bu bölümde, çoklu oturum açma işlevlerini sınayacaksınız.

Ardından, web tarayıcınızı açın ve bir siteye göz atmayı deneyin WSS, proxy trafiğini web tarayıcınızda yapılandırdıktan sonra Azure oturum açma sayfasına yönlendirilirsiniz. Azure AD'de (diğer bir deyişle, BrittaSimon) sağlanan bir test son kullanıcı kimlik bilgilerini girin ve ilişkili parola. Kimlik doğrulandıktan sonra seçtiğiniz bir Web sitesine gözatmak mümkün olacaktır. WSS tarafında BrittaSimon kullanıcı olarak o siteye göz atmak denediğinizde WSS blok sayfası görmeniz gerekir sonra belirli bir siteye göz atma BrittaSimon engellemek için bir ilke kuralı oluşturmanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/symantec-tutorial/tutorial_general_01.png
[2]: ./media/symantec-tutorial/tutorial_general_02.png
[3]: ./media/symantec-tutorial/tutorial_general_03.png
[4]: ./media/symantec-tutorial/tutorial_general_04.png

[100]: ./media/symantec-tutorial/tutorial_general_100.png

[200]: ./media/symantec-tutorial/tutorial_general_200.png
[201]: ./media/symantec-tutorial/tutorial_general_201.png
[202]: ./media/symantec-tutorial/tutorial_general_202.png
[203]: ./media/symantec-tutorial/tutorial_general_203.png

