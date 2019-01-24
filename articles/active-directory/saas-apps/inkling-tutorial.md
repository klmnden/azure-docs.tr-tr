---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Inkling | Microsoft Docs'
description: Azure Active Directory ve Inkling arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 31e00c5810e33433d58820f4db300b935ad7baaf
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811290"
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a>Öğretici: Inkling ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Inkling tümleştirme konusunda bilgi edinin.

Azure AD ile Inkling tümleştirme ile aşağıdaki avantajları sağlar:

- Inkling erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Inkling (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Inkling yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Inkling çoklu oturum açma etkin aboneliği


> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.


Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Inkling ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma


## <a name="adding-inkling-from-the-gallery"></a>Galeriden Inkling ekleme
Azure AD'de Inkling tümleştirmesini yapılandırmak için Inkling Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Inkling eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Inkling**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/tutorial_inkling_001.png)

1. Sonuçlar panelinde seçin **Inkling**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Inkling sınayın.

Tek iş için oturum açma için Azure AD ne Inkling karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Inkling ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** Inkling içinde.

Yapılandırma ve Azure AD çoklu oturum açma Inkling ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Inkling test kullanıcısı oluşturma](#creating-an-inkling-test-user)**  - Azure AD gösterimini her için bağlı Inkling Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve Inkling uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Inkling yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Inkling** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_300.png)
    
1. Üzerinde **Inkling etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_01.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://api.inkling.com/saml/v2/metadata/<user-id>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://api.inkling.com/saml/v2/acs/<user-id>`

    > [!NOTE] 
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek tanımlayıcısı ve yanıt URL'sini güncelleştirmeniz gerekiyor. İlgili kişi [Inkling Destek ekibine](mailto:press@inkling.com) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_400.png)  

1. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayıp bir **sona erme tarihi**. Ardından **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_500.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünden **yeni sertifikayı etkin hale getirin** tıklatıp **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_02.png)

1. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklayın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_general_600.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_03.png) 

1. Uygulamanız için yapılandırılmış SSO almak için iletişime geçin [Inkling Destek ekibine](mailto:press@inkling.com) ve sağlayacağını ile indirilen **meta verileri**. 


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/inkling-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 



### <a name="creating-an-inkling-test-user"></a>Bir Inkling test kullanıcısı oluşturma

Bu bölümde, Britta Simon Inkling içinde adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Inkling Destek ekibine](mailto:press@inkling.com) Inkling platform kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Inkling erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Inkling için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Inkling**.

    ![Çoklu oturum açmayı yapılandırın](./media/inkling-tutorial/tutorial_inkling_50.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Inkling kutucuğa tıkladığınızda, otomatik olarak Inkling uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/inkling-tutorial/tutorial_general_01.png
[2]: ./media/inkling-tutorial/tutorial_general_02.png
[3]: ./media/inkling-tutorial/tutorial_general_03.png
[4]: ./media/inkling-tutorial/tutorial_general_04.png

[100]: ./media/inkling-tutorial/tutorial_general_100.png

[200]: ./media/inkling-tutorial/tutorial_general_200.png
[201]: ./media/inkling-tutorial/tutorial_general_201.png
[202]: ./media/inkling-tutorial/tutorial_general_202.png
[203]: ./media/inkling-tutorial/tutorial_general_203.png