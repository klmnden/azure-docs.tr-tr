---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle EBSCO | Microsoft Docs'
description: Azure Active Directory ve EBSCO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: 50e9a65f881981964270e0a8bbc7af46a9dfd27a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047734"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Öğretici: Azure Active Directory EBSCO ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile EBSCO tümleştirme konusunda bilgi edinin.

Azure AD ile EBSCO tümleştirme ile aşağıdaki avantajları sağlar:

- EBSCO erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için EBSCO (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile EBSCO yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir EBSCO çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden EBSCO ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-ebsco-from-the-gallery"></a>Galeriden EBSCO ekleme
Azure AD'de EBSCO tümleştirmesini yapılandırmak için EBSCO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden EBSCO eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **EBSCO**seçin **EBSCO** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde EBSCO](./media/ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı EBSCO sınayın.

Tek iş için oturum açma için Azure AD ne EBSCO karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının EBSCO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma EBSCO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir EBSCO test kullanıcısı oluşturma](#create-an-ebsco-test-user)**  -sağlama EBSCOhost kullanıcı/kişiselleştirme otomatik hale getirebilirsiniz. Just-ın-Time EBSCO destekleyen kullanıcı sağlama.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve EBSCO uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile EBSCO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **EBSCO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/ebsco-tutorial/tutorial_ebsco_samlbase.png)

3. Üzerinde **EBSCO etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![EBSCO etki alanı ve URL'ler tek oturum açma bilgileri](./media/ebsco-tutorial/tutorial_ebsco_url.png)

    İçinde **tanımlayıcı** metin kutusuna bir URL: `pingsso.ebscohost.com`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![EBSCO etki alanı ve URL'ler tek oturum açma bilgileri](./media/ebsco-tutorial/tutorial_ebsco_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > Oturum açma URL değeri, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [EBSCO istemci Destek ekibine](mailto:sso@ebsco.com) değeri alınamıyor. 

    o **benzersiz öğeleri:**  

    o **CustId** Enter benzersiz EBSCO Müşteri Kimliği = 

    o **profili** = istemciler (bağlı olarak hangi EBSCO ' satın alma) belirli bir profil kullanıcılara yönlendirmek üzere bağlantıyı uyarlama. Bunlar, bir özel profil kimliğini girebilirsiniz. Ana eds (EBSCO bulma hizmeti) ve ehost (EBSOCOhost veritabanları) Kimlikleridir. Yönergeler için aynı anda [burada](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

5. EBSCO uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz "**kullanıcı öznitelikleri**" uygulama tümleştirme sayfasında bölümü. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.
    
    ![Çoklu oturum açmayı yapılandırın](./media/ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > **Adı** özniteliği zorunludur ve ile eşlenmiş **kullanıcı tanımlayıcısı** EBSCO uygulamada. Bu el ile eklemek zorunda kalmazsınız bu varsayılan olarak eklenir.
    
6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ---------------| --------------- |    
    | FirstName   | User.givenName |
    | LastName   | User.surname |
    | Email   | User.Mail |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/ebsco-tutorial/tutorial_officespace_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/ebsco-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.
    
    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. Tıklayın **Tamam**

7. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/ebsco-tutorial/tutorial_ebsco_certificate.png) 

8. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/ebsco-tutorial/tutorial_general_400.png)
    
9. Çoklu oturum açmayı yapılandırma **EBSCO** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [EBSCO Destek ekibine](mailto:sso@ebsco.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/ebsco-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/ebsco-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/ebsco-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/ebsco-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-ebsco-test-user"></a>Bir EBSCO test kullanıcısı oluşturma

EBSCO söz konusu olduğunda kullanıcı sağlamayı otomatik olarak gerçekleşir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

Azure AD, gerekli verileri EBSCO uygulamaya geçirir. EBSCO'ın kullanıcı hazırlama, otomatik olarak veya tek seferlik bir form gerektirir. Bu, istemci EBSCOhost hesapları kaydedilen kişisel ayarlarla önceden var olan çok fazla olup bağlıdır. Aynı ile açıklanan [EBSCO Destek ekibine](mailto:sso@ebsco.com) sırasında uygulama. Her iki durumda da, istemci testi önce herhangi bir EBSCOhost hesabı oluşturmak zorunda değildir.

   >[!Note]
   >Sağlama EBSCOhost kullanıcı/kişiselleştirme otomatik hale getirebilirsiniz. İlgili kişi [EBSCO Destek ekibine](mailto:sso@ebsco.com) Just-ın-Time hakkında kullanıcı sağlama. 
 
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için EBSCO erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon EBSCO için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **EBSCO**.

    ![Uygulamalar listesinde EBSCO bağlantı](./media/ebsco-tutorial/tutorial_ebsco_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

1. Erişim panelinde EBSCO kutucuğa tıkladığınızda, otomatik olarak EBSCO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

2. Uygulamada oturum açtıktan sonra tıklayarak **oturum** sağ üst köşedeki düğmesi.

    ![Uygulamalar listesinde EBSCO signın](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Kurumsal/SAML oturum açma ile eşleşmesine izin tek seferlik bir istem alırsınız bir **mevcut MyEBSCOhost hesabınız artık kurum hesabınıza bağlayın** veya **yeni MyEBSCOhost hesabı oluşturmak ve bağlantının, Kuruluş hesabı**. Hesap EBSCOhost uygulamasında kişiselleştirme için kullanılır. Seçeneğini **yeni bir hesap oluşturun** ve aşağıdaki ekran görüntüsünde gösterildiği gibi formun kişiselleştirme için saml yanıtını değerlerle önceden tamamlanmış olduğunu görürsünüz. Tıklayın **'Devam'** bu seçimini kaydetmek için.
    
     ![Uygulamalar listesinde EBSCO kullanıcı](./media/ebsco-tutorial/tutorial_ebsco_user.png)

4. Yukarıdaki Kurulum tamamlandıktan sonra tanımlama/önbellek ve oturum açma yeniden temizleyin. Signın ile el ile yeniden olmaz ve kişiselleştirme ayarlarını Anımsanmaz

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ebsco-tutorial/tutorial_general_01.png
[2]: ./media/ebsco-tutorial/tutorial_general_02.png
[3]: ./media/ebsco-tutorial/tutorial_general_03.png
[4]: ./media/ebsco-tutorial/tutorial_general_04.png

[100]: ./media/ebsco-tutorial/tutorial_general_100.png

[200]: ./media/ebsco-tutorial/tutorial_general_200.png
[201]: ./media/ebsco-tutorial/tutorial_general_201.png
[202]: ./media/ebsco-tutorial/tutorial_general_202.png
[203]: ./media/ebsco-tutorial/tutorial_general_203.png

