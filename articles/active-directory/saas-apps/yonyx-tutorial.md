---
title: 'Öğretici: Azure Active Directory Yonyx etkileşimli kılavuzları ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Yonyx etkileşimli Kılavuzlar arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: 1f337f453a5721792527d5494e8faf9ba47437f7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050638"
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Öğretici: Azure Active Directory tümleştirmesiyle Yonyx etkileşimli kılavuzları

Bu öğreticide, Azure Active Directory (Azure AD) ile Yonyx etkileşimli kılavuzları tümleştirme konusunda bilgi edinin.

Azure AD ile Yonyx etkileşimli kılavuzları tümleştirme ile aşağıdaki avantajları sağlar:

- Yonyx etkileşimli kılavuzları erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Yonyx etkileşimli kılavuzları (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile etkileşimli kılavuzları Yonyx yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Yonyx etkileşimli kılavuzları çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Yonyx etkileşimli kılavuzlarına ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Galeriden Yonyx etkileşimli kılavuzlarına ekleme
Azure AD'de Yonyx etkileşimli kılavuzları tümleştirmesini yapılandırmak için Yonyx etkileşimli kılavuzları Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Yonyx etkileşimli kılavuzları eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Yonyx etkileşimli kılavuzları**seçin **Yonyx etkileşimli kılavuzları** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Yonyx etkileşimli kılavuzları](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Yonyx etkileşimli "Britta Simon" adlı bir test kullanıcı tabanlı kılavuzları test.

Tek iş için oturum açma için Azure AD ne Yonyx etkileşimli kılavuzlarındaki karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Yonyx etkileşimli kılavuzlarındaki arasında bir bağlantı ilişki kurulması gerekir.

Değerini Yonyx etkileşimli yönergelerinde atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Yonyx etkileşimli kılavuzları ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yonyx etkileşimli kılavuzları test kullanıcısı oluşturma](#create-a-yonyx-interactive-guides-test-user)**  - kullanıcı Azure AD gösterimini bağlı Yonyx etkileşimli kılavuzlarındaki Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Yonyx etkileşimli kılavuzları uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Yonyx etkileşimli kılavuzları ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Yonyx etkileşimli kılavuzları** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. Üzerinde **Yonyx etkileşimli kılavuzları etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yonyx etkileşimli kılavuzları etki alanı ve URL'ler tek oturum açma bilgileri](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<company name>.yonyx.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Yonyx etkileşimli kılavuzları istemci Destek ekibine](mailto:support@yonyx.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/yonyx-tutorial/tutorial_general_400.png)

6. Üzerinde **Yonyx etkileşimli kılavuzları yapılandırma** bölümünde **yapılandırma Yonyx etkileşimli kılavuzları** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Yonyx etkileşimli kılavuzları yapılandırma](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. Çoklu oturum açmayı yapılandırma **Yonyx etkileşimli kılavuzları** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64)**, **oturum kapatma URL'si**, **SAML çoklu Oturum açma hizmeti URL'si** **SAML varlık kimliği** için [Yonyx etkileşimli kılavuzları Destek ekibine](mailto:support@yonyx.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

  ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](./media/yonyx-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/yonyx-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Ekle düğmesi](./media/yonyx-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/yonyx-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a>Yonyx etkileşimli kılavuzları test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Yonyx etkileşimli yönergelerinde adlı bir kullanıcı oluşturmaktır. Yonyx etkileşimli kılavuzları tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Yonyx etkileşimli kılavuzları erişme denemesi sırasında oluşturulur.

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, Yonyx etkileşimli kılavuzları destek ekibi ile iletişime geçmeniz <mailto:support@yonyx.com>. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Yonyx etkileşimli kılavuzlara erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Yonyx etkileşimli kılavuzlara atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Yonyx etkileşimli kılavuzları**.

    ![Uygulamalar listesinde Yonyx etkileşimli kılavuzları bağlantı](./media/yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Yonyx etkileşimli kılavuzları kutucuğa tıkladığınızda, otomatik olarak Yonyx etkileşimli kılavuzları uygulamanıza açan.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/yonyx-tutorial/tutorial_general_01.png
[2]: ./media/yonyx-tutorial/tutorial_general_02.png
[3]: ./media/yonyx-tutorial/tutorial_general_03.png
[4]: ./media/yonyx-tutorial/tutorial_general_04.png

[100]: ./media/yonyx-tutorial/tutorial_general_100.png

[200]: ./media/yonyx-tutorial/tutorial_general_200.png
[201]: ./media/yonyx-tutorial/tutorial_general_201.png
[202]: ./media/yonyx-tutorial/tutorial_general_202.png
[203]: ./media/yonyx-tutorial/tutorial_general_203.png

