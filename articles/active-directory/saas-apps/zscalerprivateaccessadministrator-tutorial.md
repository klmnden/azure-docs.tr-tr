---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Zscaler özel erişim Yöneticisi | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory Zscaler özel erişim Yöneticisi arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 7888798fd80eb748c882861db5d552dc4b670823
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35989223"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Öğretici: Azure Active Directory Tümleştirme ile Zscaler özel erişim Yöneticisi

Bu öğreticide, Azure Active Directory (Azure AD) ile Zscaler özel erişim Yöneticisi tümleştirme öğrenin.

Zscaler özel erişim Yöneticisi Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Erişimi için Zscaler özel erişim yöneticisine, Azure AD'de kontrol edebilirsiniz.
- Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Zscaler özel erişim yöneticisine açan kullanıcılarınıza etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme Zscaler özel erişim yöneticisiyle yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir Zscaler özel erişim Yöneticisi çoklu oturum açma abonelik etkin

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden Zscaler özel erişim Yöneticisi ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Galeriden Zscaler özel erişim Yöneticisi ekleme
Azure AD Zscaler özel erişim Yöneticisi'nin tümleştirmeye yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden Zscaler özel erişim Yöneticisi eklemeniz gerekir.

**Galeriden Zscaler özel erişim yönetici eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna yazın **Zscaler özel erişim Yöneticisi**seçin **Zscaler özel erişim Yöneticisi** sonuç panelinden ardından **Ekle** Ekle düğmesi uygulama.

    ![Sonuçlar listesinde Zscaler özel erişim Yöneticisi](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Zscaler özel erişim "Britta Simon" adlı bir test kullanıcı tabanlı Yöneticisi ile test etme.

Tekli çalışmaya oturum için Azure AD ne karşılık gelen Zscaler özel erişim Yöneticisi'nde bir kullanıcı için Azure AD içinde olduğu bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının ve ilgili kullanıcı Zscaler özel erişim Yöneticisi'nde arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Zscaler özel erişim Yöneticisi ile test etmek için aşağıdaki yapı taşları tamamlamanız gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Zscaler özel erişim Yöneticisi test kullanıcısı oluşturma](#create-a-zscaler-private-access-administrator-test-user)**  - Britta Simon, karşılık gelen Zscaler özel erişim kullanıcı Azure AD gösterimini bağlantılı Yöneticisi'nde sağlamak için.
4. **[Azure AD test kullanıcısı atayın](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Test çoklu oturum açma](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Zscaler özel erişim Yöneticisi uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma Zscaler özel erişim yöneticisiyle yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **Zscaler özel erişim Yöneticisi** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_samlbase.png)

3. Üzerinde **Zscaler özel erişim yöneticisinin etki alanı ve URL'leri** uygulamada yapılandırmak isterseniz, bölümü **IDP** modu tarafından başlatılan:

    ![Zscaler özel erişim yöneticisinin etki alanı ve URL'leri tek oturum açma bilgileri](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_url.png)

    a. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. İçinde **yanıt URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Denetleme **Göster Gelişmiş URL ayarları**

    d. İçinde **RelayState** metin kutusuna, bir değer yazın: `idpadminsso`

4.  Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu aşağıdaki adımları gerçekleştirin:

    İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.private.zscaler.com/auth/sso`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Kişi [Zscaler özel erişim Yöneticisi destek ekibi](https://help.zscaler.com/zpa-submit-ticket) bu değerleri almak için.
 
5. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_certificate.png) 

6. Tıklatın **kaydetmek** düğmesi.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_400.png)

7. Bir farklı web tarayıcısı penceresinde, oturum açma için yönetici olarak Zscaler özel erişim Yöneticisi.

8. Üstte tıklatın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırma**.

    ![Zscaler özel erişim yönetici yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

9. Sağ alt köşesinde üst tıklatın **IDP Yapılandırması Ekle**. 

    ![Zscaler özel erişim Yöneticisi addidp](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

10. Üzerinde **IDP Yapılandırması Ekle** sayfasında aşağıdaki adımları gerçekleştirin:
 
    ![Zscaler özel erişim Yöneticisi idpselect](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Tıklatın **Dosya Seç** Azure AD'de indirilen meta veri dosyasından karşıya yüklemek için **IDP meta veriler Karşıya Dosya Yükle** alan.

    b. Bunu okuyan **IDP meta veri** Azure AD'den ve aşağıda gösterildiği gibi tüm alanlar bilgisi doldurur.

    ![Zscaler özel erişim Yöneticisi idpconfig](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Seçin **çoklu oturum açmayı** olarak **yönetici**.

    d. Etki alanınızdan seçin **etki alanları** alan.
    
    e. **Kaydet**’e tıklayın.

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Sol bölmede, Azure portal'ı tıklatın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklatın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** kullanıcı Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değer aşağı yazma **parola** kutusu.

    d. **Oluştur**’a tıklayın.
  
### <a name="create-a-zscaler-private-access-administrator-test-user"></a>Zscaler özel erişim Yöneticisi test kullanıcısı oluşturma

Azure AD kullanıcılarının Zscaler özel erişim yönetici oturum açmayı etkinleştirmek için bunların içine Zscaler özel erişim Yöneticisi sağlanmalıdır. Zscaler özel erişim söz konusu olduğunda, sağlama el ile bir görev yöneticisidir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Zscaler özel erişim Yöneticisi şirket sitenize yönetici olarak oturum açın.

2. Üstte tıklatın **Yönetim** gidin **kimlik doğrulaması** bölümünde **IDP yapılandırma**.

    ![Zscaler özel erişim yönetici yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Tıklatın **Yöneticiler** menüsünün sol taraftaki.

    ![Zscaler özel erişim Yöneticisi Yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. Sağ alt köşesinde üst tıklatın **Yöneticisi Ekle**:

    ![Zscaler özel erişim Yöneticisi Yönetici ekleme](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. İçinde **Yöneticisi Ekle** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Zscaler özel erişim yönetici kullanıcı yönetici](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. İçinde **kullanıcıadı** metin kutusuna, bir kullanıcı gibi e-posta girin **BrittaSimon@contoso.com**.

    b. İçinde **parola** metin kutusuna, parolayı yazın.

    c. İçinde **parolayı onayla** metin kutusuna, parolayı yazın.

    d. Seçin **rol** olarak **Zscaler özel erişim Yöneticisi**.

    e. İçinde **e-posta** metin kutusuna, bir kullanıcı gibi e-posta girin **BrittaSimon@contoso.com**.

    f. İçinde **telefon** metin kutusuna, telefon numarasını yazın.

    g. İçinde **saat dilimi** metin kutusuna, saat dilimi seçin.

    h. **Kaydet**’e tıklayın.  

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Britta Zscaler özel erişim Yöneticisi için erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon Zscaler özel erişim yöneticiye atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **Zscaler özel erişim Yöneticisi**.

    ![Uygulamalar listesinde Zscaler özel erişim Yöneticisi bağlantı](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_app.png)  

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202]

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Ekleme atama bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Zscaler özel erişim Yöneticisi parçasında tıklattığınızda, otomatik olarak Zscaler özel erişim Yöneticisi uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_01.png
[2]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_02.png
[3]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_03.png
[4]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_04.png

[100]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_100.png

[200]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_200.png
[201]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_201.png
[202]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_202.png
[203]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_203.png

