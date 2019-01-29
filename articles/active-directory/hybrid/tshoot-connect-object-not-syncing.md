---
title: Azure AD ile eşitlenmeyen bir nesneyle ilgili sorunları giderme | Microsoft Docs
description: Bir nesne, Azure AD ile eşitlenmeyen neden sorunlarını giderin.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: d10b8760409d5deb0828d15e8c0daf50853a9624
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158273"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-to-azure-ad"></a>Azure AD ile eşitlenmeyen bir nesneyle ilgili sorunları giderme

Bir nesne, Azure AD ile beklendiği şekilde eşitlenmeyen, çeşitli nedenlerden dolayı olabilir. Azure AD'den bir hata e-posta aldığınız veya Azure AD Connect Health hataya bakın, sonra okuyun [dışarı aktarma sorunlarını giderme](tshoot-connect-sync-errors.md) yerine. Ancak burada nesne Azure AD'de değil sorun giderirken, bu konuda, olur. Bu, şirket içi bileşeni olan Azure AD Connect eşitleme hatalarını bulmak nasıl açıklar.

>[!IMPORTANT]
>İçin Azure Active Directory (AAD) dağıtım 1.1.749.0 sürümü ile bağlantı kurulamadı veya üzeri kullanın [görev sorun giderme](tshoot-connect-objectsync.md) nesne eşitleme sorunlarını gidermek için Sihirbazı'nda. 

Hataları bulmak için aşağıdaki sırayla birkaç farklı yerlerde bakmak olacak:

1. [İşlem günlükleri](#operations) içeri aktarma ve eşitleme sırasında eşitleme altyapısı tarafından tanıtılan hataları bulmak için.
2. [Bağlayıcı alanına](#connector-space-object-properties) eksik nesneleri ve Eşitleme hataları bulmak için.
3. [Meta veri deposu](#metaverse-object-properties) verilerle ilgili sorunları bulmak için.

Başlangıç [Eşitleme Hizmeti Yöneticisi](how-to-connect-sync-service-manager-ui.md) adımları başlamadan önce.

## <a name="operations"></a>İşlemler
Eşitleme Hizmeti Yöneticisi'nde işlemleri sekmesini, sorun gidermeyi nereden başlaması gerektiğini ' dir. En son işlem sonuçlardan işlemleri sekmesini gösterir.  
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/operations.png)  

Üst kısmında kronolojik sırayla tüm çalıştırmalar gösterir. Varsayılan olarak, son yedi gün saklar bilgilerini işlemleri günlüğe, ancak bu ayar değiştirilebilir [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md). Başarı durumunu göstermez her çalıştırma için görünmesini istediğiniz. Başlıklarını tıklatarak sıralama değiştirebilirsiniz.

**Durumu** sütun en önemli bilgiler, bir çalıştırma için en önemli bir sorun gösterir. Araştırmak için öncelik sırasına göre en yaygın durumlar hızlı bir özeti aşağıda verilmiştir (burada * birden çok olası hata dizeleri belirtmek).

| Durum | Açıklama |
| --- | --- |
| durduruldu-* |Çalıştırma işlemi tamamlanamadı. Örneğin, uzak sistem kapalı ve bağlantı kurulamıyor. |
| durduruldu-hata-sınırı |5. 000'den fazla hataları vardır. Çalıştırma, çok sayıda hata nedeniyle otomatik olarak durduruldu. |
| Tamamlanan -\*-hata |Çalıştırma tamamlandı, ancak araştırılması gereken hataları (az 5.000) vardır. |
| Tamamlanan -\*-uyarılar |Çalıştırması tamamlandı, ancak bazı veriler beklenen durumda değil. Hatalar varsa, daha sonra bu ileti genellikle yalnızca bir belirtisidir. Hataları da giderdik kadar uyarıları araştırmanız gereken değil. |
| başarılı |Sorun yok. |

Bir satırı seçin, çalıştırılan ayrıntılarını göstermek için alt güncelleştirir. En solda alt, liste söze sahip olabileceğiniz **adım #**. Burada her etki alanı adımı tarafından temsil edilen ormanınızdaki birden çok etki alanı varsa, bu liste yalnızca görünür. Etki alanı adı başlığı altında bulunabilir **bölüm**. Altında **eşitleme istatistikleri**, işlenen değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilmiş nesneleri listesini almak için bağlantıları tıklatabilirsiniz. Hatalarla nesneniz varsa, bu hataları görünür **eşitleme hatalarını**.

### <a name="troubleshoot-errors-in-operations-tab"></a>İşlemler sekmesinde hatalarını giderme
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Hatalar var nesne hata ve hata konusunda daha fazla bilgi sağlayan bağlantılar.

Hata dizesi tıklayarak başlayın (**eşitleme kuralı-hata-işlevi-tetiklenen** resim). İlk nesnenin bir bakış sunulur. Gerçek hata görmek için düğmeye tıklayın **yığın izlemesi**. Bu izleme, hata ayıklama düzeyi bilgileri sağlar.

Sağ tıklayabilirsiniz **çağrı yığını bilgilerini** kutusunda **Tümünü Seç**, ve **kopyalama**. Ardından, yığın kopyalayıp, Not Defteri gibi sık kullandığınız bir düzenleyicide hatasına bakabilir.

* Gelen hata ise **SyncRulesEngine**, çağrı yığını bilgilerini nesnesinde tüm öznitelikler listesi önce alınmalıdır. Başlık görene kadar kaydırın **InnerException = >**.  
  ![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)  
  Satırın sonunda hata gösterir. Yukarıdaki resimde, bir özel eşitleme kuralı oluşturulan Fabrikam bir hatadır.

Ardından hata yeterli bilgi sağlamazsa veri bırakma zamanı geldi. Nesne tanımlayıcısı ile bağlantıya tıklayın ve sorun giderme devam [bağlayıcı alanına içeri aktarılan nesneye](#cs-import).

## <a name="connector-space-object-properties"></a>Bağlayıcı alanı nesne özellikleri
Yoksa, herhangi bir hata bulunması [işlemleri](#operations) sekmesinde, meta veri ve Azure AD için bağlayıcı alanı nesnesi Active Directory'den izlemek için sonraki adım ise. Bu yolu, sorunun nerede olduğunu bulmanız gerekir.

### <a name="search-for-an-object-in-the-cs"></a>Cs nesne araması

İçinde **Eşitleme Hizmeti Yöneticisi**, tıklayın **Bağlayıcılar**, Active Directory bağlayıcısını seçin ve **arama bağlayıcı alanı**.

İçinde **kapsam**seçin **RDN** (istediğinizde CN özniteliğini aramak) veya **DN'si ya da bağlantı** (istediğinizde distinguishedName özniteliğini aramak). Bir değer girin ve tıklayın **arama**.  
![Bağlayıcı alanı arama](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Nesne bulamazsanız, aradığınız ve ardından onu ile filtre uygulanmış [etki alanı tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#domain-based-filtering) veya [OU tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering). Okuma [süzme](how-to-connect-sync-configure-filtering.md) filtreleme beklendiği gibi yapılandırıldığını doğrulamak için konu.

Azure AD Bağlayıcısı seçmek için kullanışlı bir arama olduğundan **kapsam** seçin **bekleyen alma**seçip **Ekle** onay kutusu. Bu arama bir şirket içi nesne ile ilişkilendirilemiyor, Azure ad'deki tüm eşitlenmiş nesnelerin sağlar.  
![Bağlayıcı alanı arama artık](./media/tshoot-connect-object-not-syncing/cssearchorphan.png)  
Bu nesneleri başka bir eşitleme altyapısı veya farklı bir filtre yapılandırma ile bir eşitleme altyapısı tarafından oluşturulmuştur. Bu görünüm listesidir **artık** artık yönetilen nesneler. Bu listeyi gözden geçirin ve bu nesneleri kullanarak kaldırmayı düşünün [Azure AD PowerShell](https://aka.ms/aadposh) cmdlet'leri.

### <a name="cs-import"></a>CS içeri aktarma
Cs nesnesi açtığınızda en üstünde birden çok sekme bulunur. **Alma** sekmesi, bir içeri aktarma işleminden sonra hazırlanan verileri gösterir.  
![CS nesnesi](./media/tshoot-connect-object-not-syncing/csobject.png)    
**Eski değer** Bağlan'şu anda depolanan gösterir ve **yeni değer** hangi kaynak sistemden alındı ve henüz uygulanmamış. Nesne üzerinde bir hata varsa, değişiklikleri işlenmez.

**Hata:**  
![CS nesnesi](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  
**Eşitleme hatası** sekmedir yalnızca nesne ile ilgili bir sorun varsa görünür. Daha fazla bilgi için [eşitleme hatalarıyla ilgili sorunları giderme](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS kökenini
Kökenini sekmesi, bağlayıcı alanı nesnesi için meta veri deposu nesne nasıl ilişkili olduğunu gösterir. Bağlayıcı en son bağlı sistem ve meta veri deposundaki verileri doldurmak için hangi kuralları uygulanan bir değişikliği içeri aktarıldığında görebilirsiniz.  
![CS kökenini](./media/tshoot-connect-object-not-syncing/cslineage.png)  
İçinde **eylem** sütununda bir görebilirsiniz **gelen** eşitleme kuralı eylemi **sağlama**. Bu bağlayıcı alanı nesnesi mevcut olduğu sürece, meta veri deposu nesne kalır gösterir. Eşitleme kuralları listesi yerine yön ile bir eşitleme kuralı görünmediğini **giden** ve **sağlama**, meta veri deposu nesnesi silindiğinde, bu nesne silinip silinmediğini belirtir.  
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/cslineageout.png)  
Ayrıca görebilirsiniz **PasswordSync** gelen bağlayıcı alanında katkıda bulunabilir sütun değeri bir eşitleme kuralına sahip olduğundan parola değişiklikleri **True**. Bu parola, giden kuralı aracılığıyla Azure AD'ye sonra gönderilir.

Kökenini sekmesinden tıklayarak için meta veri alabilirsiniz [meta veri deposu nesne özellikleri](#mv-attributes).

Tüm sekmeler alt kısmındaki iki düğme bulunur: **Önizleme** ve **günlük**.

### <a name="preview"></a>Önizleme
Önizleme sayfası, tek bir nesneyi eşitlemek için kullanılır. Bazı özel eşitleme kuralları giderirken ve tek bir nesne üzerinde bir değişikliğin etkilerini görmek istediğinizde yararlıdır. Aşağıdaki seçeneklerden birini seçebilirsiniz **tam eşitleme** ve **Delta eşitleme**. Arasında seçim **oluşturma Önizleme**, hangi yalnızca tutar değişikliği bellekte ve **işleme Önizleme**, güncelleştirilmiş meta veri deposu ve hedef bağlayıcı alanları yapılan tüm değişiklikleri hazırlar.  
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/preview.png)  
Nesne ve bir özel öznitelik akışı için hangi kuralın uygulanacağı inceleyebilirsiniz.  
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Günlük
Parola Eşitleme durumunu ve geçmişini görmek için günlüğü sayfasında kullanılır. Daha fazla bilgi için [parola karması eşitleme sorunlarını giderme](tshoot-connect-password-hash-synchronization.md).

## <a name="metaverse-object-properties"></a>Meta veri deposu nesne özellikleri
Active Directory kaynak aramaya başlamak daha iyi [bağlayıcı alanına](#connector-space). Ancak, meta veri deposu arama başlatabilirsiniz.

### <a name="search-for-an-object-in-the-mv"></a>MV nesnesi için arama
İçinde **Eşitleme Hizmeti Yöneticisi**, tıklayın **meta veri deposu arama**. Kullanıcı bulur bildiğiniz bir sorgu oluşturun. AccountName (sAMAccountName) ve userPrincipalName gibi ortak öznitelikleri için arama yapabilirsiniz. Daha fazla bilgi için [meta veri deposu arama](how-to-connect-sync-service-manager-ui-mvsearch.md).
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

İçinde **arama sonuçları** penceresinde nesneye tıklayın.

Nesne bulunamadı, ardından da henüz meta veri deposu ulaşılmamış. Active Directory nesnesi için aramaya devam edin [bağlayıcı alanına](#connector-space-object-properties). Meta veri deposu için yakında nesnesinin engelleme eşitleme bir hata olabilir veya filtre uygulanmış olabilir.

### <a name="mv-attributes"></a>MV öznitelikleri
Öznitelikler sekmesinde hangi bağlayıcı katkıda bulunan ve değerlerini görebilirsiniz.  
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Bir nesne eşitlenmeyen, ardından aşağıdaki özniteliklerin meta veri bakın:
- Öznitelik **cloudFiltered** sunmak ve kümesine **true**? Bunun ardından adımlarda göre uygulanmış [öznitelik tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#attribute-based-filtering).
- Öznitelik **sourceAnchor** var? Aksi durumda, bir hesap-kaynak orman topolojisini var? Bir nesne bağlı bir posta kutusu tanımlanması durumunda (öznitelik **msExchRecipientTypeDetails** 2 değerine sahiptir), sourceAnchor etkinleştirilmiş bir Active Directory hesabı ile orman tarafından katkıda bulunulan sonra. Ana hesap alınan ve doğru bir şekilde eşitlenmiş emin olun. Ana hesap yer alması gerekir [Bağlayıcılar](#mv-connectors) nesne.

### <a name="mv-connectors"></a>MV bağlayıcılar
Bağlayıcılar sekmesi, nesnenin bir gösterimi olan tüm bağlayıcı alanları gösterir.  
![Eşitleme Hizmeti Yöneticisi](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  
Bir bağlayıcıyı sahip olmalıdır:

- Her kullanıcının Active Directory ormanı temsil edilir. Bu gösterim foreignSecurityPrincipals ve kişi nesneleri içerebilir.
- Azure AD'de bir bağlayıcı.

Azure ad Bağlayıcısı eksikse, ardından okuma [MV öznitelikleri](#mv-attributes) Azure AD'ye sağlanırken ölçütlerini doğrulayın.

Bu sekme Ayrıca, gidilecek sağlar [bağlayıcı alanı nesnesi](#connector-space-object-properties). Bir satırı seçin ve tıklayın **özellikleri**.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
