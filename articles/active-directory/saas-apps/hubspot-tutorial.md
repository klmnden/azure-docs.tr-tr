---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile HubSpot | Microsoft Docs'
description: Azure Active Directory ve HubSpot arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: jeedes
ms.openlocfilehash: 2bd47c7955625eb43de40e61515bf7814a8c8355
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55815838"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Öğretici: HubSpot ile Azure Active Directory Tümleştirme

Bu öğreticide, HubSpot, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Azure AD ile HubSpot tümleştirme ile aşağıdaki avantajları sağlar:

- HubSpot erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için HubSpot (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile HubSpot yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik HubSpot çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden HubSpot ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-hubspot-from-the-gallery"></a>Galeriden HubSpot ekleme

Azure AD'de HubSpot tümleştirmesini yapılandırmak için HubSpot Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden HubSpot eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **HubSpot**. Seçin **HubSpot** sonuçlar paneli ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hubspot-tutorial/tutorial_hubspot_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı HubSpot ile test etme

Tek çalışmak için oturum açma için Azure AD ne HubSpot karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının HubSpot ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma HubSpot ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **HubSpot test kullanıcısı oluşturma** - kullanıcı Azure AD gösterimini bağlı HubSpot Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve HubSpot uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile HubSpot yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **HubSpot** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/tutorial_general_301.png)

3. Değiştirmeniz gerekiyorsa **SAML** herhangi bir başka bir modda modundan tıklayın **oturum açma tek Mod Değiştir** ekranın en üstünde.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/tutorial_general_300.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümü, uygulamada yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![HubSpot etki alanı ve URL'ler tek oturum açma bilgileri](./media/hubspot-tutorial/tutorial_hubspot_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak URL'yi yazın: `https://api.hubspot.com/login-api/v1/saml/login?portalId=<CUSTOMER ID>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak URL'yi yazın: `https://api.hubspot.com/login-api/v1/saml/acs?portalId=<CUSTOMER ID>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve bu öğreticinin ilerleyen bölümlerinde açıklanan yanıt URL'si ile güncelleştirin. İlgili kişi [HubSpot istemci Destek ekibine](https://help.hubspot.com/) bu değerleri almak için.

    c. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![HubSpot etki alanı ve URL'ler tek oturum açma bilgileri](./media/hubspot-tutorial/tutorial_hubspot_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://app.hubspot.com/login`

6. Üzerinde **SAML imzalama sertifikası** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/tutorial_hubspot_certificate.png)

7. Üzerinde **HubSpot kümesi** bölümünde, kopyalamak için Kopyala düğmesine tıklayın **oturum açma URL'si** ve **Azure AD tanımlayıcısı** değerleri.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/tutorial_hubspot_configure.png)

8. Tarayıcı ve oturum açma HubSpot yönetici hesabınıza yeni bir sekme açın.

9. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/config1.png)

10. Tıklayarak **hesap varsayılanları**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/config2.png)

11. Ekranı aşağı kaydırarak **güvenlik** tıklayın ve bölüm **ayarlanan**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/config3.png)

12. Üzerinde **tek oturum açma kurulumunu** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/config4.png)

    a. Tıklayın **kopyalama** kopyalamak için düğmesine **İzleyici URl(Service Provider Entity ID)** yapıştırın ve değer **tanımlayıcı** metin kutusunda **temel SAML Yapılandırma** bölümü Azure Portalı'nda.

    b. Tıklayın **kopyalama** kopyalamak için düğmesine **URl, ACS, alıcı ya da yeniden yönlendirme oturum** yapıştırın ve değer **yanıt URL'si** metin kutusunda **temel SAML Yapılandırma** bölümü Azure Portalı'nda.

    c. İçinde **kimlik sağlayıcı tanımlayıcısı veya veren URL'si** metin kutusu, yapıştırma **Azure AD tanımlayıcısı** Azure portaldan kopyaladığınız değeri.

    d. İçinde **kimlik sağlayıcısının çoklu oturum açma URL'si** metin kutusu, yapıştırma **oturum açma URL'si** Azure portaldan kopyaladığınız değeri.

    e. İndirilen açın **Certificate(Base64)** dosyasını Not Defteri'nde. İçeriğini sizin panoya kopyalayın ve yapıştırın kendisine **X.509 sertifikası** kutusu.

    f. **Doğrula**’ya tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hubspot-tutorial/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/hubspot-tutorial/create_aaduser_02.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="creating-a-hubspot-test-user"></a>HubSpot test kullanıcısı oluşturma

HubSpot için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların HubSpot sağlanması gerekir.
HubSpot söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **HubSpot** şirketinizin sitesi yöneticisi olarak.

2. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/config1.png)

3. Tıklayarak **kullanıcılar ve takımlar**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/user1.png)

4. Tıklayın **kullanıcı oluşturma**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/user2.png)

5. Bir kullanıcı gibi e-posta adresini girin **brittasimon@contoso.com** içinde **Ekle e-posta addess(es)** textbox tıklayın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/user3.png)

6. Üzerinde **kullanıcılar oluşturma** bölümünde Lütfen tek tek her sekmesine gidin ve uygun seçeneği tıklayın ve kullanıcı izinlerini seçip **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/user4.png)

7. Tıklayın **Gönder** kullanıcıya bir davet göndermesi için.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > Kullanıcı daveti kabul ettikten sonra devreye.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, HubSpot için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **HubSpot**.

    ![Çoklu oturum açmayı yapılandırın](./media/hubspot-tutorial/tutorial_hubspot_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli HubSpot kutucuğa tıkladığınızda, HubSpot uygulamanın oturum açma sayfasına otomatik olarak almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/hubspot-tutorial/tutorial_general_01.png
[2]: ./media/hubspot-tutorial/tutorial_general_02.png
[3]: ./media/hubspot-tutorial/tutorial_general_03.png
[4]: ./media/hubspot-tutorial/tutorial_general_04.png
[100]: ./media/hubspot-tutorial/tutorial_general_100.png
[200]: ./media/hubspot-tutorial/tutorial_general_200.png
[201]: ./media/hubspot-tutorial/tutorial_general_201.png
[202]: ./media/hubspot-tutorial/tutorial_general_202.png
[203]: ./media/hubspot-tutorial/tutorial_general_203.png
