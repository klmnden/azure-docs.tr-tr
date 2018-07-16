---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle ADP | Microsoft Docs'
description: Azure Active Directory ve ADP arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2018
ms.author: jeedes
ms.openlocfilehash: 75b84c2856373126ceba0fc536e41d270f4d2d05
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048788"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Öğretici: Azure Active Directory ADP ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile ADP tümleştirme konusunda bilgi edinin.

Azure AD ile ADP tümleştirme ile aşağıdaki avantajları sağlar:

- ADP erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan ADP (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile ADP yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Etkin ADP abonelik

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden ADP ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-adp-from-the-gallery"></a>Galeriden ADP ekleme
Azure AD'de ADP tümleştirmesini yapılandırmak için ADP Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ADP eklemek için aşağıdaki adımları gerçekleştirin:**

1.  Microsoft Azure kimlik sağlayıcısı ortamınıza bir yönetici olarak oturum açın.

2. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

3. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
4. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

5. Arama kutusuna **ADP**seçin **ADP** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ADP](./media/adpfederatedsso-tutorial/tutorial_adp_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı ADP sınayın.

Tek çalışmak için oturum açma için Azure AD ne ADP'de karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ile ilgili kullanıcı ADP'de arasında bir bağlantı ilişki kurulması gerekir.

ADP içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma ADP ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Bir ADP test kullanıcısı oluşturma](#create-an-adp-test-user)**  - kullanıcı Azure AD gösterimini bağlı ADP'de Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve ADP uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile ADP yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **ADP** uygulama tümleştirme sayfası, tıklayarak **Özellikleri sekmesi** ve aşağıdaki adımları gerçekleştirin: 

    ![Çoklu oturum açma özellikleri](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Ayarlama **kullanıcıların oturum açması etkinleştirildi** alan için değer **Evet**.

    b. Kopyalama **kullanıcı erişim URL'SİNDEN** ve yapıştırmak zorunda **yapılandırma oturum açma URL'si bölüm**, öğreticinin ilerleyen bölümlerinde açıklanmıştır.

    c. Ayarlama **kullanıcı ataması gerekli** alan için değer **Evet**.

    d. Ayarlama **kullanıcılara görünür** alan için değer **Hayır**.

2. Tıklayın **çoklu oturum açma** üzerinde **ADP** uygulama tümleştirme sayfası.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

3. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/adpfederatedsso-tutorial/tutorial_adp_samlbase.png)

4. Üzerinde **ADP etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![ADP etki alanı ve URL'ler tek oturum açma bilgileri](./media/adpfederatedsso-tutorial/tutorial_adp_url.png)

    İçinde **tanımlayıcı** metin kutusuna bir URL: `https://fed.adp.com` 
    
5. ADP uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir. Talep adı her zaman olacaktır **"PersonImmutableID"** ve değeri şu eşlendi **EmployeeID**. 

    Burada Azure AD'den kullanıcı eşlemeyi ADP üzerinde yapılır **EmployeeID** ancak bu uygulama ayarlarınıza göre farklı bir değer eşleyebilirsiniz. Bu nedenle iş Lütfen ile [ADP Destek ekibine](https://www.adp.com/contact-us/overview.aspx) önce bir kullanıcının doğru tanımlayıcı kullanın ve bu değerle eşleştirmek için **"PersonImmutableID"** talep.

    ![Çoklu oturum açmayı yapılandırın](./media/adpfederatedsso-tutorial/tutorial_adp_attribute.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı | Öznitelik Değeri |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.employeeid |
    
    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Çoklu oturum açmayı yapılandırın](./media/adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.
    
    d. **Tamam**’a tıklayın.

    > [!NOTE] 
    > SAML onaylaması yapılandırmadan önce iletişime geçmeniz, [ADP Destek ekibine](https://www.adp.com/contact-us/overview.aspx) ve kiracınız için benzersiz tanımlayıcı özniteliği değeri isteyin. Uygulamanız için özel talep yapılandırmak için bu değere ihtiyacınız. 

7. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/adpfederatedsso-tutorial/tutorial_adp_certificate.png) 

8. Çoklu oturum açmayı yapılandırma **ADP** tarafını ihtiyacınız indirilen yüklenecek **meta veri XML** üzerinde [ADP Web sitesi](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Bu işlem, birkaç gün sürebilir. 

### <a name="configure-your-adp-services-for-federated-access"></a>ADP hizmetlere federe erişim için yapılandırma

>[!Important]
> Çalışanlarınızın ADP hizmetlerinizi Federasyon erişmesi ADP service uygulamasına ve daha sonra atanması gerekir, kullanıcılar ilgili ADP hizmete atanmaları gerekir.
ADP temsilcinize onayı alındığında ADP hizmete kullanıcı erişimi denetlemek için ADP hizmetlere ve atama ve yönetme kullanıcılarınızın yapılandırın.

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **ADP**seçin **ADP** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde ADP](./media/adpfederatedsso-tutorial/tutorial_adp_addservicegallery.png)

5. Azure portalında, üzerinde **ADP** uygulama tümleştirme sayfası, tıklayarak **Özellikleri sekmesi** ve aşağıdaki adımları gerçekleştirin:  

    ![Çoklu oturum açma linkedproperties](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Ayarlama **kullanıcıların oturum açması etkinleştirildi** alan için değer **Evet**.

    b.  Ayarlama **kullanıcı ataması gerekli** alan için değer **Evet**.

    c.  Ayarlama **kullanıcılara görünür** alan için değer **Evet**.

6. Tıklayın **çoklu oturum açma** üzerinde **ADP** uygulama tümleştirme sayfası.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

7. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **bağlantılı oturum açma**. uygulamanıza bağlamak için **ADP**.

    ![Çoklu oturum bağlı açma](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Gidin **yapılandırma oturum açma URL'si** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma prop](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)
                                                              
    a. Yapıştırma **kullanıcı erişim URL'SİNDEN**, yukarıdaki kopyalanan **Özellikleri sekmesi** (uygulamadan ana ADP).
                                                             
    b. Aşağıda, 5 farklı destekleyen uygulamalar **geçiş durumu URL'leri**. Uygun eklemek zorunda **geçiş durumu URL** el ile geçirmek için belirli uygulama değer **kullanıcı erişim URL'SİNDEN**.
    
    * **Artık ADP iş gücü**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP iş gücü artık süresi geliştirildi**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **HCM ADP görüş**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **İK ADP Enterprise**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Kaydet** yaptığınız değişiklikleri.

10. Onay ekranında ADP temsilcinize alındığında, bir veya iki kullanıcı ile test başlayın.

    a. Birkaç kullanıcı ADP hizmete Federasyon erişimi test etmek için uygulama atayın.

    b. Kullanıcılar ADP hizmet app Galerisi'nde erişmek ve bunların ADP hizmete erişebilir, test başarılı olur.
 
11. Bireysel kullanıcılar ya da kullanıcı grupları, öğreticinin ilerleyen bölümlerinde açıklanmıştır ve çalışanlarınıza kullanıma konusunda daha fazla onay başarılı test federe ADP hizmet atayın. 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/adpfederatedsso-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/adpfederatedsso-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/adpfederatedsso-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/adpfederatedsso-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-an-adp-test-user"></a>Bir ADP test kullanıcısı oluşturma

Bu bölümün amacı ADP Britta Simon adlı bir kullanıcı oluşturmaktır. Çalışmak [ADP Destek ekibine](https://www.adp.com/contact-us/overview.aspx) ADP hesabında kullanıcıları eklemek için.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma ADP erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200] 

**Britta Simon ADP atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **ADP**.

    ![Uygulamalar listesinde ADP bağlantı](./media/adpfederatedsso-tutorial/tutorial_adp_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde ADP kutucuğa tıkladığınızda, otomatik olarak ADP uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/adpfederatedsso-tutorial/tutorial_general_203.png

