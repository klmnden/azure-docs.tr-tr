---
title: "Azure AD ile eşitliyorsanız değil bir nesne sorunlarını giderme | Microsoft Docs"
description: "Bir nesne için Azure AD eşitleme neden sorunlarını giderin."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 491a920ceeaac62dd37b1def3f02234056aebfb0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-to-azure-ad"></a>Azure AD ile eşitliyorsanız değil bir nesne sorun giderme

Bir nesne için Azure AD beklendiği gibi eşitlemiyor, nedeniyle birkaç nedeni olabilir. Bir hata e-posta Azure AD'den alınan veya Azure AD Connect Health hataya bakın, sonra okuma [verme hatalarında sorun giderme](active-directory-aadconnect-troubleshoot-sync-errors.md) yerine. Ancak nesne Azure AD içinde olduğu bir sorun gideriyorsanız, ardından bu konu. Şirket içi Bileşen Azure AD Connect eşitleme hatalarını bulmak nasıl açıklar.

Hataları bulmak için aşağıdaki sırayla birkaç farklı yerde bakmak olacak:

1. [İşlem günlükleri](#operations) içeri aktarma ve eşitleme sırasında eşitleme altyapısı tarafından tanımlanan hataları bulmak için.
2. [Bağlayıcı alanı](#connector-space-object-properties) eksik nesneler ve Eşitleme hataları bulmak için.
3. [Meta veri deposu](#metaverse-object-properties) veri ilgili sorunları bulmak için.

Başlat [Eşitleme Hizmeti Yöneticisi'ni](active-directory-aadconnectsync-service-manager-ui.md) adımları başlamadan önce.

## <a name="operations"></a>İşlemler
Sorun gidermeyi nereden başlamanız işlemleri sekme Eşitleme Hizmeti Yöneticisi'nde kullanılabilir. İşlem sekmesi en son işlemleri sonuçlarından gösterir.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/operations.png)  

Üst yarısındaki tüm metinler chronic sırayla gösterir. Varsayılan olarak, operations son yedi gün tutar bilgilerini günlüğe, ancak bu ayar değiştirilebilir [Zamanlayıcı](active-directory-aadconnectsync-feature-scheduler.md). Başarı durumunu göstermez herhangi çalıştırmak için aramak istediğiniz. Üstbilgilerini tıklatarak sıralama değiştirebilirsiniz.

**Durum** sütunu en önemli bilgiler ve bir çalışma için en önemli bir sorun gösterir. En sık kullanılan durumlarını araştırmak için öncelik sırasına göre hızlı bir özeti aşağıda verilmiştir (burada * birkaç olası hata dizeleri gösterir).

| Durum | Açıklama |
| --- | --- |
| durdurulmuş-* |Çalıştır tamamlanamadı. Örneğin, uzak sistem kapalı ve bağlantı kurulamıyor. |
| durduruldu-hata-sınırı |5. 000'den fazla hataları vardır. Çalıştır otomatik olarak çok sayıda hata nedeniyle durduruldu. |
| Tamamlanan -\*-hataları |Çalıştırma tamamlandı, ancak araştırılması gereken bir hata (daha az 5.000). |
| Tamamlanan -\*-uyarıları |Çalıştırma tamamlandı, ancak bazı verileri beklenen durumda değil. Hatalar varsa, bu ileti genellikle yalnızca bir belirti içindir. Hataları ele kadar uyarıları araştırmanız gereken değil. |
| başarılı |Sorunu yok. |

Bir satır seçtiğinizde, alt çalıştıran ayrıntılarını göstermek için güncelleştirir. Bir liste bildiren olabilir solundaki alt için **adım #**. Burada her etki alanı adımı tarafından temsil edilen ormanınızdaki birden çok etki alanı varsa, bu listede yalnızca görünür. Etki alanı adı başlığı altında bulunabilir **bölüm**. Altında **eşitleme istatistikleri**, işlenen değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilmiş nesneleri listesini almak için bağlantıları tıklatabilirsiniz. Hatalarla nesneniz varsa, bu hataları altında görünmesini **eşitleme hatalarını**.

### <a name="troubleshoot-errors-in-operations-tab"></a>İşlemler sekmesinde hatalarında sorun giderme
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorsync.png)  
Hatalar varsa, her iki nesne hata ve hata daha fazla bilgi sağlayan bağlantıları verilmiştir.

Başlangıç hata dizesi tıklayarak (**eşitleme kuralı-hata-işlevi-tetiklenen** resim). İlk Nesne bir bakış sunulmuştur. Gerçek hatayı görmek için düğmesini **yığın izleme**. Bu izleme hatası için hata ayıklama düzeyi bilgileri sağlar.

İçinde sağ **çağrı yığını bilgileri** kutusunda, seçin **Tümünü Seç**, ve **kopya**. Sonra yığın kopyalayın ve Not Defteri gibi sık kullanılan, Düzenleyicisi'nde hata arayın.

* Hata ise **SyncRulesEngine**, çağrı yığını bilgileri, tüm özniteliklerin listesini ilk nesnesinde yok. daha sonra. Başlık görene kadar aşağı kaydırın **InnerException = >**.  
  ![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorinnerexception.png)  
  Satırından sonra hata gösterir. Yukarıdaki resimde, bir özel eşitleme kuralı oluşturulan Fabrikam bir hatadır.

Ardından hata yeterli bilgi sağlamıyorsa veri ara zamanı geldi. Nesne tanımlayıcısı bağlantıyı tıklatın ve sorun giderme devam [bağlayıcı alanı içeri aktarılan nesnenin](#cs-import).

## <a name="connector-space-object-properties"></a>Bağlayıcı alanı nesne özellikleri
Herhangi bir hata yoksa, bulunan [operations](#operations) sekmesinde, meta veri ve Azure AD için bağlayıcı alanı nesne Active Directory'den izlemek için sonraki adım ise. Bu yolu, sorunun nerede olduğunu bulmanız gerekir.

### <a name="search-for-an-object-in-the-cs"></a>Cs nesne araması

İçinde **Eşitleme Hizmeti Yöneticisi'ni**, tıklatın **Bağlayıcılar**, Active Directory bağlayıcısını seçin ve **arama bağlayıcı alanı**.

İçinde **kapsam**seçin **RDN** (istediğinizde CN özniteliğini aramak) veya **DN ya da bağlantı** (istediğinizde distinguishedName özniteliğini aramak). Bir değer girin ve tıklayın **arama**.  
![Bağlayıcı alanı arama](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearch.png)  

Nesne bulamazsanız, aradığınız sonra onu ile filtrelendi [etki alanı tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) veya [OU tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Okuma [filtrelemeyi yapılandırma](active-directory-aadconnectsync-configure-filtering.md) konu filtreleme beklendiği şekilde yapılandırıldığından emin olun.

Azure AD Bağlayıcısı seçmek için başka bir yararlı arama olan **kapsam** seçin **bekleyen alma**seçip **Ekle** onay kutusu. Bu arama bir şirket içi nesne ile ilişkili olamaz Azure AD'de tüm eşitlenmiş nesnelerin sağlar.  
![Bağlayıcı alanı arama artık](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearchorphan.png)  
Bu nesneleri başka bir eşitleme altyapısı veya farklı bir filtreleme yapılandırması ile eşitleme altyapısı tarafından oluşturuldu. Bu görünüm listesidir **artık** artık yönetilen nesneleri. Bu listeyi gözden geçirin ve kullanarak bu nesneleri kaldırmayı düşünün [Azure AD PowerShell](http://aka.ms/aadposh) cmdlet'leri.

### <a name="cs-import"></a>CS alma
Cs nesnesini açtığınızda, en üstünde birden çok sekme vardır. **Alma** sekmesi bir içeri aktarma işleminden sonra hazırlanan veriler gösterir.  
![CS nesnesi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/csobject.png)    
**Eski değer** Bağlan'şu anda depolanan gösterir ve **yeni değer** ne kaynak sistemden alınan ve henüz uygulanmadı. Bir nesne üzerinde bir hata varsa, değişiklikler işlenmez.

**Hata**  
![CS nesnesi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssyncerror.png)  
**Eşitleme hatası** sekmesini görülebilir yalnızca nesne ile ilgili bir sorun varsa. Daha fazla bilgi için bkz: [eşitleme hatalarını giderme](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS çizgileri
Çizgileri sekmesi bağlayıcı alanı nesne meta veri deposu nesnesine nasıl ilişkili olduğunu gösterir. Bağlayıcı, bir değişiklik bağlı sistem ve meta verileri doldurmak için hangi kuralları uygulanan en son alınan zaman görebilirsiniz.  
![CS çizgileri](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineage.png)  
İçinde **eylem** sütunu, bir görebilirsiniz **gelen** eşitleme kuralı eylemiyle **sağlama**. Bu bağlayıcı alanı nesne mevcut olduğu sürece, meta veri deposu nesne kalır gösterir. Eşitleme kuralları listesini bunun yerine bir eşitleme kuralının yönünde gösterir varsa **giden** ve **sağlama**, meta veri deposu nesnesi silindiğinde, bu nesne silindi gösterir.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineageout.png)  
Ayrıca görebilirsiniz **PasswordSync** gelen bağlayıcı alanı katkıda bulunabilirsiniz sütun değeri bir eşitleme kuralı varsa bu yana parolasını değiştirir **doğru**. Bu parolayı sonra Azure AD giden kuralı üzerinden gönderilir.

Çizgileri sekmesinden tıklayarak için meta veri alabilirsiniz [meta veri deposu nesne özellikleri](#mv-attributes).

Tüm sekmeler altındaki iki düğme vardır: **Önizleme** ve **günlük**.

### <a name="preview"></a>Önizleme
Önizleme sayfası, tek tek nesne eşitlemek için kullanılır. Bazı özel eşitleme kuralları sorun giderme ve tek bir nesne üzerinde bir değişikliğin etkisini görmek istiyorsanız kullanışlıdır. Aşağıdaki seçeneklerden birini seçebilirsiniz **tam eşitleme** ve **Delta eşitleme**. Arasında seçim yapabilirsiniz **oluşturmak Önizleme**, hangi yalnızca tutar değişiklik bellekte ve **Commit Önizleme**, meta veri güncelleştirildi ve hedef bağlayıcı alanları yapılan tüm değişiklikler aşamaları.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/preview.png)  
Nesne ve belirli öznitelik akışı için hangi kuralın uygulanacağı inceleyebilirsiniz.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/previewresult.png)

### <a name="log"></a>Günlük
Günlük sayfası parola eşitleme durumunu ve geçmişini görmek için kullanılır. Daha fazla bilgi için bkz: [parola eşitleme sorunlarını giderme](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="metaverse-object-properties"></a>Meta veri deposu nesne özellikleri
Active Directory kaynağından aramaya başlamak daha iyi [bağlayıcı alanı](#connector-space). Ancak, meta veri deposu arama başlatabilirsiniz.

### <a name="search-for-an-object-in-the-mv"></a>MV nesnesi için arama
İçinde **Eşitleme Hizmeti Yöneticisi'ni**, tıklatın **meta veri deposu arama**. Kullanıcı bulur bildiğiniz bir sorgu oluşturun. AccountName (sAMAccountName) ve userPrincipalName gibi ortak öznitelikleri için arama yapabilirsiniz. Daha fazla bilgi için bkz: [meta veri deposu arama](active-directory-aadconnectsync-service-manager-ui-mvsearch.md).
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvsearch.png)  

İçinde **arama sonuçları** penceresinde nesnesine tıklayın.

Nesne bulamadı, ardından bunu henüz meta veri ulaştı değil. Active Directory nesnesi için aramaya devam [bağlayıcı alanı](#connector-space-object-properties). Meta veri deposu için gelen nesnesinin engelleme eşitleme bir hata olabilir veya bir filtre uygulanmış olabilir.

### <a name="mv-attributes"></a>MV öznitelikleri
Öznitelikler sekmesinde hangi bağlayıcı katkıda bulunan ve değerleri görebilirsiniz.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvobject.png)  

Bir nesne eşitlemiyor, ardından aşağıdaki özniteliklerin meta veri bakın:
- Öznitelik **cloudFiltered** sunmak ve kümesine **true**? Bunun ardından adımlarda göre filtre uygulanmış [özniteliği tabanlı filtreleme](active-directory-aadconnectsync-configure-filtering.md#attribute-based-filtering).
- Öznitelik **sourceAnchor** var? Değilse, bir hesap-kaynak orman topolojisini var mı? Bir nesne bağlı bir posta kutusu tanımladıysanız (öznitelik **msExchRecipientTypeDetails** değeri 2), sourceAnchor etkinleştirilmiş bir Active Directory hesabı ormanıyla tarafından katkıda bulunan sonra. Ana hesap içeri aktarılan ve doğru şekilde eşitlenmiş emin olun. Yönetici hesabı listelenmiş olmalıdır [Bağlayıcılar](#mv-connectors) nesne.

### <a name="mv-connectors"></a>MV bağlayıcılar
Bağlayıcılar sekme nesne gösterimini sahip tüm bağlayıcı alanları gösterir.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvconnectors.png)  
Bir bağlayıcı olmalıdır:

- Her bir Active Directory orman kullanıcı olarak temsil edilir. Bu gösterim foreignSecurityPrincipals ve kişi nesneleri içerebilir.
- Azure AD'de bağlayıcı.

Azure ad Bağlayıcısı eksikse, ardından okuma [MV öznitelikleri](#MV-attributes) Azure AD'ye sağlanacak ölçütlerini doğrulanamadı.

Bu sekme ayrıca gitmek sağlar [bağlayıcı alanı nesne](#connector-space-object-properties). Bir satır seçin ve tıklatın **özellikleri**.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
