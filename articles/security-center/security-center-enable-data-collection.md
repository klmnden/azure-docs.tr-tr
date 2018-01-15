---
title: "Azure Güvenlik Merkezi veri toplama | Microsoft Docs"
description: " Azure Güvenlik Merkezi'nde veri koleksiyonunu etkinleştirme hakkında bilgi edinin. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2018
ms.author: terrylan
ms.openlocfilehash: d5f2c9960b720fc44f37956f9150e89d6425d154
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="data-collection-in-azure-security-center"></a>Azure Güvenlik Merkezi veri toplama
Güvenlik Merkezi, Azure sanal makineleri (VM'ler) ve güvenlik açıkları ve tehditleri izlemek üzere Azure olmayan bilgisayarları veri toplar. Microsoft izleme çeşitli güvenlikle ilgili yapılandırmaları ve olay günlüklerini makineden okur ve verileri analiz için çalışma alanınızda kopyalar aracısı kullanarak verileri toplanır. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan işlemler, makine adı, IP adresleri, oturum açmış kullanıcı ve kiracı kimliği. Microsoft Monitoring Agent ayrıca alanınıza kilitlenme bilgi döküm dosyaları kopyalar.

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Microsoft Monitoring Agent ' otomatik sağlamayı etkinleştir     
Otomatik sağlama etkinleştirilmişse, Güvenlik Merkezi sağlarken Microsoft Monitoring Agent tüm Azure Vm'leri ve oluşturulan yeni bir tane desteklenir. Otomatik sağlama önemle tavsiye edilir ve Güvenlik Merkezi'nin standart katmanında abonelikler için gereklidir.

> [!NOTE]
> Otomatik sağlama sınırları güvenlik kaynaklarınız için izleme devre dışı bırakılıyor. Daha fazla bilgi için bkz: [otomatik sağlamayı devre dışı](security-center-enable-data-collection.md#disable-automatic-provisioning) bu makalede. Otomatik sağlamayı devre dışı bırakılmış olsa VM disk anlık görüntüler ve yapı toplama etkinleştirilir.
>
>

Microsoft Monitoring Agent ' otomatik sağlamayı etkinleştirmek için:
1. Güvenlik Merkezi ana menüsündeki seçin **Güvenlik İlkesi**.
2. Aboneliği seçin.
3. Altında **Güvenlik İlkesi**seçin **veri toplama**.
4. Altında **Onboarding**seçin **üzerinde** otomatik sağlamayı etkinleştirmek için.
5. **Kaydet**’i seçin.

![Otomatik sağlamayı etkinleştir][1]

## <a name="default-workspace-configuration"></a>Varsayılan çalışma alanı yapılandırması
Güvenlik Merkezi tarafından toplanan verileri günlük analizi çalışma alanları depolanır.  Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanları veya oluşturduğunuz var olan bir çalışma depolanan Vm'lerine toplanan verileri seçebilirler.

Var olan günlük analizi çalışma alanınızı kullanmak için:
- Çalışma alanı, seçili Azure aboneliğiniz ile ilişkilendirilmiş olması gerekir.
- En azından, çalışma alanına erişmek için Okuma izinleriniz olmalıdır.

Varolan bir günlük analizi çalışma alanını seçmek için:

1. Altında **güvenlik ilkesi – veri toplama**seçin **başka bir çalışma alanını kullanın**.

   ![Mevcut çalışma alanını seçin][2]

2. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

> [!NOTE]
> Açılır menü, erişimi ve Azure aboneliğinizde olan çalışma alanları gösterilir.
>
>

3. **Kaydet**’i seçin.
4. Seçtikten sonra **kaydetmek**, izlenen RECONFIGURE VM'ler isteyip istemediğinizi istenir.

   - Seçin **Hayır** yalnızca yeni Vm'lere uygulamak için yeni çalışma alanı ayarları istiyorsanız. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Microsoft izleme aracısı yüklü olmayan yeni bulunan VM'ler.
   - Seçin **Evet** tüm sanal makinelerin uygulamak için yeni çalışma alanı ayarları istiyorsanız. Ayrıca, çalışma alanı oluşturulduğunda Güvenlik Merkezi'ne bağlı her VM için yeni hedef çalışma alanına bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, yeni hedef çalışma alanına tüm sanal makineleri yeniden bağlanması kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanları silmemelisiniz. Bir çalışma alanı çok erken silinirse, bu işlem başarısız olur.
   >
   >

   - Seçin **iptal** işlemi iptal etmek için.

   ![Mevcut çalışma alanını seçin][3]

## <a name="data-collection-tier"></a>Veri toplama katmanı
Güvenlik Merkezi, araştırma, Denetim ve tehdit algılama için yeterli olayları korurken, olayı azaltabilir. Aracı tarafından toplanacak olayların dört kümelerinden çalışma alanları ve abonelikler için ilke filtreleme sağa seçebilirsiniz.

- **Tüm olayları** – toplanan tüm olayları emin olmak için isteyen müşteriler için. Varsayılan değer budur.
- **Ortak** – müşterilerin çoğu karşılayan ve tam denetim izi sağlayan bir dizi olayları budur.
- **En az** – olayları olay birimin en aza indirmek isteyen müşteriler için daha küçük bir dizi.
- **Hiçbiri** – güvenlik ve App Locker Günlükleri güvenlik olay toplama devre dışı bırakın. Bu seçeneği olan müşteriler için kendi güvenlik Panolar yalnızca Windows Güvenlik Duvarı günlüklerini ve kötü amaçlı yazılımdan koruma, temel ve güncelleştirme gibi öngörülü değerlendirmeleri sahiptir.

> [!NOTE]
> Bu ayarlar, tipik senaryolar için tasarlanmıştır. Hangisinin çözümü uygulamadan önce ihtiyaçlarınıza uygun değerlendirmek emin olun.
>
>

Ait olayları belirlemek için **ortak** ve **en az** olay kümeleri, biz çalışılan müşteriler ve endüstri standartları filtrelenmemiş sıklığını her olay ve kullanımı hakkında bilgi edinmek için. Bu işlemde aşağıdaki yönergeleri kullandık:

- **En az** -Bu, başarılı bir ihlal gösterebilir olayları ve çok düşük bir biriminiz önemli olayları kapsadığından emin olun. Örneğin, kullanıcı başarılı ve başarısız oturum açma (olay kimlikleri 4624 4625) bu küme içeriyor, ancak denetlemek için önemli, ancak algılama için anlamlı ve görece yüksek hacimli içeren oturum kapatma içermiyor. Bu kümesinin veri hacmi çoğunu, oturum açma olayları ve işlem oluşturma olayı (olay kimliği 4688).
- **Ortak** -tam kullanıcı denetim izi bu kümesindeki sağlayın. Örneğin, bu küme, kullanıcı oturumu ve kullanıcı oturumu (olay kimliği 4634) içerir. Güvenlik grubu değişikliklerini, anahtar etki alanı denetleyicisi Kerberos işlemleri ve endüstri kuruluşlar tarafından önerilen diğer olaylar gibi eylemleri denetim içerir.

Çok düşük birim olayları birimi küçültmek ve belirli olayları da filtre olduğu tüm olayları üzerinden seçmek için ana motivasyon yap ortak eklendi.

Güvenlik ve App Locker olay kimlikleri her küme için tam bir dökümünü şöyledir:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En az | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Ortak (varsayılan) | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> Grup İlkesi nesnesi (GPO) kullanıyorsanız, denetim ilkeleri işlem oluşturma olay 4688 etkinleştirmeniz önerilir ve *CommandLine* olay 4688 içinde alan. İşlem oluşturma olay 4688 hakkında daha fazla bilgi için bkz: Güvenlik Merkezi'nin [SSS](security-center-faq.md#what-happens-when-data-collection-is-enabled). Denetim ilkeleri bunlar hakkında daha fazla bilgi için bkz: [Denetim İlkesi önerileri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
>
>

Filtreleme ilkeniz seçmek için:
1. Üzerinde **güvenlik ilkesi & ayarları** dikey penceresinde filtreleme ilkesi altında seçin **güvenlik olaylarını**.
2. **Kaydet**’i seçin.

   ![İlke filtreleme seçin][5]

## <a name="disable-automatic-provisioning"></a>Otomatik sağlamayı devre dışı bırak
Otomatik kaynaklardan herhangi bir zamanda Güvenlik İlkesi'nde bu ayarı devre dışı bırakarak sağlamayı devre dışı bırakabilirsiniz. Otomatik sağlama güvenlik uyarıları ve sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve endpoint protection hakkında öneriler alabilmek için önerilir.

> [!NOTE]
> Otomatik sağlamayı devre dışı bırakma, Microsoft Monitoring Agent aracı bir yere sağlanan Azure VM'lerin kaldırmaz.
>
>

1. Güvenlik Merkezi ana menüye dönmek ve güvenlik ilkesini seçin.

   ![Otomatik sağlamayı devre dışı bırak][6]

2. Otomatik sağlamayı devre dışı bırakmak istediğiniz aboneliği seçin.
3. Üzerinde **güvenlik ilkesi – veri toplama** dikey altında **Onboarding** seçin **kapalı** otomatik sağlamayı devre dışı bırakmak için.
4. **Kaydet**’i seçin.  

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede gösterilen, nasıl veri toplama ve Güvenlik Merkezi çalışır otomatik sağlama. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verilerin yönetilmesi ve diğer Güvenlik Merkezi'nde korunması nasıl öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-automatic-provisioning.png
