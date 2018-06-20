---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Humanity | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Humanity arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: b0cd33370f5940e7f74fed0938320c96ae15a447
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226903"
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a>Öğretici: Azure Active Directory Tümleştirme Humanity ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Humanity tümleştirmek öğrenin.

Humanity Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Humanity erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için Humanity (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Humanity ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Humanity çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Humanity ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-humanity-from-the-gallery"></a>Galeriden Humanity ekleme
Azure AD'ye Humanity tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Humanity eklemeniz gerekir.

**Galeriden Humanity eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Humanity**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/shiftplanning-tutorial/tutorial_humanity_search.png)

5. Sonuçlar panelinde seçin **Humanity**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Humanity ile test etme

Tekli çalışmaya oturum için Azure AD Humanity karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Humanity ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Humanity içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırmak ve Azure AD çoklu oturum açma ile Humanity sınamak için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Humanity test kullanıcısı oluşturma](#creating-a-humanity-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Humanity sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Humanity uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Humanity yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Humanity** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. Üzerinde **Humanity etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/shiftplanning-tutorial/tutorial_humanity_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://company.humanity.com/includes/saml/`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://company.humanity.com/app/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Humanity istemci destek ekibi](https://www.humanity.com/support/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/shiftplanning-tutorial/tutorial_general_400.png)

6. Üzerinde **Humanity yapılandırma** 'yi tıklatın **yapılandırma Humanity** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si ve Sign-Out URL** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. Farklı web tarayıcısı penceresinde oturum açın, **Humanity** yönetici olarak şirket site.

8. Üstteki menüde tıklatın **yönetici**.
   
    ![Yönetici](./media/shiftplanning-tutorial/iC786619.png "yönetici")

9. Altında **tümleştirme**, tıklatın **çoklu oturum açma**.
   
    ![Çoklu oturum açma](./media/shiftplanning-tutorial/iC786620.png "çoklu oturum açma")

10. İçinde **çoklu oturum açma** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/shiftplanning-tutorial/iC786905.png "çoklu oturum açma")
   
    a. Seçin **SAML etkin**.

    b. Seçin **parola oturum açma izin**.

    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** içine değer **SAML veren URL'si** metin kutusu.

    d. Yapıştır **Sign-Out URL** içine değer **uzak oturum kapatma URL'si** metin kutusu.
   
    e. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** metin kutusu.

11. Tıklatın **ayarlarını kaydetmek**.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/shiftplanning-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/shiftplanning-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/shiftplanning-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/shiftplanning-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-humanity-test-user"></a>Humanity test kullanıcısı oluşturma

Azure AD kullanıcıları için Humanity oturum açmak etkinleştirmek için bunların Humanity sağlanmalıdır. Humanity söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Humanity** yönetici olarak şirket site.

2. Tıklatın **yönetici**.
   
    ![Yönetici](./media/shiftplanning-tutorial/iC786619.png "yönetici")

3. Tıklatın **personel**.
   
    ![Personel](./media/shiftplanning-tutorial/ic786623.png "personel")

4. Altında **Eylemler**, tıklatın **eklemek çalışanlar**.
   
    ![Çalışanlar eklemek](./media/shiftplanning-tutorial/iC786624.png "çalışanları ekleme")

5. İçinde **eklemek çalışanlar** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çalışanlar Kaydet](./media/shiftplanning-tutorial/iC786625.png "çalışanlar Kaydet")
   
    a. Tür **ad**, **Soyadı**, ve **e-posta** istediğiniz ilgili metin kutularına sağlamayı geçerli bir AAD hesabının.

    b. Tıklatın **çalışanlar kaydetmek**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Humanity tarafından sağlanan veya herhangi diğer Humanity kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta Humanity için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**Humanity için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Humanity**.

    ![Çoklu oturum açmayı yapılandırın](./media/shiftplanning-tutorial/tutorial_humanity_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Humanity parçasında tıklattığınızda, otomatik olarak Humanity uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/shiftplanning-tutorial/tutorial_general_203.png

