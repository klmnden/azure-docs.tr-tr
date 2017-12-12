---
title: "Öğretici: Azure Active Directory Tümleştirme ile Replicon | Microsoft Docs"
description: "Çoklu oturum açma, otomatik sağlama ve daha fazla etkinleştirmek için Azure Active Directory ile Replicon kullanmayı öğrenin!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: mtillman
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 2d735ceb8be2a3ff5bef33b1c39969591afca3dc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Öğretici: Azure Active Directory Tümleştirme Replicon ile
Bu öğreticinin amacı, Azure ve Replicon tümleştirmesini göstermektir. Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

* Geçerli bir Azure aboneliği
* Replicon Kiracı

Bu öğreticiyi tamamladıktan sonra Replicon için atanmış Azure AD kullanıcıları çoklu oturum açma Replicon şirket sitenizdeki (servis sağlayıcı tarafından başlatılan oturum açma) uygulamaya veya kullanarak okuyamayacak [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

Bu öğreticide gösterilen senaryo, aşağıdaki yapı taşlarını oluşur:

1. Uygulama tümleştirmesi Replicon için etkinleştirme
2. Çoklu oturum açma (SSO) yapılandırma
3. Kullanıcı hazırlama işleminin yapılandırılması
4. Kullanıcılar atama

![Senaryo](./media/active-directory-saas-replicon-tutorial/IC777798.png "senaryosu")

## <a name="enable-the-application-integration-for-replicon"></a>Replicon uygulama tümleştirmeyi etkinleştir
Bu bölümün amacı Replicon için uygulama tümleştirme sağlamak üzere nasıl anahat sağlamaktır.

**Replicon uygulama tümleştirmesini etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında, sol gezinti bölmesinde tıklatın **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")
2. Gelen **Directory** listesinde, directory tümleştirmesini etkinleştirmek istediğiniz dizini seçin.
3. Dizin görünümünde uygulamaları görünümü açmak için **uygulamaları** üst menüde.
   
    ![Uygulamaları](./media/active-directory-saas-replicon-tutorial/IC700994.png "uygulamalar")
4. Tıklatın **Ekle** sayfanın sonundaki.
   
    ![Uygulama ekleme](./media/active-directory-saas-replicon-tutorial/IC749321.png "uygulama ekleme")
5. Üzerinde **ne yapmak istiyorsunuz** iletişim kutusunda, tıklatın **Galeriden bir uygulama eklemek**.
   
    ![Galeriden bir uygulama eklemek](./media/active-directory-saas-replicon-tutorial/IC749322.png "Galeriden bir uygulama ekleme")
6. İçinde **arama kutusu**, türü **Replicon**.
   
    ![Uygulama Galerisi](./media/active-directory-saas-replicon-tutorial/IC777799.png "uygulama Galerisi")
7. Sonuçlar bölmesinde seçin **Replicon**ve ardından **tam** uygulama eklemek için.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırın

Bu bölümün amacı kullanıcıların Replicon için kendi hesabıyla SAML protokolünü temel Federasyon kullanarak Azure AD kimlik doğrulaması sağlamak nasıl anahat sağlamaktır.

**Çoklu oturum açma yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure Klasik portalında üzerinde **Replicon** uygulama tümleştirmesi sayfasında, tıklatın **çoklu oturum açma özelliğini yapılandırın** açmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-replicon-tutorial/IC777801.png "çoklu oturum açmayı yapılandırın")
2. Üzerinde **Replicon için oturum açmasını nasıl istiyorsunuz** sayfasında, **Microsoft Azure AD çoklu oturum açma**ve ardından **sonraki**.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-replicon-tutorial/IC777802.png "çoklu oturum açmayı yapılandırın")
3. Üzerinde **uygulama URL'sini Yapılandır** sayfasında, aşağıdaki adımları gerçekleştirin:
   
    ![Uygulama URL'sini Yapılandır](./media/active-directory-saas-replicon-tutorial/IC777803.png "uygulama URL'sini yapılandırın")
  1. İçinde **üzerinde Replicon oturum URL'si** metin kutusuna, Replicon Kiracı URL'nizi yazın (örneğin: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. İçinde **Replicon yanıt URL'si** metin, Replicon yazın **AssertionConsumerService** URL'si (örn: *https://global.replicon.com/! / saml2/şirket/sso/post*).  
      
     >[!NOTE]
     >Replicon meta verilerden URL'yi elde edebilirsiniz: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. **İleri**’ye tıklayın.

4. Üzerinde **çoklu oturum açma sırasında Replicon yapılandırma** meta verileriniz, indirme sayfasında, tıklatın **karşıdan meta veri**ve ardından meta verileri bilgisayarınıza kaydedin.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-replicon-tutorial/IC777804.png "çoklu oturum açmayı yapılandırın")
5. Farklı web tarayıcısı penceresinde Replicon şirket sitenize yönetici olarak oturum açın.

6. SAML 2.0 yapılandırmak için aşağıdaki adımları gerçekleştirin:
   
    ![SAML kimlik doğrulamasını etkinleştir](./media/active-directory-saas-replicon-tutorial/IC777805.png "etkinleştirmek SAML kimlik doğrulaması")
  
  1. Görüntülenecek **EnableSAML Authentication2** iletişim kutusunda, aşağıdaki URL'nizi, sonra şirket anahtarınızı ekleme: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * Aşağıda tam URL şeması gösterilmektedir:  
   **https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Tıklatın  **+**  genişletmek için **v20Configuration** bölümü.
   3. Tıklatın  **+**  genişletmek için **metaDataConfiguration** bölümü.
   4. Tıklatın **Dosya Seç**, kimlik sağlayıcısı meta verileri XML dosyasını seçin ve ' **gönderme**.

7. Klasik Azure portalı, çoklu oturum açma yapılandırması onay seçin ve ardından **tam** kapatmak için **tek oturum yapılandırma üzerinde aktar** iletişim.
   
    ![Çoklu oturum açma yapılandırma](./media/active-directory-saas-replicon-tutorial/IC778418.png "çoklu oturum açmayı yapılandırın")
   
## <a name="configure-user-provisioning"></a>Kullanıcı sağlamayı Yapılandır

Azure AD kullanıcıların Replicon oturum etkinleştirmek için bunların Replicon sağlanmalıdır.  

Replicon söz konusu olduğunda, sağlama bir el ile bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde Replicon şirket sitenize yönetici olarak oturum açın.
2. Git **Yönetim \> kullanıcılar**.
   
    ![Kullanıcıların](./media/active-directory-saas-replicon-tutorial/IC777806.png "kullanıcılar")
3. Tıklatın **+ kullanıcı ekleme**.
   
    ![Kullanıcı ekleme](./media/active-directory-saas-replicon-tutorial/IC777807.png "kullanıcı ekleme")
4. İçinde **kullanıcı profili** bölümünde, aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı profili](./media/active-directory-saas-replicon-tutorial/IC777808.png "kullanıcı profili")
   
  1. İçinde **oturum açma adı** metin kutusuna, sağlamak istediğiniz Azure AD kullanıcısının e-posta adresini Azure AD yazın.
  2. Olarak **kimlik doğrulama türü**seçin **SSO**.
  3. İçinde **departmanı** metin kutusuna, kullanıcının bölüm adını yazın.
  4. Olarak **çalışan türü**seçin **yönetici**.
  5. Tıklatın **kullanıcı profili Kaydet**.

>[!NOTE]
>API sağlama AAD kullanıcı hesaplarına Replicon tarafından sağlanan veya herhangi diğer Replicon kullanıcı hesabı oluşturma araçlarını kullanabilirsiniz.
> 
> 

## <a name="assign-users"></a>Kullanıcılar atama
Yapılandırmanızı test etmek için uygulama erişimi atayarak kullanarak izin vermek istediğiniz Azure AD kullanıcılarının vermeniz gerekir.

**Kullanıcılar için Replicon atamak için aşağıdaki adımları gerçekleştirin:**

1. Klasik Azure portalında bir test hesabı oluşturun.

2. Üzerinde **Replicon** uygulama tümleştirme sayfasını tıklatın **kullanıcı atama**.
   
    ![Kullanıcılar atama](./media/active-directory-saas-replicon-tutorial/IC777809.png "kullanıcı atama")

3. Test kullanıcınız seçin, **atamak**ve ardından **Evet** , atama onaylamak için.
   
    ![Evet](./media/active-directory-saas-replicon-tutorial/IC767830.png "Evet")

Çoklu oturum açma ayarlarınızı test etmek isterseniz, erişim paneli açın. Erişim paneli hakkında daha fazla ayrıntı için bkz: [erişim Paneli'ne giriş](active-directory-saas-access-panel-introduction.md).

