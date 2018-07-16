---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle SpringCM | Microsoft Docs'
description: Azure Active Directory ve SpringCM arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: cf0495e27d7fc06c54266eba6640e0c0fb99be42
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051210"
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Öğretici: Azure Active Directory SpringCM ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SpringCM tümleştirme konusunda bilgi edinin.

Azure AD ile SpringCM tümleştirme ile aşağıdaki avantajları sağlar:

- SpringCM erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için SpringCM (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SpringCM yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik SpringCM çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SpringCM ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-springcm-from-the-gallery"></a>Galeriden SpringCM ekleme
Azure AD'de SpringCM tümleştirmesini yapılandırmak için SpringCM Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SpringCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SpringCM**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/spring-cm-tutorial/tutorial_springcm_search.png)

5. Sonuçlar panelinde seçin **SpringCM**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı SpringCM ile test etme

Tek iş için oturum açma için Azure AD ne SpringCM karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SpringCM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SpringCM içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SpringCM ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SpringCM test kullanıcısı oluşturma](#creating-a-springcm-test-user)**  - kullanıcı Azure AD gösterimini bağlı SpringCM Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SpringCM uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SpringCM yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SpringCM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. Üzerinde **SpringCM etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/spring-cm-tutorial/tutorial_springcm_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [SpringCM istemci Destek ekibine](https://knowledge.springcm.com/support) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Raw)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/spring-cm-tutorial/tutorial_general_400.png)

6. Üzerinde **SpringCM yapılandırma** bölümünde **yapılandırma SpringCM** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/spring-cm-tutorial/tutorial_springcm_configure.png)     

7. Farklı bir web tarayıcı penceresinde oturum açın, **SpringCM** şirketinizin sitesi yöneticisi olarak.

8. Üstteki menüden **Git**, tıklayın **tercihleri**ve ardından **hesap tercihleri** bölümünde **SAML SSO**.
   
    ![SAML SSO](./media/spring-cm-tutorial/ic797051.png "SAML SSO")

9. Kimlik sağlayıcısı Yapılandırması bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Kimlik sağlayıcısı Yapılandırması](./media/spring-cm-tutorial/ic797052.png "kimlik sağlayıcı yapılandırması")
    
    a. İndirilen Azure Active Directory sertifikanızı karşıya yüklemek için tıklayın **veren sertifika Seç** veya **değişiklik sertifikayı**.
    
    b. Yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri **veren** metin.
    
    c. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri **hizmet sağlayıcısı (SP) tarafından başlatılan Endpoint** metin.
            
    d. Seçin **SAML etkin** olarak **etkinleştirme**.

    e. **Kaydet**’e tıklayın.
 
> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/spring-cm-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/spring-cm-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/spring-cm-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/spring-cm-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-springcm-test-user"></a>SpringCM test kullanıcısı oluşturma

Azure Active Directory Kullanıcıları için SpringCM oturum açmak etkinleştirmek için bunlar SpringCM sağlanması gerekir. SpringCM söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

>[!NOTE]
>Daha fazla bilgi için [oluşturma ve düzenleme SpringCM kullanıcı](http://knowledge.springcm.com/create-and-edit-a-springcm-user). 

**Bir kullanıcı hesabına SpringCM sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **SpringCM** şirketinizin sitesi yöneticisi olarak.

2. Tıklayın **GOTO**ve ardından **adres defteri**.
   
    ![Kullanıcı oluşturma](./media/spring-cm-tutorial/ic797054.png "kullanıcı oluşturma")

3. Tıklayın **oluşturacağı**.

4. Seçin bir **kullanıcı rolü**.

5. Seçin **etkinleştirme e-posta Gönder**.

6. Ad, Soyadı ve ilgili metin kutularına zbilgisayarlar istediğiniz geçerli bir Azure Active Directory kullanıcı hesabının e-posta adresini yazın.

7. Kullanıcıyı eklemek bir **güvenlik grubu**.

8. **Kaydet**’e tıklayın.

  >[!NOTE]
  >Herhangi diğer SpringCM kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak SpringCM tarafından sağlanan.  
  > 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için SpringCM erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon SpringCM için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SpringCM**.

    ![Çoklu oturum açmayı yapılandırın](./media/spring-cm-tutorial/tutorial_springcm_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
 
Erişim panelinde SpringCM kutucuğa tıkladığınızda, otomatik olarak SpringCM uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/spring-cm-tutorial/tutorial_general_203.png

