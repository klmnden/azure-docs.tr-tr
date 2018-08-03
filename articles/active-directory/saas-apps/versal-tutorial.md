---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Rensel | Microsoft Docs'
description: Azure Active Directory ve Rensel arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 5b2e53c0-61a3-4954-ae46-8c28c6368bfd
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeedes
ms.openlocfilehash: a6e1f73218efb11da475f3e67188863c3b99de97
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421347"
---
# <a name="tutorial-azure-active-directory-integration-with-versal"></a>Öğretici: Azure Active Directory Rensel ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Rensel tümleştirme konusunda bilgi edinin.

Azure AD ile Rensel tümleştirme ile aşağıdaki avantajları sağlar:

- Rensel erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanmış için Rensel (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Rensel yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Versal çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Rensel ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-versal-from-the-gallery"></a>Galeriden Rensel ekleme
Azure AD'de Rensel tümleştirmesini yapılandırmak için Rensel Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Rensel eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Rensel**seçin **Rensel** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde rensel](./media/versal-tutorial/tutorial_versal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Rensel sınayın.

Tek çalışmak için oturum açma için Azure AD ne Rensel karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Rensel ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Rensel içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Rensel ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Versal test kullanıcısı oluşturma](#create-a-versal-test-user)**  - kullanıcı Azure AD gösterimini bağlı Rensel Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Versal uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Rensel yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Rensel** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/versal-tutorial/tutorial_versal_samlbase.png)

1. Üzerinde **fasından yapılan etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri Versal etki alanı ve URL'ler](./media/versal-tutorial/tutorial_versal_url.png)

    a. İçinde **tanımlayıcı** metin değeri yazın: `VERSAL`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://versal.com/sso/saml/orgs/<organization_id>`

    > [!NOTE] 
    > Yanıt URL'si değeri gerçek değil. Bu değer, gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [Versal Destek ekibine](https://support.versal.com/hc/) bu değeri alınamıyor.
    
1. Uygulamanız SAML onaylamalarını özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde bekliyor. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Varsayılan değer olan **kullanıcı tanımlayıcısı** olduğu **user.userprincipalname** ancak **Rensel** bu kullanıcının e-posta adresi ile eşlenmesini bekliyor. Bunun için kullanabileceğiniz **user.mail** listeden öznitelik veya kuruluş yapılandırmanıza göre uygun öznitelik değeri kullanın.
    
    ![Kullanıcı tanımlayıcısı açılan menüsü](./media/versal-tutorial/tutorial_versal_attribute.png)

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/versal-tutorial/tutorial_versal_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/versal-tutorial/tutorial_general_400.png)
    
1. Çoklu oturum açmayı yapılandırma **Rensel** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML imzalama sertifikası** için [Versal destek ekibi ](https://support.versal.com/hc/). Bunlar her iki kenarı da düzgün ayarlandığından SAML SSO bağlantısı Versal kuruluşunuz yapılandıracaksınız.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/versal-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/versal-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/versal-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/versal-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-versal-test-user"></a>Versal test kullanıcısı oluşturma

Bu bölümde, Britta Simon Rensel içinde adlı bir kullanıcı oluşturun. Lütfen izleyin [SAML oluşturma, kullanıcı test](https://support.versal.com/hc/en-us/articles/115011672887-Creating-a-SAML-test-user) Britta Simon kuruluşunuzdaki kullanıcı oluşturmak için destek Kılavuzu. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce Rensel etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma için Rensel erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Rensel için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Rensel**.

    ![Uygulamalar listesinde fasından yapılan bağlantı](./media/versal-tutorial/tutorial_versal_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Web sitenizde katıştırılmış Versal kurs kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.
Lütfen [katıştırma kuruluş kursları](https://support.versal.com/hc/en-us/articles/203271866-Embedding-organizational-courses) **SAML çoklu oturum açma** Kılavuzu çoklu oturum açmayı Versal kurs ile Azure AD için destek ekleme hakkında yönergeler için destek. 

Bir kurs oluşturmak, kuruluşunuzla paylaşma ve kursu katıştırma test etmek için yayımlama gerekecektir. Lütfen [kurs oluşturma](https://support.versal.com/hc/en-us/articles/203722528-Create-a-course), [kurs yayımlama](https://support.versal.com/hc/en-us/articles/203753398-Publishing-a-course), ve [kurs ve learner Yönetimi](https://support.versal.com/hc/en-us/articles/206029467-Course-and-learner-management) daha fazla bilgi için.  
                     

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/versal-tutorial/tutorial_general_01.png
[2]: ./media/versal-tutorial/tutorial_general_02.png
[3]: ./media/versal-tutorial/tutorial_general_03.png
[4]: ./media/versal-tutorial/tutorial_general_04.png

[100]: ./media/versal-tutorial/tutorial_general_100.png

[200]: ./media/versal-tutorial/tutorial_general_200.png
[201]: ./media/versal-tutorial/tutorial_general_201.png
[202]: ./media/versal-tutorial/tutorial_general_202.png
[203]: ./media/versal-tutorial/tutorial_general_203.png

