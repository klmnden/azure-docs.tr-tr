---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Cezanne ik yazılım | Microsoft Docs'
description: Azure Active Directory ve Cezanne ik yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 62b42e15-c282-492d-823a-a7c1c539f2cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jeedes
ms.openlocfilehash: 60133dd6d541500db448cf107dd3c0ab193a03f7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048696"
---
# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Öğretici: Azure Active Directory Cezanne ik yazılım ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Cezanne ik yazılım tümleştirme konusunda bilgi edinin.

Azure AD ile Cezanne ik yazılım tümleştirme ile aşağıdaki avantajları sağlar:

- Cezanne ik yazılım erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) Cezanne ik yazılımı açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Cezanne ik yazılımıyla yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Cezanne ik yazılım çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Cezanne ik yazılım ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Galeriden Cezanne ik yazılım ekleme
Azure AD'de Cezanne ik yazılım tümleştirmesini yapılandırmak için Cezanne ik yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Cezanne ik yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Cezanne ik yazılım**seçin **Cezanne ik yazılım** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Cezanne ik yazılım](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Cezanne ik yazılım'ın "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne Cezanne ik yazılım karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Cezanne ik yazılımla ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Cezanne ik yazılımda değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Cezanne ik yazılım ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Cezanne ik yazılım test kullanıcısı oluşturma](#create-a-cezannehrsoftware-test-user)**  - kullanıcı Azure AD gösterimini bağlı Cezanne ik yazılım Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Cezanne ik yazılım uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Cezanne ik yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Cezanne ik yazılım** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. Üzerinde **Cezanne ik yazılım etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Cezanne ik yazılım etki alanı ve URL'ler tek oturum açma bilgileri](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://w3.cezanneondemand.com/CezanneOnDemand/-/<tenantidentifier>`

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://w3.cezanneondemand.com/CezanneOnDemand/`

    c. İçinde **yanıt URL'si** metin kutusuna URL'yi yazın: `https://w3.cezanneondemand.com:443/cezanneondemand/-/<tenantidentifier>/Saml/samlp`
    
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve yanıt URL'si ile güncelleştirin. İlgili kişi [Cezanne ik yazılım istemcisi Destek ekibine](https://cezannehr.com/services/support/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/cezannehrsoftware-tutorial/tutorial_general_400.png)

6. Üzerinde **Cezanne ik yazılım yapılandırma** bölümünde **Cezanne ik yazılımı Yapılandır** açmak için **yapılandırma oturum açma** penceresi.

    ![İK Cezanne yazılım yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png)

7. Ekranı aşağı kaydırarak **hızlı başvuru** bölümü. Kopyalama **SAML çoklu oturum açma hizmet URL'si ve SAML varlık kimliği** gelen **hızlı başvuru bölümü.**

    ![İK Cezanne yazılım yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure1.png)

8. Farklı bir web tarayıcı penceresinde Cezanne ik yazılım kiracınıza yönetici olarak oturum.

9. Sol gezinti bölmesinde **sistemi Kurulum**. Git **güvenlik ayarları**. Ardından gidin **çoklu oturum açma yapılandırması**.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

10. İçinde **şu çoklu oturum açma (SSO) hizmet kullanarak oturum açmasına imkan tanıyın** paneli, onay **SAML 2.0** kutusunda ve seçin **Gelişmiş Yapılandırma** seçeneği.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

11. Tıklayın **yeni Ekle** düğmesi.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

12. Aşağıdaki adımları gerçekleştirin **SAML 2.0 kimlik SAĞLAYICISI** bölümü.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. Kimlik sağlayıcınız olarak adını **görünen ad**.

    b. İçinde **varlığı tanımlayıcısı** metin değerini yapıştırın **SAML varlık kimliği** hangi Azure portaldan kopyaladığınız. 

    c. Değişiklik **SAML bağlama** 'Gönderisinin'.

    d. İçinde **güvenlik belirteci Hizmeti uç noktası** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** hangi Azure portaldan kopyaladığınız.

    e. Kullanıcı Kimliği öznitelik adı metin kutusuna girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. Tıklayın **karşıya** Azure portalından indirilen sertifikayı karşıya yüklemek için simge.
    
    g. **Tamam** düğmesine tıklayın. 

13. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/cezannehrsoftware-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/cezannehrsoftware-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/cezannehrsoftware-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/cezannehrsoftware-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Cezanne ik yazılım test kullanıcısı oluşturma

Azure AD kullanıcılarının Cezanne ik yazılımına oturum etkinleştirmek için bunlar Cezanne ik yazılımına sağlanması gerekir. Cezanne ik söz konusu olduğunda, sağlama elle bir görevin yazılımdır.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1.  Cezanne ik yazılım şirket sitenize yönetici olarak oturum açın.

2.  Sol gezinti bölmesinde **sistemi Kurulum**. Git **kullanıcıları yönetme**. Ardından gidin **yeni kullanıcı Ekle**.

    ![Yeni kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "yeni kullanıcı")

3.  Üzerinde **kişi ayrıntıları** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "yeni kullanıcı")
    
    a. Ayarlama **iç kullanıcı** OFF olarak.
    
    b. İçinde **ad** metin kutusu, kullanıcının ilk adını yazın ister **Britta**.  
 
    c. İçinde **Soyadı** metin kullanıcı adının türünü ister **Simon**.
    
    d. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister Brittasimon@contoso.com.

4.  Üzerinde **hesap bilgileri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "yeni kullanıcı")
    
    a. İçinde **kullanıcıadı** metin kutusuna kullanıcı e-posta türünü ister Brittasimon@contoso.com.
    
    b. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.
    
    c. Seçin **ik Professional** olarak **güvenlik rolü**.
    
    d. **Tamam**’a tıklayın.

5. Gidin **çoklu oturum açma** sekmenize **yeni Ekle** içinde **SAML 2.0 tanımlayıcıları** alan.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "kullanıcı")

6. Kimlik sağlayıcınızı seçin **kimlik sağlayıcısı** ve metin kutusundaki **kullanıcı tanımlayıcısı**, Britta Simon hesabının e-posta adresi girin.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "kullanıcı")
    
7. Tıklayın **Kaydet** düğmesi.

    ![Kullanıcı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "kullanıcı")

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Cezanne ik yazılıma erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Cezanne ik yazılımı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Cezanne ik yazılım**.

    ![Uygulamalar listesinde Cezanne ik yazılım bağlantı](./media/cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Cezanne ik yazılım kutucuğa tıkladığınızda, otomatik olarak Cezanne ik yazılım uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/cezannehrsoftware-tutorial/tutorial_general_203.png

