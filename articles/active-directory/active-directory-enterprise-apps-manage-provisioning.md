---
title: "Kullanıcı Yönetimi Azure Active Directory'de kurumsal uygulamalar için hazırlama | Microsoft Docs"
description: "Kullanıcı hesabı için Azure Active Directory'yi kullanarak kurumsal uygulamalar sağlama yönetmeyi öğrenin"
services: active-directory
documentationcenter: 
author: asmalser
manager: mtillman
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 122dbdb838377a36020f9ec692b38544004e676c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Azure portalında Kurumsal uygulamaları için sağlama kullanıcı hesabı yönetme
Bu makalede nasıl kullanılacağını açıklar [Azure portal](https://portal.azure.com) otomatik olarak bir kullanıcı hesabı sağlama ve bunu "özel" kategoriden eklenen olanları özellikle destekleyen uygulamalarda sağlamayı kaldırma özelliklerini yönetmek için [Azure Active Directory Uygulama galerisinde](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Otomatik olarak bir kullanıcı hesabı sağlama ve nasıl çalıştığı hakkında daha fazla bilgi için bkz: [otomatikleştirmek kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini Azure Active Directory ile SaaS uygulamalarına](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Portalda uygulamalarınızı bulma
Bir dizindeki çoklu oturum açma kullanarak bir dizin yönetici tarafından yapılandırılan tüm uygulamaları [Azure Active Directory Uygulama galerisinde](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), görüntülenebilir ve yönetilen [Azure portal](https://portal.azure.com). Uygulamaları bulunabilir **daha Hizmetleri** &gt; **kurumsal uygulamalar** portalı bölümü. Kurumsal uygulamalar dağıtılan ve kuruluşunuzda kullanılan uygulamalardır.

![Kurumsal uygulamalar dikey penceresi][0]

Seçme **tüm uygulamaları** bağlantıyı soldaki, Galeriden eklenen uygulamalar dahil olmak üzere yapılandırılmış tüm uygulamaların bir listesini gösterir. Bir uygulamayı seçerek burada bu uygulama için raporlar görüntülenebilir ve çeşitli ayarları yönetilebilmesi için bu uygulama için kaynak dikey yükler.

Kullanıcı hesabının ayarlarını sağlama seçerek yönetilebilir **sağlama** soldaki.

![Uygulama kaynağı dikey][1]

## <a name="provisioning-modes"></a>Sağlama modları
**Sağlama** dikey ile başlayıp bir **modu** hangi sağlama modları Kurumsal uygulama için desteklenen gösterir ve bunları yapılandırılacak veren menüsü. Mevcut seçenekler şunlardır:

* **Otomatik** -bu seçenek, Azure AD API tabanlı otomatik sağlama ve/veya bu uygulama için kullanıcı hesaplarının sağlamayı kaldırma özelliklerini destekliyorsa görünür. Bu mod seçmek, Hesap Eşleştirmeleri ve kullanıcı hesabı veri Azure AD arasında akışı nasıl olmalı tanımlamak iş akışları oluşturma, uygulamanın kullanıcı yönetimi API bağlanmak için Azure AD yapılandırma yoluyla Yöneticiler kılavuzları bir arabirim görüntüler ve uygulama ve hizmet sağlama Azure AD yönetme.
* **El ile** -Azure AD kullanıcı hesaplarının bu uygulamaya otomatik sağlamayı desteklemiyor, bu seçenek gösterilir. Bu seçenek, uygulamada depolanan kullanıcı hesabı kayıtları (içerebilen SAML Just-In-Time sağlama) bu uygulama tarafından sağlanan kullanıcı yönetimi ve sağlama yeteneklerine dayalı bir dış işlem kullanarak yönetilmelidir anlamına gelir.

## <a name="configuring-automatic-user-account-provisioning"></a>Otomatik olarak bir kullanıcı hesabı sağlama yapılandırma
Seçme **otomatik** seçeneği, dört bölümlerde bölünmüş bir ekran görüntüler:

### <a name="admin-credentials"></a>Yönetici Kimlik Bilgileri
Bu, burada API girilirse uygulamanın kullanıcı yönetimi bağlanmak Azure AD için kimlik bilgileri gerekli değildir. Gerekli giriş uygulamaya bağlı olarak değişir. Kimlik bilgisi türleri ve belirli uygulamaları için gereksinimleri hakkında bilgi edinmek için [, belirli bir uygulama için yapılandırma Öğreticisi](active-directory-saas-app-provisioning.md).

Seçme **Bağlantıyı Sına** düğmesi sağlar, kimlik bilgilerinin Azure sağlayarak test etmek uygulamaya bağlanmak için AD girişim sağlanan kimlik bilgilerini kullanarak uygulama sağlama.

### <a name="mappings"></a>Eşlemeler
Burada admins görüntüleyebilir ve Azure AD arasında hangi kullanıcı öznitelikleri akışı Düzenle budur ve kullanıcı hesaplarını sağlandığında veya güncelleştirildiğinde, hedef uygulama.

Önceden yapılandırılmış bir Azure AD kullanıcı ve her SaaS uygulamanın kullanıcı nesneleri arasındaki eşlemeleri kümesi yok. Bazı uygulamalar, diğer grupların veya kişilerin gibi nesne türlerini yönetin. Bu eşlemeler birini tablo gösterir burada bunlar görüntülenebilir ve özelleştirilebilir sağa Eşleme Düzenleyicisi'ni seçerek.

![Uygulama kaynağı dikey][2]

Desteklenen özelleştirmeler aşağıdakileri içerir:

* Etkinleştirme ve eşlemeleri SaaS uygulamanın kullanıcı nesnesi için Azure AD kullanıcı nesnesi gibi belirli nesneler için devre dışı bırakma.
* Hangi özniteliklerin Azure AD kullanıcı nesnesinden uygulamanın kullanıcı nesnesi için akış düzenleme. Özellik eşlemesi hakkında daha fazla bilgi için bkz: [özniteliği eşleme türlerini anlama](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Azure AD hedeflenen uygulamaya bağlı gerçekleştirdiği sağlama eylemleri filtreleyin. Tam eşitleme-nesnelerin Azure AD sahip olmak yerine gerçekleştirilen eylemler sınırlayabilirsiniz. Örneğin, seçerek yalnızca **güncelleştirme**, var olan kullanıcı hesaplarını bir uygulamada ve yenilerini oluşturmaz Azure AD yalnızca güncelleştirmeler. Yalnızca seçerek **oluşturma**, Azure yalnızca yeni kullanıcı hesapları oluşturur ama var olanları güncelleştirmez. Bu özellik, hesap oluşturma için farklı eşlemelerini oluşturmak ve iş akışları güncelleştirmek yöneticilerinin olanak tanır.

### <a name="settings"></a>Ayarlar
Bu bölümde yöneticilerinin başlatmak ve seçili uygulama için hizmet sağlama Azure AD yanı sıra isteğe bağlı olarak sağlama önbelleğini temizlemek hizmetini durdurun ve başlatın sağlamasına olanak tanır.

Sağlama için uygulamanın ilk kez etkinleştiriliyor, hizmette değiştirerek kapatma **sağlama durumu** için **üzerinde**. Bu neden olan olarak atanan kullanıcılar bunu okuyan burada bir ilk eşitleme gerçekleştirmek hizmet sağlama Azure AD **kullanıcılar ve gruplar** bölümü, hedef uygulama için bunları sorgular ve Azure AD içinde tanımlıdır sağlama eylemleri gerçekleştirir **eşlemeleri** bölümü. Yönetilmeyen hesapları hiçbir zaman atama kapsamında olan hedef uygulamaların içindeki operations sağlamayı kaldırma özelliklerini tarafından etkilenmez için bu işlem sırasında önbelleğe alınan veriler, yönetmekte olduğu, hangi kullanıcı hesapları hakkında sağlama hizmeti depolar. İlk eşitleme sonrasında sağlama hizmeti otomatik olarak kullanıcı ve Grup nesneleri on dakikalık bir zaman aralığı üzerinde eşitler.

Değiştirme **sağlama durumu** için **kapalı** yalnızca sağlama hizmeti duraklatır. Bu durumda Azure değil oluşturmak, güncelleştirmek veya herhangi bir kullanıcı veya grup nesneleri uygulamada kaldırın. Durumu geri üzerinde değiştirme kaldığı yerden alması hizmetinin neden olur.

Seçme **temizleme geçerli durumu ve eşitlemeyi yeniden başlatma** onay ve kaydetme hangi hesapların Azure AD hakkındaki önbelleğe alınmış verileri yönetiyor, hizmetleri yeniden başlatır ve ilk eşitlemeyi yeniden gerçekleştirir dökümleri sağlama hizmeti durdurur. Bu seçenek, sağlama dağıtım işlemini yeniden başlatmak yöneticilerinin olanak tanır.

### <a name="synchronization-details"></a>Eşitleme Ayrıntıları
Bu bölümde uygulama ve kaç kullanıcı ve Grup nesneleri yönetilmekte olan karşı sağlama hizmeti verdi ilk ve son kez dahil olmak üzere sağlama hizmeti işlemi hakkında ek ayrıntılar sağlar.

Bağlantılar için sağlanan **Etkinlik Raporu sağlama**, tüm kullanıcıların ve grupların güncelleştirilmiş ve arasında kaldırılan Azure AD oluşturulmuş bir günlük sağlar ve hedef uygulama ve **hata raporu sağlama** daha ayrıntılı hata iletilerini kullanıcı ve okumak için oluşturduğunuz grubu nesneler sağlayan güncelleştirilmiş veya kaldırılamaz. 

##<a name="feedback"></a>Geri Bildirim

Azure AD deneyiminizi gibi umuyoruz. Gelen geri bildirim unutmayın! Geri bildirim ve fikir geliştirme için post **Yönetici portalı** bölümünü bizim [geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Biz, her gün harika yeni hizmetler oluşturma hakkında heyecan ve şekil, kılavuzlar kullanabilir ve sonraki geliştirmemiz ne tanımlayın.


[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
