---
title: 'Öğretici: Azure Active Directory Lifesize bulut ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Lifesize bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b699714a2ab90fd0ad1c2f290681ccdae7aeb1ba
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052202"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Öğretici: Azure Active Directory Lifesize bulut ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Lifesize bulut tümleştirme konusunda bilgi edinin.

Azure AD ile Lifesize bulut tümleştirme ile aşağıdaki avantajları sağlar:

- Lifesize Bulutuna erişimi olan Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Lifesize buluta açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Lifesize Bulutla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Lifesize bulut çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Lifesize bulut ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Galeriden Lifesize bulut ekleme
Azure AD'de Lifesize bulut tümleştirmesini yapılandırmak için Lifesize bulut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Lifesize bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Lifesize bulut**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. Sonuçlar panelinde seçin **Lifesize bulut**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırmanız ve Azure AD çoklu oturum açmayı test Lifesize bulutla "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek çalışmak için oturum açma için Azure AD ne Lifesize bulutta karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Lifesize bulutta arasında bir bağlantı ilişki kurulması gerekir.

Değeri Lifesize buluta atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Lifesize Bulutu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Test kullanıcı Lifesize bulut oluşturma](#creating-a-lifesize-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Lifesize bulutta Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Lifesize bulut uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Lifesize Bulutla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Lifesize bulut** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. Üzerinde **Lifesize bulut etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://login.lifesizecloud.com/ls/?acs`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://login.lifesizecloud.com/<companyname>`

     
4. Denetleme **Gelişmiş URL ayarlarını göster**, aşağıdaki adımı uygulayın:    
   
    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak: `https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek oturum açma URL'si, geçiş durumu ve tanımlayıcı ile güncelleştirmeniz gerekiyor. İlgili kişi [Lifesize bulut istemci Destek ekibine](https://www.lifesize.com/support) oturum açma URL'si ve tanımlayıcı değerlerini ve almak için geçiş durumu değeri SSO yapılandırmasından öğreticinin ilerleyen bölümlerinde açıklanan alabilirsiniz.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_general_400.png)

6. Üzerinde **Lifesize bulut Yapılandırması** bölümünde **Lifesize bulut yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. Uygulamanız, yönetici ayrıcalıklarıyla Lifesize bulut uygulamasına oturum açma için yapılandırılmış SSO edinmek için.

8. Sağ üst köşede adınıza tıklayın ve ardından **Gelişmiş ayarlar**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. Gelişmiş ayarlar artık tıklayarak **SSO yapılandırma** bağlantı. Bu, Örneğiniz için SSO yapılandırma sayfası açılır.
   
    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. Artık SSO Yapılandırması kullanıcı Arabirimi aşağıdaki değerleri yapılandırın.    
   
    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. İçinde **kimlik sağlayıcısını veren** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    b.  İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.
  
    d. Ad metin kutusu için SAML öznitelik eşlemelerini değer olarak girin **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. SAML özniteliği eşlemesinde için **Soyadı** metin kutusuna bir değer olarak girin **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. SAML özniteliği eşlemesinde için **e-posta** metin kutusuna bir değer olarak girin **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. Tıklayabilirsiniz yapılandırmayı kontrol etmek için **Test** düğmesi.
   
    >[!NOTE]
    >Başarılı test etmek için Azure AD'de Yapılandırma Sihirbazı'nı tamamlamak ve ayrıca kullanıcılara veya test gerçekleştirebilirsiniz gruplara erişim sağlamak gerekir.

12. Üzerinde kontrol ederek SSO etkinleştirme **SSO etkinleştirme** düğmesi.

13. Artık tıklayarak **güncelleştirme** böylece tüm ayarları kaydedildi. Bu RelayState değeri oluşturur. Kopyalama metin kutusuna oluşturulan RelayState değeri içinde yapıştırın **geçiş durumu** metin kutusunun altında **Lifesize bulut etki alanı ve URL'ler** bölümü. 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/lifesize-cloud-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/lifesize-cloud-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Test kullanıcı Lifesize bulut oluşturma

Bu bölümde, Britta Simon Lifesize bulutta adlı bir kullanıcı oluşturun. Otomatik kullanıcı hazırlama Lifesize bulut desteklemiyor. Azure AD, başarılı kimlik doğrulamasından sonra kullanıcı uygulamayı otomatik olarak sağlanır. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Lifesize buluta erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Lifesize buluta atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Lifesize bulut**.

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Lifesize bulut kutucuğa tıkladığınızda, oturum açma sayfası Lifesize bulut uygulamasının almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/lifesize-cloud-tutorial/tutorial_general_203.png

