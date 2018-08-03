---
title: 'Öğretici: Azure Active Directory Cisco Spark ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Cisco Spark arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: bebc8d674d7448ea0ce6a1f11b7ae80335df9cdc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431707"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Öğretici: Azure Active Directory Cisco Spark ile tümleştirme

Bu öğreticide, Cisco Spark Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Cisco Spark, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Cisco Spark erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Cisco Spark'a açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Cisco Spark'ı yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Cisco Spark çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Cisco Spark galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark galeri ekleme
Azure AD'de Cisco Spark'ın tümleştirmesini yapılandırmak için Cisco Spark Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Cisco Spark Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

1. Arama kutusuna **Cisco Spark**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cisco-spark-tutorial/tutorial_ciscospark_search.png)

1. Sonuçlar panelinde seçin **Cisco Spark**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Cisco "Britta Simon." adlı bir test kullanıcı tabanlı Spark ile test etme

Tek iş için oturum açma için Azure AD ne Cisco Spark karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Cisco Spark arasında bir bağlantı ilişki kurulması gerekir.

Cisco Spark'ta değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Cisco Spark ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Bir Cisco Spark test kullanıcısı oluşturma](#creating-a-cisco-spark-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cisco Spark Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Cisco Spark uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Cisco Spark'ı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Cisco Spark** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

1. Üzerinde **Cisco Spark etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL: `https://web.ciscospark.com/#/signin`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [Cisco Spark istemci Destek ekibine](https://support.ciscospark.com/) bu değeri alınamıyor. 
 
1. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

1. Cisco Spark uygulaması SAML onaylamalarını belirli öznitelikleri içermesini bekliyor. Aşağıdaki öznitelikler bu uygulama için yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_ciscospark_07.png) 

1. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri |
    | --------------- | -------------------- |    
    |   Kullanıcı Kimliği    | User.userPrincipalName |   

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_general_400.png)

1. Oturum [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

1. Seçin **ayarları** altında **kimlik doğrulaması** bölümünde **Değiştir**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
1. Seçin **3. taraf kimlik sağlayıcısı tümleştirin. (Gelişmiş)**  ve sonraki ekrana gidin.

1. Üzerinde **IDP meta verileri içeri aktarma** sayfasında, ya da sürükle ve bırak sayfaya Azure AD'ye meta veri dosyası veya dosya tarayıcı seçeneğini bulun ve Azure AD meta veri dosyasını karşıya yüklemek için kullanın. Ardından, **meta verileri (daha güvenli) bir sertifika yetkilisi tarafından imzalanmış bir sertifika gerektir** tıklatıp **sonraki**. 
    
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_11.png)

1. Seçin **SSO Bağlantıyı Sına**ve yeni bir tarayıcı sekmesi oturum açtığında, oturum açarak Azure AD kimlik doğrulaması.

1. Geri dönüp **Cisco bulut işbirliği Yönetimi** tarayıcı sekmesinde. Test başarılı olursa seçin **bu testi başarılı. Çoklu oturum açma seçeneğini etkinleştirin** tıklatıp **sonraki**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cisco-spark-tutorial/create_aaduser_01.png) 

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cisco-spark-tutorial/create_aaduser_02.png) 

1. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cisco-spark-tutorial/create_aaduser_03.png) 

1. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cisco-spark-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cisco-spark-test-user"></a>Bir Cisco Spark test kullanıcısı oluşturma

Bu bölümde, Spark, Cisco Britta Simon adlı bir kullanıcı oluşturun. Bu bölümde, Spark, Cisco Britta Simon adlı bir kullanıcı oluşturun.

1. Git [Cisco bulut işbirliği Yönetimi](https://admin.ciscospark.com/) tam yönetici kimlik bilgilerinizle.

1. Tıklayın **kullanıcılar** ardından **kullanıcıları yönetme**.
   
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

1. İçinde **yönetme kullanıcı** penceresinde **el ile eklemek veya kullanıcıları değiştirin** tıklatıp **sonraki**.

1. Seçin **adlarını ve e-posta adresi**. Ardından, metin kutusu aşağıdaki gibi doldurun:
   
    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. İçinde **ad** metin kutusuna **Britta**. 
    
    b. İçinde **Soyadı** metin kutusuna **Simon**.
    
    c. İçinde **e-posta adresi** metin kutusuna **britta.simon@contoso.com**.

1. Britta Simon eklemek için artı işaretine tıklayın. Ardından **İleri**'ye tıklayın.

1. İçinde **Hizmetleri kullanıcıları ekleme** penceresinde tıklayın **Kaydet** ardından **son**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Cisco Spark için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Cisco Spark'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Cisco Spark**.

    ![Çoklu oturum açmayı yapılandırın](./media/cisco-spark-tutorial/tutorial_ciscospark_app.png) 

1. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD SSO yapılandırmanızı sınamanızı sağlamaktır.

Erişim panelinde Cisco Spark kutucuğa tıkladığınızda, otomatik olarak Cisco Spark uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/cisco-spark-tutorial/tutorial_general_203.png

