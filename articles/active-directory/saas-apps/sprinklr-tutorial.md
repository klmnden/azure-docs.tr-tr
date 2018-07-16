---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Sprinklr | Microsoft Docs'
description: Azure Active Directory ve Sprinklr arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: ece3509743bc3712d144a3547c5ff91f9ea101e7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050751"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Öğretici: Azure Active Directory Sprinklr ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Sprinklr tümleştirme konusunda bilgi edinin.

Azure AD ile Sprinklr tümleştirme ile aşağıdaki avantajları sağlar:

- Sprinklr erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Sprinklr (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Sprinklr yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Sprinklr çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Sprinklr ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sprinklr-from-the-gallery"></a>Galeriden Sprinklr ekleme
Azure AD'de Sprinklr tümleştirmesini yapılandırmak için Sprinklr Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Sprinklr eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Sprinklr**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/tutorial_sprinklr_search.png)

5. Sonuçlar panelinde seçin **Sprinklr**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Sprinklr ile test etme

Tek iş için oturum açma için Azure AD ne Sprinklr karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Sprinklr ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Sprinklr içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Sprinklr ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Sprinklr test kullanıcısı oluşturma](#creating-a-sprinklr-test-user)**  - kullanıcı Azure AD gösterimini bağlı Sprinklr Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Sprinklr uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Sprinklr yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Sprinklr** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. Üzerinde **Sprinklr etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.sprinklr.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek oturum açma URL'si ve tanımlayıcı değeri güncelleştirin. İlgili kişi [Sprinklr istemci Destek ekibine](https://www.sprinklr.com/contact-us/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_general_400.png)

6. Üzerinde **Sprinklr yapılandırma** bölümünde **yapılandırma Sprinklr** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

7. Farklı bir web tarayıcı penceresinde Sprinklr şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Git **Yönetim \> ayarları**.
   
    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

9. Git **iş ortağı yönetme \> çoklu oturum açma** üzerinde sol bölmeden.
   
    ![İş ortağı yönetme](./media/sprinklr-tutorial/ic782908.png "iş ortağı yönetme")

10. Tıklayın **+ çoklu oturum açma ekleme**.
   
    ![Çoklu oturum açmaların](./media/sprinklr-tutorial/ic782909.png "çoklu oturum açmaların")

11. Üzerinde **çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmaların](./media/sprinklr-tutorial/ic782910.png "çoklu oturum açmaların")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: *WAADSSOTest*).

    b. Seçin **etkin**.

    c. Seçin **yeni SSO sertifikası kullanacak**.
             
    e. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    f. Yapıştırma **SAML varlık kimliği** Azure Portalı'ndan kopyaladığınız değeri **varlık kimliği** metin.

    g. Yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure Portalı'ndan kopyaladığınız değeri **kimlik sağlayıcısı oturum açma URL'si** metin.

    h. Yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri **kimlik sağlayıcısı oturum kapatma URL'si** metin.
     
    i. Olarak **SAML kullanıcı kimliği türü**seçin **onaylamayı içeren kullanıcı "s sprinklr.com username**.

    j. Olarak **SAML kullanıcı kimliği konumu**seçin **kullanıcı kimliğidir konu deyiminin ad tanımlayıcı öğesinde**.

    k. **Kaydet**’e tıklayın.
       
    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sprinklr-test-user"></a>Sprinklr test kullanıcısı oluşturma

1. Sprinklr şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Git **Yönetim \> ayarları**.
   
    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

3. Git **yönetme istemci \> kullanıcılar** sol bölmeden.
   
    ![Ayarları](./media/sprinklr-tutorial/ic782914.png "ayarları")

4. Tıklayın **kullanıcı ekleme**.
   
    ![Ayarları](./media/sprinklr-tutorial/ic782915.png "ayarları")

5. Üzerinde **düzenleme kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcıyı Düzenle](./media/sprinklr-tutorial/ic782916.png "düzenleme kullanıcı") 

    a. İçinde **e-posta**, **ad** ve **Soyadı** metin kutuları, sağlamak istediğiniz bir Azure AD kullanıcı hesabı bilgilerini yazın.

    b. Seçin **parola devre dışı**.

    c. Seçin **dil**.

    d. Seçin **kullanıcı türü**.

    e. Tıklayın **güncelleştirme**.
   
     >[!IMPORTANT]
     >**Parola devre dışı** bir kimlik sağlayıcısı ile oturum açmak bir kullanıcı seçilmesi gerekir. 
     
6. Git **rol**ve ardından aşağıdaki adımları gerçekleştirin:
   
    ![İş ortağı rolleri](./media/sprinklr-tutorial/ic782917.png "ortak rolleri")

    a. Gelen **genel** listesinden **tüm\_izinleri**.  

    b. Tıklayın **güncelleştirme**.

>[!NOTE]
>Herhangi diğer Sprinklr kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Sprinklr tarafından sağlanan. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Sprinklr erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Sprinklr için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Sprinklr**.

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Sprinklr kutucuğa tıkladığınızda, erişim paneli hakkında daha fazla bilgi için otomatik olarak imzalanan Sprinklr uygulamanıza açma almalısınız [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/sprinklr-tutorial/tutorial_general_203.png

