---
title: 'Öğretici: Azure Active Directory Sugar CRM ile tümleştirme | Microsoft Docs'
description: Sugar CRM ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 1acaf5e530f5d5563901d8d498901ecc1bffecdb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427408"
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a>Öğretici: Azure Active Directory Sugar CRM ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Sugar CRM tümleştirme konusunda bilgi edinin.

Sugar CRM Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Sugar CRM erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Sugar CRM'ye (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Sugar CRM ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Sugar CRM çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Sugar CRM galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-sugar-crm-from-the-gallery"></a>Sugar CRM galeri ekleme
Azure AD'de Sugar CRM tümleştirmesini yapılandırmak için Sugar CRM Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Sugar CRM Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Sugar CRM**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sugarcrm-tutorial/tutorial_sugarcrm_search.png)

1. Sonuçlar panelinde seçin **Sugar CRM**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Sugar CRM ile test edin.

Tek iş için oturum açma için Azure AD ne Sugar CRM karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Sugar CRM'de arasında bir bağlantı ilişki kurulması gerekir.

Sugar CRM'de değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Sugar CRM ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Sugar CRM test kullanıcısı oluşturma](#creating-a-sugar-crm-test-user)**  - kullanıcı Azure AD gösterimini bağlı Sugar CRM'de Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Sugar CRM uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Sugar CRM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Sugar CRM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

1. Üzerinde **Sugar CRM etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Sugar CRM istemci Destek ekibine](https://support.sugarcrm.com/) değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/sugarcrm-tutorial/tutorial_general_400.png)

1. Üzerinde **Sugar CRM Yapılandırma** bölümünde **yapılandırma Sugar CRM** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

1. Farklı bir web tarayıcı penceresinde Sugar CRM şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **yönetici**.
   
    ![Yönetici](./media/sugarcrm-tutorial/ic795888.png "yönetici")

1. İçinde **Yönetim** bölümünde **parola yönetimi**.
   
    ![Yönetim](./media/sugarcrm-tutorial/ic795889.png "Yönetim")

1. Seçin **SAML kimlik doğrulamasını etkinleştirme**.
   
    ![Yönetim](./media/sugarcrm-tutorial/ic795890.png "Yönetim")

1. İçinde **SAML kimlik doğrulaması** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![SAML kimlik doğrulaması](./media/sugarcrm-tutorial/ic795891.png "SAML kimlik doğrulaması")  
 
    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
  
    b. İçinde **SLO URL** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
  
    c. Tüm sertifika içine yapıştırın, base-64 kodlanmış sertifika Not Defteri'nde açın ve içeriğini, panoya kopyalanmak **X.509 sertifikası** metin.
  
    d. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/sugarcrm-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sugarcrm-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sugarcrm-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/sugarcrm-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-sugar-crm-test-user"></a>Sugar CRM test kullanıcısı oluşturma

Sugar CRM'de oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Sugar CRM'ye sağlanması gerekir.

Sugar CRM söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Sugar CRM** şirketinizin sitesi yöneticisi olarak.

1. Git **yönetici**.
   
    ![Yönetici](./media/sugarcrm-tutorial/ic795888.png "yönetici")

1. İçinde **Yönetim** bölümünde **kullanıcı yönetimi**.
   
    ![Yönetim](./media/sugarcrm-tutorial/ic795893.png "Yönetim")

1. Git **kullanıcılar \> yeni kullanıcı oluşturma**.
   
    ![Yeni kullanıcı oluşturma](./media/sugarcrm-tutorial/ic795894.png "yeni kullanıcı oluşturma")

1. Üzerinde **kullanıcı profili** sekmesinde, aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı](./media/sugarcrm-tutorial/ic795895.png "yeni kullanıcı")

    a. Tür **kullanıcı adı**, **Soyadı**, ve **e-posta adresi** ilgili metin kutularına halinde geçerli bir Azure Active Directory kullanıcı.
  
1. Olarak **durumu**seçin **etkin**.

1. Parola sekmesinde aşağıdaki adımları gerçekleştirin:
   
    ![Yeni kullanıcı](./media/sugarcrm-tutorial/ic795896.png "yeni kullanıcı")

    a. Parolayı ilgili metin kutusuna yazın.

    b. **Kaydet**’e tıklayın.

>[!NOTE]
>Herhangi diğer Sugar CRM kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak Sugar CRM tarafından sağlanan. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Sugar CRM için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Sugar CRM'ye atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Sugar CRM**.

    ![Çoklu oturum açmayı yapılandırın](./media/sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde Sugar CRM kutucuğa tıkladığınızda, otomatik olarak Sugar CRM uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/sugarcrm-tutorial/tutorial_general_203.png

