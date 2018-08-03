---
title: 'Öğretici: Azure Active Directory Jamf Pro ile tümleştirme | Microsoft Docs'
description: Jamf Pro ile Azure Active Directory arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2018
ms.author: jeedes
ms.openlocfilehash: 94b8b935728110cd5dd07b2066e8320274e3b082
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428426"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Öğretici: Azure Active Directory Jamf Pro ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Jamf Pro tümleştirme konusunda bilgi edinin.

Jamf Pro Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Jamf Pro erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak imzalanan (çoklu oturum açma) için Jamf Pro açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Jamf Pro ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Jamf Pro çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Jamf Pro galeri ekleme
1. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro galeri ekleme
Azure AD'de Jamf Pro tümleştirmesini yapılandırmak için Jamf Pro Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Jamf Pro Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

1. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
1. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

1. Arama kutusuna **Jamf Pro**seçin **Jamf Pro** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Jamf Pro'da sonuçları listesi](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Jamf Pro ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Jamf Pro'da bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Jamf Pro'da arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Jamf Pro ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
1. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
1. **[Jamf Pro test kullanıcısı oluşturma](#create-a-jamf-pro-test-user)**  - Jamf Pro'da kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
1. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
1. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Jamf Pro uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Jamf Pro ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Jamf Pro** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

1. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

1. Üzerinde **Jamf Pro etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Jamf Pro etki alanı ve URL'ler tek oturum açma bilgileri](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.jamfcloud.com/saml/SSO`

1. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Jamf Pro etki alanı ve URL'ler tek oturum açma bilgileri](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Gerçek tanımlayıcı değerini alırsınız **çoklu oturum açma** bölümünde Jamf Pro portal, öğreticinin ilerleyen bölümlerinde açıklanmıştır. Gerçek ayıklayabilir **alt etki alanı** değer tanımlayıcısı değeri ve kullanan **alt etki alanı** bilgileri oturum açma URL'si ve yanıt URL'si.

1. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

1. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
1. Farklı bir web tarayıcı penceresinde Jamf Pro şirket sitenize yönetici olarak oturum.

1. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesindeki öğesinden.

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Tıklayarak **tekli oturum**.

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure2.png)

1. Üzerinde **çoklu oturum açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro tek](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Seçin **Jamf Pro sunucu** çoklu oturum açma erişimi sağlamak için.

    b. Seçerek **tüm kullanıcılar için izin atlama** kullanıcıların kimlik doğrulaması için kimlik sağlayıcısı oturum açma sayfasına yönlendirilmeyecek ancak Jamf Pro doğrudan bunun yerine oturum açabilirsiniz. Bir kullanıcı, kimlik sağlayıcısı ile Jamf Pro erişmeye çalıştığında, IDP tarafından başlatılan SSO'yu kimlik doğrulaması ve yetkilendirme gerçekleşir.

    c. Seçin **Nameıd** seçeneğini **kullanıcı eşleme: SAML**. Varsayılan olarak, bu ayar **Nameıd** ancak özel bir öznitelik tanımlayın.

    d. Seçin **e-posta** için **kullanıcı eşlemesi: JAMF PRO**. Jamf Pro aşağıdaki yollarla IDP tarafından gönderilen SAML öznitelikleri eşler: kullanıcılar ve gruplar. Bir kullanıcının erişmeye çalıştığında Jamf Pro, varsayılan Jamf Pro tarafından kimlik sağlayıcısından kullanıcıyla ilgili bilgileri alır ve Jamf Pro kullanıcı hesaplarına yönelik eşleşir. Jamf Pro'da gelen kullanıcı hesabı yoksa, grup adıyla eşleşen gerçekleşir.

    e. Değer yapıştırın `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` içinde **grubu ÖZNİTELİK adı** metin.
 
1. En fazla aynı sayfayı aşağı üzerinde **kimlik SAĞLAYICISI** altında **çoklu oturum açma** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Seçin **diğer** bir seçeneği olarak **kimlik SAĞLAYICISI** açılır.

    b. İçinde **diğer SAĞLAYICISI** metin girin **Azure AD'ye**.

    c. Seçin **meta veri URL'si** bir seçeneği olarak **kimlik SAĞLAYICISI meta veri kaynağı** açılır ve aşağıdaki metin kutusuna yapıştırın **uygulama Federasyon meta verileri URL'sini** değeri Azure portaldan kopyaladığınız.

    d. Kopyalama **varlık kimliği** yapıştırın ve değer **tanımlayıcı (varlık kimliği)** metin kutusunda **Jamf Pro etki alanı ve URL'ler** bölümü Azure portalı.

    >[!NOTE]
    > Burada bulanık değeri alt etki alanı parçasıdır. Oturum açma URL'si ve yanıt URL'si olarak tamamlamak için bu değeri kullanın **Jamf Pro etki alanı ve URL'ler** bölümü Azure portalı.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

1. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

1. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

1. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-jamf-pro-test-user"></a>Jamf Pro test kullanıcısı oluşturma

Jamf Pro oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jamf Pro ile sağlanması gerekir. Jamf Pro söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jamf Pro şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesindeki öğesinden.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Tıklayarak **Jamf Pro kullanıcı hesaplarını ve grupları**.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user1.png)

1. **Yeni**’ye tıklayın.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user2.png)

1. Seçin **standart hesabı oluşturma**.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user3.png)

1. Üzerinde **yeni hesabı** dailog, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user4.png)

    a. İçinde **kullanıcıadı** metin BrittaSimon tam adını yazın.

    b. Kuruluşunuz için göre uygun seçenekleri belirleyin **erişim düzeyi**, **ayrıcalık**ve **erişim durumu**.
    
    c. İçinde **tam adı** metin Britta Simon tam adını yazın.

    d. İçinde **e-posta adresi** metin Britta Simon hesabı e-posta adresini yazın.

    e. İçinde **parola** metin kutusu, kullanıcı parolasını yazın.

    f. İçinde **PAROLAYI doğrula** metin kutusu, kullanıcı parolasını yazın.

    g. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Jamf Pro için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Jamf Pro Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

1. Uygulamalar listesinde **Jamf Pro**.

    ![Uygulamalar listesinde Jamf Pro bağlantı](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

1. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

1. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

1. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

1. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

1. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Jamf Pro kutucuğa tıkladığınızda, otomatik olarak Jamf Pro uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/jamfprosamlconnector-tutorial/tutorial_general_203.png

