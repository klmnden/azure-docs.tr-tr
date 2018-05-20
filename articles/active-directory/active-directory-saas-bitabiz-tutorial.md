---
title: 'Öğretici: Azure Active Directory Tümleştirme ile BitaBIZ | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile BitaBIZ arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 031d7b11aea57b8bdd8b17e474db0c81b1bdbe76
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Öğretici: Azure Active Directory Tümleştirme BitaBIZ ile

Bu öğreticide, Azure Active Directory (Azure AD) ile BitaBIZ tümleştirmek öğrenin.

BitaBIZ Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- BitaBIZ erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için BitaBIZ (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme BitaBIZ ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir BitaBIZ çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden BitaBIZ ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-bitabiz-from-the-gallery"></a>Galeriden BitaBIZ ekleme
Azure AD BitaBIZ tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden BitaBIZ eklemeniz gerekir.

**Galeriden BitaBIZ eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **BitaBIZ**seçin **BitaBIZ** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde BitaBIZ](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı BitaBIZ sınayın.

Tekli çalışmaya oturum için Azure AD BitaBIZ karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının BitaBIZ ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

BitaBIZ içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma BitaBIZ ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[BitaBIZ test kullanıcısı oluşturma](#create-a-bitabiz-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı BitaBIZ sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma BitaBIZ uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile BitaBIZ yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **BitaBIZ** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

3. Üzerinde **BitaBIZ etki alanı ve URL'leri** bölümünde, uygulama tarafından başlatılan IDP modunda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_url.png)

    İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > Yukarıdaki URL'deki yalnızca tanıtım değerdir. Öğreticide daha sonra açıklanan gerçek tanımlayıcısı ile değeri güncelleştirin.

4. Denetleme **Göster Gelişmiş URL ayarları** ve uygulamada yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin **SP** modunda başlatılan:

    ![BitaBIZ etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna, URL'yi yazın: `https://www.bitabiz.com/dashboard`

5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-bitabiz-tutorial/tutorial_general_400.png)
    
7. Üzerinde **BitaBIZ yapılandırma** 'yi tıklatın **yapılandırma BitaBIZ** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![BitaBIZ yapılandırma](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_configure.png) 

8. Farklı web tarayıcısı penceresinde BitaBIZ kiracınız yönetici olarak oturum.

9. Tıklayın **Kurulum yönetici**.

    ![BitaBIZ yapılandırma](./media/active-directory-saas-bitabiz-tutorial/settings1.png)

10. Tıklayın **Microsoft tümleştirmeler** altında **değeri eklemek** bölümü.

    ![BitaBIZ yapılandırma](./media/active-directory-saas-bitabiz-tutorial/settings2.png)

11. Bölümüne kaydırın **Microsoft Azure AD (etkinleştir çoklu oturum açma)** ve şu adımları gerçekleştirin:

    ![BitaBIZ yapılandırma](./media/active-directory-saas-bitabiz-tutorial/settings3.png)

    a. Değerinden kopyalama **varlık kimliği (Azure AD'de "tanımlayıcısı")** textbox ve yapıştırın **tanımlayıcısı** textbox üzerinde **BitaBIZ etki alanı ve URL'leri** Azure portalı bölümünde. 
    
    b. İçinde **Azure AD çoklu oturum açma hizmet URL'si** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.
    
    c. İçinde **Azure AD SAML varlık kimliği** metin kutusuna, Yapıştır **SAML varlık kimliği**, hangi Azure portalından kopyalanır.

    d. İndirilen açmak **Certificate(Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve yapıştırın kendisine **Azure AD imzalama sertifikası (Base64 ile kodlanmış)** metin kutusu.

    e. İş e-posta etki alanınızı ekleme mycompany.com içinde başka bir deyişle, ad **etki alanı adı** SSO bu e-posta etki alanı ile şirketinizdeki kullanıcılara atamak için textbox (zorunlu).
    
    f. İşareti **etkin SSO** BitaBIZ hesabı.
    
    g. Tıklatın **Azure AD yapılandırmasını kaydetmek** kaydedin ve SSO yapılandırmasını etkinleştirin.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-bitabiz-test-user"></a>BitaBIZ test kullanıcısı oluşturma

Azure AD kullanıcıları için BitaBIZ oturum açmak etkinleştirmek için bunların BitaBIZ sağlanmalıdır.  
BitaBIZ söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. BitaBIZ şirket sitenize yönetici olarak oturum açın.

2. Tıklayın **Kurulum yönetici**.

    ![BitaBIZ kullanıcı ekleme](./media/active-directory-saas-bitabiz-tutorial/settings1.png)

3. Tıklayın **kullanıcıları eklemek** altında **kuruluş** bölümü.

    ![BitaBIZ kullanıcı ekleme](./media/active-directory-saas-bitabiz-tutorial/user1.png)

4. Tıklatın **Ekle yeni çalışan**.

    ![BitaBIZ kullanıcı ekleme](./media/active-directory-saas-bitabiz-tutorial/user2.png)

5. Üzerinde **"Yeni çalışan Ekle"** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![BitaBIZ kullanıcı ekleme](./media/active-directory-saas-bitabiz-tutorial/user3.png)

    a. İçinde **ad** metin kutusu, kullanıcı Britta gibi ilk türünün adı.

    b. İçinde **Soyadı** metin kutusuna, son kullanıcı Simon gibi yazın.

    c. İçinde **e-posta** metin kutusuna, kullanıcının e-posta adresi türü ister Brittasimon@contoso.com.

    d. Bir tarih seçin **gününü**.

    e. Kullanıcı için ayarlanabilir diğer zorunlu olmayan kullanıcı özniteliği vardır. Lütfen [çalışan Kurulum belge](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) daha fazla ayrıntı için.    
    
    f. Tıklatın **Kaydet çalışan**.
    
    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.
    
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta BitaBIZ için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**BitaBIZ için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **BitaBIZ**.

    ![Uygulamalar listesinde BitaBIZ bağlantı](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli BitaBIZ parçasında tıklattığınızda, otomatik olarak BitaBIZ uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_203.png

