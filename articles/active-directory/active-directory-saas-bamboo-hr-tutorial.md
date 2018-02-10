---
title: "Öğretici: Azure Active Directory Tümleştirme ile BambooHR | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile BambooHR arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 081144a645683d4d00ed0d464e23558378dc1b38
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Öğretici: Azure Active Directory Tümleştirme BambooHR ile

Bu öğreticide, Azure Active Directory (Azure AD) ile BambooHR tümleştirmek öğrenin.

BambooHR Azure AD ile tümleştirme aşağıdaki avantajları sağlar:

- BambooHR erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak BambooHR için Azure AD hesaplarına ile çoklu oturum açma (SSO) kullanarak oturum, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme BambooHR ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- BambooHR SSO etkin bir abonelik

> [!NOTE]
> Bu öğreticide adımları test ettiğinizde, bir üretim ortamında kullanmanızı öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [ücretsiz bir aylık deneme almak](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğretici özetlenmektedir senaryo iki ana yapı taşlarını oluşur:

1. Galeriden BambooHR ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-bamboohr-from-the-gallery"></a>Galeriden BambooHR Ekle
Azure AD BambooHR tümleştirilmesi yapılandırmak için BambooHR aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **BambooHR**. Sonuçlar listesinde **BambooHR**ve ardından **Ekle**.

    ![Sonuçlar listesinde BambooHR](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve test kullanıcı "Britta Simon." kullanarak Azure AD BambooHR SSO'su test etme

Çalışmak SSO için Azure AD içinde BambooHR karşılık gelen kullanıcı nedir bilmesi gerekir. Diğer bir deyişle, Azure AD kullanıcısının ve ilgili kullanıcı arasındaki bağlantıyı ilişki içinde BambooHR oluşturmanız gerekir.

İçinde BambooHR bağlantı ilişkisi oluşturmak için Azure AD atama **kullanıcı adı** değeri BambooHR olarak **kullanıcıadı** değeri.

Yapılandırmak ve Azure AD BambooHR SSO'su sınamak için sonraki beş bölümlerde yapı taşları tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek ve aşağıdakileri yaparak BambooHR uygulamanızda SSO yapılandırın:

1. Azure portalında üzerinde **BambooHR** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** penceresi, **modu** aşağı açılan listesinden, **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma penceresi](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Altında **BambooHR etki alanı ve URL'leri**, aşağıdakileri yapın:

    ![BambooHR etki alanı ve URL'ler bölümü](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. İçinde **URL üzerinde oturum** kutusuna bir URL aşağıdaki biçimde yazın: `https://<company>.bamboohr.com`.

    b. İçinde **tanımlayıcısı** bir değer yazın: `BambooHR-SAML`.

    > [!NOTE] 
    > **URL üzerinde oturum** değeri gerçek değil. Gerçek oturum açma URL'si ile güncelleştirin. Değeri elde etmek için başvurun [BambooHR istemci destek ekibi](https://www.bamboohr.com/contact.php). 
 
4. Altında **SAML imzalama sertifikası**seçin **sertifika (Base64)**ve ardından sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. **Kaydet**’i seçin.

    ![Kaydet düğmesi](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. Altında **BambooHR yapılandırma**seçin **yapılandırma BambooHR** açmak için **yapılandırma oturum açma** penceresi. İçinde **hızlı başvuru** bölümünde, kopyalama **SAML çoklu oturum açma hizmet URL'si** daha sonra kullanmak için.

    ![BambooHR yapılandırma](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. Yeni bir pencerede BambooHR şirket sitenize yönetici olarak oturum açın.

8. Giriş sayfasında, aşağıdakileri yapın:
   
    ![BambooHR çoklu oturum açma sayfası](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "çoklu oturum açma")   

    a. Seçin **uygulamaları**.
   
    b. İçinde **uygulamaları** bölmesinde, **çoklu oturum açma**.
   
    c. Seçin **SAML çoklu oturum açma**.

9. İçinde **SAML çoklu oturum açma** bölmesinde, aşağıdakileri yapın:
   
    ![SAML çoklu oturum açma bölmesinde](./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML çoklu oturum açma")
   
    a. İçine **SSO oturum açma URL'si** kutusunda, yapıştırma **SAML çoklu oturum açma hizmet URL'si** 6. adımda Azure portalından kopyaladığınız.
      
    b. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlanmış sertifika açın, içeriğini kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusu.
   
    c. **Kaydet**’i seçin.

> [!TIP]
> Uygulamasını ayarlıyorsanız, Bu yönergelerde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com). Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölüm, yalnızca select **çoklu oturum açma** sekmesini tıklatın ve ardından erişim Belge aracılığıyla katıştırılmış **yapılandırma** alt bölüm. Bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adlı bir test kullanıcı oluşturmaktır.

   ![Azure AD test kullanıcısı Britta Simon oluşturma][100]

Azure AD'de bir sınama kullanıcısı oluşturmak için aşağıdakileri yapın:

1. Azure portalında sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png)

3. Üstündeki **tüm kullanıcılar** bölmesinde, **Ekle**.

    ![Ekle düğmesi](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-bamboohr-test-user"></a>BambooHR test kullanıcısı oluşturma

Azure AD için BambooHR oturum açmalarını etkinleştirmek için bunları el ile BambooHR aşağıdakileri yaparak kurun:

1. Oturum açın, **BambooHR** yönetici olarak site.

2. Üst araç çubuğunda seçin **ayarları**.
   
    ![Ayarlar düğmesine](./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "ayarı")

3. Seçin **genel bakış**.

4. Sol bölmede seçin **güvenlik** > **kullanıcılar**.

5. Kullanıcı adı, parola ve e-posta adresi geçerli Azure ad ayarlamak istediğiniz hesap türü.

6. **Kaydet**’i seçin.
        
>[!NOTE]
>Azure AD kullanıcı hesaplarını ayarlamak için de BambooHR kullanıcı hesabı oluşturma araçlarını veya API'larını kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Kullanıcı Britta BambooHR için erişim vererek Azure SSO kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

Kullanıcı Britta Simon için BambooHR atamak için aşağıdakileri yapın:

1. Azure Portalı'ndaki uygulamaların görünümü açma, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. İçinde **kurumsal uygulamalar** listesinde **BambooHR**.

    ![Kurumsal uygulamalar listesinde BambooHR bağlantı](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Seçin **Ekle** düğmesine ve ardından **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi, **kullanıcılar** listesinde **Britta Simon**.

6. Seçin **seçin** düğmesi.

7. İçinde **eklemek atama** penceresinde, seçin **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Erişim paneli kullanarak Azure AD SSO yapılandırmanızı sınayın.

Seçtiğinizde, **BambooHR** döşeme erişim panelinde, otomatik olarak BambooHR uygulamanıza oturum.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

