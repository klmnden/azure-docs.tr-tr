---
title: 'Öğretici: Azure Active Directory Meta ağları Bağlayıcısı ile tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Meta ağları bağlayıcı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2018
ms.author: jeedes
ms.openlocfilehash: 8e27ba7d5b245d8857f0c07bfe2923afe9d7e3a0
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39266218"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Öğretici: Azure Active Directory Tümleştirme Meta ağları Bağlayıcısı

Bu öğreticide, Meta ağları bağlayıcı Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.

Meta ağları Bağlayıcısı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Meta ağları bağlayıcı erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Meta ağları bağlayıcıya (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Meta ağları Bağlayıcısı ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Meta ağları bağlayıcı çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden meta ağları bağlayıcı ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Galeriden meta ağları bağlayıcı ekleme
Azure AD'ye Meta ağları bağlayıcı tümleştirmesini yapılandırmak için Meta ağları bağlayıcı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden meta ağları bağlayıcı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Meta ağları bağlayıcı**seçin **Meta ağları bağlayıcı** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde meta ağları Bağlayıcısı](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Meta ağlar "Britta Simon" adlı bir test kullanıcı tabanlı Bağlayıcısı ile Azure AD çoklu oturum açma testi.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Meta ağları bağlayıcısında bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve ilgili kullanıcı Meta ağları bağlayıcısında arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Meta ağları Bağlayıcısı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Meta ağları bağlayıcı test kullanıcısı oluşturma](#create-a-meta-networks-connector-test-user)**  - kullanıcı Azure AD gösterimini bağlı Meta ağları bağlayıcısında Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Meta ağları bağlayıcı uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Meta ağları Bağlayıcısı ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Meta ağları bağlayıcı** uygulama tümleştirme sayfası, tıklayın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_samlbase.png)

3. Üzerinde **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Meta ağları bağlayıcı etki alanı ve URL'ler tek oturum açma bilgileri](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Meta ağları bağlayıcı etki alanı ve URL'ler tek oturum açma bilgileri](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_url1.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`

    b. İçinde **geçiş durumu** metin kutusuna bir URL şu biçimi kullanarak: `https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Yanıt URL'si gerçek tanımlayıcısı bu değerleri güncelleştirin ve oturum açma URL'si, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

5. Meta ağları bağlayıcı uygulama belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Öznitelik Adı | Öznitelik Değeri | AD ALANI|
    | ---------------| --------------- | -------- |
    | firstName | User.givenName | |
    | Soyadı | User.surname | |
    | emailaddress| User.Mail| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | ad | User.userPrincipalName| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | telefon | User.telephoneNumber | |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. İçinde **Namespace** metin kutusuna, bu satır için gösterilen ad alanı değeri yazın.

    e. Tıklayın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_certificate.png)

8. Üzerinde **Meta ağları Bağlayıcı yapılandırması** bölümünde **Meta ağları bağlayıcı yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_configure.png)

9. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/metanetworksconnector-tutorial/tutorial_general_400.png)

10. Tarayıcınızda yeni bir sekme açın ve Meta ağları Bağlayıcısı yönetici hesabınızda oturum açın.

    > [!NOTE]
    > Güvenli sistem meta ağları Bağlayıcıdır. Bu nedenle, portal erişmeden önce kendi tarafında genel IP adresi beyaz listeye almak gerekir. Genel IP adresi almak için izlemeniz aşağıdaki bağlantıda belirtilen [burada](https://whatismyipaddress.com/). IP adresiniz Gönder [Meta ağları bağlayıcı istemci Destek ekibine](mailto:support@metanetworks.com) IP adresi izin verilenler listesinde almak için.

11. Git **yönetici** seçip **ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure3.png)

12. Emin **Internet trafiğini günlüğe kaydetme** ve **zorla VPN MFA** kapalı olarak ayarlayın.

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure1.png)

13. Git **yönetici** seçip **SAML**.

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure4.png)

14. Aşağıdaki adımları gerçekleştirin **ayrıntıları** sayfası:

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure2.png)

    a. Kopyalama **SSO URL** yapıştırın ve değer **oturum açma URL'si** metin kutusunda **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümü.

    b. Kopyalama **alıcı URL** yapıştırın ve değer **yanıt URL'si** metin kutusunda **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümü.

    c. Kopyalama **hedef kitle URİ'si (SP varlık kimliği)** yapıştırın ve değer **tanımlayıcı (varlık kimliği)** metin kutusunda **Meta ağları bağlayıcı etki alanı ve URL'ler** bölümü.

    d. SAML'yi etkinleştir

15. Üzerinde **genel** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/metanetworksconnector-tutorial/configure5.png)

    a. İçinde **kimlik sağlayıcısının çoklu oturum açma URL'si**, Yapıştır **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    b. İçinde **kimlik sağlayıcısını veren**, Yapıştır **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.

    c. Açık Not Defteri'nde, Azure portalından indirilen sertifikanın yapıştırın içine **X.509 sertifikası** metin.

    d. Etkinleştirme **tam zamanında sağlama**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/metanetworksconnector-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/metanetworksconnector-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/metanetworksconnector-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/metanetworksconnector-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-meta-networks-connector-test-user"></a>Meta ağları bağlayıcı test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon'un Meta ağları bağlayıcısında adlı bir kullanıcı oluşturmaktır. Meta ağları bağlayıcı varsayılan olarak etkin olan tam zamanında sağlama, destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, henüz yoksa Meta ağları bağlayıcı erişme denemesi sırasında oluşturulur.

>[!Note]
>Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Meta ağları bağlayıcı istemci Destek ekibine](mailto:support@metanetworks.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma Meta ağları bağlayıcısına erişim izni verdiğinizde kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon'un Meta ağları bağlayıcıya atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Meta ağları bağlayıcı**.

    ![Uygulamalar listesini Meta ağları Bağlayıcısı bağlantıyı](./media/metanetworksconnector-tutorial/tutorial_metanetworksconnector_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Meta ağları bağlayıcı kutucuğa tıkladığınızda, otomatik olarak Meta ağları bağlayıcı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/metanetworksconnector-tutorial/tutorial_general_01.png
[2]: ./media/metanetworksconnector-tutorial/tutorial_general_02.png
[3]: ./media/metanetworksconnector-tutorial/tutorial_general_03.png
[4]: ./media/metanetworksconnector-tutorial/tutorial_general_04.png

[100]: ./media/metanetworksconnector-tutorial/tutorial_general_100.png

[200]: ./media/metanetworksconnector-tutorial/tutorial_general_200.png
[201]: ./media/metanetworksconnector-tutorial/tutorial_general_201.png
[202]: ./media/metanetworksconnector-tutorial/tutorial_general_202.png
[203]: ./media/metanetworksconnector-tutorial/tutorial_general_203.png

