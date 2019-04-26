---
title: Azure Active Directory ile eşitleniyor değil bir nesneyle ilgili sorunları giderme | Microsoft Docs
description: Azure Active Directory ile eşitleniyor değil bir nesneyle ilgili sorunları giderme.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 931865803328189d89c0fbae15caa801c3f7f7c6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60455239"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-with-azure-active-directory"></a>Azure Active Directory ile eşitlenmeyen bir nesneyle ilgili sorunları giderme

Microsoft Azure Active Directory'ye (Azure AD) beklendiği gibi bir nesne eşitliyor değil, çeşitli nedenlerden dolayı olabilir. Azure AD'den bir hata e-posta aldığınız veya Azure AD Connect Health hataya bakın, okuma [eşitleme sırasında karşılaşılan hataları giderme](tshoot-connect-sync-errors.md) yerine. Ancak burada nesne Azure AD'de değil bir sorun gideriyorsanız, bu makale sizin içindir. Bu, şirket içinde bileşen Azure AD Connect Eşitleme hataları nasıl açıklar.

>[!IMPORTANT]
>Azure AD için dağıtım 1.1.749.0 sürümü ile bağlantı kurulamadı veya üzeri kullanın [görev sorun giderme](tshoot-connect-objectsync.md) nesne eşitleme sorunları gidermek için Sihirbazı'nda. 

## <a name="synchronization-process"></a>Eşitleme işlemi

Biz eşitleme sorunlarını araştırmak önce Azure AD Connect eşitleme işlemi bakalım:

  ![Azure AD Connect eşitleme işlemi diyagramı](./media/tshoot-connect-object-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminoloji**

* **CS:** Bağlayıcı alanı, bir veritabanındaki bir tablo
* **MV:** Bir veritabanındaki bir tablo meta veri deposu

### <a name="synchronization-steps"></a>**Eşitleme adımları**
Eşitleme işlemi, aşağıdaki adımları içerir:

1. **AD Al:** Active Directory nesneleri, Active Directory CS kapsama alınır.

2. **Azure AD'den alma:** Azure AD nesnelerini Azure AD CS kapsama alınır.

3. **Eşitleme:** Gelen eşitleme kurallarını ve giden eşitleme kurallarını, gelen büyük daha düşük bir öncelik numarası sırasına göre çalıştırılır. Eşitleme kurallarını görüntülemek için eşitleme kuralları Düzenleyicisi'ndan Masaüstü uygulamaların gidin. Gelen eşitleme kuralları, MV için CS verilerinizi getirin. Giden eşitleme kuralları veri MV CS'e taşıyın.

4. **AD için dışarı aktarın:** Eşitlemeden sonra nesneleri Active Directory CS Active Directory için dışarı aktarılır.

5. **Azure AD'ye dışarı aktarın:** Eşitlemeden sonra nesneler Azure AD CS Azure AD'ye aktarılır.

## <a name="troubleshooting"></a>Sorun giderme

Hataları bulmak için aşağıdaki sırayla birkaç farklı yerlerde bakın:

1. [İşlem günlükleri](#operations) içeri aktarma ve eşitleme sırasında eşitleme altyapısı tarafından tanıtılan hataları bulmak için.
2. [Bağlayıcı alanına](#connector-space-object-properties) eksik nesneleri ve Eşitleme hataları bulmak için.
3. [Meta veri deposu](#metaverse-object-properties) verilerle ilgili sorunları bulmak için.

Başlangıç [Eşitleme Hizmeti Yöneticisi](how-to-connect-sync-service-manager-ui.md) adımları başlamadan önce.

## <a name="operations"></a>İşlemler
**İşlemleri** sekme Eşitleme Hizmeti Yöneticisi'nde, sorun gidermeyi nereden başlaması gerektiğini kullanılabilir. Bu sekme en son işlem sonuçlarını gösterir. 

![Ekran görüntüsü, eşitleme hizmeti seçili işlemleri sekmesini gösteren Yöneticisi,](./media/tshoot-connect-object-not-syncing/operations.png)  

Üst yarısında **işlemleri** sekmesi kronolojik sırayla tüm çalıştırmalar gösterir. Varsayılan olarak, son yedi gün saklar bilgilerini işlemleri günlüğe, ancak bu ayar değiştirilebilir [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md). Tüm çalıştırmak için Görünüm göstermiyor bir **başarı** durumu. Başlıklarını tıklatarak sıralama değiştirebilirsiniz.

**Durumu** sütun en önemli bilgileri içeren ve bir çalıştırma için en önemli bir sorun gösterir. Araştırma öncelik sırasına göre en yaygın durumlar hızlı bir özeti aşağıda verilmiştir (burada * birden çok olası hata dizelerini gösterir).

| Durum | Açıklama |
| --- | --- |
| durduruldu-* |Çalıştırma tamamlayamadı. Uzak sistem kapalı ve temas kurulamıyor Bu, örneğin, meydana gelebilir. |
| durduruldu-hata-sınırı |5. 000'den fazla hataları vardır. Çalıştırma, çok sayıda hata nedeniyle otomatik olarak durduruldu. |
| Tamamlanan -\*-hata |Çalıştırma tamamlandı, ancak araştırılması gereken hataları (az 5.000) vardır. |
| Tamamlanan -\*-uyarılar |Çalıştırma tamamlandı, ancak bazı veriler beklenen durumda değil. Hatalar varsa, bu ileti genellikle yalnızca bir belirtisidir. Hataları da giderdik kadar Uyarılarını Araştır yok. |
| başarılı |Sorun yok. |

Bir satır sonuna seçtiğinizde **işlemleri** sekmesi, çalıştırmanın ayrıntılarını göstermek için güncelleştirilir. Bu alan en sol tarafında başlıklı bir listesine sahip **adım #**. Bu liste yalnızca ormanınızda birden çok etki alanınız ve her etki alanı adımı tarafından temsil edilen görünür. Etki alanı adı başlığı altında bulunabilir **bölüm**. Altında **eşitleme istatistikleri** başlığı işlendi değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilmiş nesneleri listesini almak için bağlantıyı seçin. Hatalarla nesneniz varsa, bu hataları görünür **eşitleme hatalarını** başlığı.

### <a name="errors-on-the-operations-tab"></a>İşlemleri sekmesini hatalarında
Hatalar, Eşitleme Hizmeti Yöneticisi hata nesnesinde hem hata kendisini daha fazla bilgi sağlayan bağlantılar olarak gösterir.

![Eşitleme Hizmeti Yöneticisi'nde hataların ekran görüntüsü](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Hata dizesi seçerek başlatın. (Önceki resimde, hata dizedir **eşitleme kuralı-hata-işlevi-tetiklenen**.) İlk nesnenin bir bakış sunulur. Gerçek hata görmek için seçin **yığın izlemesi**. Bu izleme, hata için hata ayıklama düzeyinde bilgileri sağlar.

Sağ **çağrı yığını bilgisi** kutusunun **Tümünü Seç**ve ardından **kopyalama**. Ardından yığın kopyalayın ve Not Defteri gibi sık kullandığınız düzenleyicinizi hataya bakın.

Gelen hata ise **SyncRulesEngine**, çağrı yığını bilgilerini ilk nesnenin tüm öznitelikleri listeler. Başlık görene kadar kaydırın **InnerException = >**.  

  ![Eşitleme hizmeti InnerException başlığı altındaki hata bilgilerini gösteren Yöneticisi'nin, ekran = >](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)
  
Hata başlığı gösterir sonraki satır. Yukarıdaki resimde, Fabrikam oluşturulan bir özel eşitleme kuralı bir hatadır.

Hata yeterli bilgi sağlamazsa varsa, verileri bırakma zamanı geldi. Nesne tanımlayıcısı ile bağlantıyı seçin ve sorun giderme devam [bağlayıcı alanına içeri aktarılan nesneye](#cs-import).

## <a name="connector-space-object-properties"></a>Bağlayıcı alanı nesne özellikleri
Varsa [ **işlemleri** ](#operations) sekmesi herhangi bir hata gösterir. bağlayıcı alanı nesnesi Active Directory'den Azure AD'ye meta veri deposu izleyin. Bu yolu, sorunun nerede olduğunu bulmanız gerekir.

### <a name="searching-for-an-object-in-the-cs"></a>Cs bir nesne için arama

Eşitleme Hizmeti Yöneticisi'nde **Bağlayıcılar**, Active Directory Bağlayıcısı seçip **arama bağlayıcı alanı**.

İçinde **kapsam** kutusunda **RDN** CN özniteliğini aramak ve seçmek istediğinizde **DN'si ya da bağlantı** arama yapmak istediğinizde **distinguishedName**  özniteliği. Bir değer girin ve seçin **arama**. 
 
![Bağlayıcı alanı arama ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Nesne bulamazsanız, ile filtre uygulanmış için aradığınız [etki alanı tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#domain-based-filtering) veya [OU tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering). Filtreleme beklendiği gibi yapılandırıldığını doğrulamak için okuma [Azure AD Connect eşitleme: Filtrelemeyi Yapılandırma](how-to-connect-sync-configure-filtering.md).

Azure AD Bağlayıcısı'nı seçerek yararlı başka bir arama gerçekleştirebilir. İçinde **kapsam** kutusunda **bekleyen alma**ve ardından **Ekle** onay kutusu. Bu arama bir şirket içi nesne ile ilişkilendirilemiyor, Azure ad'deki tüm eşitlenmiş nesneleri sağlar.  

![Bağlayıcı alanı arama artık ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cssearchorphan.png) 
 
Bu nesneleri başka bir eşitleme altyapısı veya farklı bir filtre yapılandırma ile bir eşitleme altyapısı tarafından oluşturuldu. Artık bu nesneler artık yönetilmez. Bu listeyi gözden geçirin ve bu nesneleri kullanarak kaldırabilirsiniz [Azure AD PowerShell](https://aka.ms/aadposh) cmdlet'leri.

### <a name="cs-import"></a>CS içeri aktarma
CS nesnesi açtığınızda en üstünde birden çok sekme bulunur. **Alma** sekmesi, bir içeri aktarma işleminden sonra hazırlanan verileri gösterir.  

![İçeri aktarma sekmesi seçili bağlayıcı alanı nesne özellikleri penceresinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/csobject.png)    

**Eski değer** sütunda görüntülenir Bağlan şu anda depolanan ve **yeni değer** sütun hangi kaynak sistemden alındı ve henüz uygulanmadığını gösterir. Nesne üzerinde bir hata varsa, değişiklikleri işlenmez.

**Eşitleme hatası** sekme içinde görünür olduğundan **bağlayıcı alanı nesne özellikleri** yalnızca nesne ile ilgili bir sorun varsa penceresi. Daha fazla bilgi için nasıl [üzerinde eşitleme hatalarıyla ilgili sorunları giderme **işlemleri** sekmesini](#errors-on-the-operations-tab).

![Bağlayıcı alanı nesne özellikleri penceresinde eşitleme hatası sekmesinin Ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  

### <a name="cs-lineage"></a>CS kökenini
**Kökenini** sekmesinde **bağlayıcı alanı nesne özellikleri** penceresi bağlayıcı alanı nesnesi için meta veri deposu nesne nasıl ilişkili olduğunu gösterir. Ne zaman bağlayıcı en son değişiklik bağlı sistemden alınacak ve hangi kuralları meta verilerinde doldurmak için uygulanacak görebilirsiniz.  

![Bağlayıcı alanı nesne özellikleri penceresinde kökenini sekmesini gösteren ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cslineage.png)  

Yukarıdaki şekilde, **eylem** sütun gösterir bir gelen eşitleme kuralı eylemi **sağlama**. Bu bağlayıcı alanı nesnesi mevcut olduğu sürece, meta veri deposu nesne kalır gösterir. Eşitleme kuralları listesini bunun yerine bir giden eşitleme kuralı ile gösteren, bir **sağlama** eylem, bu nesne, meta veri deposu nesnesi silindiğinde silinir.  

![Bağlayıcı alanı nesne özellikleri penceresinde kökenini sekmesinde kökenini penceresinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cslineageout.png)  

Önceki şekilde de görebilirsiniz **PasswordSync** gelen bağlayıcı alanında katkıda bulunabilir sütun değeri bir eşitleme kuralına sahip olduğundan parola değişiklikleri **True**. Bu parola, Azure AD'ye giden kuralı aracılığıyla gönderilir.

Gelen **kökenini** sekmesini seçerek için meta veri alabilirsiniz [ **meta veri deposu nesne özellikleri**](#mv-attributes).

### <a name="preview"></a>Önizleme
Sol alt köşesindeki **bağlayıcı alanı nesne özellikleri** penceresi **Önizleme** düğmesi. Açmak için bu düğmeyi seçerek **Önizleme** sayfasında, burada, eşitlenebilmesi tek bir nesne. Bu sayfa, bazı özel eşitleme kuralları giderirken ve tek bir nesne üzerinde bir değişikliğin etkilerini görmek istediğinizde yararlı olur. Seçebileceğiniz bir **tam eşitleme** veya **Delta eşitleme**. Belirleyebilirsiniz **oluşturma Önizleme**, hangi yalnızca tutar değişikliği bellekte. Veya **işleme Önizleme**, meta veri güncelleştirmeleri ve hedef bağlayıcı alanları yapılan tüm değişiklikleri hazırlar.  

![Seçili önizlemeyi Başlat Önizleme sayfasının ekran görüntüsü](./media/tshoot-connect-object-not-syncing/preview.png)  

Önizlemede nesne inceleyin ve hangi kural belirli bir öznitelik akışı için bkz.  

![Önizleme sayfası içeri aktarma öznitelik akışı gösteren ekran görüntüsü](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Günlük
Yanındaki **Önizleme** düğmesini seçme **günlük** açmak için düğmeyi **günlük** sayfası. Parola Eşitleme durumunu ve geçmişini burada görebilirsiniz. Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karması eşitleme sorunlarını giderme](tshoot-connect-password-hash-synchronization.md).

## <a name="metaverse-object-properties"></a>Meta veri deposu nesne özellikleri
Genellikle Active Directory Bağlayıcısı alanına kaynağından Aramayı Başlat daha iyidir. Ancak, meta veri deposu arama başlatabilirsiniz.

### <a name="searching-for-an-object-in-the-mv"></a>MV nesnesi için arama
Eşitleme Hizmeti Yöneticisi'nde **meta veri deposu arama**, aşağıdaki şekildeki gibi. Kullanıcı bulur bildiğiniz bir sorgu oluşturun. Ortak öznitelikler için arama yapın **accountName** (**sAMAccountName**) ve **userPrincipalName**. Daha fazla bilgi için [Eşitleme Hizmeti Yöneticisi'ni meta veri deposu arama](how-to-connect-sync-service-manager-ui-mvsearch.md).

![Ekran görüntüsü, Eşitleme Hizmeti Yöneticisi, meta veri deposu Arama sekmesi seçili](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

İçinde **arama sonuçları** penceresinde nesneye tıklayın.

Nesne bulunamadı, meta veri deposu henüz ulaşılmamış. Active Directory nesnesi için aramaya devam edin [bağlayıcı alanına](#connector-space-object-properties). Active Directory Bağlayıcı alanı nesne bulursanız, yakında için meta veri deposu nesnesinin engelleyen bir eşitleme hatası olabilir veya bir eşitleme kuralı kapsam belirleme filtresi uygulanabilir.

### <a name="object-not-found-in-the-mv"></a>Nesnesi, MV içinde bulunamadı
İçinde Active Directory CS nesnedir ancak MV içinde mevcut değil, bir kapsam belirleme filtre uygulanmış değilse. Kapsam belirleme filtresi aramak için masaüstü uygulaması menüsüne gidin ve seçin **eşitleme kuralları Düzenleyicisi**. Aşağıdaki filtre ayarlayarak nesneye uygulanabilir kurallarını filtreleyin.

  ![Ekran görüntüsü, eşitleme kuralları bir gelen eşitleme kuralları arama gösteren Düzenleyicisi](./media/tshoot-connect-object-not-syncing/syncrulessearch.png)

Yukarıdaki listede her kural görüntülemek ve denetlemek **Scoping filtre**. Şu kapsam belirleme filtresi içinde varsa **isCriticalSystemObject** değeri null veya boş ya da FALSE, kapsamları dahilinde olması.

  ![Bir kapsam belirleme filtresi bir gelen eşitleme kuralı arama ekran görüntüsü](./media/tshoot-connect-object-not-syncing/scopingfilter.png)

Git [CS alma](#cs-import) öznitelik listesi ve filtre nesne için MV taşınmasını engelleyen denetleyin. **Bağlayıcı alanına** öznitelik listesi yalnızca null olmayan ve boş olmayan öznitelikleri gösterir. Örneğin, varsa **isCriticalSystemObject** değil görünmesini listesinde, bu öznitelik değeri null veya boş.

### <a name="object-not-found-in-the-azure-ad-cs"></a>Nesnesi, Azure AD CS'yi bulunamadı
Nesne Azure AD bağlayıcı alanında mevcut değil, ancak MV içinde mevcut olduğundan, karşılık gelen bağlayıcı alanına giden kuralları, kapsam belirleme filtresi arayın ve nesne olduğundan filtrenin dışında kaldı, öğrenmek [MV öznitelikleri](#mv-attributes)ölçütlerine uymayan.

Giden kapsam belirleme filtresi aramak için aşağıdaki filtre ayarlayarak nesne için uygun kuralları'nı seçin. Her kural görüntüleyin ve karşılık gelen en Ara [MV özniteliği](#mv-attributes) değeri.

  ![Eşitleme kuralları Düzenleyicisi'nde giden eşitleme kuralları aramasının ekran görüntüsü](./media/tshoot-connect-object-not-syncing/outboundfilter.png)


### <a name="mv-attributes"></a>MV öznitelikleri
Üzerinde **öznitelikleri** sekmesi, değerler ve bunların hangi bağlayıcılar katkıda görebilirsiniz.  

![Seçilen öznitelikler sekmesiyle meta veri deposu nesne özellikleri penceresinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Bir nesne eşitlemiyor meta veri deposu özniteliği durumları hakkında aşağıdaki soruları isteyin:
- Öznitelik **cloudFiltered** sunmak ve kümesine **True**? İse, bu adımları göre filtre uygulanmış [öznitelik tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#attribute-based-filtering).
- Öznitelik **sourceAnchor** var? Aksi durumda, bir hesap-kaynak orman topolojisini var? Bir nesne bağlı bir posta kutusu tanımlanması durumunda (öznitelik **msExchRecipientTypeDetails** değerine sahip **2**), **sourceAnchor** ormanı tarafından katkıda bulunulan bir Active Directory hesabı etkin. Ana hesap alınan ve doğru bir şekilde eşitlenen emin olun. Ana hesap arasında listelenmelidir [Bağlayıcılar](#mv-connectors) nesne.

### <a name="mv-connectors"></a>MV bağlayıcılar
**Bağlayıcılar** sekmesi, nesnenin bir gösterimi olan tüm bağlayıcı alanları gösterir. 
 
![Bağlayıcılar sekmesi seçili meta veri deposu nesne özellikleri penceresinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  

Bir bağlayıcıyı sahip olmalıdır:

- Her kullanıcının Active Directory ormanı temsil edilir. Bu gösterim içerebilir **foreignSecurityPrincipals** ve **kişi** nesneleri.
- Azure AD'de bir bağlayıcı.

Azure ad Bağlayıcısı kayıpsa bölümü gözden [MV öznitelikleri](#mv-attributes) Azure AD'ye sağlama ölçütlerini doğrulayın.

Gelen **Bağlayıcılar** de gidip için sekmesinde [bağlayıcı alanı nesnesi](#connector-space-object-properties). Bir satırı seçin ve tıklayın **özellikleri**.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md).
- Daha fazla bilgi edinin [karma kimlik](whatis-hybrid-identity.md).
