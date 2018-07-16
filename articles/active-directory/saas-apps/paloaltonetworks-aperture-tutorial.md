---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Palo Alto Networks - açıklık | Microsoft Docs'
description: Azure Active Directory ve Palo Alto Networks - açıklık arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a5ea18d3-3aaf-4bc6-957c-783e9371d0f1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 20292c06cf336a0245b5f5db6b1cb4894df0f1ae
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051590"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---aperture"></a>Öğretici: Azure Active Directory tümleştirmesiyle Palo Alto Networks - açıklık

Bu öğreticide, Azure Active Directory (Azure AD) ile açıklık Palo Alto Networks - tümleştirme konusunda bilgi edinin.

Palo Alto Networks tarafından sağlanan-açıklık Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Palo Alto Networks - açıklık erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan Palo Alto Networks - Azure AD hesaplarıyla (çoklu oturum açma) açıklık için açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Palo Alto Networks - açıklığı, Azure AD tümleştirmesi yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Palo Alto Networks - açıklık çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Palo Alto Networks tarafından sağlanan - galerisinden açıklık ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-palo-alto-networks---aperture-from-the-gallery"></a>Palo Alto Networks tarafından sağlanan - galerisinden açıklık ekleme
Palo Alto Networks - Azure AD'ye açıklık tümleştirmesini yapılandırmak için listenize yönetilen SaaS uygulamalarının Diyafram galerisinden Palo Alto Networks - eklemeniz gerekir.

**Palo Alto Networks - galerisinden açıklık eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Palo Alto Networks - açıklık**seçin **Palo Alto Networks - açıklık** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Palo Alto Networks tarafından sağlanan-sonuç listesinde açıklık](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile test etme - açıklık "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne Palo Alto Networks - açıklık karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Palo Alto Networks - ilgili kullanıcı arasında bir bağlantı ilişki açıklık kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Palo Alto Networks ile-test etmek için açıklığı, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Palo Alto Networks - açıklık test kullanıcısı oluşturma](#create-a-palo-alto-networks---aperture-test-user)**  - Palo Alto Networks - kullanıcı Azure AD gösterimini bağlı açıklık Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, Palo Alto Networks - açıklık uygulama yapılandırın.

**Palo Alto Networks - açıklığı, Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Palo Alto Networks - açıklık** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_samlbase.png)

3. Üzerinde **Palo Alto Networks - açıklık etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Palo Alto Networks tarafından sağlanan-açıklık etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/auth`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Palo Alto Networks tarafından sağlanan-açıklık etki alanı ve URL'ler çoklu oturum açma bilgileri](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/sign_in`
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Palo Alto Networks - açıklık istemci Destek ekibine](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) bu değerleri almak için. 

5. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/paloaltonetworks-aperture-tutorial/tutorial_general_400.png)


7. Üzerinde **Palo Alto Networks - açıklık yapılandırma** bölümünde **Palo Alto Networks yapılandırın - açıklık** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Aşağıdaki Yapılandır bağlantısını](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_configure.png)

8. Bir başka web tarayıcı penceresinde Palo Alto Networks - açıklık yönetici olarak oturum açın.

9. Üst menü çubuğunda **ayarları**.

    ![Ayarlar sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_settings.png)

10. Gidin **uygulama** bölümünde **kimlik doğrulaması** menüsünde sol tarafındaki form.

    ![Kimlik doğrulama sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_auth.png)
    
11. Üzerinde **kimlik doğrulaması** sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![Kimlik doğrulama sekmesi](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_singlesignon.png)

    a. Denetleme **etkinleştirme tek oturum-On(Supported SSP Providers are Okta, Onelogin)** gelen **çoklu oturum açma** alan.

    b. İçinde **kimlik sağlayıcı kimliği** metin değerini yapıştırın **SAML varlık kimliği**, hangi Azure Portalı'ndan kopyaladığınız.

    c. Tıklayın **Dosya Seç** Azure AD'den yüklenen sertifikayı karşıya yüklemek için **kimlik sağlayıcısı sertifikası** alan.

    d. İçinde **kimlik sağlayıcısı SSO URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    e. IDP bilgileri gözden geçirin **açıklık bilgisi** bölümünde ve'deki Sertifika Yükle **açıklık anahtar** alan.

    f. **Kaydet**’e tıklayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/paloaltonetworks-aperture-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/paloaltonetworks-aperture-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/paloaltonetworks-aperture-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/paloaltonetworks-aperture-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-palo-alto-networks---aperture-test-user"></a>Palo Alto Networks - açıklık test kullanıcısı oluşturma

Bu bölümde, Britta Simon Palo Alto Networks - açıklık adlı bir kullanıcı oluşturun. Çalışmak [Palo Alto Networks - açıklık istemci Destek ekibine](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) Palo Alto Networks - açıklık platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Palo Alto Networks - açıklık erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Palo Alto Networks - açıklığı, Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Palo Alto Networks - açıklık**.

    ![-Açıklık bağlantı uygulamalar listesinde Palo Alto ağlar](./media/paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Palo Alto Networks - açıklık kutucuk erişim Paneli'nde tıklattığınızda, otomatik olarak, Palo Alto Networks - açıklık uygulama için açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_01.png
[2]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_02.png
[3]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_03.png
[4]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_04.png

[100]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_100.png

[200]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_200.png
[201]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_201.png
[202]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_202.png
[203]: ./media/paloaltonetworks-aperture-tutorial/tutorial_general_203.png

