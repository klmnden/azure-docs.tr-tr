---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Keeper parola Yöneticisi & dijital kasa | Microsoft Docs'
description: Azure Active Directory ve Keeper parola Yöneticisi & dijital kasa arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: fb57ab0f746fcad702c18032b9da1a9ec820b809
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052426"
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a>Öğretici: Azure Active Directory parola Yöneticisi Keeper & dijital kasa ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Keeper parola Yöneticisi & dijital kasası tümleştirmeyi öğrenin.

Parola Yöneticisi Keeper & dijital kasası, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Parola Yöneticisi Keeper & dijital kasa erişimi, Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Keeper parola Yöneticisi için dijital kasası (çoklu oturum açma) & açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Keeper parola Yöneticisi ve dijital kasa ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir parola Yöneticisi Keeper & dijital kasa çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Parola Yöneticisi Keeper & dijital kasa galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-keeper-password-manager--digital-vault-from-the-gallery"></a>Parola Yöneticisi Keeper & dijital kasa galeri ekleme
Azure AD'de parola Yöneticisi Keeper & dijital kasası tümleştirmesini yapılandırmak için parola Yöneticisi Keeper & dijital kasa Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Parola Yöneticisi Keeper & dijital kasa Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Keeper parola Yöneticisi & dijital kasası**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. Sonuçlar panelinde seçin **Keeper parola Yöneticisi & dijital kasası**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Keeper parola Yöneticisi ve dijital "Britta Simon." adlı bir test kullanıcı tabanlı kasa ile test etme

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Keeper parola Yöneticisi'ni ve kasa dijital bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Keeper parola Yöneticisi ve dijital kasa arasında bir bağlantı ilişki kurulması gerekir.

Parola Yöneticisi Keeper & dijital kasa değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Keeper parola Yöneticisi ve dijital kasa ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Parola Yöneticisi Keeper & dijital kasa bir test kullanıcısı oluşturma](#creating-a-keeperpasswordmanager-test-user)**  - Britta simon'un Keeper parola Yöneticisi & kullanıcı Azure AD gösterimini bağlı dijital kasa içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Keeper parola Yöneticisi & dijital kasa uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Keeper parola Yöneticisi ve dijital kasa ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Keeper parola Yöneticisi & dijital kasası** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. Üzerinde **Keeper parola Yöneticisi & dijital kasa etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`

    c. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://{SSO CONNECT SERVER}/sso-connect`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Keeper parola Yöneticisi & dijital kasası istemcisinin Destek ekibine](https://keepersecurity.com/contact.html) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. Üzerinde **Keeper parola Yöneticisi ve dijital kasa yapılandırma** bölümünde **Keeper parola Yöneticisi'ni yapılandırma ve dijital kasası** açmak için **yapılandırma oturum açma** pencere. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. Çoklu oturum açmayı yapılandırma **Keeper parola Yöneticisi ve dijital kasa yapılandırma** , verilen yönergeleri izleyin, yan [Keeper destek Kılavuzu](https://keepersecurity.com/assets/pdf/KeeperSSOConnect_v11.pdf).

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a>Parola Yöneticisi Keeper & dijital kasa bir test kullanıcısı oluşturma

Azure AD kullanıcılarının Keeper parola Yöneticisi için dijital kasa & oturum açmayı etkinleştirmek için bunlar Keeper parola Yöneticisi & dijital kasa sağlanması gerekir. Uygulama, zaman kullanıcı sağlamayı ve kimlik doğrulaması kullanıcılar uygulamaya otomatik olarak oluşturulacak sonra sadece destekler. Sizinle iletişim [Keeper Destek](https://keepersecurity.com/contact.html), kullanıcıların el ile ayarlamak istiyorsanız.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma için parola Yöneticisi Keeper & dijital kasa erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Keeper parola Yöneticisi için dijital kasa & atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Keeper parola Yöneticisi & dijital kasası**.

    ![Çoklu oturum açmayı yapılandırın](./media/keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Keeper parola Yöneticisi & dijital kasası kutucuğa tıkladığınızda, oturum açma sayfası Keeper parola Yöneticisi & dijital kasa uygulamanın almanız gerekir. Kimlik başarıyla doğrulandıktan sonra uygulamanın içine almanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/keeperpasswordmanager-tutorial/tutorial_general_203.png

