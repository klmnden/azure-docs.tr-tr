---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim (ZPA) | Microsoft Docs'
description: Zscaler özel erişim (ZPA) ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b6471b6bd634c7fe3053fc7748eb925c049c17c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56179575"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Zscaler özel erişim (ZPA)

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler özel erişim (ZPA) tümleştirme konusunda bilgi edinin.

Zscaler özel erişim (ZPA) Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi için Zscaler özel erişim (ZPA) olan Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Zscaler özel erişim (ZPA için) (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Bir merkezi konumda - Azure Yönetim Portalı hesaplarınızı yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Zscaler özel erişim (ZPA ile) yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Zscaler özel erişim (ZPA) çoklu oturum açma etkin aboneliği


> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.


Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Bu gerekli olmadığı sürece üretim ortamınızı kullanmamanız gerekir.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Zscaler özel erişim (ZPA) ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Galeriden Zscaler özel erişim (ZPA) ekleme
Azure AD'de, Zscaler özel erişim (ZPA) tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden Zscaler özel erişim (ZPA) eklemeniz gerekir.

**Galeriden Zscaler özel erişim (ZPA) eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure Yönetim Portalı](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Tıklayın **Ekle** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Zscaler özel erişim (ZPA)**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

1. Sonuçlar panelinde seçin **Zscaler özel erişim (ZPA)** ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma ile Zscaler özel erişim ("Britta Simon" adlı bir test kullanıcı tabanlı ZPA) test edin.

Tek iş için oturum açma için Azure AD ne de Zscaler özel erişim (ZPA) karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı içinde Zscaler özel erişim (ZPA) arasında bir bağlantı ilişki kurulması gerekir.

Değerini atayarak bu bağlantı ilişki kurulduktan **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** içinde Zscaler özel erişim (ZPA).

Yapılandırma ve Azure AD çoklu oturum açma ile Zscaler özel erişim (ZPA) test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **Zscaler özel erişim (ZPA) test kullanıcısı oluşturma** - içinde Zscaler özel erişim (Azure AD gösterimini her için bağlı ZPA) Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure yönetim portalında etkinleştirin ve Zscaler özel erişim (ZPA) uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Zscaler özel erişim (ZPA ile) yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda üzerinde **Zscaler özel erişim (ZPA)** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda olarak **modu** seçin **SAML tabanlı oturum açma** için çoklu oturum açmayı etkinleştirme.
 
    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
1. Üzerinde **Zscaler özel erişim (ZPA) etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:
    
    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. İçinde **işareti bulunan URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. İçinde **tanımlayıcı** metin kutusuna: `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Bunlar gerçek değerleri olmadığına dikkat edin. Bu değerler gerçek oturum üzerinde URL'si ve tanımlayıcı güncelleştirmeniz gerekiyor. Burada URL benzersiz değeri tanımlayıcıda kullanmanızı öneririz. İlgili kişi [Zscaler özel erişim (ZPA) destek ekibi](https://help.zscaler.com/zpa-submit-ticket) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **yeni sertifika oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_general_400.png)     

1. Üzerinde **yeni sertifika oluştur** iletişim kutusunda, Takvim simgesine tıklayıp bir **sona erme tarihi**. Ardından **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_general_500.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünden **yeni sertifikayı etkin hale getirin** tıklatıp **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

1. Açılır pencere üzerinde **geçiş sertifikası** penceresinde tıklayın **Tamam**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_general_600.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

1. Farklı bir web tarayıcı penceresinde Zscaler özel erişim (ZPA) şirket sitenize yönetici olarak oturum.

1. Gidin **yönetici** ve ardından **IDP yapılandırmasını**.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

1. İçinde **IDP yapılandırmasını** bölümünde **yeni IDP Yapılandırması Ekle**.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

1. İçinde **yeni IDP yapılandırmasını** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Tıklayın **Dosya Seç** ve indirilen meta verileri dosyanızı karşıya yükleyin.

    b. Tıklayın **Kaydet** düğmesi.
    


### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, bir test kullanıcısı Britta Simon adlı Azure Yönetim Portalı'nda oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure Yönetim Portalı**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscalerprivateaccess-tutorial/create_aaduser_01.png) 

1. Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar** kullanıcılar listesini görüntüleyin.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscalerprivateaccess-tutorial/create_aaduser_02.png) 

1. İletişim kutusunun en üstünde tıklayın **Ekle** açmak için **kullanıcı** iletişim.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscalerprivateaccess-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Zscaler özel erişim (ZPA) test kullanıcısı oluşturma

Bu bölümde, Britta Simon Zscaler özel erişim (ZPA içinde) adlı bir kullanıcı oluşturun. Lütfen birlikte çalışarak [Zscaler özel erişim (ZPA) destek ekibi](https://help.zscaler.com/zpa-submit-ticket) Zscaler özel erişim (ZPA) platform kullanıcıları eklemek için.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Zscaler özel erişim (ZPA) erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Zscaler özel erişim (ZPA) atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure Yönetim Portalı'nda uygulamaları görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Zscaler özel erişim (ZPA)**.

    ![Çoklu oturum açmayı yapılandırın](./media/zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    


### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Zscaler özel erişim (ZPA) kutucuğa tıkladığınızda, otomatik olarak Zscaler özel erişim (ZPA) uygulamanıza açan.


## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/zscalerprivateaccess-tutorial/tutorial_general_203.png
