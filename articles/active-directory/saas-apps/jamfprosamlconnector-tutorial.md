---
title: 'Öğretici: Azure Active Directory Tümleştirme Jamf Pro ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Jamf Pro arasında yapılandırmayı öğrenin.
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
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: a95b3d1d35642043728831068f0e01cd16f7b999
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212956"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Öğretici: Azure Active Directory Tümleştirme ile Jamf Pro

Bu öğreticide, Azure Active Directory (Azure AD) ile Jamf Pro tümleştirmek öğrenin.

Jamf Pro Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Jamf Pro erişimi, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için Jamf Pro açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Jamf Pro ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Jamf Pro çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Jamf Pro ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-jamf-pro-from-the-gallery"></a>Galeriden Jamf Pro ekleme
Azure AD Jamf Pro tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Jamf Pro eklemeniz gerekir.

**Galeriden Jamf Pro eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Jamf Pro**seçin **Jamf Pro** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve Jamf Pro ile Azure AD çoklu oturum açmayı test "Britta Simon" adlı bir test kullanıcı tabanlı.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Jamf Pro bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Jamf Pro arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Jamf Pro ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Jamf Pro test kullanıcısı oluşturma](#create-a-jamf-pro-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Jamf Pro sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Jamf Pro uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Jamf Pro ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Jamf Pro** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

3. Üzerinde **Jamf Pro etki alanı ve URL'leri** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** modu tarafından başlatılan:

    ![Jamf Pro etki alanı ve URL'leri tek oturum açma bilgileri](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. İçinde **tanımlayıcısı (varlık kimliği)** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.jamfcloud.com/saml/SSO`

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![Jamf Pro etki alanı ve URL'leri tek oturum açma bilgileri](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler, gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Gerçek tanımlayıcı değerinden alırsınız **çoklu oturum açma** öğreticide daha sonra açıklanan Jamf Pro portalı bölümünde. Gerçek ayıklayabilirsiniz **alt etki alanı** değer tanımlayıcı değeri ve kullanan **alt etki alanı** bilgileri oturum açma URL'si ve yanıt URL'si.

5. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesini tıklatın **uygulama Federasyon meta veri URL'sini** ve Not Defteri'ne yapıştırın.

    ![Sertifika indirme bağlantısı](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
7. Farklı web tarayıcısı penceresinde Jamf Pro şirket sitenize yönetici olarak oturum açın.

8. Tıklayın **ayarlar simgesine** sayfanın sağ üst köşesinde gelen.

    ![Jamf Pro yapılandırma](./media/jamfprosamlconnector-tutorial/configure1.png)

9. Tıklayın **çoklu oturum açmayı**.

    ![Jamf Pro yapılandırma](./media/jamfprosamlconnector-tutorial/configure2.png)

10. Değerine kadar aşağı kaydırın **kimlik SAĞLAYICISI** altında **çoklu oturum açma** bölümünde ve aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro yapılandırma](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Seçin **diğer** bir seçeneği olarak **kimlik SAĞLAYICISI** açılır.

    b. İçinde **diğer sağlayıcı** metin girin **Azure AD**.

    c. Seçin **meta veri URL'sini** bir seçeneği olarak **kimlik SAĞLAYICISI meta veri kaynağı** açılır ve aşağıdaki metin kutusuna yapıştırın **uygulama Federasyon meta veri URL'sini** değeri Azure portalından kopyaladığınız.

    d. Kopya **varlık kimliği** vlaue ve yapıştırın **tanımlayıcısı (varlık kimliği)** metin kutusuna **Jamf Pro etki alanı ve URL'leri** Azure Portal'daki bölüm.

    >[!NOTE]
    > Burada `aadsso` (başvuru amaçla olan) alt etki alanı bir parçasıdır. Yanıt URL'si ve oturum açma URL'si tamamlamak için bu değeri kullanın **Jamf Pro etki alanı ve URL'leri** Azure Portal'daki bölümü.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-jamf-pro-test-user"></a>Jamf Pro test kullanıcısı oluşturma

Azure AD kullanıcılarının Jamf Pro oturum açmayı etkinleştirmek için bunların Jamf Pro sağlanmalıdır. Jamf Pro söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Jamf Pro şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **ayarlar simgesine** sayfanın sağ üst köşesinde gelen.

    ![Çalışanı ekleyin](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Tıklayın **Jamf Pro kullanıcı hesapları ve grupları**.

    ![Çalışanı ekleyin](./media/jamfprosamlconnector-tutorial/user1.png)

4. **Yeni**’ye tıklayın.

    ![Çalışanı ekleyin](./media/jamfprosamlconnector-tutorial/user2.png)

5. Seçin **standart hesabı oluşturma**.

    ![Çalışanı ekleyin](./media/jamfprosamlconnector-tutorial/user3.png)

6. Üzerinde **yeni hesabı** dailog, aşağıdaki adımları gerçekleştirin:

    ![Çalışanı ekleyin](./media/jamfprosamlconnector-tutorial/user4.png)

    a. İçinde **kullanıcıadı** metin kutusuna, BrittaSimon tam adını yazın.

    b. Kuruluşunuz için uygun şekilde uygun seçenekleri seçin **erişim düzeyi**, **ayrıcalık**ve **erişim durumu**.
    
    c. İçinde **tam adı** metin kutusuna, Britta Simon tam adını yazın.

    d. İçinde **e-posta adresi** metin kutusuna, Britta Simon hesabı e-posta adresini yazın.

    e. İçinde **parola** metin kutusuna, kullanıcının parolasını yazın.

    f. İçinde **PAROLAYI doğrula** metin kutusuna, kullanıcının parolasını yazın.

    g. **Kaydet**’e tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Jamf Pro erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Jamf Pro Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Jamf Pro**.

    ![Uygulamalar listesinde Jamf Pro bağlantı](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Jamf Pro parçasında tıklattığınızda, otomatik olarak Jamf Pro uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

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

