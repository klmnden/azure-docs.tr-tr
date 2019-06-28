---
title: Belirli bir kullanıcı bir uygulamaya erişmeye çalıştığında ne zaman sunulacaktır kullanıma bulma | Microsoft Docs
description: Kritik düzeyde önemli bir kullanıcı Azure AD ile kullanıcı sağlama için yapılandırmış olduğunuz bir uygulamaya erişmeye çalıştığında, bulma
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fd6b70e7a4542a4cad2ee95fa280ddf8fbe6553
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310019"
---
# <a name="check-the-status-of-user-provisioning"></a>Kullanıcı sağlama durumunu denetleyin

Azure AD sağlama hizmeti, kaynak ve hedef sistemi, düzenli aralıklarla artımlı döngüsü tarafından izlenen karşı bir ilk sağlama döngüsü çalıştırır. Bir uygulama için sağlama yapılandırdığınızda, sağlama hizmetinin geçerli durumunu denetleyin ve ne zaman bir kullanıcı bir uygulamaya erişmek mümkün olacaktır bakın.

## <a name="view-the-provisioning-progress-bar-preview"></a>Sağlama görüntülemek ilerleme: (Önizleme)

 Üzerinde **sağlama** sayfa bir uygulama için Azure AD sağlama hizmeti durumunu görüntüleyebilirsiniz. **Geçerli durumu** sayfanın alt kısmındaki bölümde sağlama döngüsü kullanıcı hesaplarını sağlama başlayıp başlamadığını gösterir. Döngüsü ilerlemesini izleyin, kaç kullanıcılar ve gruplar sağlanan bakın ve kaç rollerin oluşturulacağını bakın.

Otomatik sağlama, ilk kez yapılandırırken **geçerli durumu** sayfanın alt kısmındaki bölümde ilk sağlama döngüsü durumunu gösterir. Bu bölümde, artımlı bir döngü her çalıştığında güncelleştirir. Aşağıdaki ayrıntıları gösterilir:
- Çalışmakta olan veya son tamamlanan sağlama döngüsü (ilk veya artan) türü.
- A **ilerleme çubuğu** tamamlandığını sağlama döngüsü yüzdesini gösteren. Yüzde sağlanan sayfaların sayısını yansıtır. Her bir sayfa birden fazla kullanıcılar veya gruplar, yüzde doğrudan kullanıcı sayısı için bağıntısını olmayan şekilde, grupların veya rollerin sağlanan içerebilir unutmayın.
- A **Yenile** düğmesi güncelleştirilmiş görünümü tutmak için kullanabilirsiniz.
- Sayısını **kullanıcılar** ve **grupları** hazırlandı ve oluşturulan roller sayısı. İlk döngü sırasında **kullanıcılar** sayar 1 tarafından bir kullanıcı oluşturulduğunda veya güncelleştirildiğinde ve bir kullanıcı silindiğinde, 1 ile değerden aşağı sayıyor. Artımlı bir döngüsü sırasında kullanıcı güncelleştirmeleri etkilemez **kullanıcılar** sayısı; sayı değişiklikleri kullanıcılar yalnızca oluşturulduğu veya silindiği zaman.
- A **denetim günlüklerini görüntüle** bağlantı, Azure AD denetim açılır oturum tüm işlemleri hakkındaki ayrıntıları sağlama durumu bireysel kullanıcılar için de dahil olmak üzere kullanıcı sağlama hizmeti tarafından çalıştırın (bkz [kullanım denetim günlükleri](#use-audit-logs-to-check-a-users-provisioning-status)bölümüne bakın).

Sağlama döngüsü tamamlandıktan sonra **tarih istatistikleri** Bölüm kullanıcıları ve grupları tarih, tamamlanma tarihini ve son döneminin süresi ile birlikte sağlanan toplu sayılarını gösterir. **Etkinlik kimliği** en son sağlama döngüsü benzersiz olarak tanımlar. **İş kimliği** sağlama işi için benzersiz bir tanımlayıcıdır ve kiracınızdaki uygulamaya özgüdür.

Sağlama ilerleme görüntülenen Azure portalında **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; sağlama** sekmesi.

![Sayfa ilerleme çubuğu sağlama](media/application-provisioning-when-will-provisioning-finish-specific-user/provisioning-progress-bar-section.png)

## <a name="use-audit-logs-to-check-a-users-provisioning-status"></a>Bir kullanıcının sağlama durumunu denetlemek için Denetim günlükleri kullanın

Seçilen kullanıcı için sağlama durumunu görmek için Azure AD'de denetim günlüklerini inceleyin. Sağlama hizmeti kullanıcı tarafından çalıştırılan tüm işlemleri Azure AD'de kayıtlı denetim günlükleri. Bu tüm içeren okuma ve yazma işlemleri kaynak ve hedef sistemleri ve okuma veya her işlemi sırasında yazılan kullanıcı verileri için yapılır.

Sağlama denetim günlüklerinin Azure portalında erişilebilen **Azure Active Directory &gt; Kurumsal uygulamaları &gt; \[uygulama adı\] &gt; denetim günlükleri** sekmesi. Günlükleri filtreleyin **hesap sağlama** yalnızca bu uygulama için sağlama olayları görmek için kategori. "İçinde öznitelik eşlemelerini kendileri için yapılandırılan eşleşen ID" göre kullanıcılar için arama yapabilirsiniz. 

Örneğin "kullanıcı asıl adı" veya "Azure AD tarafında eşleşen öznitelik olarak e-posta adresi" yapılandırılmış ve değerini değil sağlama kullanıcı varsa, "audrey@contoso.com", ardından Denetim günlüklerini arama "audrey@contoso.com" ve ardından girişlerini gözden geçirin döndürdü.

Sağlama hizmeti tarafından gerçekleştirilen tüm işlemleri sağlama denetim günlüklerini kaydetme dahil olmak üzere:

* Azure AD sağlama kapsamında olan atanan kullanıcılar için sorgulama
* Hedef uygulama bu kullanıcıların varlığı için sorgulama
* Sistem arasındaki kullanıcı nesneleri karşılaştırma
* Ekleme, güncelleştirme veya karşılaştırma üzerine dayalı hedef sistemde kullanıcı hesabı devre dışı bırakma

Azure portalında denetim günlükleri okuma hakkında daha fazla bilgi için bkz. [sağlama raporlama Kılavuzu](check-status-user-account-provisioning.md).

## <a name="how-long-will-it-take-to-provision-users"></a>Ne kadar kullanıcıları sağlama sürer?
Bir uygulama ile otomatik kullanıcı hazırlama kullanırken, Azure AD'ye otomatik olarak sağlar ve kullanıcı hesapları gibi şeyleri temel alan bir uygulama, güncelleştirmeleri [kullanıcı ve Grup atamasına](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) bir düzenli olarak zamanlanan saat aralığı, genellikle 10 dakikada bir.

Belirli bir kullanıcının sağlanması gereken süreyi çoğunlukla sağlama iş bir ilk eşitleme ya da bir artımlı eşitleme kullanılıp kullanılmadığını bağlıdır.

- İçin **ilk eşitlemeler**, sağlama, kapsamında kullanıcıların ve grupların sayısı da dahil olmak üzere, birçok faktöre ve kullanıcı ve grup kaynak sistemindeki toplam sayısı işi zaman bağlıdır. Azure AD arasında ilk eşitleme ve uygulama herhangi bir Azure AD dizinindeki kullanıcıların sağlama kapsamında sayısı ve boyutuna bağlı olarak birkaç saat 20 dakika sürebilir. İlk eşitleme performansı etkileyen faktörleri kapsamlı bir listesi bu bölümde daha sonra özetlenir.

- İçin **artımlı eşitlemeler** ilk eşitlemeden sonra iş saatleri sağlama hizmeti performansını iyileştirme ilk eşitlemeden sonra her iki sistem durumunu temsil eden filigranlar depoları olarak (örn: 10 dakika içinde), daha hızlı olma eğilimindedir sonraki eşitlemeler. İş saati içinde sağlama, döngü algılandı değişikliklerin sayısı bağlıdır. 5\. 000'den daha az kullanıcı veya grup üyeliği değişiklikleri varsa, tek bir artımlı sağlama döngüsü içinde iş tamamlayabilir. 

Eşitleme zamanlarını sağlama yaygın senaryolar için aşağıdaki tabloda özetlenmiştir. Bu senaryolarda, Azure AD kaynaklı sistemidir ve hedef sistemde bir SaaS uygulamasıdır. Eşitleme sürelerini ServiceNow, çalışma alanı, Salesforce ve G Suite SaaS uygulamaları için eşitleme işlerinin istatistiksel çözümleme türetilmiştir.


| Kapsam yapılandırması | Kullanıcılara, gruplara veya kapsamda üyeleri | İlk eşitleme zamanı | Artımlı eşitleme zamanı |
| -------- | -------- | -------- | -------- |
| Atanan kullanıcı ve grupları yalnızca Eşitle |  < 1,000 |  < 30 dakika | < 30 dakika |
| Atanan kullanıcı ve grupları yalnızca Eşitle |  1\.000 - 10.000 | 142 - 708 dakika | < 30 dakika |
| Atanan kullanıcı ve grupları yalnızca Eşitle |   10,000 - 100,000 | 1,170 - 2,340 dakika | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  < 1,000 | < 30 dakika  | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  1\.000 - 10.000 | < 30-120 dakika | < 30 dakika |
| Azure AD'de tüm kullanıcıları ve grupları Eşitle |  10,000 - 100,000  | 713 - 1,425 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme|  < 1,000  | < 30 dakika | < 30 dakika |
| Tüm kullanıcılar Azure AD'de eşitleme | 1\.000 - 10.000  | 43 - 86 dakika | < 30 dakika |


Yapılandırma için **eşitleme atanan kullanıcı ve grupları yalnızca**, şu formüllerden yaklaşık minimum ve maksimum beklenen belirlemek için kullanabileceğiniz **ilk eşitleme** saatler:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Tamamlamak süresini etkileyen faktörler özeti bir **ilk eşitleme**:

- Kullanıcılar ve gruplar sağlama kapsamında toplam sayısı.

- Kullanıcılar, gruplar ve Grup üyeleri kaynak sistemde bulunan (Azure AD) toplam sayısı.

- Olup kullanıcıların sağlama kapsamında hedef uygulamada mevcut kullanıcılar eşleştirilir veya ilk kez oluşturulması gerekir. Eşitleme işleri, tüm kullanıcılar için ilk kez oluşturulur ele hakkında *iki kez sürece* olarak eşitleme işleri, tüm kullanıcılar için mevcut kullanıcıların eşleştirilir.

- Hataların sayısı [denetim günlükleri](check-status-user-account-provisioning.md). Çok sayıda hata ve sağlama hizmeti bir karantina duruma geçti performans daha yavaş olur.    

- İstek hız sınırları ve hedef sistem tarafından uygulanan kısıtlama. Bazı hedef sistemleri, istek hızı sınırlarını ve kısıtlama, büyük eşitleme işlemler sırasındaki performansınızı etkileyebilir uygular. Bu şartlar altında çok fazla istek çok hızlı aldığı uygulama yanıt hızını yavaş veya bağlantıyı kapatın. Performansı artırmak için bağlayıcı uygulama isteklerini uygulama bunları işleyebileceğinden daha hızlı göndererek değil ayarlamak gerekir. Microsoft tarafından oluşturulan sağlama bağlayıcılar bu ayarı yapın. 

- Atanan gruplar boyutunu ve sayı. Atanan gruplar eşitleniyor kullanıcıları eşitleme daha uzun sürer. Sayı ve boyutları atanan gruplar performansı etkileyebilir. Bir uygulama varsa [eşlemeleri için nesne eşitleme grubu etkin](customize-application-attributes.md#editing-group-attribute-mappings)grubu adları gibi Grup Özellikleri ve üyeliklerinin yanı sıra kullanıcılar eşitlenmiş. Bu ek eşitlemeler yalnızca kullanıcı, nesneyi eşitleme daha uzun sürer.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatik hale getirme](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)
