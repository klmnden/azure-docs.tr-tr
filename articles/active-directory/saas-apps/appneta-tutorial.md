---
title: 'Öğretici: AppNeta Performans İzleyicisi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve AppNeta Performans İzleyicisi'ni arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: 19d79f65746b5ee03209bfd7d8405ddaa24bb825
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194891"
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Öğretici: Azure Active Directory tümleştirmesiyle AppNeta Performans İzleyicisi

Bu öğreticide, Azure Active Directory (Azure AD) ile AppNeta Performans İzleyicisi'ni tümleştirme konusunda bilgi edinin.

AppNeta Performans İzleyicisi'ni Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Performans İzleyicisi'ni AppNeta erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) AppNeta Performans İzleyicisi açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile AppNeta Performans İzleyicisi'ni yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir AppNeta Performans İzleyicisi'ni çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden AppNeta Performans İzleyicisi ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Galeriden AppNeta Performans İzleyicisi ekleme
Azure AD'de AppNeta Performans İzleyicisi'nin tümleştirmesini yapılandırmak için AppNeta Performans İzleyicisi'ni Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden AppNeta Performans İzleyicisi eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **AppNeta Performans İzleyicisi'ni**seçin **AppNeta Performans İzleyicisi'ni** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde AppNeta Performans İzleyicisi](./media/appneta-tutorial/tutorial_appneta_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve AppNeta Performans İzleyicisi "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı AppNeta Performans İzleyicisi'nde bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı AppNeta Performans İzleyicisi'nde arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma AppNeta Performansı İzleyicisi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir AppNeta Performans İzleyicisi'ni test kullanıcısı oluşturma](#create-an-appneta-performance-monitor-test-user)**  - kullanıcı Azure AD gösterimini bağlı AppNeta Performans İzleyicisi'nde Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma AppNeta Performans İzleyicisi'ni uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma ile AppNeta Performans İzleyicisi'ni yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **AppNeta Performans İzleyicisi'ni** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/appneta-tutorial/tutorial_appneta_samlbase.png)

3. Üzerinde **AppNeta Performans İzleyicisi'ni etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![AppNeta Performans İzleyicisi'ni etki alanı ve URL'ler tek oturum açma bilgileri](./media/appneta-tutorial/tutorial_appneta_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.pm.appneta.com`

    b. İçinde **tanımlayıcı** metin değeri yazın: `PingConnect`

    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [AppNeta Performans İzleyicisi'ni istemci Destek ekibine](mailto:support@appneta.com) bu değeri alınamıyor. 

5. AppNeta Performans İzleyicisi'ni uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/appneta-tutorial/attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırın ve aşağıdaki adımları gerçekleştirin:
           
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| ----------------|
    | FirstName| User.givenName|
    | Soyadı| User.surname|
    | e-posta| User.userPrincipalName|
    | ad| User.userPrincipalName|
    | gruplar   | User.assignedroles |
    | telefon| User.telephoneNumber |
    | başlık| user.jobtitle|

    > [!NOTE]
    > 'Gruplar', 'Role' Azure AD'de eşlendi Appneta seçtiğiniz güvenlik grubunda ifade eder. Lütfen [bu](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) Azure AD'de özel roller oluşturmanızı da nasıl yapıldığını açıklayan bir belge.
        
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/appneta-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/appneta-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Ad alanı boş bırakın.
    
    e. **Tamam**’a tıklayın.  

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/appneta-tutorial/tutorial_appneta_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/appneta-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırma **AppNeta Performans İzleyicisi'ni** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [AppNeta Performans İzleyicisi'ni Destek ekibine](mailto:support@appneta.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/appneta-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/appneta-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/appneta-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/appneta-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-appneta-performance-monitor-test-user"></a>Bir AppNeta Performans İzleyicisi'ni test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon AppNeta Performans İzleyicisi'nde adlı bir kullanıcı oluşturmaktır. Performans İzleyicisi'ni AppNeta tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa AppNeta Performans İzleyicisi'ni erişme denemesi sırasında oluşturulur.
>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [AppNeta Performans İzleyicisi'ni Destek ekibine](mailto:support@appneta.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, AppNeta Performans İzleyicisi için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon AppNeta Performans İzleyicisi atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **AppNeta Performans İzleyicisi'ni**.

    ![Uygulamalar listesinde AppNeta Performans İzleyicisi bağlantı](./media/appneta-tutorial/tutorial_appneta_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli AppNeta Performans İzleyicisi'ni döşemede tıklattığınızda, otomatik olarak AppNeta Performans İzleyicisi'ni uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/appneta-tutorial/tutorial_general_01.png
[2]: ./media/appneta-tutorial/tutorial_general_02.png
[3]: ./media/appneta-tutorial/tutorial_general_03.png
[4]: ./media/appneta-tutorial/tutorial_general_04.png

[100]: ./media/appneta-tutorial/tutorial_general_100.png

[200]: ./media/appneta-tutorial/tutorial_general_200.png
[201]: ./media/appneta-tutorial/tutorial_general_201.png
[202]: ./media/appneta-tutorial/tutorial_general_202.png
[203]: ./media/appneta-tutorial/tutorial_general_203.png

