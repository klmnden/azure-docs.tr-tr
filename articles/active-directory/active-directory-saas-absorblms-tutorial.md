---
title: 'Öğretici: Azure Active Directory Tümleştirme almak LMS ile | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory almak LMS arasındaki yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jeedes
ms.openlocfilehash: f877d8fee4a94207fc01f4a5e0e7919f1286f2e4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34340674"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Öğretici: Azure Active Directory Tümleştirme almak LMS ile

Bu öğreticide, en yüksek noktaları almak LMS Azure Active Directory (Azure AD) ile tümleştirme öğrenin.

En yüksek noktaları almak LMS Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- En yüksek noktaları almak LMS erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak Al LMS (aracılığıyla çoklu oturum açma) ile Azure AD hesaplarına oturum açmak, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınızı bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile hizmet (SaaS) uygulaması tümleştirme olarak yazılım hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme almak LMS ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir almak LMS çoklu oturum açma abonelik etkin

> [!NOTE]
> Bir üretim ortamında Bu öğretici için kullanmayan öneririz.

Bu öğreticide adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, şunları yapabilirsiniz [bir aylık deneme sürümünü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

* Galeriden almak LMS ekleme
* Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="add-absorb-lms-from-the-gallery"></a>Galeriden almak LMS ekleme
Azure AD almak LMS tümleştirilmesi yapılandırmak için almak LMS Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden almak LMS eklemek için aşağıdakileri yapın:

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede seçin **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Git **kurumsal uygulamalar** > **tüm uygulamaları**.

    ![Kuruluş uygulamaları bölmesi][2]
    
3. Bir uygulama eklemek için seçin **yeni uygulama** düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **almak LMS**seçin **almak LMS** sonuç paneli ve ardından **Ekle** düğmesi.

    ![Sonuçlar listesinde LMS Al](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmak ve Azure AD çoklu oturum açma Britta Simon adlı bir test kullanıcı tabanlı LMS almak sınayın.

Tekli çalışmaya oturum için Azure AD Azure AD'de almak LMS karşılık gelen kullanıcı nedir bilmek ister. Diğer bir deyişle, Azure AD'de kullanıcı almak LMS karşılık gelen kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Atayarak bu bağlantı ilişkisini kurmak *kullanıcı adı* olarak Azure AD değeri *kullanıcıadı* almak LMS değeri.

Yapılandırma ve Azure AD çoklu oturum açma almak LMS ile test etmek için sonraki beş bölümlerde yapı taşları tamamlayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma almak LMS uygulamanızda yapılandırın.

Azure AD çoklu oturum açma almak LMS ile yapılandırmak için aşağıdakileri yapın:

1. Azure portalında üzerinde **almak LMS** uygulama tümleştirmesi sayfasında, **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. İçinde **çoklu oturum açma** iletişim kutusunda **modu** kutusunda **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açma iletişim kutusu](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. İçinde **LMS etki alanı almak ve URL'leri** bölümünde, aşağıdakileri yapın:

    ![LMS etki alanı ve URL'leri tek oturum açma bilgilerini al](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. İçinde **tanımlayıcısı** kutusunda, aşağıdaki söz dizimini kullanan bir URL yazın: `https://<subdomain>.myabsorb.com/Account/SAML`.

    b. İçinde **yanıt URL'si** kutusunda, aşağıdaki söz dizimini kullanan bir URL yazın: `https://<subdomain>.myabsorb.com/Account/SAML`.
     
    > [!NOTE] 
    > Bu URL'leri gerçek değerleri değildir. Bunları, gerçek tanımlayıcısı ve yanıt URL'leri güncelleştirin. Bu değerleri almak için başvurun [almak LMS istemci destek ekibi](https://www.absorblms.com/support). 

4. İçinde **SAML imzalama sertifikası** bölümünde **karşıdan** sütun, select **meta veri XML**ve meta veri dosyası bilgisayarınıza kaydedin.

    ![İmzalama sertifikası indirme bağlantısı](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

5. **Kaydet**’i seçin.

    ![Oturum açma tek Kaydet düğmesi yapılandırın](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
6. İçinde **almak LMS yapılandırma** bölümünde, select **almak LMS yapılandırma** açmak için **yapılandırma oturum açma** penceresi ve kopyalayın **Sign-Out URL** içinde **hızlı başvuru bölümü.**

    ![LMS yapılandırma almak bölmesi](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

7. Yeni bir web tarayıcısı penceresinde almak LMS şirket sitenize yönetici olarak oturum açın.

8. Seçin **hesap** sağ üst köşedeki düğmesi. 

    ![Hesap düğmesi](./media/active-directory-saas-absorblms-tutorial/1.png)

9. Hesap bölmesinde seçin **Portalı Ayarları**.

    ![Portal ayarlarını bağlantı](./media/active-directory-saas-absorblms-tutorial/2.png)
    
10. **Users (Kullanıcılar)** sekmesini seçin.

    ![Kullanıcılar sekmesi](./media/active-directory-saas-absorblms-tutorial/3.png)

11. Çoklu oturum açma yapılandırma sayfasında, aşağıdakileri yapın:

    ![Çoklu oturum açma yapılandırma sayfası](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. İçinde **modu** kutusunda **kimlik sağlayıcısı tarafından başlatılan**.

    b. Azure portalından indirdiğiniz sertifika Not Defteri'nde açın. Kaldırma **---başlangıç sertifika---** ve **---son SERTİFİKAYI---** etiketler. Ardından **anahtar** kutusunda, kalan içeriği yapıştırın.
    
    c. İçinde **ID özelliği** kutusunda, Azure AD'de kullanıcı tanımlayıcısı olarak yapılandırılmış özniteliği seçin. Örneğin, varsa *userPrincipalName* Azure AD'de seçin seçili **kullanıcıadı**.

    d. İçinde **oturum açma URL'si** kutusunda, yapıştırma **kullanıcı erişim URL'si** uygulamanın gelen **özellikleri** Azure portal sayfası.

    e. İçinde **oturum kapatma URL'si**, yapıştırma **Sign-Out URL** öğesinden kopyalanan değeri **yapılandırma oturum açma** Azure portalının penceresi.

12. İki durumlu **yalnızca SSO oturum açma izin** için **üzerinde**.

    ![Yalnızca izin SSO oturum açma Değiştir](./media/active-directory-saas-absorblms-tutorial/5.png)

13. Seçin **kaydedin.**

> [!TIP]
> Bu yönergelerde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com) uygulaması kuruluyor sırada. Uygulamadan ekledikten sonra **Active Directory** > **kurumsal uygulamalar** bölümünde, select **çoklu oturum açma** sekmesinde ve katıştırılmış erişim belgeleri etraflıca **yapılandırma** alt bölüm. Daha fazla bilgi için bkz: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında test kullanıcısı Britta Simon oluşturun.

![Bir Azure AD test kullanıcısı oluşturma][100]

Azure AD'de bir sınama kullanıcısı oluşturmak için aşağıdakileri yapın:

1. Azure portalında sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için seçin **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
    
    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantılar](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. İletişim kutusunun üstündeki seçin **Ekle**.
 
    ![Ekle düğmesi](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdakileri yapın:
 
    ![Kullanıcı iletişim kutusu](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, Britta Simon e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından değeri not edin **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="create-an-absorb-lms-test-user"></a>Bir almak LMS test kullanıcısı oluşturma

Azure AD kullanıcılarının LMS almak için oturum açmak bunlar almak LMS ayarlanması gerekir.  

LMS almak için Kurulum el ile bir görevdir.

Bir kullanıcı hesabı ayarlamak için aşağıdakileri yapın:

1. En yüksek noktaları almak LMS şirket sitenize yönetici olarak oturum açın.

2. Sol bölmede seçin **kullanıcılar**.

    ![LMS kullanıcıları Al bağlantısı](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. İçinde **kullanıcılar** bölmesinde, **kullanıcılar**.

    ![Kullanıcılar bağlantı](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4. İçinde **yeni Ekle** aşağı açılan listesinden, **kullanıcı**.

    ![Yeni Ekle aşağı açılan liste](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Üzerinde **Kullanıcı Ekle** sayfasında, aşağıdakileri yapın:

    ![Kullanıcı Ekle sayfası](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. İçinde **ad** ad gibi yazın **Britta**.

    b. İçinde **Soyadı** Soyadı gibi yazın **Simon**.
    
    c. İçinde **kullanıcıadı** kutusunda, bir tam ad gibi yazın **Britta Simon**.

    d. İçinde **parola** Britta Simon'ın parolayı yazın.

    e. İçinde **parolayı onayla** kutusunda, parolayı yeniden yazın.
    
    f. Ayarlama **etkindir** geç **etkin**.  

6. Seçin **kaydedin.**
 
### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, kullanıcı Britta almak LMS erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

Kullanıcı Britta Simon almak LMS atamak için aşağıdakileri yapın:

1. Azure Portalı'ndaki uygulamaların görünümü açma, dizin görünümüne gidin ve ardından **kurumsal uygulamalar** > **tüm uygulamaları**.

    !["Tüm uygulamaları" bağlantı][201] 

2. İçinde **uygulamaları** listesinde **almak LMS**.

    ![Uygulamalar listesini almak LMS bağlantıyı](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. Sol bölmede seçin **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantı][202] 

4. Seçin **Ekle** , daha sonra **eklemek atama** bölmesinde, **kullanıcılar ve gruplar**.

    ![Ekleme atama bölmesi][203]

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **kullanıcılar** listesinde **Britta Simon**.

6. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **seçin** düğmesi.

7. İçinde **eklemek atama** iletişim kutusunda **atamak** düğmesi.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde seçme **almak LMS** döşeme otomatik olarak oturum açtığında, almak LMS uygulamanıza. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

