---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle 4me | Microsoft Docs'
description: Azure Active Directory ve 4me arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 983eecc6-41f8-49b7-b7f6-dcf833dde121
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.author: jeedes
ms.openlocfilehash: c9134ceebca696ed2b3376a69e26c2ea06f4f0f6
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506209"
---
# <a name="tutorial-azure-active-directory-integration-with-4me"></a>Öğretici: Azure Active Directory 4me ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile 4me tümleştirme konusunda bilgi edinin.

Azure AD ile 4me tümleştirme ile aşağıdaki avantajları sağlar:

- 4me erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan 4me için açma, kullanıcılarınızın etkinleştirebilirsiniz (çoklu oturum açma) ile Azure AD hesaplarına.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile 4me yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik 4me çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden 4me ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-4me-from-the-gallery"></a>Galeriden 4me ekleme
Azure AD'de 4me tümleştirmesini yapılandırmak için 4me Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden 4me eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **4me**seçin **4me** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![sonuç listesinde 4me](./media/4me-tutorial/tutorial_4me_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı 4me sınayın.

Tek iş için oturum açma için Azure AD ne 4me karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının 4me ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma 4me ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[4me test kullanıcısı oluşturma](#create-a-4me-test-user)**  - Britta Simon kullanıcı Azure AD gösterimini bağlı 4me içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve 4me uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile 4me yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **4me** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/4me-tutorial/tutorial_4me_samlbase.png)

3. Üzerinde **4me etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![4me etki alanı ve URL'ler tek oturum açma bilgileri](./media/4me-tutorial/tutorial_4me_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak:

    | Ortam| URL'si|
    |---|---|
    | ÜRETİM | `https://<SUBDOMAIN>.4me.com`|
    | QA| `https://<SUBDOMAIN>.4me.qa`|
  
    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    
    | Ortam| URL'si|
    |---|---|
    | ÜRETİM | `https://<SUBDOMAIN>.4me.com`|
    | QA| `https://<SUBDOMAIN>.4me.qa`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [4me istemci Destek ekibine](mailto:support@4me.com) bu değerleri almak için. 
 
4. 4me uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/4me-tutorial/tutorial_4me_attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | first_name | User.givenName |
    | Soyadı | User.surname |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/4me-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/4me-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    d. Tıklayın **Tamam**

6. Üzerinde **SAML imzalama sertifikası** bölümünde, kopya **parmak İZİ** bilgisayarınızda değeri.

    ![Sertifika indirme bağlantısı](./media/4me-tutorial/tutorial_4me_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/4me-tutorial/tutorial_general_400.png)

8. Üzerinde **4me yapılandırma** bölümünde **4me yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![4me yapılandırma](./media/4me-tutorial/tutorial_4me_configure.png) 

9. Bir başka web tarayıcı penceresinde 4me yönetici olarak oturum açın.

10. Sol Üstten, tıklayarak **ayarları** logosu ve sol tarafındaki çubuğu öğesini **çoklu oturum açma**.

    ![4me ayarları](./media/4me-tutorial/tutorial_4me_settings.png)

11. Üzerinde **çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![4me singleasignon](./media/4me-tutorial/tutorial_4me_singlesignon.png)

    a. Seçin **etkin** seçeneği.

    b. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    c. Altında **SAML** bölümünde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız.

    d. İçinde **sertifika parmak izi** metin kutusu, yapıştırma **parmak İZİ** duplets sırada (AA:BB:CC:DD:EE:FF:GG:HH:II), Azure Portalı'ndan kopyaladığınız üste değeri listeleyin.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/4me-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/4me-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/4me-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/4me-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-4me-test-user"></a>4me test kullanıcısı oluşturma

Bu bölümün amacı 4me Britta Simon adlı bir kullanıcı oluşturmaktır. 4me tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa 4me erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [4me Destek ekibine](mailto:support@4me.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma 4me erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon 4me için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **4me**.

    ![Uygulamalar listesinde 4me bağlantı](./media/4me-tutorial/tutorial_4me_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde 4me kutucuğa tıkladığınızda, otomatik olarak 4me uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/4me-tutorial/tutorial_general_01.png
[2]: ./media/4me-tutorial/tutorial_general_02.png
[3]: ./media/4me-tutorial/tutorial_general_03.png
[4]: ./media/4me-tutorial/tutorial_general_04.png

[100]: ./media/4me-tutorial/tutorial_general_100.png

[200]: ./media/4me-tutorial/tutorial_general_200.png
[201]: ./media/4me-tutorial/tutorial_general_201.png
[202]: ./media/4me-tutorial/tutorial_general_202.png
[203]: ./media/4me-tutorial/tutorial_general_203.png

