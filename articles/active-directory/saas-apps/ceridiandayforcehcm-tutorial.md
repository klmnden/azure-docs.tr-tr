---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Ceridian Dayforce HCM | Microsoft Docs'
description: Azure Active Directory ve Ceridian Dayforce HCM arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 6577955b275adfda3f0cfafe99a8f95efd16403c
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714589"
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Öğretici: Azure Active Directory Ceridian Dayforce HCM ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Ceridian Dayforce HCM tümleştirme konusunda bilgi edinin.

Azure AD ile Ceridian Dayforce HCM tümleştirme ile aşağıdaki avantajları sağlar:

- Ceridian Dayforce HCM erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Ceridian Dayforce HCM açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Ceridian Dayforce HCM yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Ceridian Dayforce HCM çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Ceridian Dayforce HCM ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Galeriden Ceridian Dayforce HCM ekleme
Azure AD'de Ceridian Dayforce HCM tümleştirmesini yapılandırmak için Ceridian Dayforce HCM Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Ceridian Dayforce HCM eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Ceridian Dayforce HCM**seçin **Ceridian Dayforce HCM** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Ceridian Dayforce HCM](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Ceridian Dayforce HCM sınayın.

Tek iş için oturum açma için Azure AD ne Ceridian Dayforce HCM karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Ceridian Dayforce HCM ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değerini Ceridian Dayforce HCM Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Ceridian Dayforce HCM ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on) ** - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) ** - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Ceridian Dayforce HCM test kullanıcısı oluşturma](#create-a-ceridian-dayforce-hcm-test-user) ** - kullanıcı Azure AD gösterimini bağlı Ceridian Dayforce HCM Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) ** - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on) ** - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Ceridian Dayforce HCM uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Ceridian Dayforce HCM ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Ceridian Dayforce HCM** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

1. Üzerinde **Ceridian Dayforce HCM etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    a. İçinde **işareti bulunan URL'si** metin kutusu, türü URL kullanıcılarınız oturum açmaya Ceridian Dayforce HCM uygulamanıza tarafından kullanılıyor.
    
    | Ortam | URL'si |
    | :-- | :-- |
    | Üretim için | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | Test için | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    
    | Ortam | URL'si |
    | :-- | :-- |
    | Üretim için | `https://ncpingfederate.dayforcehcm.com/sp` |
    | Test için | `https://fs-test.dayforcehcm.com/sp` |
    
    c. İçinde **yanıt URL'si** metin kutusu, türü URL kullanılan Azure AD tarafından yanıta gönderilecek.
    
    | Ortam | URL'si |
    | :-- | :-- |
    | Üretim için | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | Test için | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Ceridian Dayforce HCM istemci Destek ekibine](https://www.ceridian.com/support) bu değerleri almak için.

1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

1. Ceridian Dayforce HCM uygulamanızı belirli bir biçimde SAML onaylamalarını bekliyor. Çalışmak [Ceridian Dayforce HCM Destek ekibine](https://www.ceridian.com/support) ilk doğru kullanıcı tanımlayıcısını tanımlamak için. Microsoft, kullanılmasını önerir **"name"** kullanıcı tanımlayıcısı olarak özniteliği. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.  

    ![Çoklu oturum açmayı yapılandırın](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri |
    | --------------- | -------------------- |    
    | ad  | User.extensionattribute2 |    

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. İçinde **değer** listesinde, uygulamanız için kullanmak istediğiniz kullanıcı özniteliğini seçin.
    Örneğin, EmployeeID kullanmak istiyorsanız benzersiz bir kullanıcı tanımlayıcısı ve öznitelik değeri içinde ExtensionAttribute2 depoladığınız, ardından **user.extensionattribute2**.
    
    d. **Tamam**’a tıklayın.

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
1. Üzerinde **Ceridian Dayforce HCM yapılandırma** bölümünde **yapılandırma Ceridian Dayforce HCM** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Ceridian Dayforce HCM yapılandırma](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

1. Çoklu oturum açmayı yapılandırma **Ceridian Dayforce HCM** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** ve **SAML çoklu oturum açma hizmeti URL'sioturumkapatmaURL'siveSAMLvarlıkkimliği** için [Ceridian Dayforce HCM Destek ekibine](https://www.ceridian.com/support).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde ** Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ceridiandayforcehcm-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/ceridiandayforcehcm-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ceridiandayforcehcm-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a>Ceridian Dayforce HCM test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon Ceridian Dayforce HCM adlı bir kullanıcı oluşturmaktır. Çalışmak [Ceridian Dayforce HCM Destek ekibine](https://www.ceridian.com/support) Ceridian Dayforce HCM uygulamaya eklenen kullanıcılar alınamıyor. 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Ceridian Dayforce HCM erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Ceridian Dayforce HCM için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Ceridian Dayforce HCM**.

    ![Çoklu oturum açmayı yapılandırın](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Ceridian Dayforce HCM erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Ceridian Dayforce HCM için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Ceridian Dayforce HCM**.

    ![Uygulamalar listesinde Ceridian Dayforce HCM bağlantı](./media/ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.  
Erişim panelinde Ceridian Dayforce HCM kutucuğa tıkladığınızda, otomatik olarak Ceridian Dayforce HCM uygulamanıza açan. 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/ceridiandayforcehcm-tutorial/tutorial_general_203.png

