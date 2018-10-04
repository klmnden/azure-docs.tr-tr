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
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: d28e28a2c4f8144da16c4838f07c9b8bb5ce67f0
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268166"
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

- Azure AD aboneliği
- Jamf Pro çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin.
Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Jamf Pro galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro galeri ekleme

Azure AD'de Jamf Pro tümleştirmesini yapılandırmak için Jamf Pro Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Jamf Pro Galeriden eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![image](./media/jamfprosamlconnector-tutorial/selectazuread.png)

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![image](./media/jamfprosamlconnector-tutorial/a_select_app.png)
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![image](./media/jamfprosamlconnector-tutorial/a_new_app.png)

4. Arama kutusuna **Jamf Pro**seçin **Jamf Pro** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![image](./media/jamfprosamlconnector-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Jamf Pro ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Jamf Pro'da bir kullanıcının Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı Jamf Pro'da arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Jamf Pro ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Jamf Pro test kullanıcısı oluşturma](#create-a-jamf-pro-test-user)**  - Jamf Pro'da kullanıcı Azure AD gösterimini bağlı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Jamf Pro uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma Jamf Pro ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde [Azure portalında](https://portal.azure.com/), **Jamf Pro** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_select_sso.png)

2. Tıklayın **değişiklik çoklu oturum açma modunu** seçmek için ekranın en üstünde **SAML** modu.

      ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_ssso.png)

3. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunu tıklatın **seçin** için **SAML** modu, çoklu oturum açmayı etkinleştirmek için.

    ![image](./media/jamfprosamlconnector-tutorial/b1_b2_saml_sso.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **temel SAML yapılandırma** iletişim.

    ![image](./media/jamfprosamlconnector-tutorial/b1-domains_and_urlsedit.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.jamfcloud.com/saml/metadata`.

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.jamfcloud.com/saml/SSO`.

    ![image](./media/jamfprosamlconnector-tutorial//b2-domains_and_urls.png)

    c. Tıklayın **ek URL'lerini ayarlayın**.

    d. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.jamfcloud.com`.

    ![image](./media/jamfprosamlconnector-tutorial//b4-domains_and_urls.png)

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Gerçek tanımlayıcı değerini alırsınız **çoklu oturum açma** bölümünde Jamf Pro portal, öğreticinin ilerleyen bölümlerinde açıklanmıştır. Gerçek ayıklayabilir **alt etki alanı** değer tanımlayıcısı değeri ve kullanan **alt etki alanı** bilgileri oturum açma URL'si ve yanıt URL'si.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![image](./media/jamfprosamlconnector-tutorial/C2_certificate.png)

7. Jamf Pro içinde yapılandırmasını otomatik hale getirmenizi yüklemeniz gerekir **My Apps güvenli oturum açma tarayıcı uzantısı** tıklayarak **uzantıyı yükleme**.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)
 
8. Uzantı tarayıcıya ekledikten sonra tıklayarak **Jamf Pro Kurulum** Jamf Pro uygulamaya yönlendirir. Burada, Jamf Pro ile oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı otomatik olarak sizin için uygulamayı yapılandırma ve adımları 9-12 otomatikleştirin.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

9. Jamf Pro el ile ayarlamak istiyorsanız, yeni bir web tarayıcı penceresi ve günlük Jamf Pro şirketinizin sitesi yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

10. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesindeki öğesinden.

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure1.png)

11. Tıklayarak **tekli oturum**.

    ![Jamf Pro yapılandırması](./media/jamfprosamlconnector-tutorial/configure2.png)

12. Üzerinde **çoklu oturum açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro tek](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Seçin **Jamf Pro sunucu** çoklu oturum açma erişimi sağlamak için.

    b. Seçerek **tüm kullanıcılar için izin atlama** kullanıcıların kimlik doğrulaması için kimlik sağlayıcısı oturum açma sayfasına yönlendirilmeyecek ancak Jamf Pro doğrudan bunun yerine oturum açabilirsiniz. Bir kullanıcı, kimlik sağlayıcısı ile Jamf Pro erişmeye çalıştığında, IDP tarafından başlatılan SSO'yu kimlik doğrulaması ve yetkilendirme gerçekleşir.

    c. Seçin **Nameıd** seçeneğini **kullanıcı eşleme: SAML**. Varsayılan olarak, bu ayar **Nameıd** ancak özel bir öznitelik tanımlayın.

    d. Seçin **e-posta** için **kullanıcı eşlemesi: JAMF PRO**. Jamf Pro aşağıdaki yollarla IDP tarafından gönderilen SAML öznitelikleri eşler: kullanıcılar ve gruplar. Bir kullanıcının erişmeye çalıştığında Jamf Pro, varsayılan Jamf Pro tarafından kimlik sağlayıcısından kullanıcıyla ilgili bilgileri alır ve Jamf Pro kullanıcı hesaplarına yönelik eşleşir. Jamf Pro'da gelen kullanıcı hesabı yoksa, grup adıyla eşleşen gerçekleşir.

    e. Değer yapıştırın `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` içinde **grubu ÖZNİTELİK adı** metin.

13. En fazla aynı sayfayı aşağı üzerinde **kimlik SAĞLAYICISI** altında **çoklu oturum açma** bölümünde ve aşağıdaki adımları gerçekleştirin:

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

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    ![image](./media/jamfprosamlconnector-tutorial/d_users_and_groups.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![image](./media/jamfprosamlconnector-tutorial/d_adduser.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![image](./media/jamfprosamlconnector-tutorial/d_userproperties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **özellikleri**seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’u seçin.

### <a name="create-a-jamf-pro-test-user"></a>Jamf Pro test kullanıcısı oluşturma

Jamf Pro oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar Jamf Pro ile sağlanması gerekir. Jamf Pro söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jamf Pro şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayarak **ayarlar simgesine** sayfanın sağ üst köşesindeki öğesinden.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Tıklayarak **Jamf Pro kullanıcı hesaplarını ve grupları**.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user1.png)

4. **Yeni**’ye tıklayın.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user2.png)

5. Seçin **standart hesabı oluşturma**.

    ![Çalışan Ekle](./media/jamfprosamlconnector-tutorial/user3.png)

6. Üzerinde **yeni hesabı** dailog, aşağıdaki adımları gerçekleştirin:

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

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Jamf Pro**.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_applications.png)

2. Uygulamalar listesinde **Jamf Pro**.

    ![image](./media/jamfprosamlconnector-tutorial/d_all_proapplications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![image](./media/jamfprosamlconnector-tutorial/d_leftpaneusers.png)

4. Seçin **Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![image](./media/jamfprosamlconnector-tutorial/d_assign_user.png)

4. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

5. İçinde **atama Ekle** iletişim kutusunda **atama** düğmesi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Jamf Pro kutucuğa tıkladığınızda, otomatik olarak Jamf Pro uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
