---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Salesforce | Microsoft Docs'
description: Azure Active Directory ile Salesforce arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 36f1bd9c11c8932968a3501ef22fdb7153411256
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867570"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Öğretici: Azure Active Directory Salesforce ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Salesforce tümleştirme konusunda bilgi edinin.

Azure AD ile Salesforce tümleştirme ile aşağıdaki avantajları sağlar:

- Salesforce erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Salesforce'a (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Salesforce yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir Salesforce çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Salesforce galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce galeri ekleme

Azure AD'de Salesforce tümleştirmesini yapılandırmak için Salesforce Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Salesforce Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Salesforce**seçin **Salesforce** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Salesforce](./media/salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Salesforce "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek çalışmak için oturum açma için Azure AD ne karşılık gelen kullanıcı salesforce'ta bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve salesforce'ta ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Salesforce'ta değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Salesforce ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Salesforce test kullanıcısı oluşturma](#create-a-salesforce-test-user)**  - kullanıcı Azure AD gösterimini bağlı Salesforce Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Salesforce uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Salesforce yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Salesforce** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

    ![Çoklu oturum açma bağlantısı yapılandırma](./media/salesforce-tutorial/tutorial_general_300.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma bağlantısı yapılandırma](./media/salesforce-tutorial/tutorial_general_301.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.
   
    ![Çoklu oturum açma bağlantısı yapılandırma](./media/salesforce-tutorial/tutorial_general_302.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Salesforce etki alanı ve URL'ler tek oturum açma bilgileri](./media/salesforce-tutorial/tutorial_salesforce_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak değeri yazın:

    Kuruluş hesabı: `https://<subdomain>.my.salesforce.com`

    Geliştirici hesabı: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak değeri yazın:

    Kuruluş hesabı: `https://<subdomain>.my.salesforce.com`

    Geliştirici hesabı: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri tanımlayıcısı ve gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Salesforce müşteri destek ekibi](https://help.salesforce.com/support) bu değerleri almak için.

6. Üzerinde **SAML imzalama sertifikası** bölümü tıklatın üzerinde **indirme** indirmek için **Federasyon meta verileri XML** ve bilgisayarınızda xml dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/salesforce-tutorial/tutorial_salesforce_certificate.png) 

7. Salesforce yönetici hesabınızda oturum açın ve tarayıcı içinde yeni bir sekme açın.

8. Tıklayarak **Kurulum** altında **ayarlar simgesine** sayfanın sağ üst köşesinde.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/configure1.png)

9. Ekranı aşağı kaydırarak **ayarları** Gezinti bölmesinden **kimlik** ilgili bölümü genişletin. Ardından **çoklu oturum açma ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-admin-sso.png)

10. Üzerinde **çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Çoklu oturum açma ayarlarını Salesforce hesabınız için etkinleştirmek erişemiyorsanız başvurmanız gerekebilir [Salesforce müşteri destek ekibi](https://help.salesforce.com/support).

11. Seçin **SAML etkin**ve ardından **Kaydet**.

      ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-enable-saml.png)

12. SAML çoklu oturum açma ayarlarınızı yapılandırmak için tıklayın **yeni meta veri dosyasından**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-admin-sso-new.png)

13. Tıklayın **Dosya Seç** Azure portal'ı seçin ve indirilen meta veri XML dosyasını karşıya yüklemek için **Oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/xmlchoose.png)

14. Üzerinde **SAML çoklu oturum açma ayarları** sayfasında alanları otomatik olarak doldurur ve Kaydet'e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/salesforcexml.png)

15. Sol gezinti bölmesinde üzerinde Salesforce **şirket ayarları** ilgili bölümü genişletin ve ardından **My Domain**.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-my-domain.png)

16. Ekranı aşağı kaydırarak **kimlik doğrulama Yapılandırması** bölümünde ve'ı tıklatın **Düzenle** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-edit-auth-config.png)

17. İçinde **kimlik doğrulama Yapılandırması** bölümünde, onay **AzureSSO** olarak **kimlik doğrulaması listeleme** SAML SSO yapılandırmasını ve ardından **Kaydet** .

    ![Çoklu oturum açmayı yapılandırın](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Birden fazla kimlik doğrulama hizmeti seçili ise, kullanıcıların hangi kimlik doğrulama hizmeti ile çoklu oturum açma Salesforce ortamınıza başlatma oturum ister seçmek için istenir. Olmasını istemediğiniz sonra yapmanız gerekenler **diğer tüm kimlik doğrulama hizmetleri işaretlemeyin**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![Azure AD kullanıcısı oluşturun][100]

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/salesforce-tutorial/create_aaduser_01.png) 

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/salesforce-tutorial/create_aaduser_02.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-a-salesforce-test-user"></a>Salesforce test kullanıcısı oluşturma

Bu bölümde, Salesforce'ta Britta Simon adlı bir kullanıcı oluşturuldu. Salesforce tam zamanında sağlama, varsayılan olarak etkin olduğu destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Salesforce erişmeye çalıştığında, Salesforce'ta bir kullanıcı zaten mevcut değilse yeni bir tane oluşturulur. Salesforce da destekler otomatik kullanıcı hazırlama, daha fazla ayrıntı bulabilirsiniz [burada](salesforce-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Salesforce'a erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Salesforce'a atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Salesforce**.

    ![Uygulamalar listesinde Salesforce bağlantısı](./media/salesforce-tutorial/tutorial_salesforce_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Kullanıcı Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Salesforce kutucuğa tıkladığınızda, otomatik olarak Salesforce uygulamanızı açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-tutorial/tutorial_general_203.png