---
title: "SaaS uygulaması Azure AD'de kullanıcı sağlamayı otomatik | Microsoft Docs"
description: "Otomatik olarak sağlamak için Azure AD nasıl kullanabileceğiniz bir giriş sağlanmasını ve sürekli olarak kullanıcı hesaplarını birden çok üçüncü taraf SaaS uygulamalarında güncelleştirin."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: 3fe57e9c22d04a3557978093ce3fe86613c5c1d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>SaaS uygulamaları için kullanıcı sağlamayı otomatik nedir?
Azure Active Directory (Azure AD) oluşturulması, Bakım ve kullanıcı kimlikleri bulutta kaldırılmasını otomatikleştirmenizi sağlar ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) Dropbox, Salesforce, ServiceNow ve daha fazlası gibi uygulamalar.

**Aşağıda bu özellik, yapmak nelere izin ilişkin bazı örnekler şunlardır:**

* Otomatik olarak ekip veya kuruluş katıldığınızda yeni kişiler için doğru sistemlerinde yeni hesapları oluşturun.
* Otomatik olarak kişiler ekip veya kuruluş ayrıldığında sağ sistemlerinde hesapları devre dışı bırakın.
* Uygulamalar ve sistemler kimlikleri dizini ya da İnsan Kaynakları sisteminizi değişikliklere göre güncel tutulduğundan emin olun.
* Bunları destekleyen uygulamalar için gruplar gibi kullanıcı olmayan nesneler sağlayın.

**Otomatik kullanıcı hazırlama aşağıdaki işlevleri içerir:**

* Kaynak ve hedef sistemleri arasında varolan kimlikleri eşleşen yeteneği.
* Yardım'a Azure AD özelleştirme seçenekleri kuruluşunuzun şu anda kullanmakta olduğu sistemlerini ve uygulamaları, geçerli yapılandırmaları uygun.
* Hazırlama hataları için isteğe bağlı bir e-posta uyarıları.
* İzleme ve sorun giderme Yardımı için raporlama ve etkinlik günlükleri.

## <a name="why-use-automated-provisioning"></a>Otomatik sağlama neden kullanılır?
Bu özelliği kullanmak için bazı ortak sözleri şunları içerir:

* Maliyetleri, verimsiz ve işlemler sağlama el ile ilişkili İnsan hatası önlemek için.
* Barındırma ve özel geliştirilmiş sağlama çözümleri ve komut dosyaları korumanın ilişkili maliyetleri önlemek için
* Kuruluşunuz kuruluştan ayrılan anında kullanıcıların kimliklerini anahtar SaaS uygulamalardan kaldırarak güvenli hale getirmek için.
* Kullanıcıları toplu sayısı belirli bir SaaS uygulama veya sistem kolayca almak için.
* Çözüm, sağlama erişmenin keyfini için Azure AD çoklu oturum açma için tanımlanan uygulama erişim ilkeleri dışına çalıştırın.


## <a name="how-does-automatic-provisioning-work"></a>Otomatik sağlama nasıl çalışır?
    
**Azure AD hizmeti sağlama** her uygulama satıcısı tarafından sağlanan kullanıcı yönetimi API uç noktaları bağlanarak kullanıcılara SaaS uygulamaları ve diğer sistemler sağlar. Bu kullanıcı yönetim API uç noktaları program aracılığıyla oluşturmak, güncelleştirmek ve kullanıcıları kaldırmak Azure AD verin. Sağlama hizmeti de oluşturabilir, seçilen uygulamalar için güncelleştirme ve grupları ve rolleri gibi ek kimlikle ilgili nesneleri kaldırın. 

![Sağlama](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*Şekil 1: hizmet sağlama Azure AD*

![Giden sağlama](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*Şekil 2: "Giden" kullanıcı Azure AD'den iş akışı popüler SaaS uygulamaları için hazırlama*

![Gelen hazırlama](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*Şekil 3: "Giriş" kullanıcı Azure Active Directory ve Windows Server Active Directory popüler İnsan sermaye Yönetimi (HCM) uygulamalardan iş akışı sağlama*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Hangi uygulamalar ve sistemler Azure AD otomatik kullanıcı sağlamayı kullanabilir miyim?

Azure AD özelliklerini önceden tümleştirilmiş belirli kısımlarını uygulamak uygulamaları için genel destek yanı sıra çeşitli popüler SaaS uygulamaları ve İnsan Kaynakları sistemleri için destek [SCIM'yi 2.0 standart](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-scim-provisioning).

Tüm Azure AD uygulama galerisinde "Öne çıkan" uygulamalarının otomatik kullanıcı sağlamayı destekler. [Öne çıkan uygulamalar listesini burada görüntülenebilir.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Sırayla otomatik kullanıcı hazırlama desteklemek için bir uygulama için önce gerekli kullanıcı dış programların oluşturulması, Bakım ve kullanıcıların kaldırılmasını otomatik hale getirmek için izin yönetim uç noktaları sağlamanız gerekir. Bu nedenle, tüm SaaS uygulamaları bu özellik ile uyumlu değildir. Kullanıcı Yönetimi API'leri destekleyen uygulamaları için Azure AD mühendislik ekibi ardından uygulamalarla sağlama bir bağlayıcı oluşturmak mümkün olacaktır ve bu iş geçerli ve olası müşteriler gereksinimlerinize göre öncelik. 

Azure AD kişiye üzerinden bir ileti gönderme mühendislik ekibi ek uygulamalar için sağlama destek istemek için [Azure Active Directory geri bildirim Forumunda](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning). 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Bir uygulamaya otomatik sağlamayı nasıl ayarlarım?

Seçilen bir uygulamaya başlar için hizmet sağlama Azure ad yapılandırma  **[Azure portal](https://potal.azure.com)**. İçinde **Azure Active Directory > Kurumsal uygulamalar** bölümünde, select **Ekle**, ardından **tüm**, senaryonuza bağlı olarak aşağıdakilerden birini ekleyin:

* Tüm uygulamalar **özellikli uygulamalara** destek bölümüne otomatik sağlama

* Özel geliştirilmiş SCIM'yi tümleştirmeler için "galeri olmayan uygulama" seçeneğini kullanın

![Galeri](./media/active-directory-saas-app-provisioning/gallery.png)

Uygulama Yönetimi ekran sağlama yapılandırılan **sağlama** sekmesi.

![Ayarlar](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Yönetici kimlik bilgileri** kullanıcı yönetim API'si uygulama tarafından sağlanan bağlanmak için sağlayacak hizmet sağlama Azure ad sağlanmalıdır.

* **Öznitelik eşlemelerini** yapılandırılabilir belirtmek, kaynak sistemde alanlar (örnek: Azure AD) hedef sistemde hangi alanlar için içeriklerini eşitlenmemiş (örnek: ServiceNow). Hedef uygulama destekliyorsa, bu bölümde, isteğe bağlı olarak kullanıcı hesaplarının yanı sıra gruplarının sağlama yapılandırmanıza izin verir. "Eşleşen Özellikler" hangi alanların hesapları sistemleri arasında eşleştirmek için kullanılan seçmenize olanak tanır. "[İfadeleri](active-directory-saas-writing-expressions-for-attribute-mappings.md)" değiştirebilir ve hedef sisteme yazılmadan önce kaynak sistemden alınan değerleri dönüştüren olanak sağlar. Daha fazla bilgi için bkz: [öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md).

![Ayarlar](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Kapsam belirleme filtreleri** hangi kullanıcılara sağlama hizmeti söyleyin ve kaynak sistemi grubunda sağlanan ve/veya hedef sistem sağlaması kaldırılıyor. sağlaması. Kapsam belirleme birlikte değerlendirildiğini filtreleri için iki nokta belirleyen sağlama kapsamında kim:

* **Öznitelik değerleri üzerinde filtre** -öznitelik eşlemelerini "Kaynak nesnenin Scope" Menüde özel öznitelik değerlerine göre filtrelemeye olanak tanır. Örneğin, yalnızca bir "Departman" özniteliği "Satış" kullanıcılarla sağlama kapsamında olması gerektiğini belirtebilirsiniz.

* **Atamaları filtresini** -Hazırlama "Scope" menüde > portal ayarları bölümünde olup yalnızca "atanan" Kullanıcılar ve gruplar sağlama kapsamında olması gerekir veya tüm kullanıcılar Azure AD dizininde olup olmayacağını belirtmenize olanak verir sağlandı. "Kullanıcılar ve gruplar atama" hakkında daha fazla bilgi için bkz: [bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamak](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Ayarları** veya şu anda çalışıyor olup olmadığını da dahil olmak üzere, bir uygulama için sağlama hizmet işlemini denetleyen.

* **Denetim günlükleri** kayıtları hizmet sağlama Azure AD tarafından gerçekleştirilen her işlemin sağlar. Daha fazla ayrıntı için bkz: [sağlama raporlama Kılavuzu](active-directory-saas-provisioning-reporting.md).

![Ayarlar](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

## <a name="what-happens-during-provisioning"></a>Sağlama işlemi sırasında ne olur?

1. İlk kez bir uygulama için sağlama etkinleştirdiğinizde, aşağıdaki eylemleri gerçekleştirilir:
   * Azure AD dizininde karşılık gelen kimliklerini SaaS uygulama var olan tüm kullanıcıları eşlemek deneyecek. Bir kullanıcı eşleştiğinde oldukları *değil* çoklu oturum açma için otomatik olarak etkinleştirilmiş. Uygulama erişimi bir kullanıcı için sırayla açıkça uygulamaya Azure AD'de doğrudan veya grup üyeliği üzerinden atanmaları gerekir.
   * Hangi kullanıcıların uygulamayı atanmalıdır zaten belirttiyseniz ve bu kullanıcılar için var olan hesapları bulmak Azure AD başarısız olursa, Azure AD yeni hesapları için uygulamada hazırlayacağınız.
2. Yukarıda açıklandığı gibi ilk eşitleme tamamlandıktan sonra Azure AD 20 dakikada aşağıdaki değişiklikler için kontrol eder:
   * Yeni kullanıcılar uygulamaya atanan SaaS uygulama yeni bir hesap ile (doğrudan veya grup üyeliği aracılığıyla), ardından sağlandı.
   * Bir kullanıcının erişimi kaldırıldıktan sonra SaaS uygulama hesabında devre dışı olarak işaretlenmiş (kullanıcıların hiçbir zaman tamamen silinir, hangi, bir yapılandırma hatası durumunda veri kaybına karşı korur).
   * Bir kullanıcıya son uygulamaya atandığı ve bunlar bir hesap SaaS uygulamada zaten varsa, o hesabı etkin olarak işaretlenir ve dizine karşılaştırıldığında güncel olup olmadığını belirli kullanıcı özelliklerini güncelleştirilebilir.
   * Kullanıcının bilgileri (örneğin, telefon numarası, ofis konumu) dizinde değiştirildiğinde, bu bilgileri ayrıca SaaS uygulamada güncelleştirilir.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**Ne sıklıkta Azure AD directory değişiklikleri için SaaS uygulama yazmak?**

İlk tam eşitleme tamamlandıktan sonra hizmet genellikle sağlama Azure AD değişikliklerini her 20 dakikada denetler. 

SaaS uygulama birkaç hataları (geçersiz yönetici kimlik bilgileri durumunda olduğu gibi) döndürüyorsa, Azure AD kademeli olarak kendi sıklığını hataları çözülene kadar günde bir kez kadar yavaşlatır. Bu durumda, Azure AD sağlama işi "karantina" durumuna girdiğinde ve bu gösterir [özet raporu sağlama](active-directory-saas-provisioning-reporting.md).

**Ne kadar bu Kullanıcılarım sağlayacak sürer?**

İlk tam eşitleme tamamlandıktan sonra artımlı değişiklikler genellikle 20-40 dakika içinde gerçekleşir. Ardından dizininizi çoğunu sağlayacak çalışıyorsanız, kullanıcılar ve gruplar elinizde sayısına bağlıdır. Performans, kullanıcı yönetimi sağlama hizmetleri kullanan kaynak sistemden veri okumak ve hedef sisteme veri yazmak için API performansını bağımlıdır. 

Performans birçok hata varsa daha yavaş de (kaydedilen [denetim günlüklerini](active-directory-saas-provisioning-reporting.md)) ve sağlama hizmeti "karantina" durumuna geçti.

**İlk tam eşitleme nedir ve neden sonraki eşitlemeler uzun sürer?**

Belirli bir uygulamanın ilk kez hizmet sağlama Azure AD çalıştırın, kullanıcılar "anlık görüntüsünü" alır (ve isteğe bağlı olarak gruplar) kaynak dizin. Bu anlık görüntü kaynak ve hedef API'leri gidiş dönüş sayısını azaltmak sağlama hizmetini etkinleştirir ve daha verimli bir şekilde davranmasına sonraki "delta" eşitlemeler sağlar. 

Kullanıcılar ilk tam eşitlemesi genellikle çok küçük dizinler için dakika içinde tamamlanabilir, ancak daha büyük dizinler için birkaç saat sürebilir. Yüz binlerce kullanıcıların Kurumsal dizinlerle tamamlamak ilk eşitleme için kaç saat sürebilir. Ancak, ilk eşitleme sonrasında sonraki "delta" eşitlemeler çok daha hızlı bir şekilde gerçekleşir.

> [!NOTE]
> Gruplar ve grup üyeliklerini sağlamayı desteklemeyen uygulamalar için bu büyük ölçüde etkinleştirme tamamlanması için tam eşitleme süresini artırır. Grup adları ve grup üyeliklerini uygulamanıza sağlayacak istemiyorsanız, bu devre dışı bırakabilirsiniz [öznitelik eşlemelerini](active-directory-saas-customizing-attribute-mappings.md) sağlama yapılandırmanızın.

**Geçerli sağlama işinin ilerleme durumunu nasıl izleyebilir miyim?**

Bkz: [sağlama raporlama Kılavuzu](active-directory-saas-provisioning-reporting.md).

**Kullanıcıların düzgün sağlanan alma başarısız olursa nasıl anlarım?**

Tüm hataları Azure AD'de kayıtlı denetim günlükleri. Daha fazla bilgi için bkz: [sağlama raporlama Kılavuzu](active-directory-saas-provisioning-reporting.md).

**Mühendislik ekibine geribildirim nasıl gönderebilir miyim?**

Aracılığıyla Bize Ulaşın [Azure Active Directory geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/).


## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Kullanıcı sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md)
* [Özellik eşlemeleri için ifade yazma](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kapsam belirleme filtreleri kullanıcı sağlama](active-directory-saas-scoping-filters.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md)
* [Hesap sağlama bildirimleri](active-directory-saas-account-provisioning-notifications.md)
* [SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](active-directory-saas-tutorial-list.md)

