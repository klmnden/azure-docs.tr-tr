---
title: "Öğretici: İş için Dropbox Azure Active Directory Tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma Azure Active Directory ve iş için Dropbox arasında yapılandırmayı öğrenin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: c41760d60d53dee7be36b2af287cd6755605b708
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlamak için Dropbox iş için yapılandırma

Bu öğreticinin amacı, iş ve Azure AD otomatik olarak sağlamak ve kullanıcı hesaplarına Azure AD'den iş için Dropbox sağlanmasını için Dropbox gerçekleştirmek için gereken adımları Göster sağlamaktır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide gösterilen senaryo, aşağıdaki öğeleri zaten sahip olduğunuzu varsayar:

*   Bir Azure Active directory kiracısı.
*   Bir Dropbox iş çoklu oturum açma etkin abonelik için.
*   İş için Dropbox takım yönetici izinlerine sahip bir kullanıcı hesabının.

## <a name="assigning-users-to-dropbox-for-business"></a>İş için Dropbox kullanıcılar atama

Azure Active Directory "atamaları" adlı bir kavram hangi kullanıcıların seçili uygulamalara erişim alması belirlemek için kullanır. Otomatik olarak bir kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların "Azure AD uygulamada atanmış" eşitlenir.

Yapılandırma ve sağlama hizmeti etkinleştirmeden önce hangi kullanıcılara ve/veya Azure AD grupları için iş uygulaması, Dropbox erişmek isteyen kullanıcıların temsil eden karar vermeniz gerekir. Karar sonra bu kullanıcılar, Dropbox iş uygulaması için buradaki yönergeleri izleyerek atayabilirsiniz:

[Bir kullanıcı veya grup için bir kuruluş uygulama atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>İş için Dropbox kullanıcılara atamak için önemli ipuçları

*   Önerilir tek bir Azure AD kullanıcısının iş sağlama yapılandırmayı test etmek için Dropbox atanır. Ek kullanıcı ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcı, iş için Dropbox atarken, geçerli bir kullanıcı rolünün seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz...

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde API sağlama işletmenin kullanıcı hesabı için Azure AD Dropbox'a konusunda size rehberlik eder ve oluşturmak için sağlama hizmeti yapılandırma güncelleştirin ve Azure AD'de kullanıcı ve grup atama temel iş için Dropbox atanan kullanıcı hesaplarında devre dışı bırakın.

>[!Tip]
>Da tercih edebilirsiniz etkin SAML tabanlı çoklu oturum açma iş için dropbox, yönergeleri izleyerek sağlanan [Azure portal](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlamayı bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik olarak bir kullanıcı hesabı sağlama yapılandırmak için:

1. İçinde [Azure portal](https://portal.azure.com), Gözat **Azure Active Directory > Kurumsal uygulamaları > tüm uygulamaları** bölümü.

2. Çoklu oturum açma için iş için Dropbox zaten yapılandırdıysanız arama alanı kullanarak iş için Dropbox örneğiniz arayın. Aksi takdirde seçin **Ekle** arayın ve **iş için Dropbox** uygulama galerisinde. İş için Dropbox Arama sonuçlarından seçin ve uygulamaları listenize ekleyin.

3. İş için Dropbox örneğiniz seçin, sonra seçin **sağlama** sekmesi.

4. Ayarlama **sağlama modunda** için **otomatik**. 

    ![Sağlama](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. Altında **yönetici kimlik bilgileri** 'yi tıklatın **Authorize**. Bir Dropbox iş oturum açma iletişim için yeni bir tarayıcı penceresinde açar.

6. Üzerinde **oturum açma için Azure AD ile bağlamak için Dropbox** iletişim kutusunda, Dropbox iş Kiracı için oturum açın.

     ![Kullanıcı sağlamayı](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "kullanıcı hazırlama")

7. Azure Active Directory, Dropbox iş Kiracı için değişiklik izni vermek istediğiniz onaylayın. Tıklatın **izin**.
    
      ![Kullanıcı sağlamayı](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "kullanıcı hazırlama")

8. Azure portalında tıklatın **Bağlantıyı Sına** Azure emin olmak için AD, Dropbox iş uygulamasının bağlanabilir. Bağlantı başarısız olursa, iş hesabı Team yönetici izinleri olan için Dropbox emin olun ve deneyin **"Yetkilendir"** adım yeniden uygulayın.

9. Bir kişi veya sağlama hata bildirimleri alması gereken Grup e-posta adresini girin **bildirim e-posta** alan ve onay kutusunu işaretleyin.

10. Tıklatın **kaydedin.**

11. Eşlemeleri bölümü altında seçin **eşitleme Azure Active Directory Kullanıcıları iş için Dropbox için.**

12. İçinde **öznitelik eşlemelerini** bölümünde, iş için Dropbox için Azure AD'den eşitlenen kullanıcı öznitelikleri gözden geçirin. Seçilen öznitelikler **eşleşen** özellikleri, iş için Dropbox kullanıcı hesaplarında güncelleştirme işlemleri için eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

13. İş için dropbox hizmet sağlama Azure AD etkinleştirmek için değiştirmek **sağlama durumu** için **üzerinde** ayarları bölümünde

14. Tıklatın **kaydedin.**

Herhangi bir kullanıcı ve/veya Dropbox kullanıcılar ve Gruplar bölümünde iş için atanan grupları ilk eşitleme başlatır. İlk eşitleme gerçekleştirmek yaklaşık 20 dakikada çalıştığı sürece oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve, Dropbox sağlama hizmeti iş uygulaması tarafından gerçekleştirilen tüm eylemler açıklanmaktadır etkinlik raporları sağlamak için bağlantıları izleyin.

Şimdi sınama hesabı oluşturabilirsiniz. İş için Dropbox hesabı eşitlendiğinden emin doğrulamak için en çok 20 dakika bekleyin.

Başarıyla tamamlanan kullanıcı döngüsü hazırlama ilgili durumuna göre belirtilir.

![Kullanıcılar atama](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "kullanıcı atama")


## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı Kurumsal uygulamaları için sağlama yönetme](active-directory-saas-tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md)
* [Çoklu oturum açmayı yapılandırın](active-directory-saas-dropboxforbusiness-tutorial.md)