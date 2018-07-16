---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Citrix ShareFile | Microsoft Docs'
description: Citrix ShareFile ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: e27a1c834c48b640ab5ed7ab8d6e54f7d1784abd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045949"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Öğretici: Citrix ShareFile Azure Active Directory Tümleştirme

Bu öğreticide, Citrix ShareFile, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.

Citrix ShareFile, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Citrix ShareFile erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Citrix ShareFile açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Citrix ShareFile ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Citrix ShareFile çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Citrix ShareFile Galeriden Ekle
2. Yapılandırma ve Azure AD çoklu oturum açmayı test etme

## <a name="add-citrix-sharefile-from-the-gallery"></a>Citrix ShareFile Galeriden Ekle
Azure AD'de Citrix ShareFile tümleştirmesini yapılandırmak için Citrix ShareFile galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Citrix ShareFile galerideki eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Citrix ShareFile**seçin **Citrix ShareFile** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Citrix ShareFile sonuç listesinde](./media/sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Citrix ShareFile ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Citrix ShareFile karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve Citrix ShareFile ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Citrix ShareFile, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Citrix ShareFile ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Citrix ShareFile test kullanıcısı oluşturma](#create-a-citrix-sharefile-test-user)**  - bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı Citrix ShareFile sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Citrix ShareFile uygulamanızda çoklu oturum açmayı yapılandırın.

**Citrix ShareFile ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Citrix ShareFile** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. Üzerinde **Citrix ShareFile etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Citrix ShareFile etki alanı ve URL'ler tek oturum açma bilgileri](./media/sharefile-tutorial/tutorial_sharefile_url.png)
    
    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<tenant-name>.sharefile.com/saml/login`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak:

    | |
    |---|
    | `https://<tenant-name>.sharefile.com`|
    | `https://<tenant-name>.sharefile.com/saml/info`|
    | `https://<tenant-name>.sharefile1.com/saml/info`|
    | `https://<tenant-name>.sharefile1.eu/saml/info`|
    | `https://<tenant-name>.sharefile.eu/saml/info`|
    | |
    
    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |---|
    | `https://<tenant-name>.sharefile.com/saml/acs`|
    | `https://<tenant-name>.sharefile.eu/saml/<URL path>`|
    | `https://<tenant-name>.sharefile.com/saml/<URL path>`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Citrix ShareFile istemci Destek ekibine](https://www.citrix.co.in/products/sharefile/support.html) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/sharefile-tutorial/tutorial_sharefile_certificate.png)

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/sharefile-tutorial/tutorial_general_400.png)

6. Üzerinde **Citrix ShareFile Yapılandırması** bölümünde **yapılandırma Citrix ShareFile** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Citrix ShareFile yapılandırma](./media/sharefile-tutorial/tutorial_sharefile_configure.png)

7. Oturum başka bir web tarayıcı penceresinde açın, **Citrix ShareFile** yönetici olarak şirketin site.

8. Üst araç çubuğunda tıklatın **yönetici**.

9. Sol gezinti bölmesinde seçin **yapılandırma çoklu oturum açma**.
   
    ![Hesap Yönetimi](./media/sharefile-tutorial/ic773627.png "hesap yönetimi")

10. Üzerinde **çoklu oturum açma / SAML 2.0 Yapılandırması** iletişim sayfanın altında **temel ayarları**, aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/sharefile-tutorial/ic773628.png "çoklu oturum açma")
   
    a. Tıklayın **etkinleştirme SAML**.
    
    b. İçinde **bilgisayarınızı IDP veren / varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.

    c. Tıklayın **değişiklik** yanındaki **X.509 sertifikası** alan ve sonra Azure portalından indirdiğiniz sertifika karşıya yükleyin.
    
    d. İçinde **oturum açma URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    e. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

11. Tıklayın **Kaydet** Citrix ShareFile Yönetim Portalı'nda.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/sharefile-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/sharefile-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/sharefile-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/sharefile-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-citrix-sharefile-test-user"></a>Citrix ShareFile test kullanıcısı oluşturma

Citrix ShareFile açarken Azure AD kullanıcılarının etkinleştirmek için bunlar uygulamasına Citrix ShareFile sağlanması gerekir. Citrix ShareFile söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **Citrix ShareFile** Kiracı.

2. Tıklayın **kullanıcıları yönetme \> kullanıcıların giriş yönetme \> + çalışan Oluştur**.
   
   ![Çalışan oluşturma](./media/sharefile-tutorial/IC781050.png "çalışan oluşturun")

3. Üzerinde **temel bilgileri** bölümünde, aşağıdaki adımları gerçekleştirin:
   
   ![Temel bilgiler](./media/sharefile-tutorial/IC799951.png "temel bilgileri")
   
   a. İçinde **e-posta adresi** metin Britta Simon e-posta adresini yazın **brittasimon@contoso.com**.
   
   b. İçinde **ad** metin kutusuna **ad** kullanıcının **Britta**.
   
   c. İçinde **Soyadı** metin kutusuna **Soyadı** kullanıcının **Simon**.

4. Tıklayın **kullanıcı ekleme**.
  
   >[!NOTE]
   >Azure AD hesap sahibinin e-posta alır ve bunu etkinleştirilmeden önce hesabını onaylamak için bağlantıyı izleyin. Azure AD kullanıcı hesapları sağlamak için herhangi bir Citrix ShareFile kullanıcı hesabı oluşturma araçları veya Citrix ShareFile tarafından sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Citrix ShareFile erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Citrix ShareFile Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Citrix ShareFile**.

    ![Citrix ShareFile bağlantıya uygulamalar listesi](./media/sharefile-tutorial/tutorial_sharefile_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Citrix ShareFile kutucuğa tıkladığınızda, otomatik olarak Citrix ShareFile uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sharefile-tutorial/tutorial_general_01.png
[2]: ./media/sharefile-tutorial/tutorial_general_02.png
[3]: ./media/sharefile-tutorial/tutorial_general_03.png
[4]: ./media/sharefile-tutorial/tutorial_general_04.png

[100]: ./media/sharefile-tutorial/tutorial_general_100.png

[200]: ./media/sharefile-tutorial/tutorial_general_200.png
[201]: ./media/sharefile-tutorial/tutorial_general_201.png
[202]: ./media/sharefile-tutorial/tutorial_general_202.png
[203]: ./media/sharefile-tutorial/tutorial_general_203.png
