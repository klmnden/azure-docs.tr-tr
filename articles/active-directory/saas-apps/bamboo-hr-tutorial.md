---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle BambooHR | Microsoft Docs'
description: Azure Active Directory ve BambooHR arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 77625296797ec8ed8364e7d8bff3e5a15b4b74b5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048047"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Öğretici: Azure Active Directory BambooHR ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile BambooHR tümleştirme konusunda bilgi edinin.

BambooHR Azure AD ile tümleştirme aşağıdaki avantajları sağlar:

- BambooHR erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak BambooHR için çoklu oturum açma (SSO) ile Azure AD hesaplarına kullanarak oturum açıp, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile BambooHR yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- BambooHR SSO etkin bir abonelik

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bir üretim ortamında kullanmanızı öneririz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [ücretsiz, bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. 

Bu öğreticide özetler senaryo iki temel yapı taşları oluşur:

1. Galeriden BambooHR ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="add-bamboohr-from-the-gallery"></a>Galeriden BambooHR Ekle
Azure AD'de BambooHR tümleştirmesini yapılandırmak için BambooHR aşağıdakileri yaparak Galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**. 

    ![Azure Active Directory düğmesi][1]

2. Seçin **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kurumsal uygulamalar bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama**.

    !["Yeni uygulama" düğmesi][3]

4. Arama kutusuna **BambooHR**. Sonuç listesinden **BambooHR**ve ardından **Ekle**.

    ![Sonuç listesinde BambooHR](./media/bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve test kullanıcı "Britta Simon" kullanarak Azure AD SSO BambooHR ile test etme

Çalışmak SSO için Azure AD BambooHR içinde karşılık gelen kullanıcı ne olduğunu bilmeniz gerekir. Diğer bir deyişle, Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki içinde BambooHR oluşturmanız gerekir.

İçinde BambooHR bağlantı kurmak için Azure AD atama **kullanıcı adı** BambooHR değeri **kullanıcıadı** değeri.

Yapılandırma ve Azure AD SSO BambooHR ile test etme için yapı taşlarını sonraki beş bölümlerde tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirmek ve aşağıdakileri yaparak BambooHR uygulamanıza SSO yapılandırın:

1. Azure portalında, üzerinde **BambooHR** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** penceresi içinde **modu** aşağı açılan listesinden **SAML tabanlı oturum açma**.
 
    ![Çoklu oturum açma penceresi](./media/bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Altında **BambooHR etki alanı ve URL'ler**, aşağıdakileri yapın:

    ![BambooHR etki alanı ve URL'ler bölümü](./media/bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. İçinde **oturum açma URL'si** kutusuna bir URL aşağıdaki biçimde yazın: `https://<company>.bamboohr.com`.

    b. İçinde **tanımlayıcı** bir değer yazın: `BambooHR-SAML`.

    > [!NOTE] 
    > **Oturum açma URL'si** değeri gerçek değil. Bu, gerçek oturum açma URL'si ile güncelleştirin. Değer elde etmek için ilgili kişi [BambooHR istemci Destek ekibine](https://www.bamboohr.com/contact.php). 
 
4. Altında **SAML imzalama sertifikası**seçin **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. **Kaydet**’i seçin.

    ![Kaydet düğmesi](./media/bamboo-hr-tutorial/tutorial_general_400.png)

6. Altında **BambooHR yapılandırma**seçin **yapılandırma BambooHR** açmak için **yapılandırma oturum açma** penceresi. İçinde **hızlı başvuru** bölümünde, kopya **SAML çoklu oturum açma hizmeti URL'si** daha sonra kullanmak için.

    ![BambooHR yapılandırma](./media/bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. Yeni bir pencerede BambooHR şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Giriş sayfasında, aşağıdakileri yapın:
   
    ![BambooHR çoklu oturum açma sayfası](./media/bamboo-hr-tutorial/ic796691.png "çoklu oturum açma")   

    a. Seçin **uygulamaları**.
   
    b. İçinde **uygulamaları** bölmesinde **çoklu oturum açma**.
   
    c. Seçin **SAML çoklu oturum açma**.

9. İçinde **SAML çoklu oturum açma** bölmesinde aşağıdakileri yapın:
   
    ![SAML çoklu oturum açma bölmesi](./media/bamboo-hr-tutorial/IC796692.png "SAML çoklu oturum açma")
   
    a. İçine **SSO oturum açma URL'si** kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** 6. adımda Azure portaldan kopyaladığınız.
      
    b. Not Defteri'nde, Azure portalından indirdiğiniz base-64 kodlanmış sertifika açın, içeriğini kopyalayın ve ardından yapıştırın **X.509 sertifikası** kutusu.
   
    c. **Kaydet**’i seçin.

> [!TIP]
> Uygulamayı hazırlama ayarlıyorsanız, Bu yönergelerde kısa bir sürümünü edinebilirsiniz [Azure portalında](https://portal.azure.com). Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünden yalnızca **çoklu oturum açma** sekmesine ve ardından erişim embedded belgeleri aracılığıyla **yapılandırma** alttaki bölümü. Bilgi için [Azure AD belgeleri katıştırılmış]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adlı bir test kullanıcı oluşturmaktır.

   ![Azure AD test kullanıcısı Britta Simon oluşturun][100]

Azure AD'de bir test kullanıcısı oluşturmak için aşağıdakileri yapın:

1. Azure portalında, sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/bamboo-hr-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/bamboo-hr-tutorial/create_aaduser_02.png)

3. Üst kısmındaki **tüm kullanıcılar** bölmesinde **Ekle**.

    ![Ekle düğmesi](./media/bamboo-hr-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** penceresinde aşağıdakileri yapın:

    ![Kullanıcı penceresi](./media/bamboo-hr-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.
 
### <a name="create-a-bamboohr-test-user"></a>BambooHR test kullanıcısı oluşturma

BambooHR için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunları el ile BambooHR aşağıdakileri yaparak kurun:

1. Oturum açın, **BambooHR** yönetici olarak site.

2. Üst araç çubuğunda, seçin **ayarları**.
   
    ![Ayarlar düğmesi](./media/bamboo-hr-tutorial/IC796694.png "ayarı")

3. **Genel Bakış**’ı seçin.

4. Sol bölmede seçin **güvenlik** > **kullanıcılar**.

5. Kullanıcı adı, parola ve geçerli Azure AD, e-posta adresi ayarlamak istediğiniz hesap türü.

6. **Kaydet**’i seçin.
        
>[!NOTE]
>Azure AD kullanıcı hesaplarını ayarlamak için de BambooHR kullanıcı hesabı oluşturma araçları veya API'leri kullanabilirsiniz.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Kullanıcı Azure SSO için BambooHR erişim vererek kullanacak şekilde Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

Britta Simon kullanıcı için BambooHR atamak için aşağıdakileri yapın:

1. Azure portalında uygulama görünümünü açın, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. İçinde **kurumsal uygulamalar** listesinden **BambooHR**.

    ![Kurumsal uygulamalar listesinde BambooHR bağlantı](./media/bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Seçin **Ekle** düğmesine ve ardından **atama Ekle** bölmesinde **kullanıcılar ve gruplar**.

    ![Atama Ekle bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** penceresi içinde **kullanıcılar** listesinden **Britta Simon**.

6. Seçin **seçin** düğmesi.

7. İçinde **atama Ekle** penceresinde **atama** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Azure AD SSO yapılandırmanızı erişim panelini kullanarak test edin.

Seçtiğinizde, **BambooHR** oturumunuz otomatik olarak BambooHR uygulamanıza erişim Paneli'nde; kutucuk.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme konusundaki öğreticilerin listesine](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-hr-tutorial/tutorial_general_203.png

