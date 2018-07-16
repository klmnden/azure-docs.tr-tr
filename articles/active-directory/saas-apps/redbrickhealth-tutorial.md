---
title: 'Öğretici: Azure Active Directory RedBrick sistem durumu ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve RedBrick sistem durumu arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26290c65-9aa3-42ab-8ba5-901b14dc8e73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jeedes
ms.openlocfilehash: d852b30568acff4f1d56a1e208528e8c90b5b1f0
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051787"
---
# <a name="tutorial-azure-active-directory-integration-with-redbrick-health"></a>Öğretici: Azure Active Directory tümleştirmesiyle RedBrick sistem durumu

Bu öğreticide, RedBrick durumu Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

RedBrick sistem durumu, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- RedBrick sistem erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan RedBrick sağlık verilerinin (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RedBrick sistem durumunu yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik RedBrick sistem durumu çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden RedBrick durum ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-redbrick-health-from-the-gallery"></a>Galeriden RedBrick durum ekleme
Azure AD'de RedBrick sistem durumu tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden RedBrick sistem durumu eklemeniz gerekir.

**Galeriden RedBrick sistem durumu eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **RedBrick sistem durumu**seçin **RedBrick sistem durumu** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde redBrick sistem durumu](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma RedBrick "Britta Simon" adlı bir test kullanıcısı üzerinde temel sistem durumu test.

Tek çalışmak için oturum açma için Azure AD ne RedBrick sistem durumu karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı RedBrick sistem arasında bir bağlantı ilişki kurulması gerekir.

RedBrick durumunu değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma RedBrick sistem durumu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[RedBrick sistem durumu test kullanıcısı oluşturma](#create-a-redbrick-health-test-user)**  - bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı RedBrick sistem durumu sağlamak için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma RedBrick sistem durumu uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma ile RedBrick sistem durumunu yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **RedBrick sistem durumu** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_samlbase.png)

3. Üzerinde **RedBrick sistem durumu etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri redBrick etki sağlık ve URL'leri](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL: `http://www.redbrickhealth.com`
    
    b. İçinde **yanıt URL'si** metin kutusuna bir URL: `https://sso-intg.redbrickhealth.com/sp/ACS.saml2`
    
    Üretim ortamı için: `https://sso.redbrickhealth.com/sp/ACS.saml2`

    c. Tıklayın **Gelişmiş URL ayarlarını göster**.
    
    ![Çoklu oturum açma bilgileri redBrick etki sağlık ve URL'leri](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url1.png)

    d. İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak: `https://api-sso2.redbricktest.com/identity/sso/nbound?target=https://vanity9-sso2.redbrickdev.com/portal&connection=<companyname>conn1`
    
    > [!NOTE] 
    > Geçiş durumu değeri gerçek değil. Bu değer ile gerçek geçiş durumunu güncelleştirin. İlgili kişi [RedBrick sistem durumu Destek ekibine](https://home.redbrickhealth.com/contact/) bu değeri alınamıyor.

4. RedBrick sistem durumu uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Bu talep, müşteriye özgü olan ve ihtiyacınıza bağlıdır. Aşağıdaki isteğe bağlı taleplerdir örnek yalnızca, uygulamanız için yapılandırabilirsiniz. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/redbrickhealth-tutorial/attribute.png)

5. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| ----------------|
    | Asıl adı | ********** |
    | istemci kimliği | ********** |
    | katılımcı kimliği | ********** |
    
    > [!NOTE]
    > Bu değerler yalnızca başvuru amaçlı. Kuruluşunuzun gereksinime uygun şekilde öznitelikleri tanımlamak gerekir. Lütfen başvurun [RedBrick sistem durumu Destek ekibine](https://home.redbrickhealth.com/contact/) gerekli talepler hakkında daha fazla bilgi almak için.
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.
    
    ![Çoklu oturum açmayı yapılandırın](./media/redbrickhealth-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/redbrickhealth-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

6. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_certificate.png) 

7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/redbrickhealth-tutorial/tutorial_general_400.png)

8. Üzerinde **RedBrick durum yapılandırmasını** bölümünde **yapılandırma RedBrick durumu** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![RedBrick sistem durumu yapılandırması](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_configure.png) 

9. Çoklu oturum açmayı yapılandırma **RedBrick sistem durumu** tarafı, indirilen göndermek için ihtiyacınız **Certificate(Base64)** ve **SAML varlık kimliği** için [RedBrick sistem durumu Destek](https://home.redbrickhealth.com/contact/). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/redbrickhealth-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/redbrickhealth-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/redbrickhealth-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/redbrickhealth-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-redbrick-health-test-user"></a>RedBrick sistem durumu test kullanıcısı oluşturma

Bu bölümde, Britta Simon RedBrick durumunu adlı bir kullanıcı oluşturun. Çalışmak [RedBrick sistem durumu Destek ekibine](https://home.redbrickhealth.com/contact/) RedBrick sistem durumu platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, RedBrick sistem durumu için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon RedBrick sağlık atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **RedBrick sistem durumu**.

    ![Uygulamalar listesinde RedBrick durumu bağlantısı](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli RedBrick durumu kutucuğuna tıkladığınızda, otomatik olarak RedBrick sistem durumu uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/redbrickhealth-tutorial/tutorial_general_01.png
[2]: ./media/redbrickhealth-tutorial/tutorial_general_02.png
[3]: ./media/redbrickhealth-tutorial/tutorial_general_03.png
[4]: ./media/redbrickhealth-tutorial/tutorial_general_04.png

[100]: ./media/redbrickhealth-tutorial/tutorial_general_100.png

[200]: ./media/redbrickhealth-tutorial/tutorial_general_200.png
[201]: ./media/redbrickhealth-tutorial/tutorial_general_201.png
[202]: ./media/redbrickhealth-tutorial/tutorial_general_202.png
[203]: ./media/redbrickhealth-tutorial/tutorial_general_203.png

