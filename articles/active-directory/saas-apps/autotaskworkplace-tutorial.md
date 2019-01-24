---
title: 'Öğretici: Autotask çalışma alanına Azure Active Directory tümleştirmesiyle | Microsoft Docs'
description: Azure Active Directory ve Autotask iş yeri arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 8768e74455983c9cc405687ed8dbf7f3894cf9e5
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809505"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Öğretici: Autotask çalışma alanı ile Azure Active Directory Tümleştirme

Bu öğreticide, Autotask çalışma alanına Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Autotask çalışma alanına Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Autotask çalışma alanına erişimi olan Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Autotask çalışma alanına (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Autotask iş yeri yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Autotask çalışma alanına çoklu oturum açma etkin aboneliği
- Bir yönetici veya süper Yönetici çalışma alanında olması gerekir.
- Azure AD'de bir yönetici hesabınızın olması gerekir.
- Bu özellik yararlanacaktır kullanıcıların çalışma alanı ve Azure AD içindeki hesaplar olmalıdır ve e-posta adreslerini hem de eşleşmesi gerekir.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Autotask çalışma alanına ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-autotask-workplace-from-the-gallery"></a>Galeriden Autotask çalışma alanına ekleme
Azure AD'ye Autotask çalışma alanına tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Autotask çalışma alanı eklemeniz gerekir.

**Galeriden Autotask çalışma alanına eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Autotask çalışma alanına**seçin **Autotask çalışma alanına** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Autotask çalışma alanı](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Autotask çalışma alanı ile Azure AD çoklu oturum açmayı test "Britta Simon." adlı bir test kullanıcı tabanlı

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Autotask çalışma alanında bir kullanıcı için Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Autotask çalışma arasında bir bağlantı ilişki kurulması gerekir.

Değerini Autotask çalışma alanında, atamak **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Autotask çalışma alanı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Autotask çalışma alanına bir test kullanıcısı oluşturma](#create-an-autotask-workplace-test-user)**  - kullanıcı Azure AD gösterimini bağlı Autotask çalışma alanında Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Autotask çalışma alanına uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma ile Autotask Workplace yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Autotask çalışma alanına** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

1. Uygulamada yapılandırmak istiyorsanız **IDP** başlatılan modu, aşağıdaki adımları gerçekleştirin **Autotask çalışma alanına etki alanı ve URL'ler** bölümü:

    ![Etki alanı ve URL'ler tek oturum açma bilgilerini IDP Autotask çalışma](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

1. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu, onay **Gelişmiş URL ayarlarını göster** ve aşağıdaki adımları gerçekleştirin:

    ![Etki alanı ve URL'ler tek oturum açma bilgilerini SP Autotask çalışma](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Autotask çalışma alanına istemci Destek ekibine](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/autotaskworkplace-tutorial/tutorial_general_400.png)

1. Farklı bir web tarayıcı penceresinde çalışma alanı yönetici kimlik bilgilerini kullanarak Online'da oturum açın.

    >[!Note]
    >Idp'nin yapılandırırken, bir alt etki alanı belirtilmesi gerekir. Doğru alt etki alanı, çalışma alanına çevrimiçi oturum açma onaylamak için. Oturum açtıktan sonra URL'deki alt etki alanı için not edin.
    >Alt etki alanı, "https://" ile ".awp.autotask.net/" arasındaki bir parçasıdır ve ABD, AB, ca veya au olmalıdır.

1. Git **yapılandırma** > **çoklu oturum açma** ve aşağıdaki adımları gerçekleştirin:

    ![Autotask tek oturum açma yapılandırması](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Seçin **XML meta veri dosyası** seçeneğini belirtin ve ardından karşıya **meta veri XML** Azure portalından indirdiğiniz.

    b. Tıklayın **SSO etkinleştirme**.
    
    ![Autotask çoklu oturum açma yapılandırması Onayla](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Seçin **bu bilgilerin doğru olduğundan ve bu IDP güvendiğim onaylıyorum** onay kutusu.

    d. Tıklayın **onaylama**.
     
>[!Note]
>Lütfen Autotask çalışma alanı yapılandırma ile ilgili Yardım gerekiyorsa bkz [bu sayfayı](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) iş yeri hesabınızı ile ilgili Yardım almak için.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/autotaskworkplace-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/autotaskworkplace-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/autotaskworkplace-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/autotaskworkplace-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-autotask-workplace-test-user"></a>Autotask çalışma alanına bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon Autotask içinde adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Autotask çalışma alanına Destek ekibine](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) Autotask çalışma alanına platform kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Autotask çalışma alanına erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Autotask çalışma alanına atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Autotask çalışma alanına**.

    ![Uygulamalar listesinde Autotask çalışma alanına bağlantısı](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Autotask çalışma alanına kutucuğa tıkladığınızda, size otomatik olarak Autotask çalışma alanına uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/autotaskworkplace-tutorial/tutorial_general_203.png

