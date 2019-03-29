---
title: Güvenlik ilkeleri ile çalışma | Microsoft Docs
description: Bu makalede Azure Güvenlik Merkezi'nde güvenlik ilkeleri ile çalışmaya nasıl.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: monhaber
ms.openlocfilehash: 1931026869e930caef2ff2f92fb85dade15a9c8c
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578450"
---
# <a name="working-with-security-policies"></a>Güvenlik ilkeleriyle çalışma

Bu makalede, güvenlik ilkeleri nasıl yapılandırılır ve bunları Güvenlik Merkezi'nde görüntülemek nasıl açıklanmaktadır. Azure Güvenlik Merkezi otomatik olarak atar, [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md) eklendikten her abonelikte. Bunları yapılandırabilirsiniz [Azure İlkesi](../governance/policy/overview.md), ayrıca sağlayan yönetim grupları arasında ve birden çok aboneliğe ilkeleri ayarlamak.

PowerShell kullanarak ilkeler ayarlama konusunda yönergeler için bkz: [hızlı başlangıç: Azure PowerShell modülü kullanarak uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturma](../governance/policy/assign-policy-powershell.md).

>[!NOTE]
> Güvenlik Merkezi tümleştirmesi, Azure İlkesi ile başlatıldı. Mevcut müşteriler, Azure İlkesi ' nde yerleşik yeni girişim yerine önceki Güvenlik Merkezi'nde güvenlik ilkeleri için otomatik olarak geçirilecektir. Bu değişiklik, kaynakları veya Azure İlkesi'nde yeni girişim varlığını dışında ortamının etkilemez.

## <a name="what-are-security-policies"></a>Güvenlik ilkeleri nedir?
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Azure İlkesi'nde Azure Abonelikleriniz için ilkeler tanımlayın ve bunları iş yükü türüne veya verilerinizin duyarlılığına göre uygun hale getirin. Örneğin, kişisel bilgiler gibi düzenlenen veriler kullanan uygulamalar, diğer iş yükleri yüksek seviyede güvenliği gerektirebilir. Abonelikler arasında ya da Yönetim grupları bir ilke ayarlamak için bunları kümesinde [Azure İlkesi](../governance/policy/overview.md).

Güvenlik ilkelerinizi size Azure Güvenlik Merkezi'nde güvenlik önerilerini. Uyumluluk, olası zayıflıkları belirlemek ve tehditleri önlemeye yardımcı olmak için onlarla izleyebilirsiniz. Listesini sizin için uygun seçeneği belirleme hakkında daha fazla bilgi için bkz. [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md).

Güvenlik Merkezi'ni etkinleştirdiğinizde, yerleşik Güvenlik Merkezi güvenlik ilkesi, yerleşik bir girişim kategorisi Güvenlik Merkezi altında olarak Azure İlkesi'nde yansıtılır. Yerleşik girişim, tüm Güvenlik Merkezi kayıtlı abonelikler (ücretsiz veya standart katmanları) otomatik olarak atanır. Yerleşik girişim yalnızca denetim ilkeleri içerir.


### <a name="management-groups"></a>Yönetim grupları
Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure Yönetim Grupları, aboneliklerin üzerinde bir kapsam sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve yönetim ilkelerinizi bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan ilkeleri devralır. Her dizinde "kök" yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu bulunur. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu kök yönetim grubu, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasını sağlar. Azure Güvenlik Merkezi ile kullanmak için Yönetim grupları ayarlamak için yönergeleri izleyin. [kazanmak için Azure Güvenlik Merkezi, Kiracı genelinde görünürlük](security-center-management-groups.md).

> [!NOTE]
> Yönetim gruplarının ve aboneliklerin hiyerarşisini anlamanız önemlidir. Yönetim grupları, kök yönetimi ve yönetim grubu erişimi hakkında daha fazla bilgi edinmek için bkz. [Kaynaklarınızı Azure Yönetim Gruplarıyla düzenleme](../governance/management-groups/index.md#root-management-group-for-each-directory).
>

## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. Şunları yapmak için Azure İlkesi ilkeleri düzenleyebilirsiniz:
- Yeni ilke tanımları oluşturma.
- Bir kuruluşun tamamını veya kuruluş içindeki iş birimini temsil edebilen yönetim gruplarına ve aboneliklere ilkeler atama.
- İlke uyumluluğunu izleme.

Azure İlkesi hakkında daha fazla bilgi için [Uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md) konusunu inceleyin.

Bir Azure ilkesi aşağıdaki bileşenlerden oluşur:

- A **ilke** bir kuraldır.
- Bir **girişim** ilkeleri oluşan bir koleksiyondur.
- Bir **atama** bir ilke veya girişim uygulama belirli bir kapsama (Yönetim grubu, abonelik veya kaynak grubu).

## <a name="view-security-policies"></a>Güvenlik ilkelerini görüntüleme

Güvenlik Merkezi'nde güvenlik ilkelerinizi görüntüleme:

1. İçinde **Güvenlik Merkezi** panoyu seçin **Güvenlik İlkesi**.

    ![İlke Yönetimi bölmesi](./media/security-center-policies/security-center-policy-mgt.png)

   İçinde **İlkesi Yönetimi** ekran, Yönetim grupları, abonelikleri ve çalışma alanlarının yanı sıra, yönetim grubu yapısı sayısını görebilirsiniz.

   > [!NOTE]
   > - Güvenlik Merkezi panosunu aboneliklerin daha yüksek bir sayı gösterebilir **abonelik kapsamı** altında gösterilen aboneliklerin sayısından **İlkesi Yönetimi**. Abonelik kapsamı Standart, Ücretsiz ve “kapsanmayan” aboneliklerin sayısını gösterir. "Kapsanmayan" abonelik, Güvenlik Merkezi'nin etkin olmayan ve altında görüntülenmez **İlkesi Yönetimi**.
   >

   Tablodaki sütunlar şunları gösterir:

   - **İlke girişimi atama** – Güvenlik Merkezi [yerleşik ilkeleri](security-center-policy-definitions.md) ve bir abonelik veya yönetim grubuna atanmış olan girişim.
   - **Kapsamı** – boş veya yönetim grubu, abonelik veya çalışma çalıştığı standart fiyatlandırma katmanı tanımlar.  Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
   - **Ayarları** – aboneliğiniz bağlantıyı **ayarlarını Düzenle**. Seçme **ayarlarını Düzenle** güncelleştirmenize olanak tanır, [Güvenlik Merkezi Ayarları](security-center-policies-overview.md) her abonelik veya yönetim grubu için.
   - **Güvenli puanı** - [güvenli puanı](security-center-secure-score.md) nasıl güvenli bir iş yükü güvenlik duruşunu bir ölçü sağlar ve iyileştirme önerileri önceliğini belirlemeye yardımcı olur.

2. İlkeleri görüntülemek istediğiniz abonelik veya yönetim grubunu seçin.

   - **Güvenlik İlkesi** ekran seçtiğiniz abonelik veya yönetim grubunda atanan ilkeleri tarafından gerçekleştirilecek eylemi yansıtır.
   - En üstünde, her İlkesi'ni açmak için sağlanan bağlantıları kullanabilirsiniz **atama** abonelik veya yönetim grubuna yöneliktir. Atama erişmek ve düzenlemek veya ilkeyi devre dışı bırakmak için bağlantıları kullanabilirsiniz. Örneğin, belirli bir ilke ataması etkin uç nokta koruma engelleme görürseniz, erişim ilkesi ve düzenleyebilir ya da devre dışı bırakmak için bağlantıyı kullanabilirsiniz.
   - İlkeler listesinde, abonelik veya yönetim grubu üzerinde etkili uygulama ilkesi görebilirsiniz. Bu kapsama uygulanan her ilke ayarlarını dikkate alınır ve ilke tarafından hangi eylemlerin, toplu sonucu ile sağlanan anlamına gelir. Örneğin bir atama ilkesi devre dışıdır, ancak başka bir programda Auditıfnotexist için ayarlanmış toplu etkisi Auditıfnotexist geçerlidir. Etkin etkisi her zaman önceliklidir.
   - İlkeleri etkisi olabilir: Append, Denetim, AuditIfNotExists, reddetme, Deployıfnotexists, devre dışı bırakıldı. Etkileri nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [anlamak ilke etkileri](../governance/policy/concepts/effects.md).

   ![İlkesi ekranı](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Görüntülersiniz ilkeleri atandığında, birden çok atamaları görebilir ve nasıl her atama, kendi yapılandırıldığını görebilirsiniz.

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Her Azure aboneliği ve'daki yönetim grupları için varsayılan güvenlik ilkesini düzenleyebilirsiniz [Azure İlkesi](../governance/policy/tutorials/create-and-manage.md). Bir güvenlik ilkesini değiştirmek için abonelikte veya ilkeyi içeren yönetim grubunda sahip, katkıda bulunan veya güvenlik yöneticisi rolüne sahip olmanız gerekir.

Azure İlkesi'nde bir güvenlik ilkesi düzenleme hakkında yönergeler için bkz ve [oluşturma ve yönetme uyumluluğu zorlamak için ilke](../governance/policy/tutorials/create-and-manage.md).

Azure İlkesi portalı, REST API veya Windows PowerShell'i kullanarak aracılığıyla aracılığıyla güvenlik ilkelerini düzenleyebilirsiniz. Aşağıdaki örnek, REST API kullanarak düzenlemek için yönergeler sağlar.


## <a name="disable-security-policies"></a>Güvenlik ilkelerini devre dışı bırak
Varsayılan güvenlik ilkesini, ortamınız için uygun değilse bir öneri oluşturuyorsa, öneri gönderen bir ilke tanımı'nı devre dışı bırakma durdurabilirsiniz.
Öneriler hakkında daha fazla bilgi için bkz: [güvenlik önerilerini yönetme](security-center-recommendations.md).

1. Güvenlik Merkezi'nde gelen **ilke ve Uyumluluk** bölümünde **Güvenlik İlkesi**.

   ![İlke yönetimi](./media/tutorial-security-policy/policy-management.png)

2. Öneri devre dışı bırakmak istediğiniz abonelik veya yönetim grubuna tıklayın.

1. Atanan ilke'ye tıklayın.

   ![İlkeyi devre dışı bırak](./media/tutorial-security-policy/security-policy.png)

1. İçinde **parametreleri** bölümü, arama, ilkeyi devre dışı bırakmak istediğiniz öneri çağırır ve aşağı açılan listeden seçin **devre dışı**

   ![İlkeyi devre dışı bırak](./media/tutorial-security-policy/disable-policy.png)
1. **Kaydet**’e tıklayın.
   > [!Note]
   > Devre dışı bırakma ilke değişikliklerin etkili olması için 12 saat sürebilir.


### <a name="configure-a-security-policy-using-the-rest-api"></a>REST API kullanarak bir güvenlik ilkesi yapılandırma

Azure İlkesi ile yerel tümleştirme bir parçası olarak, Azure Güvenlik Merkezi ilke ataması oluşturmak için avantajı Azure İlkesi'nin REST API yararlanmanıza olanak sağlar. Aşağıdaki yönergeler, ilke atamaları oluşturulmasını yanı sıra mevcut atamaları özelleştirmesini yol. 

Azure İlkesi önemli Kavramları: 

- A **ilke tanımı** bir kuralı 

- Bir **girişim** ilke tanımları (kuralları) oluşan bir koleksiyondur 

- Bir **atama** girişim veya bir ilke bir uygulama belirli bir kapsama (Yönetim grubu, abonelik, vb.) 

Güvenlik Merkezi, güvenlik ilkelerini içeren yerleşik bir girişim sahiptir. Azure kaynaklarınızın Güvenlik Merkezi'nin ilkelerini değerlendirmek için yönetim grubu veya abonelik değerlendirmek istediğiniz atama oluşturmanız gerekir.  

Yerleşik girişim varsayılan olarak etkin Güvenlik Merkezi'nin ilkeler vardır. Yerleşik girişim belirli ilkelerden devre dışı bırakmayı seçebilirsiniz, örneğin Güvenlik Merkezi'nin ilkeler uygulayabilirsiniz **web uygulaması güvenlik duvarı**, ilkenin etkisi parametresinin değerini değiştirerek için **Devre dışı bırakılmış**. 

### <a name="api-examples"></a>API örnekleri

Aşağıdaki örneklerde, bu değişkenleri değiştirin:

- **{} kapsamı**  yönetim grubu adını girin veya abonelik ilkeyi uygulamak.
- **{policyAssignmentName}**  girin [ilgili ilke ataması adı](#policy-names).
- **{name}**  adınızı veya ilke değişikliği onaylayan yöneticisinin adını girin.

Bu örnek, bir abonelik veya yönetim grubu yerleşik Güvenlik Merkezi girişimine atama gösterir
 
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 

Bu örnekte, yerleşik Güvenlik Merkezi girişimine aboneliği devre dışı aşağıdaki ilkeleriyle atama gösterir: 

- Sistem güncelleştirmeleri ("systemUpdatesMonitoringEffect") 

- Security configurations ("systemConfigurationsMonitoringEffect") 

- Uç nokta Koruması ("endpointProtectionMonitoringEffect") 


    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
    
    İstek gövdesi (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName": "İzlemeyi etkinleştir Azure Güvenlik Merkezi'nde", 
    
    "meta veri": { 
    
    "assignedBy": "{Name}" 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parametre": { 
    
    "systemUpdatesMonitoringEffect": {"value": "Disabled"}, 
    
    "systemConfigurationsMonitoringEffect": {"value": "Disabled"}, 
    
    "endpointProtectionMonitoringEffect": {"value": "Disabled"}, 
    
    }, 
    
     } 
    
    } 

Bu örnek, bir atamayı silmeyi işlemini göstermektedir:

    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 


### İlke adları Başvurusu <a name="policy-names"></a>

|Güvenlik Merkezi'ndeki ilke adı|Azure İlkesi'nde görüntülenen bir ilke adı |İlke etkisi parametresi adı|
|----|----|----|
|SQL Şifrelemesi |Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını İzle |sqlEncryptionMonitoringEffect| 
|SQL Denetimi |Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme |sqlAuditingMonitoringEffect|
|Sistem güncelleştirmeleri |Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme |systemUpdatesMonitoringEffect|
|Depolama şifrelemesi |Depolama hesapları için eksik blob şifrelemesini denetler |storageEncryptionMonitoringEffect|
|JIT ağ erişimi |Azure Güvenlik Merkezi'nde olası ağ yalnızca zamanında (JIT) erişimi izleme |jitNetworkAccessMonitoringEffect |
|Uyarlamalı uygulama denetimleri |Olası uygulama beyaz listesini Azure Güvenlik Merkezi'nde izleme |adaptiveApplicationControlsMonitoringEffect|
|Ağ güvenlik grupları |Azure Güvenlik Merkezi'nde esnek ağın genelini erişimi izleme |networkSecurityGroupsMonitoringEffect| 
|Güvenlik yapılandırmaları |Azure Güvenlik Merkezi'nde işletim sistemi güvenlik açıklarını izleyin |systemConfigurationsMonitoringEffect| 
|Uç nokta koruması |Azure Güvenlik Merkezi'nde eksik Endpoint Protection'ı izleme |endpointProtectionMonitoringEffect |
|Disk şifrelemesi |Azure Güvenlik Merkezi'nde şifrelenmemiş VM disklerini izleyin |diskEncryptionMonitoringEffect|
|Güvenlik açığı değerlendirmesi |Azure Güvenlik Merkezi'nde VM Güvenlik Açıklarını İzleme |vulnerabilityAssessmentMonitoringEffect|
|Web uygulaması güvenlik duvarı |Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme |webApplicationFirewallMonitoringEffect |
|Yeni nesil güvenlik duvarı |Azure Güvenlik Merkezi'nde korumasız ağ uç noktalarını izleme| |


### <a name="who-can-edit-security-policies"></a>Güvenlik ilkeleri düzenleyebilecek kişiler
Güvenlik Merkezi, kullanıcıları, grupları ve Azure Hizmetleri için atanan yerleşik roller sağlayan rol tabanlı erişim denetimi (RBAC) kullanır. Kullanıcı Güvenlik Merkezi'ni açtığında erişime sahip oldukları kaynakları ile ilgili bilgiler yalnızca görürler. Kullanıcılar sahibi, katkıda bulunan veya okuyucu rolünün bir kaynak barındıran abonelik veya kaynak grubuna atanan anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- Güvenlik okuyucusu: Güvenlik Merkezi, öneriler, uyarılar, ilke ve sistem durumu içeren görünümü haklarına sahip ancak bunlar üzerinde değişiklik yapamaz.
- Güvenlik Yöneticisi: Güvenlik okuyucusu aynı görünümü haklarına sahiptir ve bunlar da güvenlik ilkesini güncelleştirebilir ve öneriler ve Uyarıları kapat.



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure İlkesi'nde güvenlik ilkelerini düzenleme öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md): Planlama ve tasarım konuları Azure Güvenlik Merkezi hakkında anlama hakkında bilgi edinin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi için bir kiracı genelinde görünürlük elde](security-center-management-groups.md): Azure Güvenlik Merkezi Yönetim grupları ayarlama konusunda bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../governance/policy/overview.md)
