---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Freshservice | Microsoft Docs'
description: Azure Active Directory ve Freshservice arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: cccbe2052336012b9ac98b3e28dc6481cbf9aefb
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144541"
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Öğretici: Azure Active Directory Freshservice ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Freshservice tümleştirme konusunda bilgi edinin.

Azure AD ile Freshservice tümleştirme ile aşağıdaki avantajları sağlar:

- Freshservice erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Freshservice (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Freshservice yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Freshservice çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Freshservice ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-freshservice-from-the-gallery"></a>Galeriden Freshservice ekleme
Azure AD'de Freshservice tümleştirmesini yapılandırmak için Freshservice Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Freshservice eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Freshservice**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshservice-tutorial/tutorial_freshservice_search.png)

5. Sonuçlar panelinde seçin **Freshservice**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Freshservice "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Freshservice karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Freshservice ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Freshservice içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Freshservice ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Freshservice test kullanıcısı oluşturma](#creating-a-freshservice-test-user)**  - kullanıcı Azure AD gösterimini bağlı Freshservice Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Freshservice uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Freshservice yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Freshservice** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. Üzerinde **Freshservice etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/freshservice-tutorial/tutorial_freshservice_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<democompany>.freshservice.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Freshservice istemci Destek ekibine](https://support.freshservice.com/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** sertifika değeri.

    ![Çoklu oturum açmayı yapılandırın](./media/freshservice-tutorial/tutorial_freshservice_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/freshservice-tutorial/tutorial_general_400.png)

6. Üzerinde **Freshservice yapılandırma** bölümünde **yapılandırma Freshservice** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/freshservice-tutorial/tutorial_freshservice_configure.png) 

7. Farklı bir web tarayıcı penceresinde Freshservice şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Üstteki menüden **yönetici**.
   
    ![Yönetici](./media/freshservice-tutorial/ic790814.png "yönetici")

9. İçinde **müşteri portalı**, tıklayın **güvenlik**.
   
    ![Güvenlik](./media/freshservice-tutorial/ic790815.png "güvenlik")

10. İçinde **güvenlik** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum](./media/freshservice-tutorial/ic790816.png "tekli oturum")
   
    a. Anahtar **tekli oturum**.

    b. Seçin **SAML SSO**.

    c. İçinde **SAML oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    d. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. İçinde **güvenlik sertifikası parmak izi** metin kutusu, yapıştırma **parmak İZİ** Azure Portalı'ndan kopyaladığınız sertifika değeri.

    f. **Kaydet**’e tıklayın

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshservice-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshservice-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshservice-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/freshservice-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-freshservice-test-user"></a>Freshservice test kullanıcısı oluşturma

FreshService için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların FreshService sağlanması gerekir. FreshService söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **FreshService** yönetici olarak şirketin site.

2. Üstteki menüden **yönetici**.
   
    ![Yönetici](./media/freshservice-tutorial/ic790814.png "yönetici")

3. İçinde **kullanıcı yönetimi** bölümünde **isteyenlere**.
   
    ![Kararınız istekte](./media/freshservice-tutorial/ic790818.png "isteyenlere")

4. Tıklayın **yeni bir istek sahibi**.
   
    ![Yeni isteyenlere](./media/freshservice-tutorial/ic790819.png "yeni isteyenlere")

5. İçinde **yeni bir istek sahibi** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni Talep sahibinin](./media/freshservice-tutorial/ic790820.png "yeni bir istek sahibi")   

    a. Girin **ad** ve **e-posta** istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir Azure Active Directory hesabının öznitelikleri.

    b. **Kaydet**’e tıklayın.
   
    >[!NOTE]
    >Azure Active Directory hesap sahibinin etkin hale gelir önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
    >  

>[!NOTE]
>Herhangi diğer FreshService kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak FreshService tarafından sağlanan.
>  

![Kullanıcı Ata][200] 

**Britta Simon Freshservice için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Freshservice**.

    ![Çoklu oturum açmayı yapılandırın](./media/freshservice-tutorial/tutorial_freshservice_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Freshservice kutucuğa tıkladığınızda, otomatik olarak Freshservice uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/freshservice-tutorial/tutorial_general_01.png
[2]: ./media/freshservice-tutorial/tutorial_general_02.png
[3]: ./media/freshservice-tutorial/tutorial_general_03.png
[4]: ./media/freshservice-tutorial/tutorial_general_04.png

[100]: ./media/freshservice-tutorial/tutorial_general_100.png

[200]: ./media/freshservice-tutorial/tutorial_general_200.png
[201]: ./media/freshservice-tutorial/tutorial_general_201.png
[202]: ./media/freshservice-tutorial/tutorial_general_202.png
[203]: ./media/freshservice-tutorial/tutorial_general_203.png

