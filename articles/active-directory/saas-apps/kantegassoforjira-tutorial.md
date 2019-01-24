---
title: 'Öğretici: JIRA için Kantega SSO ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Kantega SSO için JIRA arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1c9e372ed46b9cabb51ade06862f01c11b3db3f1
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816730"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>Öğretici: JIRA için Kantega SSO ile Azure Active Directory Tümleştirme

Bu öğreticide, Kantega SSO JIRA için Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

JIRA için Kantega SSO, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- JIRA Kantega SSO erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan için Kantega SSO (çoklu oturum açma) JIRA için açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi için JIRA Kantega SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir JIRA çoklu oturum açma için Kantega SSO etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden JIRA için Kantega SSO ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>Galeriden JIRA için Kantega SSO ekleme
Azure AD'de Kantega SSO için JIRA tümleştirmesini yapılandırmak için Kantega SSO için JIRA Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Kantega SSO için JIRA eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Kantega SSO için JIRA**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_search.png)

1. Sonuçlar panelinde seçin **Kantega SSO için JIRA**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma için "Britta Simon" adlı bir test kullanıcı tabanlı JIRA Kantega SSO ile test edin.

Tek iş için oturum açma için Azure AD ne Kantega SSO JIRA için karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Kantega SSO JIRA için ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

JIRA Kantega SSO, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma için JIRA Kantega SSO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[JIRA test kullanıcısı için bir Kantega SSO oluşturma](#creating-a-kantega-sso-for-jira-test-user)**  - Kantega SSO için kullanıcı Azure AD gösterimini bağlı JIRA Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve, Kantega SSO JIRA uygulama için çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma için JIRA Kantega SSO ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Kantega SSO JIRA için** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_samlbase.png)

1. İçinde **IDP** modunda başlatıldı **JIRA etki alanı ve URL'ler için Kantega SSO** bölümü aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

1. İçinde **SP** başlatılan modu, onay **Gelişmiş URL ayarlarını göster** ve aşağıdaki adımı uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Bu değerler, öğreticinin ilerleyen bölümlerinde açıklanan Jıra eklentisi, yapılandırma sırasında alınır.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/tutorial_general_400.png)
    
1. Farklı bir web tarayıcı penceresinde bir JIRA şirket içi sunucunuza yönetici olarak oturum açın.

1. Dişlisine gelin ve tıklayın **eklentileri**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon1.png)

1. Eklentiler sekmesi bölümü altında **yeni eklentileri bulma**. Arama **Kantega SSO JIRA (SAML & Kerberos) için** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon2.png)

1. Eklenti yükleme başlar.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon3.png)

1. Yükleme tamamlandıktan sonra. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon33.png)

1.  **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon34.png)
    
1. Yeni eklenti altında listelenen **TÜMLEŞTİRMELER**. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon35.png)

1. İçinde **SAML** bölümü. Seçin **Azure Active Directory (Azure AD)** gelen **Ekle kimlik sağlayıcısı** açılır.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon4.png)

1. Abonelik düzeyi olarak **temel**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon5.png)       

1. Üzerinde **uygulama özellikleri** bölümünde, aşağıdaki adımları uygulayın: 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon6.png)

    a. Kopyalama **uygulama kimliği URI'si** olarak kullanın ve değerini **tanımlayıcısı, yanıt URL'si ve oturum açma URL'si** üzerinde **JIRA etki alanı ve URL'ler için Kantega SSO** bölümü Azure Portalı'nda.

    b. **İleri**’ye tıklayın.

1. Üzerinde **meta veri içeri aktarma** bölümünde, aşağıdaki adımları uygulayın: 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon7.png)

    a. Seçin **meta veri dosyası bilgisayarımda**ve Azure portalından indirdiğiniz meta veri dosyası karşıya yükleme.

    b. **İleri**’ye tıklayın.

1. Üzerinde **adı ve SSO konumunu** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon8.png)
    
    a. Kimlik sağlayıcısı adını eklemek **kimlik sağlayıcı adı** metin (ör. Azure AD).

    b. **İleri**’ye tıklayın.

1. İmzalama sertifikası doğrulayın ve tıklayın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon9.png)

1. Üzerinde **JIRA kullanıcı hesaplarını** bölümünde, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon10.png)

    a. Seçin **kullanıcılar JIRA'ın iç dizinde gerekirse oluşturun** ve kullanıcılar için uygun Grup adını girin (olabilir birden çok yok. virgülle ayırarak grupları).

    b. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.    

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon11.png)

1. Üzerinde **etki alanları için Azure AD bilinen** bölümünde, aşağıdaki adımları uygulayın: 

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/addon12.png)

    a. Seçin **etki alanları** sayfasının sol panelden.

    b. Etki alanı adını girin **etki alanları** metin.

    c. **Kaydet**’e tıklayın. 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforjira-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforjira-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforjira-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/kantegassoforjira-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-kantega-sso-for-jira-test-user"></a>Kantega SSO için JIRA test kullanıcısı oluşturma

JIRA'ya oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların JIRA sağlanması gerekir. JIRA Kantega SSO, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuza yönetici olarak oturum açın.

1. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user1.png) 

1. Altında **kullanıcı yönetimi** sekmesinde bölüm **kullanıcı oluşturma**.

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user2.png) 

1. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user3.png) 

    a. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    e. Tıklayın **kullanıcı oluşturma**.   

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için JIRA Kantega SSO için erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Kantega SSO için JIRA atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Kantega SSO için JIRA**.

    ![Çoklu oturum açmayı yapılandırın](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Kantega SSO erişim panelinde JIRA kutucuğa tıkladığınızda, otomatik olarak, Kantega SSO için JIRA uygulamayı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/kantegassoforjira-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforjira-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforjira-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforjira-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforjira-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforjira-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforjira-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforjira-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforjira-tutorial/tutorial_general_203.png

