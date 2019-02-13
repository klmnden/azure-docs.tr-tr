---
title: 'Öğretici: Yaptığımız yolu ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory arasında yaptığımız gibi çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5dc6d8e2cf7ac4786f30484325406a1fe696dff3
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165136"
---
# <a name="tutorial-azure-active-directory-integration-with-way-we-do"></a>Öğretici: Yaptığımız yolu ile Azure Active Directory Tümleştirme

Bu öğreticide, yaptığımız gibi Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Azure AD ile tümleştirme gibi yaptığımız ile aşağıdaki avantajları sağlar:

- Erişimi yaptığımız gibi Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan şekilde yapmamız için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi gibi biz yaparız ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir şekilde size çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Yaptığımız gibi galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-way-we-do-from-the-gallery"></a>Yaptığımız gibi galeri ekleme
Yaptığımız gibi Azure AD'de tümleştirmesini yapılandırmak için yaptığımız gibi galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden yaptığımız yolu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **yaptığımız gibi**seçin **yaptığımız gibi** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuçlar listesinde şekilde desteklemiyoruz](./media/waywedo-tutorial/tutorial_waywedo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma yolu "Britta Simon" adlı bir test kullanıcıya bağlı olan yaptığımız ile test edin.

Tek iş için oturum açma için Azure AD ne şekilde yaptığımız karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili yaptığımız gibi kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma yolu yaptığımız ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Yaptığımız gibi bir test kullanıcısı oluşturma](#create-a-way-we-do-test-user)**  - bir karşılığı Britta simon'un şekilde kullanıcı Azure AD gösterimini bağlı bunu sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve yaptığımız gibi uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma yolu yaptığımız ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **yaptığımız gibi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/waywedo-tutorial/tutorial_waywedo_samlbase.png)

3. Üzerinde **şekilde biz yapmak etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yol biz yapmak etki alanı ve URL'ler tek oturum açma bilgileri](./media/waywedo-tutorial/tutorial_waywedo_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [şekilde biz yapmak istemci Destek ekibine](mailto:support@waywedo.com) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (ham)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/waywedo-tutorial/tutorial_waywedo_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/waywedo-tutorial/tutorial_general_400.png)

6. Üzerinde **şekilde biz yapmak yapılandırma** bölümünde **yapılandırma yolu yaptığımız** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Yapılandırma yaptığımız yolu](./media/waywedo-tutorial/tutorial_waywedo_configure.png) 

7. Bir başka web tarayıcı penceresinde şekilde yaptığımız bir güvenlik yöneticisi olarak oturum açın.

8. Tıklayın **kişi simgesi** yaptığımız gibi herhangi bir sayfasında sağ üst köşesinde, ardından **hesabı** açılır menüdeki.

    ![Yol yaptığımız hesabı](./media/waywedo-tutorial/tutorial_waywedo_account.png) 

9. Tıklayın **menüsü simgesi** anında iletme Gezinti menüsüne ve ardından açmak için **çoklu oturum açma**.

    ![Yol yaptığımız tek](./media/waywedo-tutorial/tutorial_waywedo_single.png)

10. Üzerinde **tek oturum açma Kurulumu** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Yol yaptığımız Kaydet](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Tıklayın **üzerinde çoklu oturum açmayı etkinleştirmek** geç **Evet** çoklu oturum açmayı etkinleştirmek için.

    b. İçinde **tek oturum açma adı** metin adınızı girin.

    c. İçinde **varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure portaldan kopyaladığınız.

    d. İçinde **SAML SSO URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure portaldan kopyaladığınız.

    e. Tıklayarak sertifikayı karşıya yüklemek **seçin** düğmesinin yanındaki **sertifika**.

    f. **İsteğe bağlı ayarlar** -
    
    * Parolaları - etkinleştirme bu seçeneği devre dışı bırakıldığında, böylece kullanıcıların yalnızca çoklu oturum açma kullanabilirsiniz normal parola şekilde yapmamız için çalışır.

    * Otomatik sağlamayı - etkinleştirme etkinleştirildiğinde, oturum açma için kullanılan e-posta adresine otomatik olarak yaptığımız gibi kullanıcıların listesine karşılaştırılır. Yaptığımız gibi etkin bir kullanıcı e-posta adresi ile eşleşmiyorsa, oturum açma, eksik bilgileri isteyen kişi için otomatik olarak yeni bir kullanıcı hesabı ekler.

      > [!NOTE]
      > Çoklu oturum açma ile eklenen kullanıcılar genel kullanıcılar olarak eklenir ve sistemde bir rolü atanmaz. Bir yönetici, Git ve bir düzenleyici veya yönetici olarak güvenlik rolü değiştirebilirsiniz ve ayrıca bir veya birden çok kuruluş şeması roller atayabilirsiniz. 

    g. Tıklayın **Kaydet** ayarlarınızı kalıcı hale getirmek için.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/waywedo-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/waywedo-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/waywedo-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/waywedo-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-way-we-do-test-user"></a>Yaptığımız gibi bir test kullanıcısı oluşturma

Bu bölümün amacı, şu şekilde yaptığınız Britta Simon adlı bir kullanıcı oluşturmaktır. Yol yaptığımız tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa yaptığımız gibi erişme denemesi sırasında oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [şekilde biz yapmak istemci Destek ekibine](mailto:support@waywedo.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma yolu yapmamız için erişim izni verme kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon şekilde yapmamız için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **yaptığımız gibi**.

    ![Uygulamalar listesinde yaptığımız gibi bağlantı](./media/waywedo-tutorial/tutorial_waywedo_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli yaptığımız gibi kutucuğa tıkladığınızda, otomatik olarak yaptığımız gibi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/waywedo-tutorial/tutorial_general_01.png
[2]: ./media/waywedo-tutorial/tutorial_general_02.png
[3]: ./media/waywedo-tutorial/tutorial_general_03.png
[4]: ./media/waywedo-tutorial/tutorial_general_04.png

[100]: ./media/waywedo-tutorial/tutorial_general_100.png

[200]: ./media/waywedo-tutorial/tutorial_general_200.png
[201]: ./media/waywedo-tutorial/tutorial_general_201.png
[202]: ./media/waywedo-tutorial/tutorial_general_202.png
[203]: ./media/waywedo-tutorial/tutorial_general_203.png

