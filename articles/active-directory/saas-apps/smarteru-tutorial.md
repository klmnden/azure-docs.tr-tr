---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle SmarterU | Microsoft Docs'
description: Azure Active Directory ve SmarterU arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: e07ed8f131697d6340b899ef76c988aed215556b
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283420"
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Öğretici: Azure Active Directory SmarterU ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile SmarterU tümleştirme konusunda bilgi edinin.

Azure AD ile SmarterU tümleştirme ile aşağıdaki avantajları sağlar:

- SmarterU erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için SmarterU (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile SmarterU yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik SmarterU çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden SmarterU ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-smarteru-from-the-gallery"></a>Galeriden SmarterU ekleme
Azure AD'de SmarterU tümleştirmesini yapılandırmak için SmarterU Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SmarterU eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **SmarterU**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smarteru-tutorial/tutorial_smarteru_search.png)

5. Sonuçlar panelinde seçin **SmarterU**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı SmarterU ile test etme

Tek iş için oturum açma için Azure AD ne SmarterU karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının SmarterU ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

SmarterU içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma SmarterU ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[SmarterU test kullanıcısı oluşturma](#creating-a-smarteru-test-user)**  - kullanıcı Azure AD gösterimini bağlı SmarterU Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve SmarterU uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile SmarterU yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **SmarterU** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. Üzerinde **SmarterU etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin: 

    ![Çoklu oturum açmayı yapılandırın](./media/smarteru-tutorial/tutorial_smarteru_url.png)

    İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://www.smarteru.com/`

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/smarteru-tutorial/tutorial_general_400.png)

6. Farklı bir web tarayıcı penceresinde SmarterU şirketinizin sitesi için bir yönetici olarak oturum açın.

7. Üst araç çubuğunda tıklatın **hesap ayarları**.
   
    ![Hesap ayarları](./media/smarteru-tutorial/accountsettings.png)

8. Hesabı yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Dış yetkilendirme](./media/smarteru-tutorial/externalauthorizationconfiguration.png) 
 
      a. Seçin **etkinleştirme dış yetkilendirme**.
  
      b. İçinde **asıl oturum açma denetimi** bölümünden **SmarterU** sekmesi.
  
      c. İçinde **kullanıcı varsayılan oturum açma** bölümünden **SmarterU** sekmesi.
  
      d. Seçin **etkinleştirme SAML**.
  
      e. İndirilen meta veri dosyasının içeriğini kopyalayın ve ardından yapıştırın **IDP meta verileri** metin.
      
      f. Seçin bir **tanımlayıcı öznitelik/talep**.
  
      g. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smarteru-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smarteru-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smarteru-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/smarteru-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-smarteru-test-user"></a>SmarterU test kullanıcısı oluşturma

SmarterU için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların SmarterU sağlanması gerekir.

SmarterU, sağlama elle bir görevin olduğunda.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **SmarterU** Kiracı.

2. Git **kullanıcılar**.

3. Kullanıcı bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Yeni Kullanıcı](./media/smarteru-tutorial/adduser.png)  

    a. Tıklayın **+ kullanıcı**.
    
    b. Azure AD kullanıcı hesabıyla ilişkili öznitelik değerlerini aşağıdaki metin kutularına yazın: **birincil e-posta**, **çalışan kimliği**, **parola**, **doğrulayın Parola**, **verilen ad**, **Soyadı**.
    
    c. Tıklayın **etkin**. 
    
    d. **Kaydet**’e tıklayın.

>[!NOTE]
>Herhangi diğer SmarterU kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak SmarterU tarafından sağlanan.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için SmarterU erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon SmarterU için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **SmarterU**.

    ![Çoklu oturum açmayı yapılandırın](./media/smarteru-tutorial/tutorial_smarteru_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
 
Erişim panelinde SmarterU kutucuğa tıkladığınızda, otomatik olarak SmarterU uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/smarteru-tutorial/tutorial_general_01.png
[2]: ./media/smarteru-tutorial/tutorial_general_02.png
[3]: ./media/smarteru-tutorial/tutorial_general_03.png
[4]: ./media/smarteru-tutorial/tutorial_general_04.png

[100]: ./media/smarteru-tutorial/tutorial_general_100.png

[200]: ./media/smarteru-tutorial/tutorial_general_200.png
[201]: ./media/smarteru-tutorial/tutorial_general_201.png
[202]: ./media/smarteru-tutorial/tutorial_general_202.png
[203]: ./media/smarteru-tutorial/tutorial_general_203.png

