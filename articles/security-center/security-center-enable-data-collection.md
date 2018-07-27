---
title: Azure Güvenlik Merkezi'nde veri toplamayı | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde veri toplamayı etkinleştirmeyi öğrenin. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: rkarlin
ms.openlocfilehash: a5151d1f9498b29c79638445a58a8337abff8961
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281931"
---
# <a name="data-collection-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde veri toplamayı
Güvenlik Merkezi, Azure sanal makineleri (VM'ler) ve Azure harici bilgisayarları güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan işlemler, makine adı, IP adresleri, oturum açmış kullanıcı, AppLocker olaylarını ve Kiracı kimliği Microsoft Monitoring Agent, ayrıca kilitlenme bilgi dökümü dosyalarını çalışma alanınıza kopyalar.

> [!NOTE]
> İçin veri toplamayı etkinleştirmek için [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md), Güvenlik Merkezi, tüm uygulamalara izin vermek üzere Denetim modunda yerel bir AppLocker ilkesini yapılandırır. Bu, daha sonra toplanan ve Güvenlik Merkezi tarafından kullanılabilir olaylar oluşturmak AppLocker neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 
>

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Microsoft Monitoring Agent'ın otomatik sağlamayı etkinleştirme     
Otomatik sağlama varsayılan olarak kapalıdır. Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi Microsoft Monitoring Agent'ı tüm Azure Vm'lere ve oluşturulan tüm yeni vm'lere desteklenen hazırlar. Otomatik sağlama önemle tavsiye edilir ancak el ile aracı yüklemelerini da kullanılabilir. [Microsoft Monitoring Agent uzantısını nasıl yükleyeceğiniz öğrenin](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension).

> [!NOTE]
> - Otomatik sağlamanın devre dışı bırakılması, kaynaklarınızın güvenliğinin izlenmesini kısıtlar. Daha fazla bilgi için bkz. [otomatik sağlamayı devre dışı bırak](security-center-enable-data-collection.md#disable-automatic-provisioning) bu makaledeki. Otomatik sağlamayı devre dışı olsa bile, VM diski anlık görüntüleri ve yapıt toplama etkinleştirilir.
>

Microsoft Monitoring Agent için otomatik sağlamayı etkinleştirmek üzere:
1. Güvenlik Merkezi ana menüsünde seçin **Güvenlik İlkesi**.
2. Aboneliği seçin.

  ![Abonelik seçme][7]

3. **Güvenlik ilkesi** bölümünde **Veri Toplama**’yı seçin.
4. Altında **otomatik sağlama**seçin **üzerinde** otomatik sağlamayı etkinleştirmek için.
5. **Kaydet**’i seçin.

  ![Otomatik sağlamayı etkinleştirme][1]

## <a name="default-workspace-configuration"></a>Varsayılan çalışma alanı yapılandırması
Güvenlik Merkezi tarafından toplanan veriler, Log Analytics çalışma Alanlarınızda depolanır.  Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında veya kendi oluşturduğunuz mevcut bir çalışma depolanan vm'lerden toplanan verileri seçebilirsiniz.

Mevcut bir Log Analytics çalışma alanınızı kullanmak için:
- Çalışma alanı, seçili Azure aboneliğiniz ile ilişkilendirilmesi gerekir.
- En azından, çalışma alanına erişmek için Okuma izinlerine sahip olmalıdır.

Mevcut bir Log Analytics çalışma alanı seçmek için:

1. Altında **varsayılan çalışma alanı yapılandırması**seçin **başka bir çalışma alanını kullanabilmesini**.

   ![Mevcut bir çalışma alanı seçin][2]

2. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

  > [!NOTE]
  > Tüm aboneliklerinizi tüm çalışma alanları açılır menü, kullanılabilir. Bkz: [çapraz abonelik çalışma alanı seçimi](security-center-enable-data-collection.md#cross-subscription-workspace-selection) daha fazla bilgi için.
  >
  >

3. **Kaydet**’i seçin.
4. Seçtikten sonra **Kaydet**, izlenen Vm'leri yeniden yapılandırmak istiyorsanız istenir.

   - Seçin **Hayır** yalnızca yeni Vm'lere uygulamak için yeni çalışma alanı ayarlarını istiyorsanız. Yeni çalışma alanı ayarları, yalnızca yeni aracı yüklemelerini için geçerlidir; Microsoft Monitoring Agent yüklüyse olmayan yeni bulunmuş VM'ler.
   - Seçin **Evet** tüm sanal makinelere uygulamak için yeni çalışma alanı ayarlarını istiyorsanız. Ayrıca, çalışma alanı oluşturulduğunda bir güvenlik Merkezi'ne bağlı her bir VM yeni hedef çalışma alanına bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, tüm sanal makineler yeni hedef çalışma alanına bağlandı kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmemelisiniz. Bir çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - Seçin **iptal** işlemi iptal etme.

     ![Mevcut bir çalışma alanı seçin][3]

## <a name="cross-subscription-workspace-selection"></a>Çapraz abonelik çalışma alanı seçimi
Tüm aboneliklerinizi tüm çalışma alanları, verilerinizi depolamak için bir çalışma alanı seçtiğinizde kullanılabilir. Çapraz abonelik, farklı Aboneliklerde çalışan sanal makinelerden veri toplama ve tercih ettiğiniz çalışma alanında depolamak çalışma alanı seçimi sağlar. Bu özellik, Linux ve Windows çalıştıran her iki sanal makineler için çalışır.

> [!NOTE]
> Çapraz abonelik çalışma alanı seçimi Azure Güvenlik Merkezi'nin ücretsiz katmanı bir parçasıdır. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

## <a name="data-collection-tier"></a>Veri koleksiyonu katmanı
Güvenlik Merkezi, araştırma, Denetim ve tehdit algılama için yeterli olayları korurken olayların hacmine azaltabilir. Aracı tarafından toplanacak olayların dört kümelerinden çalışma alanları ve abonelikler için ilke filtreleme sağ seçebilirsiniz.

- **Tüm olaylar** – toplanan tüm olayları emin olmak isteyen müşteriler için. Bu varsayılan değerdir.
- **Ortak** – tam denetim kaydı sağlar ve müşterilerin çoğu karşılayan bir dizi olay budur.
- **En az** – olayları olay birimi en aza indirmek isteyen müşteriler için daha küçük bir dizi.
- **Hiçbiri** – güvenlik ve App Locker Günlükleri güvenlik olay toplama devre dışı bırakın. Bu seçeneği müşteriler, yalnızca Windows Güvenlik duvarı günlükleri ve proaktif değerlendirmeleri kötü amaçlı yazılımdan koruma, temel ve güncelleştirme gibi güvenlik panolarına sahip.

> [!NOTE]
> Bu güvenlik olayları kümeleri yalnızca Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
Bu ayarlar, tipik senaryoları için tasarlanmıştır. Uygulamadan önce gereksinimlerinize hangisinin uyduğunu değerlendirmek üzere emin olun.
>
>

Ait olayları belirlemek üzere **ortak** ve **Minimal** olay kümeleri çalıştık filtrelenmemiş sıklığını her olay ve kullanımları hakkında bilgi edinmek için müşteriler ve endüstri standartları. Bu işlem aşağıdaki yönergeleri kullandık:

- **En az** -bu küme yalnızca başarılı ihlal gösterebilir olayları ve çok düşük bir birime sahip önemli olayları kapsadığından emin olun. Örneğin, bu kullanıcının başarılı ve başarısız oturum açma (olay kimliği 4624 4625) içerir, ancak denetim için önemlidir, ancak algılama için anlamlı ve görece yüksek hacimli olan oturum kapatma içermiyor. Bu veri hacmini çoğunu, oturum açma olayları ve işlem oluşturma olayı (olay kimliği 4688) olur.
- **Ortak** -bu kümesindeki bir tam kullanıcı denetim kaydı sağlar. Örneğin, bu ayarla, hem kullanıcı oturum açma bilgileri ve kullanıcı oturum kapatma (olay kimliği 4634) içerir. Güvenlik grubu değişikliklerini, anahtar etki alanı denetleyicisi Kerberos işlemleri ve sektör kuruluşlar tarafından önerilen diğer olaylar gibi eylemleri denetimi ekliyoruz.

Tüm olayları üzerine miktarının azaltılmasını ve belirli olay filtre için olduğunu seçin için ana motivasyon olarak ortak çok düşük bir birime sahip olayları dahil edilmişti.

Güvenlik ve App Locker olay kimlikleri her küme için tam bir dökümü aşağıda verilmiştir:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En az | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Grup İlkesi nesnesi (GPO) kullanıyorsanız, denetim ilkeleri işlem oluşturma olay 4688 etkinleştirmeniz önerilir ve *CommandLine* olay 4688 içindeki alan. İşlem oluşturma olay 4688 hakkında daha fazla bilgi için bkz: Güvenlik Merkezi'nin [SSS](security-center-faq.md#what-happens-when-data-collection-is-enabled). Denetim ilkeleri bunlar hakkında daha fazla bilgi için bkz: [Denetim İlkesi önerileri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  İçin veri toplamayı etkinleştirmek için [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md), Güvenlik Merkezi, tüm uygulamalara izin vermek üzere Denetim modunda yerel bir AppLocker ilkesini yapılandırır. Bu, daha sonra toplanan ve Güvenlik Merkezi tarafından kullanılabilir olaylar oluşturmak AppLocker neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 
>

Filtreleme ilkenizi seçmek için:
1. Üzerinde **güvenlik ilkesinde veri toplamayı** dikey penceresinde, filtreleme ilkesini altında seçin **güvenlik olaylarını**.
2. **Kaydet**’i seçin.

   ![Filtreleme ilkesi seçin][5]

## <a name="disable-automatic-provisioning"></a>Otomatik sağlamayı devre dışı bırak
Otomatik kaynaklardan herhangi bir zamanda bu güvenlik ilkesi ayarı devre dışı bırakarak sağlama devre dışı bırakabilirsiniz. Otomatik sağlama güvenlik uyarıları ve sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve uç nokta koruma hakkında öneriler almak için kesinlikle önerilir.

> [!NOTE]
> Otomatik sağlama devre dışı bırakıldığında Microsoft Monitoring Agent’ın sağlandığı Azure VM’lerinden aracı kaldırılmaz.
>
>

1. Güvenlik Merkezi ana menüsüne geri dönün ve Güvenlik İlkesi'ni seçin.
2. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin.
3. Üzerinde **güvenlik ilkesi – veri toplama** dikey altında **otomatik sağlama** seçin **kapalı**.
4. **Kaydet**’i seçin.

  ![Otomatik sağlamayı devre dışı bırak][6]

Otomatik sağlama (Kapalı) devre dışı bırakıldığında, varsayılan çalışma alanı yapılandırma bölümü görüntülemez.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede gösterilen, nasıl veri toplama ve otomatik sağlama Güvenlik Merkezi çalışır. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md) -verileri nasıl yönetildiği ve korunduğu Güvenlik Merkezi'nde öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
