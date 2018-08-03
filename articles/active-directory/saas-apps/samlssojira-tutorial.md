---
title: 'Öğretici: Azure Active Directory ile tümleştirme için Jıra SAML SSO GmbH çözünürlüğün | Microsoft Docs'
description: Çoklu oturum açma SAML SSO Jıra için Azure Active Directory arasındaki GmbH çözünürlüğün yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 6ae8256f3485d49d42efeb2927a6838252a1aeee
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442916"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Öğretici: Azure Active Directory ile tümleştirme için Jıra SAML SSO GmbH çözünürlüğün

Bu öğreticide, Azure Active Directory (Azure AD) ile GmbH çözünürlüğün Jıra için SAML SSO tümleştirme konusunda bilgi edinin.

SAML SSO için Jıra çözünürlüğün GmbH Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- SAML SSO için Jıra GmbH çözünürlüğün erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanmış için Jıra için SAML SSO (çoklu oturum açma) GmbH çözünürlüğün Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi GmbH çözünürlüğüyle Jıra için SAML SSO ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- SAML SSO etkin abonelikte GmbH çoklu oturum çözünürlüğün Jıra için

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. SAML SSO için Jıra GmbH çözünürlüğüyle galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a>SAML SSO için Jıra GmbH çözünürlüğüyle galeri ekleme
Jıra için SAML SSO, Azure AD'de tümleştirmesini GmbH çözünürlüğün yapılandırmak için Jıra için SAML SSO GmbH çözünürlüğüyle Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**SAML SSO için Jıra GmbH çözünürlüğüyle Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **GmbH çözünürlüğün Jıra için SAML SSO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssojira-tutorial/tutorial_samlssojira_search.png)

1. Sonuçlar panelinde seçin **GmbH çözünürlüğün Jıra için SAML SSO**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Jıra ile GmbH "Britta Simon." adlı bir test kullanıcı tabanlı bir çözüm olarak test etme

Tek çalışmak için oturum açma için Azure AD hangi karşılık gelen kullanıcı SAML SSO için Jıra çözünürlüğün GmbH, Azure AD'de bir kullanıcı için olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve SAML SSO Jıra için ilgili kullanıcı çözünürlüğün arasında bir bağlantı ilişki GmbH kurulması gerekir.

SAML SSO GmbH çözünürlüğün Jıra, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SAML SSO için Jıra ile GmbH çözünürlüğün sınamak için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[SAML SSO için çözüm GmbH test kullanıcısı tarafından Jıra oluşturma](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  - kullanıcı Azure AD gösterimini bağlı GmbH çözünürlüğün Jıra için SAML SSO içinde bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, SAML SSO için Jıra GmbH uygulama çözünürlüğüne göre yapılandırın.

**Azure AD çoklu oturum açma SAML SSO için Jıra ile GmbH çözünürlüğüyle yapılandırmak için aşağıdaki adımları uygulayın:**

1. Azure portalında, üzerinde **GmbH çözünürlüğün Jıra için SAML SSO** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

1. Üzerinde **Jıra çözünürlüğün GmbH etki alanı ve URL'ler için SAML SSO** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`

1. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [SAML SSO Jıra çözünürlüğün GmbH istemci için Destek ekibine](https://www.resolution.de/go/support) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/tutorial_general_400.png)
    
1. Farklı bir web tarayıcı penceresinde oturum açın, **SAML SSO için çözüm GmbH Yönetici portalı tarafından Jıra** yönetici olarak.

1. Dişlisine gelin ve tıklayın **eklentileri**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon1.png)

1. Yönetici erişimi sayfasına yönlendirilirsiniz. Girin **parola** tıklatıp **Onayla** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon2.png)

1. Eklentiler sekmesi bölümü altında **yeni eklentileri bulma**. Arama **SAML çoklu oturum açma (SSO) için JIRA** tıklatıp **yükleme** yeni SAML eklentisini yüklemek için düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon7.png)

1. Eklenti yüklemesi başlatılır. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon8.png)

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon9.png)

1.  **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon10.png)
    
1. Tıklayın **yapılandırma** yeni eklentiyi yapılandırmak için.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon11.png)

1. Üzerinde **SAML SingleSignOn eklentisi yapılandırma** sayfasında **yeni IDP ekleme** kimlik sağlayıcısı ayarlarını yapılandırmak için düğmeye.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon4.png)

1. Üzerinde **SAML kimlik sağlayıcınızı seçin** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5a.png)
 
    a. Ayarlama **Azure AD'ye** IDP türü.
    
    b. Ekleme **adı** kimlik sağlayıcısının (ör. Azure AD).
    
    c. Ekleme **açıklama** kimlik sağlayıcısının (ör. Azure AD).
    
    d. **İleri**’ye tıklayın.
    
1. Üzerinde **kimlik sağlayıcı Yapılandırması** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5b.png)

1. Üzerinde **meta verileri içeri aktarma SAML IDP** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5c.png)

    a. Tıklayın **yük dosyası** düğmesi ve 5. adımda indirdiğiniz meta veri XML dosyasını seçin.

    b. Tıklayın **alma** düğmesi.
    
    c. İçeri aktarma başarılı olana kadar kısa bir süre bekleyin.
    
    d. Tıklayın **sonraki** düğmesi.
    
1. Üzerinde **kullanıcı kimliği öznitelik ve dönüştürme** sayfasında **sonraki** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon5d.png)
    
1. Üzerinde **kullanıcı oluşturma ve güncelleştirme** sayfasında **sonraki & Kaydet** ayarları kaydedin.    
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6a.png)
    
1. Üzerinde **ayarlarınızı test etme** sayfasında **test atlayın ve el ile yapılandırma** kullanıcı test şimdilik atlamak için. Bu, sonraki bölümde gerçekleştirilir ve bazı ayarlar Azure portalında gerektirir. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6b.png)
    
1. Apprearing iletişim okuma **test yolları atlanıyor...** , tıklayın **Tamam**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/addon6c.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssojira-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssojira-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssojira-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/samlssojira-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>SAML SSO için çözüm GmbH test kullanıcısı tarafından Jıra oluşturma

SAML SSO için Jıra GmbH çözünürlüğüyle oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jıra için SAML SSO içine GmbH çözüm tarafından sağlanması gerekir.  
SAML SSO GmbH çözünürlüğün Jıra, sağlama, el ile bir görev olur.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak, SAML SSO Jıra çözümleme GmbH şirket site tarafından için oturum açın.

1. Dişlisine gelin ve tıklayın **kullanıcı yönetimi**.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user1.png) 

1. Girmek için yönetici erişimi sayfasına yönlendirilirsiniz **parola** tıklatıp **Onayla** düğmesi.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user2.png) 

1. Altında **kullanıcı yönetimi** sekmesinde bölüm **oluşturacağı**.

    ![Çalışan Ekle](./media/samlssojira-tutorial/user3.png) 

1. Üzerinde **"Yeni kullanıcı oluşturma"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/samlssojira-tutorial/user4.png) 

    a. İçinde **e-posta adresi** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

    b. İçinde **tam adı** metin Britta Simon gibi kullanıcının tam adını yazın.

    c. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.

    d. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    e. Tıklayın **kullanıcı oluşturma**.   

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma GmbH çözünürlüğüyle Jıra için SAML SSO için erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Jıra için SAML SSO GmbH çözünürlüğüyle atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **GmbH çözünürlüğün Jıra için SAML SSO**.

    ![Çoklu oturum açmayı yapılandırın](./media/samlssojira-tutorial/tutorial_samlssojira_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Tarafından erişim panelinde çözümleme GmbH kutucuk Jıra için SAML SSO'ye tıkladığınızda, otomatik olarak Jıra için SAML SSO için çözümleme GmbH uygulama tarafından açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/samlssojira-tutorial/tutorial_general_203.png

