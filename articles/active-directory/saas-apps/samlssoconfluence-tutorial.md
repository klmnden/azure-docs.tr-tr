---
title: 'Öğretici: Azure Active Directory ile tümleştirme için Confluence SAML SSO GmbH çözünürlüğün | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Confluence için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: c1a1126026f3d2618a0669e4bd69a84cc1c6c54c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431632"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Öğretici: Azure Active Directory ile tümleştirme için Confluence SAML SSO GmbH çözünürlüğün

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Confluence için SAML SSO tümleştirme konusunda bilgi edinin.

SAML SSO için Confluence çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAML SSO için Confluence GmbH çözünürlüğün erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanmış için Confluence için SAML SSO (çoklu oturum açma) GmbH çözünürlüğün Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Confluence için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- SAML SSO etkin abonelikte GmbH çoklu oturum çözünürlüğün Confluence için

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAML SSO Confluence için GmbH çözünürlüğüyle galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-the-gallery"></a>SAML SSO Confluence için GmbH çözünürlüğüyle galeri ekleme

Confluence için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için SAML SSO için Confluence GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAML SSO için Confluence GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **GmbH çözünürlüğün Confluence için SAML SSO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

1. Sonuçlar panelinde seçin **GmbH çözünürlüğün Confluence için SAML SSO**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Confluence ile GmbH "Britta Simon." adlı bir test kullanıcı tabanlı bir çözüm olarak test etme

Tek çalışmak için oturum açma için Azure AD hangi karşılık gelen kullanıcı SAML SSO için Confluence çözünürlüğün GmbH, Azure AD'de bir kullanıcı için olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve SAML SSO Confluence için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

SAML SSO GmbH çözünürlüğün Confluence, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Confluence ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Confluence oluşturma](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Confluence için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAML SSO için Confluence GmbH uygulama çözünürlüğüne göre yapılandırın.

**Azure AD çoklu oturum açma SAML SSO için Confluence ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **GmbH çözünürlüğün Confluence için SAML SSO** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

1. Üzerinde **Confluence çözünürlüğün GmbH etki alanı ve URL'ler için SAML SSO** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO Confluence çözünürlüğün GmbH istemci için Destek ekibine](https://www.resolution.de/go/support) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_general_400.png)    
    
1. Farklı bir web tarayıcı penceresinde oturum açın, **SAML SSO için çözüm GmbH Yönetici portalı tarafından Confluence** yönetici olarak.

1. Dişlisine gelin ve tıklayın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon1.png)

1. Yönetici erişimi sayfasına yönlendirilirsiniz. Parolayı girin ve tıklatın **Onayla** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon2.png)

1. Altında **ATLASSIAN Market** sekmesinde **yeni eklentileri bulma**. 

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon.png)

1. Arama **SAML çoklu oturum açma (SSO) için Confluence** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon7.png)

1. Eklenti yüklemesi başlatılır. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon8.png)

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon9.png)

1.  **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon10.png)
    
1. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon11.png)

1. Bu yeni eklenti ayrıca altında bulunabilir **kullanıcılar ve güvenlik** sekmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon3.png)
    
1. Üzerinde **SAML SingleSignOn eklentisi yapılandırma** sayfasında **yeni IDP ekleme** kimlik sağlayıcısı ayarlarını yapılandırmak için düğmeye.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon4.png)

1. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5a.png)
 
    a. Ayarlama **Azure AD'ye** IDP türü.
    
    b. Ekleme **adı** kimlik sağlayıcısının (ör. Azure AD).
    
    c. Ekleme **açıklama** kimlik sağlayıcısının (ör. Azure AD).
    
    d. **İleri**’ye tıklayın.
    
1. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5b.png)

1. Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5c.png)

    a. Tıklayın **yük dosyası** düğmesi ve 5. adımda indirdiğiniz meta veri XML dosyasını seçin.

    b. Tıklayın **alma** düğmesi.
    
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.
    
    d. Tıklayın **sonraki** düğmesi.
    
1. Üzerinde **kullanıcı kimliği öznitelik ve dönüştürme** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon5d.png)
    
1. Üzerinde **kullanıcı oluşturma ve güncelleştirme** sayfasında **sonraki & Kaydet** ayarları kaydedin.    
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6a.png)
    
1. Üzerinde **ayarlarınızı test etme** sayfasında **test atlayın ve el ile yapılandırma** kullanıcı test şimdilik atlamak için. Bu, sonraki bölümde gerçekleştirilir ve bazı ayarlar Azure portalında gerektirir. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6b.png)
    
1. Apprearing iletişim okuma **test yolları atlanıyor...** , tıklayın **Tamam**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/addon6c.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssoconfluence-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>SAML SSO için çözüm GmbH test kullanıcısı tarafından Confluence oluşturma

SAML SSO için Confluence GmbH çözünürlüğüyle oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Confluence için SAML SSO içine GmbH çözüm tarafından sağlanması gerekir.  
SAML SSO GmbH çözünürlüğün Confluence, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak, SAML SSO Confluence çözümleme GmbH şirket site tarafından için oturum açın.

1. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user1.png) 

1. Kullanıcılar bölümü altında **kullanıcı ekleme** sekmesi. Üzerinde **"Bir kullanıcı Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/samlssoconfluence-tutorial/user2.png) 

    a. İçinde **kullanıcıadı** metin Britta Simon gibi kullanıcının e-posta yazın.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin Britta Simon için parolayı yazın.

    e. Tıklayın **parolayı onayla** parolayı yeniden girin.
    
    f. Tıklayın **Ekle** düğmesi.    

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Confluence için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Confluence için SAML SSO GmbH çözünürlüğüyle atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **GmbH çözünürlüğün Confluence için SAML SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Tarafından erişim panelinde çözümleme GmbH kutucuk Confluence için SAML SSO'ye tıkladığınızda, otomatik olarak Confluence için SAML SSO için çözümleme GmbH uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/samlssoconfluence-tutorial/tutorial_general_203.png

