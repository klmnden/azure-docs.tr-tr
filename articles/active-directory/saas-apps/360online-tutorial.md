---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile 360 çevrimiçi | Microsoft Docs'
description: Azure Active Directory ve 360 çevrimiçi arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: cda8eba6-843f-4a09-8c55-0aaf6e593d75
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 8f5a60566d65ea35934ff3847778d37990f4dc8c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54807941"
---
# <a name="tutorial-azure-active-directory-integration-with-360-online"></a>Öğretici: 360 Online ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile 360 çevrimiçi tümleştirme konusunda bilgi edinin.

360 çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- 360 çevrimiçi erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için 360 çevrimiçi (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile 360 çevrimiçi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir 360 çevrimiçi çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. 360 çevrimiçi galeriden ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-360-online-from-the-gallery"></a>360 çevrimiçi galeriden ekleme
Azure AD'de 360 çevrimiçi tümleştirmesini yapılandırmak için 360 çevrimiçi galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**360 çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **360 çevrimiçi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/360online-tutorial/tutorial_360online_search.png)

5. Sonuçlar panelinde seçin **360 çevrimiçi**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/360online-tutorial/tutorial_360online_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı 360 çevrimiçi ile test etme

Tek iş için oturum açma için Azure AD ne 360 çevrimiçi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı 360 arasında bir bağlantı ilişki çevrimiçi kurulması gerekir.

360 çevrimiçi değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma 360 çevrimiçi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[360 çevrimiçi test kullanıcısı oluşturma](#creating-a-360-online-test-user)**  - kullanıcı Azure AD gösterimini bağlı 360 çevrimiçi Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve 360 çevrimiçi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile 360 çevrimiçi yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **360 çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/360online-tutorial/tutorial_360online_samlbase.png)

3. Üzerinde **360 çevrimiçi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/360online-tutorial/tutorial_360online_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.public360online.com`

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [360 çevrimiçi istemci Destek ekibine](mailto:360online@software-innovation.com) değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/360online-tutorial/tutorial_360online_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/360online-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **360 çevrimiçi** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [360 Destek ekibine](mailto:360online@software-innovation.com). 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/360online-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/360online-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/360online-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/360online-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-360-online-test-user"></a>360 çevrimiçi test kullanıcısı oluşturma

Bu bölümde, Britta Simon 360 çevrimiçi adlı bir kullanıcı oluşturun. iletişime geçmeniz [360 Destek ekibine](mailto:360online@software-innovation.com).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Britta Simon'ın 360 çevrimiçi erişim vererek Azure çoklu oturum açmayı kullanmak etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon 360 çevrimiçi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **360 çevrimiçi**.

    ![Çoklu oturum açmayı yapılandırın](./media/360online-tutorial/tutorial_360online_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

360 çevrimiçi kutucuğa tıkladığınızda, erişim Paneli'nde, otomatik olarak 360 çevrimiçi uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/360online-tutorial/tutorial_general_01.png
[2]: ./media/360online-tutorial/tutorial_general_02.png
[3]: ./media/360online-tutorial/tutorial_general_03.png
[4]: ./media/360online-tutorial/tutorial_general_04.png

[100]: ./media/360online-tutorial/tutorial_general_100.png

[200]: ./media/360online-tutorial/tutorial_general_200.png
[201]: ./media/360online-tutorial/tutorial_general_201.png
[202]: ./media/360online-tutorial/tutorial_general_202.png
[203]: ./media/360online-tutorial/tutorial_general_203.png

