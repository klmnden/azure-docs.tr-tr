---
title: 'Öğretici: Azure Active Directory Yardi e-ÖĞRENİM ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Yardi öğrenim arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7ea58b54-ec5b-4576-8586-814b11d0f4fb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 9a6bbb716957621daf4667a7e819c31eeedaa53a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437207"
---
# <a name="tutorial-azure-active-directory-integration-with-yardi-elearning"></a>Öğretici: Azure Active Directory Yardi e-ÖĞRENİM ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Yardi öğrenim tümleştirme konusunda bilgi edinin.

Yardi tümleştirmek Azure AD ile e-ÖĞRENİM ile aşağıdaki avantajları sağlar:

- Yardi öğrenim erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Yardi e-ÖĞRENİM (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Yardi e-ÖĞRENİM ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Yardi e-ÖĞRENİM çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Yardi öğrenim ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-yardi-elearning-from-the-gallery"></a>Galeriden Yardi öğrenim ekleme
Azure AD'de Yardi öğrenim tümleştirmesini yapılandırmak için Yardi öğrenim Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Yardi öğrenim eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Yardi öğrenim**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/yardielearning-tutorial/tutorial_yardielearning_search.png)

1. Sonuçlar panelinde seçin **Yardi öğrenim**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/yardielearning-tutorial/tutorial_yardielearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Yardi e-ÖĞRENİM ile test etme

Tek iş için oturum açma için Azure AD ne Yardi öğrenim karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Yardi öğrenim ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Yardi öğrenim ' atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Yardi e-ÖĞRENİM ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Yardi e-ÖĞRENİM test kullanıcısı oluşturma](#creating-a-yardi-elearning-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı Yardi öğrenim içinde bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Yardi e-ÖĞRENİM uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Yardi e-ÖĞRENİM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Yardi e-ÖĞRENİM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/yardielearning-tutorial/tutorial_yardielearning_samlbase.png)

1. Üzerinde **Yardi öğrenim etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/yardielearning-tutorial/tutorial_yardielearning_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.yardielearning.com/login`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.yardielearning.com/trust`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [Yardi e-ÖĞRENİM istemci Destek ekibine](mailto:elearning@yardi.com) bu değerleri almak için. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/yardielearning-tutorial/tutorial_yardielearning_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/yardielearning-tutorial/tutorial_general_400.png)

1. Çoklu oturum açmayı yapılandırma **Yardi öğrenim** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Yardi e-ÖĞRENİM Destek ekibine](mailto:elearning@yardi.com). 

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/yardielearning-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/yardielearning-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/yardielearning-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/yardielearning-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-yardi-elearning-test-user"></a>Yardi e-ÖĞRENİM test kullanıcısı oluşturma

Bu bölümün amacı Yardi öğrenim Britta Simon adlı bir kullanıcı oluşturmaktır. Yardi öğrenim tam zamanında sağlama, varsayılan olarak etkin olan destekler.

Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Yardi öğrenim erişme denemesi sırasında oluşturulur. 

>[!NOTE]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, iletişime geçmeniz [Yardi e-ÖĞRENİM Destek ekibine](mailto:elearning@yardi.com).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma Yardi öğrenim erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Yardi öğrenim için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Yardi öğrenim**.

    ![Çoklu oturum açmayı yapılandırın](./media/yardielearning-tutorial/tutorial_yardielearning_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Yardi e-ÖĞRENİM kutucuğa tıkladığınızda, otomatik olarak Yardi e-ÖĞRENİM uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/yardielearning-tutorial/tutorial_general_01.png
[2]: ./media/yardielearning-tutorial/tutorial_general_02.png
[3]: ./media/yardielearning-tutorial/tutorial_general_03.png
[4]: ./media/yardielearning-tutorial/tutorial_general_04.png

[100]: ./media/yardielearning-tutorial/tutorial_general_100.png

[200]: ./media/yardielearning-tutorial/tutorial_general_200.png
[201]: ./media/yardielearning-tutorial/tutorial_general_201.png
[202]: ./media/yardielearning-tutorial/tutorial_general_202.png
[203]: ./media/yardielearning-tutorial/tutorial_general_203.png

