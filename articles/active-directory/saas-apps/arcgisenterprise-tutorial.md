---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Arcgıs Kurumsal | Microsoft Docs'
description: Azure Active Directory ve Arcgıs kuruluş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24809e9d-a4aa-4504-95a9-e4fcf484f431
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: ca5bf7ae49cf120c0566419ccadeff92433c6467
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820250"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Öğretici: Azure Active Directory Arcgıs Enterprise ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Arcgıs Kurumsal tümleştirme konusunda bilgi edinin.

Azure AD ile Arcgıs Kurumsal tümleştirme ile aşağıdaki avantajları sağlar:

- Arcgıs Kurumsal erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan Arcgıs kuruluş (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Önkoşullar

Arcgıs kuruluş Azure AD'ye tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Bir Arcgıs Kurumsal çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Arcgıs Kurumsal galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-arcgis-enterprise-from-the-gallery"></a>Arcgıs Kurumsal galeri ekleme

Azure AD'de Arcgıs Enterprise'nın tümleştirmesini yapılandırmak için Arcgıs Kurumsal Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Arcgıs kuruluş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Arcgıs Kurumsal**seçin **Arcgıs Kurumsal** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Arcgıs Enterprise](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Arcgıs Enterprise ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Arcgıs kurumsal bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Arcgıs Kurumsal arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Arcgıs Enterprise ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Arcgıs Kurumsal test kullanıcısı oluşturma](#create-an-arcgis-enterprise-test-user)**  - Arcgıs kuruluştaki kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Arcgıs Kurumsal uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Arcgıs Enterprise ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Arcgıs Kurumsal** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_samlbase.png)

3. Üzerinde **Arcgıs Kurumsal etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Arcgıs Kurumsal etki alanı ve URL'ler tek oturum açma bilgileri](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url1.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `<EXTERNAL_DNS_NAME>.portal`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Arcgıs Kurumsal etki alanı ve URL'ler tek oturum açma bilgileri](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [Arcgıs Kurumsal İstemci Destek ekibine](mailto:nshampur@esri.com) bu değerleri almak için. Tanımlayıcı değeri erişmenizi sağlayacak **ayarlanmış bir kimlik sağlayıcı** bölümünde, bu öğreticinin ilerleyen bölümlerinde açıklanmıştır.

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_certificate.png)

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/arcgisenterprise-tutorial/tutorial_general_400.png)

7. Farklı bir web tarayıcı penceresinde bir Arcgıs Kurumsal şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Seçin **kuruluş > ayarları düzenleme**.

    ![Arcgıs Kurumsal yapılandırma](./media/arcgisenterprise-tutorial/configure1.png)

9. Seçin **güvenlik** sekmesi.

    ![Arcgıs Kurumsal yapılandırma](./media/arcgisenterprise-tutorial/configure2.png)

10. Ekranı aşağı kaydırarak **Kurumsal oturum açma SAML aracılığıyla** bölümünde ve seçin **Kurumsal oturum açma ayarlama**.

    ![Arcgıs Kurumsal yapılandırma](./media/arcgisenterprise-tutorial/configure3.png)

11. Üzerinde **ayarlanmış bir kimlik sağlayıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Arcgıs Kurumsal yapılandırma](./media/arcgisenterprise-tutorial/configure4.png)

    a. Lütfen gibi bir ad sağlayın **Azure Active Directory Test** içinde **adı** metin.

    b. İçinde **URL** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portaldan kopyaladığınız değeri.

    c. Tıklayın **Gelişmiş ayarları göster** ve kopyalama **varlık kimliği** yapıştırın ve değer **tanımlayıcı** metin kutusunda **Arcgıs Kurumsal etki alanı ve URL'ler** bölümü Azure Portalı'nda.
    
    ![Arcgıs Kurumsal yapılandırma](./media/arcgisenterprise-tutorial/configure5.png)

    d. Tıklayın **güncelleştirme kimlik SAĞLAYICISI**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/arcgisenterprise-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/arcgisenterprise-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/arcgisenterprise-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/arcgisenterprise-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-an-arcgis-enterprise-test-user"></a>Bir Arcgıs Kurumsal test kullanıcısı oluşturma

Bu bölümün amacı, Arcgıs kuruluşta Britta Simon adlı bir kullanıcı oluşturmaktır. Arcgıs Kurumsal tam zamanında sağlama, varsayılan olarak etkin olan destekler. Bu bölümde, hiçbir eylem öğesini yoktur. Yeni bir kullanıcı, Arcgıs Kurumsal henüz mevcut değilse erişme denemesi sırasında oluşturulur.

> [!Note]
> Bir kullanıcı el ile oluşturmanız gerekiyorsa, kişi [Arcgıs Kurumsal Destek ekibine](mailto:nshampur@esri.com).

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Arcgıs kuruluş erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Arcgıs kuruluş atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Arcgıs Kurumsal**.

    ![Uygulamalar listesinde Arcgıs Kurumsal bağlantı](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Arcgıs Kurumsal kutucuğa tıkladığınızda, otomatik olarak Arcgıs Kurumsal uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/arcgisenterprise-tutorial/tutorial_general_01.png
[2]: ./media/arcgisenterprise-tutorial/tutorial_general_02.png
[3]: ./media/arcgisenterprise-tutorial/tutorial_general_03.png
[4]: ./media/arcgisenterprise-tutorial/tutorial_general_04.png

[100]: ./media/arcgisenterprise-tutorial/tutorial_general_100.png

[200]: ./media/arcgisenterprise-tutorial/tutorial_general_200.png
[201]: ./media/arcgisenterprise-tutorial/tutorial_general_201.png
[202]: ./media/arcgisenterprise-tutorial/tutorial_general_202.png
[203]: ./media/arcgisenterprise-tutorial/tutorial_general_203.png