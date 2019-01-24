---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle halojensiz yazılım | Microsoft Docs'
description: Azure Active Directory ve halojensiz yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8220f94be58cd5602ffcb814d8b24db4c19e1f41
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827984"
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Öğretici: Halojensiz yazılım ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile halojensiz yazılım tümleştirme konusunda bilgi edinin.

Halojensiz yazılım Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Halojensiz yazılım erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan halojensiz yazılımı (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi halojensiz yazılımıyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik halojensiz yazılım çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden halojensiz yazılım ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-halogen-software-from-the-gallery"></a>Galeriden halojensiz yazılım ekleme

Azure AD'de halojensiz yazılım tümleştirmesini yapılandırmak için halojensiz yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden halojensiz yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **halojensiz yazılım**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/halogen-software-tutorial/tutorial_halogensoftware_search.png)

1. Sonuçlar panelinde seçin **halojensiz yazılım**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma halojensiz yazılım'ın "Britta Simon" adlı bir test kullanıcı tabanlı test.

Tek iş için oturum açma için Azure AD ne halojensiz yazılım karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının halojensiz yazılımla ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Halojensiz yazılımda değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma halojensiz yazılım ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Halojensiz yazılım test kullanıcısı oluşturma](#creating-a-halogen-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı halojensiz yazılım Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve halojensiz yazılım uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma halojensiz yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **halojensiz yazılım** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

1. Üzerinde **halojensiz yazılım etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://global.hgncloud.com/<companyname>`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [halojensiz yazılım istemcisi Destek ekibine](https://support.halogensoftware.com/) bu değerleri almak için. 
 


1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/halogen-software-tutorial/tutorial_general_400.png)

1. Bir farklı bir tarayıcı penceresinde için oturum açma, **halojensiz yazılım** uygulamasını yönetici olarak.

1. Tıklayın **seçenekleri** sekmesi. 
   
    ![Azure AD Connect nedir?][12]

1. Sol gezinti bölmesinden **SAML yapılandırma**. 
   
    ![Azure AD Connect nedir?][13]

1. Üzerinde **SAML yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin: 

    ![Azure AD Connect nedir?][14]

     a. Olarak **benzersiz tanımlayıcı**seçin **Nameıd**.

     b. Olarak **benzersiz tanımlayıcı eşlemeleri için**seçin **Username**.
  
     c. İndirilen meta veri dosyası karşıya yüklemek için tıklayın **Gözat** dosyasını seçin ve ardından **dosyasını karşıya yükle**.
 
     d. Test yapılandırması için Yardım düğmesini tıklatın **Test çalıştırması**. 
    
    >[!NOTE]
    >Öğesinin ileti beklemesi gerekiyor "*SAML sınav tamamlanana. Lütfen bu pencereyi kapatın*". Ardından, açılan tarayıcı penceresini kapatın. **Etkinleştirme SAML** onay kutusu yalnızca etkin olmadığını test tamamlandı. 
     
     e. Seçin **etkinleştirme SAML**.
    
     f. Tıklayın **değişiklikleri kaydetmek**. 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/halogen-software-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/halogen-software-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/halogen-software-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/halogen-software-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin türü adı olarak **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-halogen-software-test-user"></a>Halojensiz yazılım test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon halojensiz yazılım adlı bir kullanıcı oluşturmaktır.

**Britta Simon halojensiz yazılım adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **halojensiz yazılım** uygulamasını yönetici olarak.

1. Tıklayın **kullanıcı Merkezi** sekmesine ve ardından **Create User**.
   
    ![Azure AD Connect nedir?][300]  

1. Üzerinde **yeni kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Azure AD Connect nedir?][301]

    a. İçinde **ad** metin adı gibi kullanıcı türü **Britta**.
    
    b. İçinde **Soyadı** metin türü Soyadı gibi kullanıcının **Simon**. 

    c. İçinde **kullanıcıadı** metin kutusuna **Britta Simon**, Azure portalında olduğu gibi kullanıcı adı.

    d. İçinde **parola** metin Britta parolasını yazın.
    
    e. **Kaydet**’e tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma halojensiz yazılıma erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon halojensiz yazılımı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **halojensiz yazılım**.

    ![Çoklu oturum açmayı yapılandırın](./media/halogen-software-tutorial/tutorial_halogensoftware_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde halojensiz yazılım kutucuğa tıkladığınızda, otomatik olarak halojensiz yazılım uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/halogen-software-tutorial/tutorial_halogen_301.png
