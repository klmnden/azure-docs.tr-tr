---
title: 'Öğretici: TINFOIL SECURITY ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: TINFOIL SECURITY ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: ae136c9aa0e21b2a5dabf06b050727871bddf03b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191984"
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Öğretici: TINFOIL SECURITY ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile TINFOIL SECURITY tümleştirme konusunda bilgi edinin.

TINFOIL SECURITY Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- TINFOIL SECURITY erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için TINFOIL SECURITY açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

TINFOIL SECURITY ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- TINFOIL SECURITY çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. TINFOIL SECURITY Galeriden Ekle
1. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-tinfoil-security-from-the-gallery"></a>TINFOIL SECURITY Galeriden Ekle
Azure AD'de TINFOIL SECURITY tümleştirmesini yapılandırmak için TINFOIL SECURITY Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**TINFOIL SECURITY Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **TINFOIL SECURITY**seçin **TINFOIL SECURITY** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![TINFOIL SECURITY Galerisi](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı TINFOIL SECURITY ile test edin.

Tek iş için oturum açma için Azure AD ne TINFOIL SECURITY karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı TINFOIL Security arasında bir bağlantı ilişki kurulması gerekir.

TINFOIL güvenlik değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma TINFOIL SECURITY ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[TINFOIL SECURITY test kullanıcısı oluşturma](#create-a-tinfoil-security-test-user)**  - kullanıcı Azure AD gösterimini bağlı TINFOIL Security Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve TINFOIL SECURITY uygulamanızda çoklu oturum açmayı yapılandırın.

**TINFOIL SECURITY ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TINFOIL SECURITY** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Oturum açma SAML tabanlı](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

1. Üzerinde **TINFOIL güvenlik etki alanı ve URL'ler** bölümü, kullanıcı gerekmez uygulama zaten Azure ile önceden tümleştirilmiştir gibi tüm adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırın](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** değeri.

    ![SAML imzalama sertifikası bölümü](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

1. Gerekli öznitelik eşlemelerini eklemek için aşağıdaki adımları gerçekleştirin:
    
    ![Öznitelikleri](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "öznitelikleri")
    
    | Öznitelik Adı    |   Öznitelik Değeri |
    | ------------------- | -------------------- |
    | Hesap Kimliği | UXXXXXXXXXXXXX |
    
    a. Tıklayın **kullanıcı özniteliğini eklemek**.
    
    ![Öznitelik Ekle](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "öznitelikleri")
    
    ![Öznitelik Ekle](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "öznitelikleri")
    
    b. İçinde **öznitelik adı** metin kutusuna **AccountID**.
    
    c. İçinde **öznitelik değeri** metin kutusuna, daha sonra öğreticide alacak Yapıştır hesap kimliği değeri.
    
    d. **Tamam**’a tıklayın.    

1. Tıklayın **Kaydet** düğmesi.

    ![Kaydet düğmesi](./media/tinfoil-security-tutorial/tutorial_general_400.png)

1. Üzerinde **TINFOIL Güvenlik Yapılandırması** bölümünde **TINFOIL Güvenlik Yapılandırması** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![TINFOIL güvenlik yapılandırması](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

1. Farklı bir web tarayıcı penceresinde TINFOIL SECURITY şirket sitenize yönetici olarak oturum.

1. Üst araç çubuğunda tıklatın **hesabım**.
   
    ![Pano](./media/tinfoil-security-tutorial/ic798971.png "Panosu")

1. Tıklayın **güvenlik**.
   
    ![Güvenlik](./media/tinfoil-security-tutorial/ic798972.png "güvenlik")

1. Üzerinde **çoklu oturum açma** yapılandırma sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/tinfoil-security-tutorial/ic798973.png "çoklu oturum açma")
   
    a. Seçin **etkinleştirme SAML**.
   
    b. Tıklayın **el ile yapılandırma**.
   
    c. İçinde **SAML gönderme URL'sini** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız
   
    d. İçinde **SAML sertifikası parmak izi** metin değerini yapıştırın **parmak izi** kopyalanan **SAML imzalama sertifikası** bölümü.
  
    e. Kopyalama **bilgisayarınızı hesap kimliği** değeri ve değer yapıştırın **öznitelik değeri** metin kutusunun altında **öznitelik Ekle** bölümü Azure Portalı'nda.
   
    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/tinfoil-security-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Tüm kullanıcılar, kullanıcılar ve Gruplar -> ](./media/tinfoil-security-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Kullanıcı](./media/tinfoil-security-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/tinfoil-security-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-tinfoil-security-test-user"></a>TINFOIL SECURITY test kullanıcısı oluşturma

TINFOIL SECURİTY'de oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların TINFOIL SECURITY sağlanması gerekir. TINFOIL SECURITY söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Sağlanan kullanıcı almak için aşağıdaki adımları gerçekleştirin:**

1. Kullanıcı bir kurumsal hesap parçasıysa yapmanız [TINFOIL SECURITY Destek ekibine başvurun](https://www.tinfoilsecurity.com/contact) oluşturduğunuz kullanıcı hesabı.

1. TINFOIL SECURITY SaaS normal bir kullanıcı kullanıcı ise daha sonra kullanıcı ortak çalışan herhangi bir kullanıcının siteler ekleyebilirsiniz. Bu, yeni bir TINFOIL SECURITY kullanıcı hesabı oluşturmak için bir davet belirtilen e-posta göndermek için bir işlemi tetikler.

> [!NOTE]
> Azure AD kullanıcı hesapları sağlamak için herhangi bir TINFOIL SECURITY kullanıcı hesabı oluşturma araçları veya TINFOIL SECURITY tarafından sağlanan API'leri kullanabilirsiniz.
> 
> 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için TINFOIL SECURITY erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**TINFOIL SECURITY Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **TINFOIL SECURITY**.

    ![TINFOIL SECURITY seçin](./media/tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli TINFOIL SECURITY kutucuğa tıkladığınızda, size otomatik olarak TINFOIL SECURITY uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/tinfoil-security-tutorial/tutorial_general_203.png

