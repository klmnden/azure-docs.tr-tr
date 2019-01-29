---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Birst Çevik İş analizi | Microsoft Docs'
description: Azure Active Directory ve Çevik İş analizi Birst arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 61c95d17493dff493787be00fb56ec2f799b5d19
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175086"
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a>Öğretici: Azure Active Directory Tümleştirmesi ile Birst Çevik İş analizi

Bu öğreticide, Azure Active Directory (Azure AD) ile Birst Çevik İş analizi tümleştirme konusunda bilgi edinin.

Çevik İş analizi Birst Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Çevik İş analizi Birst erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Birst Çevik İş analizi için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Birst Çevik İş Analizi ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Çevik İş analizi Birst çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Birst Çevik İş analizi ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-birst-agile-business-analytics-from-the-gallery"></a>Galeriden Birst Çevik İş analizi ekleme
Azure AD'de Birst Çevik İş analizi tümleştirmesini yapılandırmak için Birst Çevik İş analizi Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Birst Çevik İş analizi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Birst Çevik İş analizi**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/birst-tutorial/tutorial_birst_search.png)

1. Sonuçlar panelinde seçin **Birst Çevik İş analizi**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Birst Çevik iş "Britta Simon." adlı bir test kullanıcı tabanlı analiz ile test etme

Tek iş için oturum açma için Azure AD ne Birst Çevik İş analizi karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Çevik İş analizi Birst ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Değerini Birst Çevik iş Analytics'te atama **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Birst Çevik İş Analizi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Çevik İş analizi Birst test kullanıcısı oluşturma](#creating-a-birst-agile-business-analytics-test-user)**  - analytics'te Birst Çevik iş kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Birst Çevik İş analizi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Birst Çevik İş Analizi ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Birst Çevik İş analizi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/birst-tutorial/tutorial_birst_samlbase.png)

1. Üzerinde **Birst Çevik İş analizi etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/birst-tutorial/tutorial_birst_url.png)

     İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

     URL Birst hesabınızın bulunduğu veri merkezine bağlıdır: 

     * Düzenini izleyerek ABD veri merkezinde kullanmak için: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID` 

     * Avrupa veri merkezi için şu biçimi kullanın: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`

    > [!NOTE] 
    > Bu değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Birst Çevik İş analizi istemci Destek ekibine](mailto:info@birst.com) değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/birst-tutorial/tutorial_birst_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/birst-tutorial/tutorial_general_400.png)

1. Üzerinde **Birst Çevik İş analizi yapılandırma** bölümünde **yapılandırma Birst Çevik İş analizi** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/birst-tutorial/tutorial_birst_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Birst Çevik İş analizi** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)**, **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma Hizmet URL'si** için [Birst Çevik İş analizi Destek ekibine](mailto:info@birst.com). 

    > [!NOTE]
    > Bu tümleştirme SHA256 algoritmasını gerektiğini Birst ekibine belirttiğinizden (SHA1 değil desteklenir) ilgili sunucuda SSO ayarlayabilirsiniz gibi **app2101** vb.
  

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi embedded belgeleri özelliği burada hakkında: [Azure AD embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/birst-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/birst-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/birst-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/birst-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a>Çevik İş analizi Birst test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Birst Çevik İş analizi adlı bir kullanıcı oluşturmaktır. Çalışmak [Birst Çevik İş analizi Destek ekibine](mailto:info@birst.com) Birst hesabında kullanıcıları eklemek için. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Birst Çevik İş analizi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Birst Çevik İş analizi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Birst Çevik İş analizi**.

    ![Çoklu oturum açmayı yapılandırın](./media/birst-tutorial/tutorial_birst_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde Birst Çevik İş analizi kutucuğa tıkladığınızda, otomatik olarak Birst Çevik İş analizi uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/birst-tutorial/tutorial_general_01.png
[2]: ./media/birst-tutorial/tutorial_general_02.png
[3]: ./media/birst-tutorial/tutorial_general_03.png
[4]: ./media/birst-tutorial/tutorial_general_04.png

[100]: ./media/birst-tutorial/tutorial_general_100.png

[200]: ./media/birst-tutorial/tutorial_general_200.png
[201]: ./media/birst-tutorial/tutorial_general_201.png
[202]: ./media/birst-tutorial/tutorial_general_202.png
[203]: ./media/birst-tutorial/tutorial_general_203.png

