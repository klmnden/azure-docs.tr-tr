---
title: 'Öğretici: Azure Active Directory Tümleştirme Uyarlamalı Insights ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ile Uyarlamalı Öngörüler arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: jeedes
ms.openlocfilehash: c5ac63bc97924d322ce1a6e734f8ccf21367aafc
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35982933"
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-insights"></a>Öğretici: Azure Active Directory Tümleştirme Uyarlamalı Insights ile

Bu öğreticide, Azure Active Directory (Azure AD) ile Uyarlamalı Öngörüler tümleştirmek öğrenin.

Azure AD ile Uyarlamalı Insights tümleştirme ile aşağıdaki avantajları sağlar:

- Uyarlamalı Öngörüler erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak Uyarlamalı Öngörüler'e (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Uyarlamalı Insights ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Uyarlamalı Öngörüler çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Uyarlamalı Öngörüler ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-adaptive-insights-from-the-gallery"></a>Galeriden Uyarlamalı Öngörüler ekleme
Azure AD Uyarlamalı Öngörüler tümleştirmesini yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Uyarlamalı Öngörüler eklemeniz gerekir.

**Galeriden Uyarlamalı Öngörüler eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Uyarlamalı Öngörüler**seçin **Uyarlamalı Öngörüler** sonuç panelinden ardından **Ekle** uygulama eklemek için düğmeyi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Uyarlamalı "Britta Simon." olarak adlandırılan bir test kullanıcı dayalı Öngörüler ile test etme

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Uyarlamalı ilişkin bilgiler bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının Uyarlamalı Öngörüler ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Uyarlamalı Insights ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Uyarlamalı Öngörüler test kullanıcısı oluşturma](#creating-an-adaptive-insights-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı Uyarlamalı Öngörüler sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Uyarlamalı Öngörüler uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Uyarlamalı Insights ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Uyarlamalı Öngörüler** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. Üzerinde **Uyarlamalı Öngörüler etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    a. İçinde **tanımlayıcısı (varlık kimliği)** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Uyarlamalı ilişkin bilgiler 's tanımlayıcısı (varlık kimliği) ve yanıt URL'si değerleri alabilirsiniz **SAML SSO ayarları** sayfası.

4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **sertifika (Base64)** ve sertifika dosyayı bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png)

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/adaptivesuite-tutorial/tutorial_general_400.png)

6. Üzerinde **Uyarlamalı Insights Yapılandırması** 'yi tıklatın **yapılandırma Uyarlamalı Öngörüler** açmak için **yapılandırma oturum açma** penceresi. Kopya **SAML varlık kimliği ve SAML çoklu oturum açma hizmet URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. Farklı web tarayıcısı penceresinde Uyarlamalı Öngörüler şirket sitenize yönetici olarak oturum açın.

8. Git **yönetici**.

    ![Yönetici](./media/adaptivesuite-tutorial/IC805644.png "yönetici")

9. İçinde **kullanıcılar ve roller** 'yi tıklatın **SAML SSO ayarları yönetme**.

    ![SAML SSO ayarlarını Yönet](./media/adaptivesuite-tutorial/IC805645.png "SAML SSO ayarlarını yönet")

10. Üzerinde **SAML SSO ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML SSO ayarları](./media/adaptivesuite-tutorial/IC805646.png "SAML SSO ayarları")

    a. İçinde **kimlik sağlayıcı adı** metin kutusuna, yapılandırmanız için bir ad yazın.

    b. Yapıştır **SAML varlık kimliği** değeri kopyalanan Azure portalından **kimlik sağlayıcısı varlık kimliği** metin kutusu.

    c. Yapıştır **SAML çoklu oturum açma hizmet URL'si** değeri kopyalanan Azure portalından **kimlik sağlayıcısı SSO URL** metin kutusu.

    d. Yapıştır **SAML çoklu oturum açma hizmet URL'si** değeri kopyalanan Azure portalından **özel oturum kapatma URL'si** metin kutusu.

    e. İndirilen sertifikanızı karşıya yüklemek için tıklayın **dosya**.

    f. İçin aşağıdakileri seçin:

    * **SAML kullanıcı kimliği**seçin **Uyarlamalı Öngörüler kullanıcının adını**.

    * **SAML kullanıcı kimliği konumu**seçin **NameID, konu kullanıcı kimliği**.

    * **SAML NameID biçimi**seçin **e-posta adresi**.

    * **SAML'yi etkinleştir**seçin **izin SAML SSO ve doğrudan Uyarlamalı Öngörüler oturum açma**.

    g. Kopya **Uyarlamalı Öngörüler SSO URL** kopyalayıp yapıştırabilir **tanımlayıcısı (varlık kimliği)** ve **yanıt URL'si** metin kutuları içinde **Uyarlamalı Öngörüler etki alanı ve URL'leri** Azure portalı bölümünde.

    h. **Kaydet**’e tıklayın.

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adaptivesuite-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adaptivesuite-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adaptivesuite-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/adaptivesuite-tutorial/create_aaduser_04.png)

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-an-adaptive-insights-test-user"></a>Uyarlamalı Öngörüler test kullanıcısı oluşturma

Uyarlamalı Insights'a oturum açmak Azure AD kullanıcıları etkinleştirmek için bunlar Uyarlamalı Öngörüler sağlanmalıdır. Uyarlamalı Öngörüler söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:** 

1. Oturum, **Uyarlamalı Öngörüler** yönetici olarak şirket site.
2. Git **yönetici**.

   ![Yönetici](./media/adaptivesuite-tutorial/IC805644.png "yönetici")
3. İçinde **kullanıcılar ve roller** 'yi tıklatın **Kullanıcı Ekle**.

   ![Kullanıcı ekleme](./media/adaptivesuite-tutorial/IC805648.png "kullanıcı ekleme")
4. İçinde **yeni kullanıcı** bölümünde, aşağıdaki adımları gerçekleştirin:

   ![Gönderme](./media/adaptivesuite-tutorial/IC805649.png "Gönder")

   a. Tür **adı**, **oturum açma**, **e-posta**, **parola** istediğiniz ilgili metin kutularına sağlamayı geçerli bir Azure Active Directory kullanıcı.

   b. Seçin bir **rol**.

   c. Tıklatın **gönderme**.

>[!NOTE]
>API tarafından Uyarlamalı Öngörüler sağlama AAD kullanıcı hesaplarına sağlanan veya herhangi diğer Uyarlamalı Öngörüler kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
>

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Uyarlamalı Öngörüler'e erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

![Kullanıcı atama][200]

**Uyarlamalı Insights'a Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201]

2. Uygulamalar listesinde **Uyarlamalı Öngörüler**.

    ![Çoklu oturum açmayı yapılandırın](./media/adaptivesuite-tutorial/tutorial_adaptivesuite_app.png)

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.

### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Microsoft Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim paneli Uyarlamalı Öngörüler parçasında tıklattığınızda, otomatik olarak Uyarlamalı Öngörüler uygulamanıza açan.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/adaptivesuite-tutorial/tutorial_general_203.png
