---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle IdeaScale | Microsoft Docs'
description: Azure Active Directory ve IdeaScale arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ecb73e4b520936b573254f2cf209d4a02c0fdd32
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848757"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Öğretici: Azure Active Directory IdeaScale ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile IdeaScale tümleştirme konusunda bilgi edinin.

Azure AD ile IdeaScale tümleştirme ile aşağıdaki avantajları sağlar:

- IdeaScale erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için IdeaScale (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile IdeaScale yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir IdeaScale çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden IdeaScale ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ideascale-from-the-gallery"></a>Galeriden IdeaScale ekleme
Azure AD'de IdeaScale tümleştirmesini yapılandırmak için IdeaScale Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden IdeaScale eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **IdeaScale**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ideascale-tutorial/tutorial_ideascale_search.png)

1. Sonuçlar panelinde seçin **IdeaScale**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı IdeaScale ile test etme

Tek iş için oturum açma için Azure AD ne IdeaScale karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının IdeaScale ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

IdeaScale içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma IdeaScale ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir IdeaScale test kullanıcısı oluşturma](#creating-an-ideascale-test-user)**  - kullanıcı Azure AD gösterimini bağlı IdeaScale Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve IdeaScale uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile IdeaScale yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **IdeaScale** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/ideascale-tutorial/tutorial_ideascale_samlbase.png)

1. Üzerinde **IdeaScale etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/ideascale-tutorial/tutorial_ideascale_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.ideascale.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [IdeaScale istemci Destek ekibine](https://support.ideascale.com/) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/ideascale-tutorial/tutorial_ideascale_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/ideascale-tutorial/tutorial_general_400.png)

1. Üzerinde **IdeaScale yapılandırma** bölümünde **yapılandırma IdeaScale** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML varlık kimliği** gelen **hızlı başvuru bölümüne**.

    ![Çoklu oturum açmayı yapılandırın](./media/ideascale-tutorial/tutorial_ideascale_configure.png) 

1. Farklı bir web tarayıcı penceresinde IdeaScale şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **topluluk ayarları**.
   
    ![Topluluk ayarları](./media/ideascale-tutorial/ic790847.png "topluluk ayarları")

1. Git **güvenlik \> çoklu oturum açma ayarları**.
   
    ![Çoklu oturum açma ayarları](./media/ideascale-tutorial/ic790848.png "çoklu oturum açma ayarları")

1. Olarak **çoklu oturum açma türü**seçin **SAML 2.0**.
   
    ![Çoklu oturum açma türü](./media/ideascale-tutorial/ic790849.png "çoklu oturum açma türü")

1. Üzerinde **çoklu oturum açma ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma ayarları](./media/ideascale-tutorial/ic790850.png "çoklu oturum açma ayarları")
   
    a. İçinde **SAML IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    b. Azure Portalı'ndan indirilen meta veri dosyasının içeriği kopyalayın ve yapıştırın **SAML IDP meta verileri** metin.

    c. İçinde **oturum kapatma başarı URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/ideascale-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ideascale-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ideascale-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/ideascale-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-an-ideascale-test-user"></a>Bir IdeaScale test kullanıcısı oluşturma

Azure AD kullanıcılarının IdeaScale oturum etkinleştirmek için bunlar içinde IdeaScale için sağlanması gerekir. IdeaScale söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **IdeaScale** şirketinizin sitesi yöneticisi olarak.

1. Git **topluluk ayarları**.
   
    ![Topluluk ayarları](./media/ideascale-tutorial/ic790847.png "topluluk ayarları")

1. Git **temel ayarları \> üye yönetim**.

1. Tıklayın **üye ekleme**.
   
    ![Üye yönetim](./media/ideascale-tutorial/ic790852.png "üye yönetim")

1. Yeni Üye Ekle bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Yeni üye ekler](./media/ideascale-tutorial/ic790853.png "yeni üye ekler")
   
    a. İçinde **e-posta adreslerini** metin kutusuna geçerli bir AAD hesabı sağlamak istediğiniz e-posta adresini yazın.
   
    b. Tıklayın **değişiklikleri kaydetmek**. 
   
    >[!NOTE]
    >Azure Active Directory hesap sahibi bu etkinleştirilmeden önce hesabı onaylamak için bir bağlantı içeren bir e-posta alır.
      
>[!NOTE]
>Herhangi diğer IdeaScale kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak IdeaScale tarafından sağlanan.
 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için IdeaScale erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon IdeaScale için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **IdeaScale**.

    ![Çoklu oturum açmayı yapılandırın](./media/ideascale-tutorial/tutorial_ideascale_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi


Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde IdeaScale kutucuğa tıkladığınızda, otomatik olarak IdeaScale uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ideascale-tutorial/tutorial_general_01.png
[2]: ./media/ideascale-tutorial/tutorial_general_02.png
[3]: ./media/ideascale-tutorial/tutorial_general_03.png
[4]: ./media/ideascale-tutorial/tutorial_general_04.png

[100]: ./media/ideascale-tutorial/tutorial_general_100.png

[200]: ./media/ideascale-tutorial/tutorial_general_200.png
[201]: ./media/ideascale-tutorial/tutorial_general_201.png
[202]: ./media/ideascale-tutorial/tutorial_general_202.png
[203]: ./media/ideascale-tutorial/tutorial_general_203.png

