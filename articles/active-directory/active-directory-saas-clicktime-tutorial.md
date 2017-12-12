---
title: "Öğretici: Azure Active Directory Tümleştirme ile ClickTime | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ile ClickTime arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: f19e1968c736cb21a2a80b9807fa86461e05ee42
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Öğretici: Azure Active Directory Tümleştirme ClickTime ile

Bu öğreticide, Azure Active Directory (Azure AD) ile ClickTime tümleştirmek öğrenin.

ClickTime Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- ClickTime erişimi, Azure AD'de kontrol edebilirsiniz
- Otomatik olarak için ClickTime (çoklu oturum açma) ile Azure AD hesaplarına açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirme ClickTime ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir ClickTime çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden ClickTime ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-clicktime-from-the-gallery"></a>Galeriden ClickTime ekleme
Azure AD ClickTime tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden ClickTime eklemeniz gerekir.

**Galeriden ClickTime eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ClickTime**seçin **ClickTime** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Sonuçlar listesinde ClickTime](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ClickTime sınayın.

Tekli çalışmaya oturum için Azure AD ClickTime karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ClickTime ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

ClickTime içinde değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma ClickTime ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[ClickTime test kullanıcısı oluşturma](#create-a-clicktime-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı ClickTime sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma ClickTime uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile ClickTime yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **ClickTime** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. Üzerinde **ClickTime etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ClickTime etki alanı ve URL'leri tek oturum açma bilgileri](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL'yi yazın:`https://app.clicktime.com/sp/`
    
    b. İçinde **yanıt URL'si** metin kutusuna, aşağıdaki desenleri kullanarak URL'sini yazın: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **Certificate(Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. Üzerinde **ClickTime yapılandırma** 'yi tıklatın **yapılandırma ClickTime** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![ClickTime yapılandırma](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. Farklı web tarayıcısı penceresinde ClickTime şirket sitenize yönetici olarak oturum açın.

8. Üstteki araç çubuğunda tıklatın **Tercihler**ve ardından **güvenlik ayarlarını**.

9. İçinde **tek oturum açma tercihleri** yapılandırma bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Güvenlik ayarları](./media/active-directory-saas-clicktime-tutorial/tic777280.png "güvenlik ayarları")
   
    a.  Seçin **izin** çoklu oturum açma (SSO) ile kullanarak oturum açın **Azure AD**.
   
    b. İçinde **kimlik sağlayıcısı Endpoint** metin kutusuna, Yapıştır **SAML çoklu oturum açma hizmet URL'si** Azure portalından kopyalanan.
   
    c.  Açık **base-64 kodlamalı sertifika** Azure portalında indirilen **not defteri**içeriği Kopyala ve ardından yapıştırın **X.509 sertifikası** metin kutusu.
   
    d.  **Kaydet** düğmesine tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.
 
    ![Ekle düğmesi](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**'a tıklayın.
 
### <a name="create-a-clicktime-test-user"></a>ClickTime test kullanıcısı oluşturma

Azure AD kullanıcıların ClickTime oturum etkinleştirmek için bunların ClickTime sağlanmalıdır.  
ClickTime söz konusu olduğunda, sağlama bir el ile bir görevdir.

> [!NOTE]
> API Azure AD kullanıcı hesaplarını sağlamak için ClickTime tarafından sağlanan veya herhangi diğer ClickTime kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**
1. Oturum, **ClickTime** Kiracı.
2. Üstteki araç çubuğunda tıklatın **şirket**ve ardından **kişiler**.
   
    ![Kişiler](./media/active-directory-saas-clicktime-tutorial/tic777282.png "kişiler")
3. Tıklatın **kişiyi ekler**.
   
    ![Kişi Ekle](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Kişi Ekle")
4. Yeni bir kişiye bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kişiler](./media/active-directory-saas-clicktime-tutorial/tic777284.png "kişiler")
   
    a.  İçinde **tam adı** metin kutusuna, tam ad kullanıcı türünü gibi **Britta Simon**. 
  
    b.  İçinde **e-posta adresi** metin kutusu, kullanıcı e-posta türünü ister  **brittasimon@contoso.com** .
       
    > [!NOTE]
    > İsterseniz, yeni kişi nesnesi ek özellikleri ayarlayabilirsiniz.
   
    c.  **Kaydet** düğmesine tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta ClickTime için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**ClickTime için Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **ClickTime**.

    ![Uygulamalar listesinde ClickTimne bağlantı](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açmayı test edin

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli ClickTime parçasında tıklattığınızda, otomatik olarak ClickTime uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

