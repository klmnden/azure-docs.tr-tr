---
title: 'Öğretici: T & E Express ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve T & E Express arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 77e9bc8be6b85cdd49a3ca675c360f868f582f6e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155417"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Öğretici: T & E Express ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile T & E Express tümleştirme konusunda bilgi edinin.

T & E Express, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- T & E Express erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanmış-T & E Express (çoklu oturum açma) ile kendi Azure AD hesapları kullanıcılarınızın etkinleştirebilirsiniz.
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

T & E Express ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir T & E Express çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. T & E Express galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-te-express-from-the-gallery"></a>T & E Express galeri ekleme
Azure AD'de T & E Express tümleştirmesini yapılandırmak için T & E Express Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**T & E Express Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **T & E Express**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tyeexpress-tutorial/tutorial_tyeexpress_search.png)

1. Sonuçlar panelinde seçin **T & E Express**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma T & E "Britta Simon" adlı bir test kullanıcı tabanlı Express ile test edin.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcının T & E Express bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcının T & E Express arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** T & E Express.

Yapılandırma ve Azure AD çoklu oturum açma T & E Express ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[T & E Express bir test kullanıcısı oluşturma](#creating-a-te-express-test-user)**  - T & E Azure AD'ye gösterimini her için bağlı olan Express Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve T & E Express uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma T & E Express ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **T & E Express** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

1. Üzerinde **T & E Express etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    a. İçinde **tanımlayıcı** metin değeri olarak yazın: `https://<domain>.tyeexpress.com`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE] 
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı ve yanıt URL'sini güncelleştirmeniz gerekiyor. Burada benzersiz dize değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [T & E Express Destek ekibine](http://www.tyeexpress.com/contacto.aspx) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda XML dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **T & E Express** tarafı, oturum açma T & E express SAML çoklu oturum açma olmadan uygulama yönetici kimlik bilgilerini kullanarak.

1. Altında **yönetici** sekmesinde, üzerinde **SAML etki alanı** SAML ayarlar sayfasını açın.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tye-SAML.png)

1. Seçin **Activar(Activate)** seçeneğini **Hayır** için **SI(Yes)**. İçinde **kimlik sağlayıcısı meta verileri** metin kutusu, sahip olduğunuz bir XML meta veri yapıştırma Azure portalından indirildi.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tyeAdmin.png)

1. Tıklayarak **Guardar(Save)** düğmesini kullanarak ayarları kaydedin.  


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tyeexpress-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tyeexpress-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tyeexpress-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tyeexpress-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-te-express-test-user"></a>T & E Express bir test kullanıcısı oluşturma

T & E Express açarken Azure AD kullanıcılarının etkinleştirmek için bunlar T & E Express sağlanması gerekir.  
T & E Express olması durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:**

1. T & E Express şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Yönetici etiketi altında kullanıcıları kullanıcılar ana sayfasını açmak için tıklayın.

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-adminusers.png)

1. Giriş sayfasında tıklayarak **+** kullanıcıları eklemek için.

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-usershome.png)

1. Ayrıntılarını kaydetmek için Kaydet düğmesine tıklayın ve form içinde sorulan zorunlu olan tüm ayrıntıları girin.

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![Çalışan Ekle](./media/tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için T & E Express erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon T & E Express atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **T & E Express**.

    ![Çoklu oturum açmayı yapılandırın](./media/tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde T & E Express kutucuğa tıkladığınızda, otomatik olarak T & E Express uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/tyeexpress-tutorial/tutorial_general_203.png

