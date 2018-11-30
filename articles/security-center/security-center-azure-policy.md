---
title: Azure Güvenlik Merkezi güvenlik ilkelerini Azure ilkeleri bir parçası olarak ve Güvenlik Merkezi'nde görüntülenen | Microsoft Docs
description: Bu belge, Azure İlkesi'nde ilkeleri ayarlamak veya bunları Azure Güvenlik Merkezi'nde görüntülemek için yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/25/2018
ms.author: rkarlin
ms.openlocfilehash: 330b66e64484417e50f39c35cf90a6fd62b1e888
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334678"
---
# <a name="view-security-policies"></a>Güvenlik ilkeleri görüntüle

Bu makalede, güvenlik ilkeleri nasıl yapılandırılır ve bunları Güvenlik Merkezi'nde görüntülemek nasıl açıklanmaktadır. Azure Güvenlik Merkezi otomatik olarak atar, [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md) eklendikten her abonelikte. Bunları yapılandırabilirsiniz [Azure İlkesi](../azure-policy/azure-policy-introduction.md), ayrıca sağlayan yönetim grupları arasında ve birden çok aboneliğe ilkeleri ayarlamak.

PowerShell kullanarak ilkeler ayarlama konusunda yönergeler için bkz: [hızlı başlangıç: Azure RM PowerShell modülünü kullanarak uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturma](../azure-policy/assign-policy-definition-ps.md).



## <a name="what-are-security-policies"></a>Güvenlik ilkeleri nedir?
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Azure İlkesi'nde Azure Abonelikleriniz için ilkeler tanımlayın ve bunları iş yükü türüne veya verilerinizin duyarlılığına göre uygun hale getirin. Örneğin, kişisel bilgiler gibi düzenlenen veriler kullanan uygulamalar, diğer iş yükleri yüksek seviyede güvenliği gerektirebilir. Abonelikler arasında ya da Yönetim grupları bir ilke ayarlamak için bunları kümesinde [Azure İlkesi](../azure-policy/azure-policy-introduction.md).



Güvenlik ilkelerinizi size Azure Güvenlik Merkezi'nde güvenlik önerilerini. Uyumluluk, olası zayıflıkları belirlemek ve tehditleri önlemeye yardımcı olmak için onlarla izleyebilirsiniz. Listesini sizin için uygun seçeneği belirleme hakkında daha fazla bilgi için bkz. [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md).

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

Azure İlkesi hakkında daha fazla bilgi için [Uyumluluğu zorlamak için ilke oluşturma ve yönetme](../azure-policy/create-manage-policy.md) konusunu inceleyin.

Bir Azure ilkesi aşağıdaki bileşenlerden oluşur:

- A **ilke** bir kuraldır.
- Bir **girişim** ilkeleri oluşan bir koleksiyondur.
- Bir **atama** bir ilke veya girişim uygulama belirli bir kapsama (Yönetim grubu, abonelik veya kaynak grubu).

Bir kaynak, kendisine atanmış olan ilkelere göre değerlendirilir ve kaynağın uyumlu olduğu ilke sayısına göre bir uyumluluk oranına sahip olur.

## <a name="who-can-edit-security-policies"></a>Güvenlik ilkeleri düzenleyebilecek kişiler
Güvenlik Merkezi, kullanıcıları, grupları ve Azure Hizmetleri için atanan yerleşik roller sağlayan rol tabanlı erişim denetimi (RBAC) kullanır. Kullanıcı Güvenlik Merkezi'ni açtığında erişime sahip oldukları kaynakları ile ilgili bilgiler yalnızca görürler. Kullanıcılar sahibi, katkıda bulunan veya okuyucu rolünün bir kaynak barındıran abonelik veya kaynak grubuna atanan anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- Güvenlik okuyucu: sahip görünümü hakları içerir. öneriler, uyarılar, ilke ve sistem durumu, Güvenlik Merkezi için ancak bunlar üzerinde değişiklik yapamaz.
- Güvenlik Yöneticisi: güvenlik okuyucusu olarak aynı görüntüleme haklarına sahip ve bunlar da güvenlik ilkesini güncelleştirebilir ve öneriler ve Uyarıları kapat.

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Her Azure aboneliği ve'daki yönetim grupları için varsayılan güvenlik ilkesini düzenleyebilirsiniz [Azure İlkesi](../governance/policy/tutorials/create-and-manage.md). Bir güvenlik ilkesini değiştirmek için abonelikte veya ilkeyi içeren yönetim grubunda sahip, katkıda bulunan veya güvenlik yöneticisi rolüne sahip olmanız gerekir.

Azure İlkesi'nde bir güvenlik ilkesi düzenleme hakkında yönergeler için bkz ve [oluşturma ve yönetme uyumluluğu zorlamak için ilke](../governance/policy/tutorials/create-and-manage.md).

## <a name="view-security-policies"></a>Güvenlik ilkeleri görüntüle

Güvenlik Merkezi'nde güvenlik ilkelerinizi görüntüleme:

1. İçinde **Güvenlik Merkezi** panoyu seçin **Güvenlik İlkesi**.

    ![İlke Yönetimi bölmesi](./media/security-center-policies/security-center-policy-mgt.png)

  İçinde **İlkesi Yönetimi** ekran, Yönetim grupları, abonelikleri ve çalışma alanlarının yanı sıra, yönetim grubu yapısı sayısını görebilirsiniz.

  > [!NOTE]
  > - Güvenlik Merkezi panosunu aboneliklerin daha yüksek bir sayı gösterebilir **abonelik kapsamı** altında gösterilen aboneliklerin sayısından **İlkesi Yönetimi**. Abonelik kapsamı Standart, Ücretsiz ve “kapsanmayan” aboneliklerin sayısını gösterir. "Kapsanmayan" abonelik, Güvenlik Merkezi'nin etkin olmayan ve altında görüntülenmez **İlkesi Yönetimi**.
  >

  Tablodaki sütunlar şunları gösterir:

 - **İlke girişimi atama** – Güvenlik Merkezi [yerleşik ilkeleri](security-center-policy-definitions.md) ve bir abonelik veya yönetim grubuna atanmış olan girişim.
 - **Uyumluluk** – genel bir yönetim grubu, abonelik veya çalışma alanı için Uyumluluk puanı. Puan, atamaların ağırlıklı ortalamasıdır. Ağırlıklı ortalama, tek bir atamadaki ilke sayısını ve atamanın uygulandığı kaynak sayısını etkiler.

 Örneğin aboneliğinizde iki VM ve atanmış beş ilkeli bir girişim varsa aboneliğinizde 10 atama olur. VM'lerin biri ilkelerin ikisiyle uyumlu değilse aboneliğinizin genel uyumluluk puanı %80 olur.

 - **Kapsamı** – boş veya yönetim grubu, abonelik veya çalışma çalıştığı standart fiyatlandırma katmanı tanımlar.  Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
 - **Ayarları** – aboneliğiniz bağlantıyı **ayarlarını Düzenle**. Seçme **ayarlarını Düzenle** güncelleştirmenize olanak tanır, [Güvenlik Merkezi Ayarları](security-center-policies-overview.md) her abonelik veya yönetim grubu için.

2. İlkeleri görüntülemek istediğiniz abonelik veya yönetim grubunu seçin.

  - **Güvenlik İlkesi** ekran seçtiğiniz abonelik veya yönetim grubunda atanan ilkeleri tarafından gerçekleştirilecek eylemi yansıtır.
  - En üstünde, her İlkesi'ni açmak için sağlanan bağlantıları kullanabilirsiniz **atama** abonelik veya yönetim grubuna yöneliktir. Atama erişmek ve düzenlemek veya ilkeyi devre dışı bırakmak için bağlantıları kullanabilirsiniz. Örneğin, belirli bir ilke ataması etkin uç nokta koruma engelleme görürseniz, erişim ilkesi ve düzenleyebilir ya da devre dışı bırakmak için bağlantıyı kullanabilirsiniz.
  - İlkeler listesinde, abonelik veya yönetim grubu üzerinde etkili uygulama ilkesi görebilirsiniz. Bu kapsama uygulanan her ilke ayarlarını dikkate alınır ve ilke tarafından hangi eylemlerin, toplu sonucu ile sağlanan anlamına gelir. Örneğin bir atama ilkesi devre dışıdır, ancak başka bir programda Auditıfnotexist için ayarlanmış toplu etkisi Auditıfnotexist geçerlidir. Etkin etkisi her zaman önceliklidir.
  - İlkeleri etkisi olabilir: ekleme, Denetim, AuditIfNotExists, reddetme, Deployıfnotexists, devre dışı bırakıldı. Etkileri nasıl uygulanacağı hakkında daha fazla bilgi için bkz. [anlamak ilke etkileri](../governance/policy/concepts/effects.md).

   ![İlkesi ekranı](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Görüntülersiniz ilkeleri atandığında, birden çok atamaları görebilir ve nasıl her atama, kendi yapılandırıldığını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve ele alma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi için kiracı genelinde görünürlük kazanma](security-center-management-groups.md): Yönetim gruplarını Azure Güvenlik Merkezi için ayarlamayı öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

Azure İlkesi hakkında daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)
