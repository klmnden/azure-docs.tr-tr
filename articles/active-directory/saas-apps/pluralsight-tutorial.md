---
title: 'Öğretici: Azure Active Directory Pluralsight ile tümleştirme | Microsoft Docs'
description: Pluralsight ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: jeedes
ms.openlocfilehash: 81f7e6f7610eb855bd4df1eaf45c6d016befc133
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443623"
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Öğretici: Azure Active Directory Pluralsight ile tümleştirme

Bu öğreticide, Pluralsight Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Pluralsight, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Pluralsight erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Pluralsight'ın (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Pluralsight ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir Pluralsight çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Pluralsight galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-pluralsight-from-the-gallery"></a>Pluralsight galeri ekleme
Azure AD'de Pluralsight tümleştirmesini yapılandırmak için Pluralsight Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Pluralsight galerideki eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Pluralsight**seçin **Pluralsight** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Pluralsight](./media/pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Pluralsight ile test edin.

Tek iş için oturum açma için Azure AD ne Pluralsight karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Pluralsight ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Pluralsight'ın değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Pluralsight ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Pluralsight test kullanıcısı oluşturma](#create-a-pluralsight-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pluralsight Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Pluralsight uygulamanızda çoklu oturum açmayı yapılandırın.

**Pluralsight ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Pluralsight** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

1. Üzerinde **Pluralsight etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Pluralsight etki alanı ve URL'ler tek oturum açma bilgileri](./media/pluralsight-tutorial/tutorial_pluralsight_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<instancename>.pluralsight.com/sso/<companyname>`

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `www.pluralsight.com`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<instancename>.pluralsight.com/sp/ACS.saml2`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Pluralsight istemci Destek ekibine](mailto:support@pluralsight.com) bu değerleri almak için. 

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

1. Bu bölümün amacı, Azure AD çoklu oturum açma Azure portalındaki etkinleştirmek ve Pluralsight uygulamada SSO yapılandırmak için ' dir.

    Pluralsight uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    >Ayrıca ekleyebilirsiniz **"Benzersiz kimliği"** EmployeeID veya başka bir şey gibi kuruluşunuz için uygun uygun değere sahip bir öznitelik. Ayrıca bu gerekli öznitelik olmadığını unutmayın; Ancak, benzersiz bir kullanıcıyı tanımlamak için ekleyebilirsiniz. 

1. Gerekli eklemek için **SAML belirteci öznitelikleri**, aşağıdaki tabloda gösterilen her satır için aşağıdaki adımları gerçekleştirin:
   
   | Öznitelik Adı | Öznitelik Değeri |
   | ---| --- |
   | Ad |User.givenName |
   | Soyadı |User.surname |
   | Email |User.Mail |
   
   a. Tıklayın **kullanıcı özniteliğini eklemek** açmak için **kullanıcı özniteliğini eklemek** iletişim.
    
     ![Çoklu oturum açmayı yapılandırın](./media/pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   b. İçinde **öznitelik adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
  
   c. Gelen **öznitelik değeri** listesinde, ilgili satır için gösterilen öznitelik değeri seçin.
  
   d. **Tamam**’a tıklayın.    

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/pluralsight-tutorial/tutorial_general_400.png)

1. Uygulamanız için yapılandırılmış SSO almak için iletişime geçin [Pluralsight Profesyonel Hizmetler](mailTo:professionalservices@pluralsight.com) takım ve indirilen meta veri dosyası sağlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/pluralsight-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/pluralsight-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/pluralsight-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/pluralsight-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-pluralsight-test-user"></a>Pluralsight test kullanıcısı oluşturma

Bu bölümün amacı Pluralsight Britta Simon adlı bir kullanıcı oluşturmaktır. Lütfen birlikte çalışarak [Pluralsight istemci Destek ekibine](mailto:support@pluralsight.com) Pluralsight hesabı kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Pluralsight erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Pluralsight'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Pluralsight**.

    ![Uygulamalar listesini Pluralsight bağlantıdaki](./media/pluralsight-tutorial/tutorial_pluralsight_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Pluralsight kutucuğa tıkladığınızda, otomatik olarak Pluralsight uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/pluralsight-tutorial/tutorial_general_203.png

