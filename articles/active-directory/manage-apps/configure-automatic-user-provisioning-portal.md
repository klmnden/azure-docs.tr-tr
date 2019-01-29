---
title: Kullanıcı Yönetimi Azure Active Directory'de kurumsal uygulamalar için hazırlama | Microsoft Docs
description: Kullanıcı hesabı Azure Active Directory'yi kullanarak kurumsal uygulamalar için sağlama yönetmeyi öğrenin
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: ca0be0d4e2b9f76a521d8fc04195311b3603e411
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180560"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Azure portalının kurumsal uygulamalar için sağlama kullanıcı hesabı yönetme
Bu makalede nasıl kullanılacağını [Azure portalında](https://portal.azure.com) hesabı otomatik kullanıcı hazırlama ve onu destekleyen uygulamalar için sağlamayı yönetme. Otomatik kullanıcı hesabı sağlama ve nasıl çalıştığı hakkında daha fazla bilgi için bkz: [sağlama kaldırmayı Azure Active Directory ile SaaS uygulamalarına kullanıcı sağlamayı otomatikleştirin ve](user-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Portalda uygulamalarınızı bulma
Çoklu oturum açmayı bir dizinde görüntülenebilen ve yönetilebilen için yapılandırılan tüm uygulamalar [Azure portalında](https://portal.azure.com). Uygulamaları bulunabilir **tüm hizmetleri** &gt; **kurumsal uygulamalar** Portalı'nın bölümü. Kurumsal uygulamalar, dağıtıldığı ve kuruluşunuzda kullanılan uygulamalardır.

![Kurumsal uygulamalar bölmesi](./media/configure-automatic-user-provisioning-portal/enterprise-apps-pane.png)

Seçme **tüm uygulamaları** soldaki bağlantı Galeriden eklenen uygulamaları dahil olmak üzere yapılandırılmış tüm uygulamaların bir listesini gösterir. Bir uygulama seçilmesi, burada raporları bu uygulama için görüntülenebilir ve çeşitli ayarları yönetilebilir bu uygulama için kaynak bölmesini yükler.

Kullanıcı hesabı sağlama ayarları seçerek yönetilebilir **sağlama** soldaki.

![Uygulama kaynağı bölmesi](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>Sağlama modları
**Sağlama** Bölmesi ile başlayan bir **modu** bu hangi sağlama modları için kurumsal uygulama desteklendiğini gösterir ve bunları yapılandırılması menü. Kullanılabilir seçenekler şunlardır:

* **Otomatik** -Azure AD API tabanlı otomatik sağlama ve/veya bu uygulamaya yönelik kullanıcı hesaplarının sağlamayı destekler. Bu seçenek görüntülenir. Bu modu seçmek, uygulamanın kullanıcı yönetimi API'sine bağlanmak için Azure AD'yi yapılandırma, Hesap Eşleştirmeleri ve nasıl kullanıcı hesabı verileri Azure AD akışını tanımlamanızı iş akışları oluşturma yöneticileri bilgilendiren bir arabirim görüntüler ve Uygulama ve Azure AD sağlama hizmeti yönetme.
* **El ile** -Azure AD kullanıcı hesaplarının bu uygulama için otomatik sağlamayı desteklemez, bu seçeneği gösterilir. Bu seçenek, uygulama içinde depolanan kullanıcı hesabı kayıtlarını (içerebilen SAML tam zamanında sağlama) bu uygulama tarafından sağlanan kullanıcı yönetimi ve sağlama yeteneklerine dayalı bir dış işlemi kullanılarak yönetilmelidir anlamına gelir.

## <a name="configuring-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı yapılandırma
Seçme **otomatik** seçeneği dört bölümde bölünmüş bir ekran görüntüler:

### <a name="admin-credentials"></a>Yönetici Kimlik Bilgileri
Bu bölüm, burada uygulamanın kullanıcı yönetimi için API girilen bağlanmak Azure AD için kimlik bilgileri gerekli değildir. Gerekli giriş uygulamaya bağlı olarak değişir. Kimlik bilgisi türlerinin ve belirli uygulamalar için gereksinimleri hakkında bilgi edinmek için bkz. [belirli bir uygulama için yapılandırma öğreticisini](user-provisioning.md).

Seçme **Bağlantıyı Sına** düğmesi sağlar, Azure'ı sağlayarak kimlik bilgilerini test etmek uygulamaya bağlanmak için AD girişim sağlanan kimlik bilgilerini kullanarak sağlama.

### <a name="mappings"></a>Eşlemeler
Bu bölüm Yöneticiler burada görüntüleyebilir ve Azure AD arasında hangi kullanıcı öznitelikleri akışı Düzenle yöneliktir ve kullanıcı hesaplarını sağlandığında veya güncelleştirildiğinde, hedef uygulama.

Önceden yapılandırılmış bir Azure AD kullanıcı nesnelerinin ve her SaaS uygulamasının kullanıcı nesneleri arasında eşleme yoktur. Bazı uygulamalar diğer grupların veya kişilerin gibi nesnelerin türlerini yönetin. Bu eşlemelerin birini tablo programlarını nerede bunlar görüntülenebilir ve özelleştirilebilir sağa eşleme düzenleyicisini seçme.

![Uygulama kaynağı bölmesi](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

Desteklenen özelleştirmeleri içerir:

* Etkinleştirme ve SaaS uygulamasının kullanıcı nesnesi Azure AD kullanıcı nesnesini gibi belirli nesneler için eşlemelerini devre dışı bırakma.
* Uygulamanın kullanıcı nesnesini Azure AD'ye kullanıcı nesneden akış öznitelikleri düzenleme. Öznitelik eşlemesi hakkında daha fazla bilgi için bkz. [öznitelik eşlemesi türlerini anlama](customize-application-attributes.md#understanding-attribute-mapping-types).
* Azure AD hedeflenen uygulamaya gerçekleştirir sağlama işlemleri filtreleyin. Azure AD'ye tam eşitleme sahip olmak yerine gerçekleştirilen eylemlerin sınırlayabilirsiniz. Örneğin, seçerek yalnızca **güncelleştirme**, var olan kullanıcı hesaplarını bir uygulamada ve yenilerini oluşturmaz yalnızca güncelleştirmeleri Azure AD. Yalnızca seçerek **Oluştur**, Azure yalnızca yeni kullanıcı hesaplarını oluşturur ancak mevcut olanları güncelleştirmez. Bu özellik, yöneticilerin iş akışları için hesap oluşturma farklı eşlemeleri oluşturmak sağlar.

### <a name="settings"></a>Ayarlar
Bu bölümde, yöneticilerin başlatın ve Azure AD sağlama hizmeti seçili uygulamanın yanı sıra isteğe bağlı olarak sağlama önbelleği temizlemek hizmetini durdurup yeniden başlatın olanak sağlar.

Sağlama için bir uygulama ilk kez etkinleştiriliyor, hizmette değiştirerek kapatma **sağlama durumu** için **üzerinde**. Bu değişiklik okuduğu burada atanan kullanıcılar, bir ilk eşitleme gerçekleştirmek Azure AD sağlama hizmeti neden **kullanıcılar ve gruplar** bölümü, hedef uygulama için bunları sorgular ve ardından sağlama eylemleri gerçekleştirir Azure AD'de tanımlanan **eşlemeleri** bölümü. Yönetilmeyen hesapları hiçbir zaman atama için kapsamda olan hedef uygulamalarının içinde işlemler sağlamayı tarafından etkilenmez. Bu nedenle bu işlem sırasında sağlama hizmeti, yönetmekte olduğu, hangi kullanıcı hesaplarını ilgili önbelleğe alınan verileri depolar. İlk eşitlemeden sonra sağlama hizmeti, kullanıcı ve Grup nesneleri otomatik olarak bir on dakikalık aralıklarla eşitler.

Değiştirme **sağlama durumu** için **kapalı** sağlama hizmeti yalnızca duraklatır. Bu durumda, Azure değil oluşturmak, güncelleştirmek veya uygulamada herhangi bir kullanıcı veya grup nesneleri kaldırın. Durumu geri üzerinde değiştirme kaldığı yerden devam edebiliyorduk hizmetinin neden olur.

Seçme **geçerli durumu temizleyin ve eşitlemeyi yeniden başlatın** onay kutusunu ve kaydetme durdurur sağlama hizmeti, Azure AD hesapları ilgili önbelleğe alınan verileri yönetme, hizmetleri yeniden başlatır ve ilk gerçekleştirir dökümleri yeniden eşitleme. Bu seçenek, yöneticilerin sağlama dağıtım işlemini baştan başlatmak olanak sağlar.

### <a name="synchronization-details"></a>Eşitleme Ayrıntıları
Bu bölümde sağlama hizmeti uygulama ve kaç kullanıcı ve Grup nesneleri yönetilmekte olan karşı çalıştırıldı ilk ve son kez dahil olmak üzere sağlama hizmetinin işlemi hakkında ek ayrıntılar sağlar.

Bağlantılar için sağlanan **sağlama etkinliği raporunu** tüm kullanıcıları ve grupları Azure AD güncelleştirilmiş ve kaldırılan arasında oluşturulan günlüğünü sağlayan ve hedef uygulama ve **hata raporusağlama** sağlayan daha ayrıntılı hata iletileri okumak, oluşturulmuş, güncelleştirilmiş veya kaldırılacak kullanıcı ve Grup nesneler için. 



