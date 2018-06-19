---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Sprinklr | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Sprinklr arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7037c509eeb4b7adf1232ebd29b7f7034237f22b
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35980612"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Öğretici: Azure Active Directory Tümleştirme Sprinklr ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Sprinklr tümleştirmek öğrenin.

Sprinklr Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Sprinklr erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Sprinklr (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Sprinklr ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Sprinklr çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Sprinklr ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-sprinklr-from-the-gallery"></a>Galeriden Sprinklr ekleme
Azure AD Sprinklr tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Sprinklr eklemeniz gerekir.

**Galeriden Sprinklr eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Sprinklr**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/tutorial_sprinklr_search.png)

5. Sonuçlar panelinde seçin **Sprinklr**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Sprinklr ile test etme

Tekli çalışmaya oturum için Azure AD Sprinklr karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Sprinklr ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Sprinklr içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Sprinklr ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Sprinklr test kullanıcısı oluşturma](#creating-a-sprinklr-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Sprinklr sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Sprinklr uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Sprinklr yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Sprinklr** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. Üzerinde **Sprinklr etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.sprinklr.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Gerçek oturum açma URL'si ve tanımlayıcı değeri güncelleştirin. Kişi [Sprinklr istemci destek ekibi](https://www.sprinklr.com/contact-us/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_general_400.png)

6. Üzerinde **Sprinklr yapılandırma** 'yi tıklatın **yapılandırma Sprinklr** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

7. Farklı web tarayıcısı penceresinde Sprinklr şirket sitenize yönetici olarak oturum açın.

8. Git **Yönetim \> ayarları**.
   
    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

9. Git **iş ortağı yönetme \> çoklu oturum açma** üzerinde sol bölmeden.
   
    ![İş ortağı yönetme](./media/sprinklr-tutorial/ic782908.png "iş ortağı yönetme")

10. Tıklatın **+ çoklu oturum açmaların eklemek**.
   
    ![Çoklu oturum açmaların](./media/sprinklr-tutorial/ic782909.png "çoklu oturum açmaların")

11. Üzerinde **çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açmaların](./media/sprinklr-tutorial/ic782910.png "çoklu oturum açmaların")

    a. İçinde **adı** metin kutusuna, yapılandırmanız için bir ad yazın (örneğin: *WAADSSOTest*).

    b. Seçin **etkin**.

    c. Seçin **yeni SSO sertifikayı kullanmak**.
             
    e. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **kimlik sağlayıcısı sertifikası** metin kutusu.

    f. Yapıştır **SAML varlık kimliği** Azure portalından kopyaladığınız değeri **varlık kimliği** metin kutusu.

    g. Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyaladığınız değeri **kimlik sağlayıcısı oturum açma URL'si** metin kutusu.

    h. Yapıştır **Sign-Out URL** Azure portalından kopyaladığınız değeri **kimlik sağlayıcısı oturum kapatma URL'si** metin kutusu.
     
    i. Olarak **SAML kullanıcı kimliği türü**seçin **onaylamayı içeren kullanıcı "s sprinklr.com kullanıcıadı**.

    j. Olarak **SAML kullanıcı kimliği konumu**seçin **kullanıcı kimliğidir konu deyim ad tanımlayıcısı öğesinde**.

    k. **Kaydet**’e tıklayın.
       
    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sprinklr-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sprinklr-test-user"></a>Sprinklr test kullanıcısı oluşturma

1. Sprinklr şirket sitenize yönetici olarak oturum açın.

2. Git **Yönetim \> ayarları**.
   
    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

3. Git **yönetmek istemci \> kullanıcılar** sol bölmeden.
   
    ![Ayarları](./media/sprinklr-tutorial/ic782914.png "ayarları")

4. Tıklatın **kullanıcı ekleme**.
   
    ![Ayarları](./media/sprinklr-tutorial/ic782915.png "ayarları")

5. Üzerinde **düzenleme kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı düzenleme](./media/sprinklr-tutorial/ic782916.png "düzenleme kullanıcı") 

    a. İçinde **e-posta**, **ad** ve **Soyadı** metin kutuları, sağlamak istediğiniz bir Azure AD kullanıcı hesabı bilgilerini yazın.

    b. Seçin **parola devre dışı**.

    c. Seçin **dil**.

    d. Seçin **kullanıcı türü**.

    e. Tıklatın **güncelleştirme**.
   
     >[!IMPORTANT]
     >**Parola devre dışı** bir kimlik sağlayıcısı oturum açma kullanıcı seçilmesi gerekir. 
     
6. Git **rol**ve ardından aşağıdaki adımları gerçekleştirin:
   
    ![İş ortağı rolleri](./media/sprinklr-tutorial/ic782917.png "ortak rolleri")

    a. Gelen **Global** listesinde **tüm\_izinleri**.  

    b. Tıklatın **güncelleştirme**.

>[!NOTE]
>API Azure AD kullanıcı hesaplarını sağlamak için Sprinklr tarafından sağlanan veya herhangi diğer Sprinklr kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Sprinklr için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Sprinklr için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Sprinklr**.

    ![Çoklu oturum açmayı yapılandırın](./media/sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Sprinklr parçasında tıkladığınızda, erişim paneli hakkında daha fazla bilgi için bkz, otomatik olarak Sprinklr uygulamanıza açan almalısınız [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



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

