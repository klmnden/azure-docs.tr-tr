---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle TOPdesk - güvenli | Microsoft Docs'
description: Azure Active Directory ve TOPdesk - güvenli arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 2da2a2cae3993f7c29726b842db6767d4300cacc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045266"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Öğretici: Azure Active Directory tümleştirmesiyle TOPdesk - güvenli

Bu öğreticide, şunların nasıl tümleştireceğinizi TOPdesk - Azure Active Directory (Azure AD) ile güvenli hale getirme.

TOPdesk - güvenli tümleştirme ile Azure AD ile aşağıdaki avantajları sağlar:

- TOPdesk - güvenli erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan TOPdesk için - güvenli (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile TOPdesk - yapılandırmak için güvenli, aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- TOPdesk - bir aboneliği etkin güvenli çoklu oturum açma

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. TOPdesk - ekleme Galeriden güvenliğini sağlama
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-topdesk---secure-from-the-gallery"></a>TOPdesk - ekleme Galeriden güvenliğini sağlama
Güvenli TOPdesk - tümleştirmesini yapılandırmak için Azure AD ile Galeriden yönetilen SaaS listenize uygulamalarının güvenliğini sağlama - TOPdesk eklemek için ihtiyacınız.

**TOPdesk - eklemek için galerideki güvenli, aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **TOPdesk - güvenli**seçin **TOPdesk - güvenli** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![TOPdesk - sonuçlar listesinde güvenliğini sağlama](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile test etme - güvenli "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı TOPdesk - güvenli bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının TOPdesk - güvenli ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

TOPdesk - güvenli, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma TOPdesk ile-test etmek için güvenli, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[TOPdesk - güvenli bir test kullanıcısı oluşturma](#create-a-topdesk---secure-test-user)**  - kullanıcı Azure AD gösterimini bağlı Britta simon'un TOPdesk - güvenli içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, TOPdesk - güvenli uygulama yapılandırın.

**Azure AD çoklu oturum açma - TOPdesk ile yapılandırmak için güvenli, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **TOPdesk - güvenli** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

3. Üzerinde **TOPdesk - güvenli etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri TOPdesk - güvenli etki alanı ve URL'ler](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Yanıt URL'si, bu öğreticinin sonraki bölümlerinde açıklanmıştır. İlgili kişi [TOPdesk - güvenli istemci Destek ekibine](http://www.topdesk.com/us/support) bu değerleri almak için. 

4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/topdesk-secure-tutorial/tutorial_general_400.png)

6. Üzerinde **TOPdesk - güvenli yapılandırma** bölümünde **TOPdesk - güvenli yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![TOPdesk - güvenli yapılandırma](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
7. Oturum açın, **TOPdesk - güvenli** yönetici olarak şirketin site.

8. İçinde **TOPdesk** menüsünü tıklatın **ayarları**.

    ![Ayarları](./media/topdesk-secure-tutorial/ic790598.png "ayarları")

9. Tıklayın **oturum açma ayarları**.

    ![Oturum açma ayarları](./media/topdesk-secure-tutorial/ic790599.png "oturum açma ayarları")

10. Genişletin **oturum açma ayarları** menüsüne ve ardından **genel**.

    ![Genel](./media/topdesk-secure-tutorial/ic790600.png "genel")

11. İçinde **güvenli** bölümünü **SAML oturum açma** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Teknik ayarlarla](./media/topdesk-secure-tutorial/ic790855.png "teknik ayarları")
   
    a. Tıklayın **indirme** ortak meta veri dosyası indirin ve bilgisayarınıza yerel olarak kaydedin.
   
    b. Meta veri dosyası açın ve ardından bulun **AssertionConsumerService** düğümü.
    
    ![Onaylama tüketici hizmeti](./media/topdesk-secure-tutorial/ic790856.png "onaylama tüketici hizmeti")
   
    c. Kopyalama **AssertionConsumerService** değeri, bu değer yanıt URL'si metin kutusuna yapıştırın **TOPdesk - güvenli etki alanı ve URL'ler** bölümü.

12. Bir sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/topdesk-secure-tutorial/ic790606.png "sertifika")
    
    a. Azure Portalı'ndan indirilen meta veri dosyası açın.

    b. Genişletin **verilerde Securitytokenservicetype** sahip düğüm bir **xsi: type** , **beslenir: ApplicationServiceType**.

    c. Değerini kopyalayın **X509Certificate** düğümü.

    d. Kopyalanan Kaydet **X509Certificate** yerel olarak bilgisayarınızda bir dosyadaki değeri.

13. İçinde **genel** bölümünde **Ekle**.
    
    ![Ekleme](./media/topdesk-secure-tutorial/ic790607.png "Ekle")

14. Üzerinde **SAML yapılandırma Yardımcısı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![SAML yapılandırma Yardımcısı](./media/topdesk-secure-tutorial/ic790608.png "SAML yapılandırma Yardımcısı")
    
    a. Azure portalından indirilen meta verileri dosyanızı altında karşıya yüklemek için **Federasyon meta verileri**, tıklayın **Gözat**.

    b. Altında sertifika dosyası karşıya **sertifika (RSA)**, tıklayın **Gözat**.

    c. Aldığınız TOPdesk destek ekibinden altında logosu dosyayı karşıya yüklemeyi **logosu simgesi**, tıklayın **Gözat**.

    d. İçinde **kullanıcı adı özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. İçinde **görünen adı** metin yapılandırmanız için bir ad yazın.

    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/topdesk-secure-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/topdesk-secure-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/topdesk-secure-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/topdesk-secure-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-topdesk---secure-test-user"></a>TOPdesk - güvenli bir test kullanıcısı oluşturma

Azure AD kullanıcılarının TOPdesk - oturum etkinleştirmek için güvenli, bunların TOPdesk - güvenli sağlanması gerekir.  
Söz konusu olduğunda TOPdesk - sağlama elle bir görevin güvenlidir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:
1. Oturum açın, **TOPdesk - güvenli** şirketinizin sitesi yöneticisi olarak.
2. Üstteki menüden **TOPdesk \> yeni \> destek dosyalarını \> işleci**.
   
    ![İşleç](./media/topdesk-secure-tutorial/ic790610.png "işleci")

3. Üzerinde **New işleci** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
    ![New işleci](./media/topdesk-secure-tutorial/ic790611.png "New işleci")
   
    a. Tıklayın **genel** sekmesi.
   
    b. İçinde **Soyadı** kullanıcının Soyadı türü metin ister **Simon**.
   
    c. Seçin bir **Site** hesap **konumu** bölümü.
   
    d. İçinde **oturum açma adı** textbox'ın **TOPdesk oturum açma** bölümünde, bir kullanıcı için oturum açma adı yazın.
   
    e. **Kaydet**’e tıklayın.

> [!NOTE]
> Herhangi diğer TOPdesk - güvenli kullanıcı hesabı oluşturma araçları veya tarafından TOPdesk - AAD kullanıcı hesapları sağlamak için güvenli sağlanan API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açmayı kullanmak için TOPdesk - güvenli erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon - TOPdesk için atamak için güvenli, aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **TOPdesk - güvenli**.

    ![TOPdesk - uygulamalar listesinde güvenli bağlantı](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

TOPdesk - güvenli kutucuk erişim Paneli'nde tıkladığınızda, otomatik olarak imzalanmış, TOPdesk - güvenli uygulama açma.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-secure-tutorial/tutorial_general_203.png

