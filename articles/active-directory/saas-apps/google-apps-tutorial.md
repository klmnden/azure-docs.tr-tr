---
title: 'Öğretici: Azure Active Directory Tümleştirme ile G Suite | Microsoft Docs'
description: G Suite ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: jeedes
ms.openlocfilehash: c8ec2b8e312b9bedbd19cb203c0a59177c7fd6a5
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265021"
---
# <a name="tutorial-azure-active-directory-integration-with-g-suite"></a>Öğretici: Azure Active Directory G Suite ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile G Suite tümleştirme konusunda bilgi edinin.

G Suite Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- G Suite erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için G Suite açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

G Suite ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir G Suite çoklu oturum açma etkin
- Google Apps aboneliği veya Google Cloud Platform abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular
1.  **S: Azure AD ile tümleştirme Google Cloud Platform SSO Bu tümleştirme desteği mu?**
    
    C: Evet. Google Cloud Platform ve Google Apps, aynı kimlik doğrulaması platformunun paylaşın. Bu nedenle GCP tümleştirme yapmak için Google Apps ile SSO yapılandırmanız gerekir.


2. **S: Azure AD çoklu oturum açma ile uyumlu Chromebooks ve diğer Chrome cihazları mı?**
   
    C: Evet, kullanıcılar, Azure AD kimlik bilgilerini kullanarak Chromebook cihazlarını oturum açabilir. Bkz. Bu [G Suite Destek makalesi](https://support.google.com/chrome/a/answer/6060880) neden hakkında bilgi için kimlik bilgilerini iki kez kullanıcılardan.

3. **S: çoklu oturum açma etkinleştirebilirim, kullanıcıların Google Classroom, GMail, Google Drive, YouTube ve benzeri gibi tüm Google ürün oturum açmak için Azure AD kimlik bilgilerini kullanmanız mümkün olacak mı?**
   
    C: Evet, bağlı olarak [hangi G Suite](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) etkinleştirmek veya kuruluşunuz için devre dışı bırakmak seçin.

4. **Yalnızca bir alt kümesini G Suite Kullanıcılarım için çoklu oturum açmayı etkinleştirme miyim?**
   
    C: Hayır, çoklu oturum açmayı üzerinde hemen kapatma kendi Azure AD kimlik bilgileriyle kimlik doğrulaması tüm G Suite kullanıcılarınız gerektirir. G Suite sahip birden çok kimlik sağlayıcı desteklemediğinden, G Suite ortamınız için kimlik sağlayıcısı ya da Azure AD olabilir veya Google--ikisi aynı anda.

5. **S: bir kullanıcının Windows oturum açtığı, otomatik olarak G Suite için bir parola girmeniz istenir alma olmadan kimlik doğrulaması misiniz?**
   
    Y: Bu senaryoyu etkinleştirmek için iki seçenek vardır. İlk olarak, kullanıcılar Windows 10 cihazları oturum [Azure Active Directory Join](../device-management-introduction.md). Alternatif olarak, kullanıcıların etki alanına katılmış bir şirket içi Active Directory'ye Azure ad çoklu oturum açma için etkinleştirilmiş olan Windows cihazları oturum bir [Active Directory Federasyon Hizmetleri (AD FS)](../connect/active-directory-aadconnect-user-signin.md) dağıtım. Azure AD arasında çoklu oturum açmayı etkinleştirmek için aşağıdaki öğreticiye adımları gerçekleştirmek iki seçenek de gerektirir ve G Suite.

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. G Suite galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-g-suite-from-the-gallery"></a>G Suite galeri ekleme
Azure AD'de G Suite tümleştirmesini yapılandırmak için G Suite Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**G Suite Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **G Suite**seçin **G Suite** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde G Suite](./media/google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve G Suite "Britta Simon" adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.

Tek iş için oturum açma için Azure AD ne G Suite karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve G Suite ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

G Suite içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma G Suite ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[G Suite test kullanıcısı oluşturma](#create-a-g-suite-test-user)**  - kullanıcı Azure AD gösterimini bağlı G Suite Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve G Suite uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma G Suite ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **G Suite** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. Üzerinde **G Suite etki alanı ve URL'ler** bölüm için yapılandırmak istiyorsanız **Gmail** aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'ler tek oturum açma bilgileri](./media/google-apps-tutorial/tutorial_googleapps_urlgmail.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://mail.google.com`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
 
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [G Suite istemci Destek ekibine](https://www.google.com/contact/) bu değerleri almak için.

4. Üzerinde **G Suite etki alanı ve URL'ler** bölüm için yapılandırmak istiyorsanız **Google Cloud Platform** aşağıdaki adımları gerçekleştirin:

    ![G Suite etki alanı ve URL'ler tek oturum açma bilgileri](./media/google-apps-tutorial/tutorial_googleapps_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://www.google.com/a/<yourdomain.com>/ServiceLogin?continue=https://console.cloud.google.com `

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: 
    | |
    |--|
    | `google.com/a/<yourdomain.com>` |
    | `google.com` |
    | `http://google.com` |
    | `http://google.com/a/<yourdomain.com>` |
    
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [G Suite istemci Destek ekibine](https://www.google.com/contact/) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/google-apps-tutorial/tutorial_googleapps_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/google-apps-tutorial/tutorial_general_400.png)

7. Üzerinde **G Suite Yapılandırması** bölümünde **G Suite'i yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML çoklu oturum açma hizmet URL'si ve değişiklik parola URL'si** gelen **hızlı başvuru bölümü.**

    ![G Suite yapılandırması](./media/google-apps-tutorial/tutorial_googleapps_configure.png) 

8. Tarayıcınızda yeni bir sekme açın ve oturum [G Suite Yönetici Konsolu](http://admin.google.com/) yönetici hesabınızı kullanarak.

9. Tıklayın **güvenlik**. Bağlantıyı görmüyorsanız, bunun altında gizlenebilir **diğer denetimler** ekranın alt kısmındaki menü.
   
    ![Güvenlik'e tıklayın.][10]

10. Üzerinde **güvenlik** sayfasında **çoklu oturum açmayı (SSO) ayarlayın.**
   
    ![SSO tıklayın.][11]

11. Şu yapılandırma değişiklikleri gerçekleştirin:
   
    ![SSO yapılandırma][12]
   
    a. Seçin **üçüncü taraf kimlik sağlayıcısı ile Kurulum SSO**.

    b. İçinde **oturum açma sayfası URL'si** alan G Suite'te, değerini yapıştırın **çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **sayfa oturum kapatma URL'si** alan G Suite'te, değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız. 

    d. İçinde **değiştirme parola URL'si** alan G Suite'te, değerini yapıştırın **değiştirme parola URL'si** , Azure Portalı'ndan kopyaladığınız. 

    e. G Suite'teki için **doğrulama sertifikası**, Azure portalından indirdiğiniz sertifikasını karşıya yükleyin.

    f. Seçin **belirli bir etki alanı gönderici kullanın**.

    g. Tıklayın **değişiklikleri kaydetmek**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/google-apps-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/google-apps-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/google-apps-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/google-apps-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-g-suite-test-user"></a>G Suite test kullanıcısı oluşturma

Bu bölümün amacı, G Suite yazılımı Britta Simon adlı bir kullanıcı oluşturmaktır. G Suite otomatik sağlamayı, varsayılan olarak etkin olan destekler. Bu bölümde hiçbir şey yoktur. Bir kullanıcı, G Suite yazılımı zaten yoksa, G Suite yazılımı erişmeye çalıştığında yeni bir tane oluşturulur.

>[!NOTE] 
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Google Destek ekibine](https://www.google.com/contact/).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, G Suite erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**G Suite Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **G Suite**.

    ![Uygulamalar listesini G Suite bağlantıdaki](./media/google-apps-tutorial/tutorial_googleapps_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde G Suite kutucuğa tıkladığınızda, otomatik olarak G Suite uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/googleapps-tutorial/tutorial_general_01.png
[2]: ./media/googleapps-tutorial/tutorial_general_02.png
[3]: ./media/googleapps-tutorial/tutorial_general_03.png
[4]: ./media/googleapps-tutorial/tutorial_general_04.png

[100]: ./media/googleapps-tutorial/tutorial_general_100.png

[200]: ./media/googleapps-tutorial/tutorial_general_200.png
[201]: ./media/googleapps-tutorial/tutorial_general_201.png
[202]: ./media/googleapps-tutorial/tutorial_general_202.png
[203]: ./media/googleapps-tutorial/tutorial_general_203.png
[10]: ./media/googleapps-tutorial/gapps-security.png
[11]: ./media/googleapps-tutorial/security-gapps.png
[12]: ./media/googleapps-tutorial/gapps-sso-config.png

