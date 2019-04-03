---
title: 'Öğretici: Tableau Online ile Azure Active Directory Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Tableau Online arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd5e3087c21908600be9cd369a15f3036e5acb2f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884712"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Öğretici: Tableau Online ile Azure Active Directory Tümleştirmesi

Bu öğreticide, Tableau çevrimiçi Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Tableau çevrimiçi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tableau Online'a erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Tableau Online'a (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Tableau Online ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Tableau çevrimiçi çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Tableau çevrimiçi galeriden ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau çevrimiçi galeriden ekleme
Azure AD'de Tableau Online'nın tümleştirmesini yapılandırmak için Tableau çevrimiçi galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Tableau çevrimiçi Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Tableau çevrimiçi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_search.png)

1. Sonuçlar panelinde seçin **Tableau çevrimiçi**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı çevrimiçi Tableau ile test etme

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı Tableau çevrimiçi bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Tableau Online arasında bir bağlantı ilişki kurulması gerekir.

Tableau Online'da değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Tableau Online ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Tableau çevrimiçi bir test kullanıcısı oluşturma](#creating-a-tableau-online-test-user)**  - Tableau kullanıcı Azure AD gösterimini bağlı olan çevrimiçi bir karşılığı Britta simon'un sağlamak için.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Tableau çevrimiçi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Tableau Online ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Tableau çevrimiçi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

1. Üzerinde **Tableau çevrimiçi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://sso.online.tableau.com`

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_general_400.png)

1. Farklı bir tarayıcı penceresinde, Tableau çevrimiçi uygulamanıza oturum. Git **ayarları** ardından **kimlik doğrulaması**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)
    
1. SAML, altında etkinleştirmek için **kimlik doğrulama türleri** bölümü. Denetleme **çoklu oturum açma SAML ile** onay kutusu.
   
    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

1. Kadar aşağı kaydırın **alma meta veri dosyası içine Tableau çevrimiçi** bölümü.  Gözat'a tıklayın ve Azure AD'den yüklediğiniz meta veri dosyası içeri aktarın. ' A tıklayarak **Uygula**.
   
   ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

1. İçinde **eşleşen onaylar** bölümünde, eklemek için karşılık gelen kimlik sağlayıcı onaylama adı **e-posta adresi**, **ad**, ve **Soyadı**. Azure AD'den bu bilgileri almak için: 
  
    a. Azure portalında, go **Tableau çevrimiçi** uygulama tümleştirme sayfası.
    
    b. Öznitelikleri bölümünde **"görüntüleyin ve diğer tüm kullanıcı özniteliklerini düzenleyin"** onay kutusu. 
    
   ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/attributesection.png)
      
    c. Bu öznitelikleri ad alanı değerini kopyalayın: givenname, e-posta ve aşağıdaki adımları kullanarak Soyadı:

   ![Azure AD çoklu oturum açma](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Tıklayın **user.givenname** değeri 
    
    e. Değeri Şuradan Kopyala: **ad alanı** metin.

   ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/attributesection2.png)

    f. Ad alanı kopyalamak için değerler soyadı ve e-posta için yukarıdaki adımları izleyin.

    g. Tableau çevrimiçi uygulamaya geçiş yapın ve ardından ayarlamak **Tableau çevrimiçi öznitelikleri** gibi bölümünde:
     * E-posta: **posta** veya **userprincipalname**
     * Ad: **givenname**
     * Soyadı: **Soyadı**
   
   ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-tableau-online-test-user"></a>Tableau çevrimiçi bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon Tableau Online'da adlı bir kullanıcı oluşturun.

1. Üzerinde **Tableau çevrimiçi**, tıklayın **ayarları** ardından **kimlik doğrulaması** bölümü. Ekranı aşağı kaydırarak **kullanıcıların** bölümü. Tıklayın **kullanıcı ekleme** ardından **e-posta adreslerini girin**.
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)
1. Seçin **çoklu oturum açma (SSO) kimlik doğrulaması için kullanıcı ekleme**. İçinde **e-posta adreslerini girin** metin kutusu ekleme britta.simon@contoso.com
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)
1. **Oluştur**’a tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Tableau çevrimiçi erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Tableau çevrimiçi olarak atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Tableau çevrimiçi**.

    ![Çoklu oturum açmayı yapılandırın](./media/tableauonline-tutorial/tutorial_tableauonline_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim paneli Tableau çevrimiçi kutucuğa tıkladığınızda, size otomatik olarak Tableau çevrimiçi uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/tableauonline-tutorial/tutorial_general_203.png
