---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Onit | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Onit arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 95a0968345c4f0a57460f07505bd934e84a1c12b
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35971905"
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a>Öğretici: Azure Active Directory Tümleştirme Onit ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Onit tümleştirmek öğrenin.

Onit Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Onit erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak için Onit (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Onit ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Onit çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Onit ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-onit-from-the-gallery"></a>Galeriden Onit ekleme
Azure AD Onit tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Onit eklemeniz gerekir.

**Galeriden Onit eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Onit**seçin **Onit** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde Onit](./media/onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı Onit ile test etme

Tekli çalışmaya oturum için Azure AD Onit karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Onit ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Onit içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma Onit ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir Onit test kullanıcısı oluşturma](#create-an-onit-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Onit sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Onit uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile Onit yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Onit** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/onit-tutorial/tutorial_onit_samlbase.png)

3. Üzerinde **Onit etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Onit etki alanı ve URL'leri tek oturum açma bilgileri](./media/onit-tutorial/tutorial_onit_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<sub-domain>.onit.com`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<sub-domain>.onit.com`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [Onit istemci destek ekibi](https://www.onit.com/support) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalama **parmak İZİ** sertifika değeri.

    ![Sertifika indirme bağlantısı](./media/onit-tutorial/tutorial_onit_certificate.png) 

5. Onit uygulaması SAML onaylar belirli bir biçimde bekliyor. Lütfen bu uygulama için aşağıdaki talep yapılandırın. Bu öznitelik değerlerini yönetebilirsiniz **"Atrribute"** uygulama sekmesinde. Aşağıdaki ekran görüntüsünde bunun bir örneği gösterir. 

    ![Çoklu oturum açmayı yapılandırın](./media/onit-tutorial/tutorial_onit_attribute.png) 

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği görüntüde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |
    | e-posta | User.Mail |
    
    a. Tıklatın **Ekle özniteliği** açmak için **özniteliği eklemek** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/onit-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/onit-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, ilgili satır için gösterilen öznitelik adı yazın.

    c. Gelen **değeri** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** boş.
    
    e. **Tamam**’a tıklayın.

7. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/onit-tutorial/tutorial_general_400.png)

8. Üzerinde **Onit yapılandırma** 'yi tıklatın **yapılandırma Onit** açmak için **yapılandırma oturum açma** penceresi. Kopya **Sign-Out URL, SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Onit yapılandırma](./media/onit-tutorial/tutorial_onit_configure.png)

9. Farklı web tarayıcısı penceresinde Onit şirket sitenize yönetici olarak oturum açın.

10. Üstteki menüde tıklatın **Yönetim**.
   
   ![Yönetim](./media/onit-tutorial/IC791174.png "Yönetim")
11. Tıklatın **düzenleme Corporation**.
   
   ![Düzen Corporation](./media/onit-tutorial/IC791175.png "düzenleme Corporation")
   
12. Tıklatın **güvenlik** sekmesi.
    
    ![Düzen şirket bilgileri](./media/onit-tutorial/IC791176.png "düzenleme şirket bilgileri")

13. Üzerinde **güvenlik** sekmesinde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma](./media/onit-tutorial/IC791177.png "çoklu oturum açma")

    a. Olarak **kimlik doğrulama stratejisi**seçin **çoklu oturum açma ve parola**.
    
    b. İçinde **IDP hedef URL** metin değerini yapıştırın **SAML çoklu oturum açma hizmet URL'si**, Azure portalından kopyalanan.

    c. İçinde **IDP oturum kapatma URL'si** metin değerini yapıştırın **Sign-Out URL**, Azure portalından kopyalanan.

    d. İçinde **IDP sertifika parmak izi (SHA1)** metin kutusuna, Yapıştır **parmak izi** Azure portalından kopyaladığınız sertifika değeri.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/onit-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/onit-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/onit-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/onit-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-onit-test-user"></a>Bir Onit test kullanıcısı oluşturma

Azure AD kullanıcıların Onit oturum etkinleştirmek için bunların Onit sağlanmalıdır.  

Onit söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Oturum, **Onit** yönetici olarak şirket site.
2. Tıklatın **kullanıcı ekleme**.
   
   ![Yönetim](./media/onit-tutorial/IC791180.png "Yönetim")
3. Üzerinde **Kullanıcı Ekle** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
   
   ![Kullanıcı ekleme](./media/onit-tutorial/IC791181.png "kullanıcı ekleme")
   
  1. Tür **adı** ve **e-posta adresi** geçerli bir Azure AD hesabının istediğiniz ilgili metin kutularına sağlamayı.
  2. **Oluştur**’a tıklayın.    
   
 > [!NOTE]
 > Azure Active Directory hesap sahibi bir e-posta alır ve bunu etkinleştirilmeden önce kendi hesabı onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Onit için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Onit için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Onit**.

    ![Uygulamalar listesinde Onit bağlantı](./media/onit-tutorial/tutorial_onit_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Onit parçasında tıklattığınızda, otomatik olarak Onit uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/onit-tutorial/tutorial_general_01.png
[2]: ./media/onit-tutorial/tutorial_general_02.png
[3]: ./media/onit-tutorial/tutorial_general_03.png
[4]: ./media/onit-tutorial/tutorial_general_04.png

[100]: ./media/onit-tutorial/tutorial_general_100.png

[200]: ./media/onit-tutorial/tutorial_general_200.png
[201]: ./media/onit-tutorial/tutorial_general_201.png
[202]: ./media/onit-tutorial/tutorial_general_202.png
[203]: ./media/onit-tutorial/tutorial_general_203.png

