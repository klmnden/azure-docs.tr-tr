---
title: Azure güvenlik ve uyumluluk şeması FedRAMP Web uygulamaları, Otomasyon - sistem ve bilgi tutarlılığı
description: FedRAMP Web uygulamaları Otomasyon - sistem ve bilgi tutarlılığı
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 2ff2778b-2c37-41b5-a39c-6594b3e3b10b
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 0eca3c82aea287f6582bd56574512dce5e8e86c7
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="system-and-information-integrity-si"></a>Sistem ve bilgilerin bütünlüğünü (sı)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-si-1"></a>NIST 800 53 denetim SI-1

#### <a name="system-and-information-integrity-policy-and-procedures"></a>Sistem ve bilgi bütünlüğü İlkesi ve yordamları

**SI-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim taahhüt bir sistem ve bilgi bütünlüğü İlkesi Kurumsal varlıklar ve uyumluluk arasında koordinasyon; ve sistem bilgi bütünlüğü ilkesini ve ilişkili sistem ve bilgi tutarlılığı denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli sistem ve bilgi bütünlüğü İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve sistem ve bilgi tutarlılığı yordamları [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde sistem ve bilgi bütünlüğü ilke ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-2a"></a>NIST 800 53 denetim SI-2.a

#### <a name="flaw-remediation"></a>Kusur düzeltme

**SI 2.a** kuruluş tanımlar, raporları ve bilgileri sistem açıkları düzeltir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması bu mimaride dağıtılan Windows sanal makineleri için güncelleştirmeleri durumunu izlemek için otomasyon ve denetim çözümü dağıtır. Panodan kusur düzeltme durumu dağıtılan tüm Windows sunucuları için güncelleştirme yönetimi kutucuğu görüntüler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-2b"></a>NIST 800 53 denetim SI-2.b

#### <a name="flaw-remediation"></a>Kusur düzeltme

**SI 2.b** kuruluş verimliliğini ve olası yan etkileri yüklemeden önce kusur düzeltme ilgili yazılım ve bellenim güncelleştirmeleri test eder.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, verimliliğini ve müşteri tarafından dağıtılan kaynaklar yüklemeden önce olası yan etkileri kusur düzeltme ilgili güncelleştirmeleri test etmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-2c"></a>NIST 800 53 denetim SI-2.c

#### <a name="flaw-remediation"></a>Kusur düzeltme

**SI 2.c** kuruluş içinde ilgili güvenlik yazılımı ve bellenim güncelleştirmeleri yükler [atama: kuruluş tanımlı süre] güncelleştirme sürümü.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca güncelleştirme dağıtımları düzeltme ekleri gerektiğinde Windows sunucularına dağıtmak için oluşturulabileceği otomasyon ve denetim çözümü dağıtır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-2d"></a>NIST 800 53 denetim SI-2.d

#### <a name="flaw-remediation"></a>Kusur düzeltme

**SI 2.d** kuruluş Kurumsal yapılandırma yönetimi işlemine kusur düzeltme içerir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, kusur düzeltme yapılandırma yönetimi olarak dahil etmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-2-1"></a>NIST 800 53 denetim SI-2 (1)

#### <a name="flaw-remediation--central-management"></a>Kusur düzeltme | Merkezi Yönetim

**SI-2 (1)** kuruluşun merkezi olarak kusur düzeltme işlemini yönetir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması bu mimaride dağıtılan Windows sanal makineleri için güncelleştirmeleri durumunu izlemek için otomasyon ve denetim çözümü dağıtır. Panodan kusur düzeltme durumu dağıtılan tüm Windows sunucuları için güncelleştirme yönetimi kutucuğu görüntüler. Güncelleştirme dağıtımları, düzeltme ekleri gerektiğinde Windows sunucularına dağıtmak için oluşturulabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-2-2"></a>NIST 800 53 denetim SI-2 (2)

#### <a name="flaw-remediation--automated-flaw-remediation-status"></a>Kusur düzeltme | Otomatik kusur düzeltme durumu

**SI-2 (2)** kuruluş otomatik mekanizmaları kullanan [atama: kuruluş tarafından tanımlanan sıklığı] kusur düzeltme ilgili bilgileri sistem bileşenlerinin durumunu belirlemek için.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması bu mimaride dağıtılan Windows sanal makineleri için güncelleştirmeleri durumunu izlemek için otomasyon ve denetim çözümü dağıtır. Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API’si çağrılarak son güncelleştirme zamanı sorgulanır; böylelikle durumun değişip değişmediği saptanır ve değişmişse bir uyumluluk taraması başlatılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-2-3a"></a>NIST 800 53 SI-2 (3) bir denetim

#### <a name="flaw-remediation--time-to-remediate-flaws--benchmarks-for-corrective-actions"></a>Kusur düzeltme | Açıkları düzeltmek için zaman / Kıyaslama noktaları için düzeltme eylemleri

**SI-2 (3) bir** kuruluş kusur tanımlama ve kusur düzeltme arasındaki süreyi ölçer.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde düzelterek açıkları sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-2-3b"></a>NIST 800 53 SI-2 (3) .b denetleme

#### <a name="flaw-remediation--time-to-remediate-flaws--benchmarks-for-corrective-actions"></a>Kusur düzeltme | Açıkları düzeltmek için zaman / Kıyaslama noktaları için düzeltme eylemleri

**SI-2 (3) .b** kuruluş kurar [atama: kuruluş tarafından tanımlanan kıyaslamaları] sorunu düzeltecek eylemleri gerçekleştirmek için.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri kusur düzeltme işlemleri için kuruluş düzeyi kıyaslamaları bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-3a"></a>NIST 800 53 denetim SI-3.a

#### <a name="malicious-code-protection"></a>Kötü amaçlı kod koruma

**SI 3.a** kötü amaçlı kod koruma mekanizmalarını noktalarda Algıla ve kötü amaçlı kod yok bilgileri sistem giriş ve çıkış kuruluş kullanır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tüm dağıtılan Windows sanal Microsoft Antimalware sanal makine uzantısı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-3b"></a>NIST 800 53 denetim SI-3.b

#### <a name="malicious-code-protection"></a>Kötü amaçlı kod koruma

**SI 3.b** yeni sürümler Kuruluş yapılandırması yönetim ilkesi ve yordamları uygun olarak kullanılabilir olduğunda kuruluş kötü amaçlı kod koruma mekanizmalarını güncelleştirir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tüm dağıtılan Windows sanal Microsoft Antimalware sanal makine uzantısı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. Bu uzantı, kötü amaçlı yazılımdan koruma altyapısı ve koruma imzaları kullanılabilir hale yayın otomatik olarak güncelleştirmek için yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-3c"></a>NIST 800 53 denetim SI-3.c

#### <a name="malicious-code-protection"></a>Kötü amaçlı kod koruma

**SI 3.c** kuruluş bilgi sisteminin düzenli taramalar gerçekleştirmek için kötü amaçlı kod koruma mekanizmalarını yapılandırır [atama: kuruluş tarafından tanımlanan sıklığı] ve dış kaynaklardan dosyaların gerçek zamanlı tarama [seçimi (tek veya Daha fazla); uç nokta; ağ giriş/çıkış noktaları] indirilen dosyaları gibi açılamıyor veya kuruluş güvenliği politikasının uygun olarak yürütülen; ve [seçimi (bir veya daha fazla): kötü amaçlı kod engelle; kötü amaçlı kod; yönetici; gönderme uyarısını karantinaya Al [Atama: kuruluş tarafından tanımlanan eylem]] kötü amaçlı kod algılama yanıt olarak.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tüm dağıtılan Windows sanal Microsoft Antimalware sanal makine uzantısı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. Bu uzantı (haftalık) gerçek zamanlı ve düzenli taramalar gerçekleştirmek, otomatik olarak kötü amaçlı yazılımdan koruma altyapısı ve koruma imzalarını güncelleştirmesi ve otomatik düzeltme eylemleri gerçekleştirmek için yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-3d"></a>NIST 800 53 denetim SI-3.d

#### <a name="malicious-code-protection"></a>Kötü amaçlı kod koruma

**SI 3.d** kötü amaçlı kod algılama ve eradication ve sonuçta elde edilen olası etkisini bilgileri sistem kullanılabilirliğini sırasında hatalı pozitif sonuç alındığını kuruluş giderir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, kötü amaçlı kod karşı korumak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-3-1"></a>NIST 800 53 denetim SI-3 (1)

#### <a name="malicious-code-protection--central-management"></a>Kötü amaçlı kod koruma | Merkezi Yönetim

**SI-3 (1)** kuruluş kötü amaçlı kod koruma mekanizmalarını merkezi olarak yönetir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tüm dağıtılan Windows sanal Microsoft Antimalware sanal makine uzantısı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. Azure kötü amaçlı yazılımdan koruma çözümünü geçerli durumunu gözden geçirmek için merkezi bir özellik sunar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-3-2"></a>NIST 800 53 denetim SI-3 (2)

#### <a name="malicious-code-protection--automatic-updates"></a>Kötü amaçlı kod koruma | Otomatik Güncelleştirmeler

**SI-3 (2)** bilgi sistemi kötü amaçlı kod koruma mekanizmalarını otomatik olarak güncelleştirir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tüm dağıtılan Windows sanal Microsoft Antimalware sanal makine uzantısı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. Bu uzantı, kötü amaçlı yazılımdan koruma altyapısı ve koruma imzaları kullanılabilir hale yayın otomatik olarak güncelleştirmek için yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-3-7"></a>NIST 800 53 denetim SI-3 (7)

#### <a name="malicious-code-protection--nonsignature-based-detection"></a>Kötü amaçlı kod koruma | Nonsignature tabanlı algılama

**SI-3 (7)** bilgi sistemi nonsignature tabanlı kötü amaçlı kod algılama mekanizmaları uygular.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tüm dağıtılan Windows sanal Microsoft Antimalware sanal makine uzantısı kullanılarak uygulanan makineler için kötü amaçlı yazılımdan koruma ana bilgisayar tabanlı korumalar dağıtır. Bu uzantı, sezgisel algılama gerçekleştirmek için yapılandırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4a"></a>NIST 800 53 denetim SI-4.a

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.a** kuruluş saldırıları ve göstergeleri ile uyumlu olarak olası saldırıları algılamak için bilgi sistemi izler [atama: kuruluş tanımlı hedeflerini izleme]; ve yetkisiz yerel, ağ ve Uzaktan bağlantılar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı günlük analizi ve güvenlik ve denetim çözümü dağıtır. Bu çözüm güvenlik yaklaşımı, saldırıları ve olası saldırılara karşı göstergelerini kapsamlı bir görünümünü sağlar. Güvenlik ve Denetim Panosu, dağıtılan yönetim çözümleri arasında kullanılabilir veri kullanılarak dağıtılan kaynakların güvenlik durumu üst düzey bir anlayış sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4b"></a>NIST 800 53 denetim SI-4.b

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.b** kuruluş bilgi sistemi yetkisiz kullanımını tanımlayan [atama: kuruluş tarafından tanımlanan teknikleri ve yöntemleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması güvenlik ve denetim çözümü dağıtır. Tanımlayın ve erişim etki alanı başarısız oturum açma girişimleri sayısı ve oturum açmış geçerli hesapları sayısı gibi bilgiler sistem kimlik durumu özetini içeren bir Pano sağlar. Bu Panoda kullanılabilir bilgiler potansiyel şüpheli Etkinlik Kimliği'nde yardımcı olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4c"></a>NIST 800 53 denetim SI-4.c

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.c** kuruluş izleme aygıtlar önemli bilgiler; kuruluşunuz tarafından belirlenen toplamak üzere stratejik bilgi sistemi içinde ve geçici konumlara hareketlerinin belirli türlerdeki izlemek için sistem içinde dağıtır. Kuruluş ilgilendiren.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı günlük analizi ve güvenlik ve denetim çözümü dağıtır. Güvenlik ve Denetim Panosu veri kullanılabilir VM işletim sistemi izleme verilerini bir anlayış dahil olmak üzere, dağıtılan yönetim çözümleri kullanılarak dağıtılan kaynakların güvenlik durumu üst düzey bir anlayış sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4d"></a>NIST 800 53 denetim SI-4.d

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.d** kuruluş izinsiz izleme alınan bilgileri korur yetkisiz erişim, değiştirilmesi ve silinmesini araçlarından.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Yetkisiz erişim, değiştirilmesi ve silinmesini bu şeması içindeki izleme bilgileri korumak için kullanılan mantıksal erişimi denetler. Azure Active Directory rol tabanlı Grup üyeliklerini kullanma onaylanan mantıksal erişimini zorunlu kılar. İzleme bilgilerini görüntülemek ve izleme araçları kullanmak için özelliği bu izni gerektiren kullanıcılar sınırlı olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4e"></a>NIST 800 53 denetim SI-4.e

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.e** Kuruluş bilgileri sistem kuruluş operations ve varlıklar, kişiler, diğer kuruluşların veya yasalar zorlama bilgilere göre Ulus riskinin belirtisi olduğunda Etkinlik izleme düzeyini getirilmesini sağlayan, Bilgileri'ni veya güvenilir diğer bilgi kaynakları.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları izlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4f"></a>NIST 800 53 denetim SI-4.f

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.f** kuruluş federal yasaların, Executive siparişler, yönergeleri, ilkeleri veya yönetmeliklere uygun olarak etkinlikleri izleme bilgileri sistem ilgili yasal fikir alır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları izlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-4g"></a>NIST 800 53 denetim SI-4.g

#### <a name="information-system-monitoring"></a>Sistem bilgileri izleme

**SI 4.g** kuruluş sağlar [atama: kuruluş tarafından tanımlanan bilgileri sistem izleme bilgilerini] için [atama: kuruluş tarafından tanımlanan personel ya da roller] [seçimi (bir veya daha fazla): gerektiğinde; [Atama: kuruluş tarafından tanımlanan sıklığı]].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları izlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-1"></a>NIST 800 53 denetim SI-4 (1)

#### <a name="information-system-monitoring--system-wide-intrusion-detection-system"></a>Sistem izleme bilgilerini | Sistem genelinde izinsiz giriş algılama sistem

**SI-4 (1)** kuruluş bağlanır ve tek tek izinsiz giriş algılama araçları bilgileri sistem genelinde izinsiz giriş algılama sisteme yapılandırır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları izlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-2"></a>NIST 800 53 denetim SI-4 (2)

#### <a name="information-system-monitoring--automated-tools-for-real-time-analysis"></a>Sistem izleme bilgilerini | Gerçek zamanlı analiz için otomatik araçları

**SI-4 (2)** kuruluş gerçek zamanlı analiz olayların desteklemek için otomatik araçları kullanır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı günlük analizi ve güvenlik ve denetim çözüm de dahil olmak üzere çeşitli yönetim çözümleri dağıtır. Günlük analizi gerçek zamanlı analiz olayların dağıtılan kaynaklar sağlar. Yönetim çözümleri çözüm etki alanları arasında güvenlik tutumunu kapsamlı bir görünümünü sağlar. Günlük analizi dağıtılan yönetim çözümleri arasında kullanılabilir veri kullanılarak dağıtılan kaynakların güvenlik durumu hakkında bilgi sağlar. Günlük analizi tanımlanan ölçütlere göre uyarıları oluşturmak için yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-4"></a>NIST 800 53 denetim SI-4 (4)

#### <a name="information-system-monitoring--inbound-and-outbound-communications-traffic"></a>Sistem izleme bilgilerini | Gelen ve giden iletişim trafiği

**SI-4 (4)** bilgi sistemi izler gelen ve giden iletişim trafiğini [atama: kuruluş tarafından tanımlanan sıklığı] olağan dışı ya da yetkisiz etkinlikleri veya koşulların.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakları izlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-5"></a>NIST 800 53 denetim SI-4 (5)

#### <a name="information-system-monitoring--system-generated-alerts"></a>Sistem izleme bilgilerini | Sistem tarafından oluşturulan uyarılar

**SI-4 (5)** bilgileri sistem uyarıları [atama: kuruluş tarafından tanımlanan personel ya da roller] risk veya olası riski belirtebilen aşağıdaki belirtileri ortaya çıktığında: [atama: kuruluş tarafından tanımlanan güvenliğinin aşılmasına göstergeleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması güvenlik ve denetim çözümü dahil olmak üzere çeşitli yönetim çözümleri dağıtır. Günlük analizi gerçek zamanlı analiz olayların dağıtılan kaynaklar sağlar. Yönetim çözümleri çözüm etki alanları arasında güvenlik tutumunu kapsamlı bir görünümünü sağlar. Günlük analizi tanımlanan ölçütlere göre uyarıları oluşturmak için yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-11"></a>NIST 800 53 denetim SI-4 (11)

#### <a name="information-system-monitoring--analyze-communications-traffic-anomalies"></a>Sistem izleme bilgilerini | İletişim trafiği anormallikleri Çözümle

**SI-4 (11)** kuruluşun dış sınır bilgileri sisteminin giden iletişimler trafiğini analiz eder ve seçili [atama: kuruluş tarafından tanımlanan iç işaret (örneğin, alt ağlar, alt sistemleri) sisteminde] için anormallikleri bulma.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için iletişim trafiği anormallikleri çözümlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-14"></a>NIST 800 53 denetim SI-4 (14)

#### <a name="information-system-monitoring--wireless-intrusion-detection"></a>Sistem izleme bilgilerini | Kablosuz izinsiz giriş algılama

**SI-4 (14)** dolandırıcı kablosuz cihazlar tanımlamak ve saldırı algılamak için kablosuz izinsiz giriş algılama sistem dener kuruluş kullanır ve olası güvenlik ihlalleri/ihlallerine bilgi sistemi.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Kablosuz cihazlar dahil olmak üzere müşteri tarafından denetlenen donanım, Azure veri merkezlerinde izin verilir. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure düzenli olarak üç aylık olarak yanlış kablosuz sinyalleri için AC-18'de anlatıldığı gibi izler. <br /> Microsoft Azure PaaS ve Iaas müşteriler adına bu denetimi uygular. |


 ### <a name="nist-800-53-control-si-4-16"></a>NIST 800 53 denetim SI-4 (16)

#### <a name="information-system-monitoring--correlate-monitoring-information"></a>Sistem izleme bilgilerini | İzleme bilgilerini ilişkilendirmek

**SI-4 (16)** kuruluş araçları bilgileri sistem genelinde değişiklik izleme bilgilerinin karşılık gelen.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı günlük analizi ve güvenlik ve denetim çözüm de dahil olmak üzere çeşitli yönetim çözümleri dağıtır. Günlük analizi dağıtılan yönetim çözümleri arasında kullanılabilir veri kullanılarak dağıtılan kaynakların güvenlik durumu hakkında bilgi sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-18"></a>NIST 800 53 denetim SI-4 (18)

#### <a name="information-system-monitoring--analyze-traffic--covert-exfiltration"></a>Sistem izleme bilgilerini | Trafiğini analiz / gizli Exfiltration

**SI-4 (18)** kuruluşun dış sınır bilgileri sisteminin (yani, sistem çevre) hem de giden iletişimler trafiğinin çözümler [atama: kuruluş tarafından tanımlanan iç işaret (örneğin, alt sistemleri, sistem içinde bilgilerin gizli exfiltration algılamak için alt ağlar)].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için iletişim trafiğini çözümlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-19"></a>NIST 800 53 denetim SI-4 (19)

#### <a name="information-system-monitoring--individuals-posing-greater-risk"></a>Sistem izleme bilgilerini | Kişiler büyük riski taşıyor

**SI-4 (19)** kuruluş uygular [atama: kuruluş tarafından tanımlanan ek izleme] tarafından tanımlanan kişilerin [atama: kuruluş tarafından tanımlanan kaynakları] risk daha yüksek bir düzeyde taşıyor olarak.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, büyük bir riski kişiler izlemekten sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-20"></a>NIST 800 53 denetim SI-4 (20)

#### <a name="information-system-monitoring--privileged-users"></a>Sistem izleme bilgilerini | Ayrıcalıklı kullanıcılar

**SI-4 (20)** kuruluş Implements [atama: kuruluş tarafından tanımlanan ek izleme] ayrıcalıklı kullanıcı.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, ayrıcalıklı kullanıcıların izlemekten sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-22"></a>NIST 800 53 denetim SI-4 (22)

#### <a name="information-system-monitoring--unauthorized-network-services"></a>Sistem izleme bilgilerini | Yetkisiz Ağ Hizmetleri

**SI-4 (22)** yetkili edilmemiş veya tarafından onaylanan Ağ Hizmetleri bilgi sistemi algılar [atama: kuruluş tarafından tanımlanan yetkilendirme veya onay işlemleri] ve [seçimi (bir veya daha fazla): denetler; uyarıları [atama: "kuruluş tarafından tanımlanan personel veya rolleri]].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yetkisiz Ağ Hizmetleri algılamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-23"></a>NIST 800 53 denetim SI-4 (23)

#### <a name="information-system-monitoring--host-based-devices"></a>Sistem izleme bilgilerini | Ana bilgisayar tabanlı cihazlar

**SI-4 (23)** kuruluş uygular [atama: kuruluş tanımlı ana bilgisayar tabanlı izleme mekanizmaları] konumundaki [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması dağıtılan kaynakları ana bilgisayar tabanlı izleme yeteneklerini verileri de dahil olmak üzere, izleme verilerini toplar. Microsoft Monitoring Agent, tüm Windows sanal makinelerde günlük analizi ve diğer yönetim çözümleri tarafından kullanılan izleme verilerini toplamak için yüklenir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-4-24"></a>NIST 800 53 denetim SI-4 (24)

#### <a name="information-system-monitoring--indicators-of-compromise"></a>Sistem izleme bilgilerini | Tehlike göstergeleri

**SI-4 (24)** bilgi sistemi bulur, toplar, dağıtır ve güvenliğin göstergeleri kullanır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri bulmak, toplama, dağıtma ve müşteri tarafından dağıtılan kaynaklara tehlike göstergeleri kullanarak sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-5a"></a>NIST 800 53 denetim SI-5.a

#### <a name="security-alerts-advisories-and-directives"></a>Güvenlik Uyarıları, danışma ve yönergeleri

**SI 5.a** kuruluş bilgileri sistem güvenlik uyarıları, danışma ve gelen yönergeleri alan [atama: kuruluş tarafından tanımlanan dış kuruluşlar] düzenli olarak.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, güvenlik uyarıları, danışma ve müşteri tarafından dağıtılan kaynaklar (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil olmak üzere) için yönergeleri yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-5b"></a>NIST 800 53 denetim SI-5.b

#### <a name="security-alerts-advisories-and-directives"></a>Güvenlik Uyarıları, danışma ve yönergeleri

**SI 5.b** gerekli görüldüğü gibi kuruluş iç güvenlik uyarıları, danışma ve yönergeleri oluşturur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, güvenlik uyarıları, danışma ve müşteri tarafından dağıtılan kaynaklar için yönergeleri yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-5c"></a>NIST 800 53 denetim SI-5.c

#### <a name="security-alerts-advisories-and-directives"></a>Güvenlik Uyarıları, danışma ve yönergeleri

**SI 5.c** kuruluşun güvenlik uyarıları, danışma ve yönergeleri disseminates: [seçimi (bir veya daha fazla): [atama: kuruluş tarafından tanımlanan personel ya da roller]; [Atama: kuruluş içinde kuruluşunuz tarafından tanımlanan öğelerin]; [Atama: kuruluş tarafından tanımlanan dış kuruluşlar]].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, güvenlik uyarıları, danışma ve müşteri tarafından dağıtılan kaynaklar için yönergeleri yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-5d"></a>NIST 800 53 denetim SI-5.d

#### <a name="security-alerts-advisories-and-directives"></a>Güvenlik Uyarıları, danışma ve yönergeleri

**SI 5.d** kuruluşun güvenlik yönergeleri kurulan aralıklarına göre uygulayan veya uyumsuzluk derecesini sertifikayı veren kuruluşu bildirir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, güvenlik uyarıları, danışma ve müşteri tarafından dağıtılan kaynaklar için yönergeleri yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-5-1"></a>NIST 800 53 denetim SI-5 (1)

#### <a name="security-alerts-advisories-and-directives--automated-alerts-and-advisories"></a>Güvenlik Uyarıları, danışma ve yönergeleri | Otomatik uyarı ve danışma

**SI-5 (1)** kuruluşun güvenlik yapmak için otomatik mekanizmaları kullanan uyarı ve danışma bilgileri kuruluş genelinde kullanılabilir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, güvenlik uyarıları, danışma ve müşteri tarafından dağıtılan kaynaklar için yönergeleri yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-6a"></a>NIST 800 53 denetim SI-6.a

#### <a name="security-function-verification"></a>Güvenlik işlevi doğrulama

**SI 6.a** doğru işlem bilgileri sistem doğrular [atama: kuruluş tanımlanan güvenlik işlevleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil olmak üzere) için güvenlik işlevi doğrulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-6b"></a>NIST 800 53 denetim SI-6.b

#### <a name="security-function-verification"></a>Güvenlik işlevi doğrulama

**SI 6.b** bu doğrulama bilgileri sistem gerçekleştirir [seçimi (bir veya daha fazla): [atama: kuruluş tarafından tanımlanan sistem geçici durumlarını]; uygun ayrıcalık; sahip kullanıcı tarafından komut bağlı [Atama: kuruluş tarafından tanımlanan sıklığı]].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için güvenlik işlevi doğrulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-6c"></a>NIST 800 53 denetim SI-6.c

#### <a name="security-function-verification"></a>Güvenlik işlevi doğrulama

**SI 6.c** bilgi sistemi bildirir [atama: kuruluş tarafından tanımlanan personel ya da roller] başarısız güvenlik doğrulama testlerinin.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için güvenlik işlevi doğrulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-6d"></a>NIST 800 53 denetim SI-6.d

#### <a name="security-function-verification"></a>Güvenlik işlevi doğrulama

**SI 6.d** bilgi sistemi [seçimi (bir veya daha fazla): bilgi sistem kapanıp; Information system; yeniden başlatır [Atama: kuruluş tanımlı alternatif eylemleri]] anormallikleri bulunduğunda.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için güvenlik işlevi doğrulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-7"></a>NIST 800 53 denetim SI-7

#### <a name="software-firmware-and-information-integrity"></a>Yazılım, bellenim ve bilgi tutarlılığı

**SI-7** kuruluş yetkisiz değişiklikler algılamak için bütünlüğünü doğrulama araçları kullanır [atama: kuruluş tanımlı yazılım, bellenim ve bilgileri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows, gerçek zamanlı dosya bütünlüğünü doğrulama, koruma ve kurtarma, Windows veya Windows Kaynak Koruması (WRP) özelliği ile yetkili Windows Sistem Güncelleştirmeler'in bir parçası olarak yüklenen çekirdek sistem dosyalarının sağlar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-7-1"></a>NIST 800 53 denetim SI-7 (1)

#### <a name="software-firmware-and-information-integrity--integrity-checks"></a>Yazılım, bellenim ve bilgilerin bütünlüğünü | Bütünlük denetimlerinin

**SI-7 (1)** bilgi sistemi bir bütünlük denetimi gerçekleştirir [atama: kuruluş tanımlı yazılım, bellenim ve bilgileri] [seçimi (bir veya daha fazla): başlangıçta; konumunda [atama: kuruluş tanımlanmış geçici durumlar veya Güvenlik ilgili olayları]; [Atama: kuruluş tarafından tanımlanan sıklığı]].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows, gerçek zamanlı dosya bütünlüğünü doğrulama, koruma ve kurtarma, Windows veya Windows Kaynak Koruması (WRP) özelliği ile yetkili Windows Sistem Güncelleştirmeler'in bir parçası olarak yüklenen çekirdek sistem dosyalarının sağlar. WRP gerçek zamanlı bütünlüğü denetimini etkinleştirir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-7-2"></a>NIST 800 53 denetim SI-7 (2)

#### <a name="software-firmware-and-information-integrity--automated-notifications-of-integrity-violations"></a>Yazılım, bellenim ve bilgilerin bütünlüğünü | Bütünlük ihlallerinin otomatik bildirimler

**SI-7 (2)** kuruluş için bildirim sağlamak otomatik araçları kullanır [atama: kuruluş tarafından tanımlanan personel ya da roller] bütünlüğünü doğrulama sırasında tutarsızlıklar keşfetme bağlıdır.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows, gerçek zamanlı dosya bütünlüğünü doğrulama, koruma ve kurtarma, Windows veya Windows Kaynak Koruması (WRP) özelliği ile yetkili Windows Sistem Güncelleştirmeler'in bir parçası olarak yüklenen çekirdek sistem dosyalarının sağlar.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-7-5"></a>NIST 800 53 denetim SI-7 (5)

#### <a name="software-firmware-and-information-integrity--automated-response-to-integrity-violations"></a>Yazılım, bellenim ve bilgilerin bütünlüğünü | Bütünlüğünü ihlal eden otomatik yanıt

**SI-7 (5)** bilgi sistemi otomatik olarak [seçimi (bir veya daha fazla): bilgi sistemi kapanır, uygular; bilgileri sistem yeniden başlatıldıktan [atama: kuruluş tanımlanan güvenlik önlemlerinin]] bütünlüğünü ihlal eden olduğunda bulunan.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde bütünlüğünü ihlal eden için otomatik olarak yanıt sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-7-7"></a>NIST 800 53 denetim SI-7 (7)

#### <a name="software-firmware-and-information-integrity--integration-of-detection-and-response"></a>Yazılım, bellenim ve bilgilerin bütünlüğünü | Algılama ve yanıt tümleştirme

**SI-7 (7)** yetkisiz algılanması kuruluş içerir [atama: kuruluş tarafından tanımlanan güvenlik ilgili değişiklikleri bilgi sisteme] kuruluş olay yanıtlama yetenek içine.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için yazılım ve bilgi tutarlılığı korumak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-7-14"></a>NIST 800 53 denetim SI-7 (14)

#### <a name="software-firmware-and-information-integrity--binary-or-machine-executable-code"></a>Yazılım, bellenim ve bilgilerin bütünlüğünü | İkili veya makine yürütülebilir kod

**SI-7 (14)** kuruluş sınırlı veya hiçbir garanti ve kaynak kodu; sağlanmasını olmadan kaynaklardan ikili veya makine yürütülebilir kod kullanımını engeller ve ilgi çekici görev için yalnızca kaynak kodu gereksinimi için özel durumlar sağlar / işletimsel gereksinimlerini ve yetki vererek resmi onayla.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde sistem ve bilgi tutarlılığı yordamları bazı kaynaklardan kaynak kodunu ikili veya makine yürütülebilir kod almak için gereksinimleri oluşturabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-8a"></a>NIST 800 53 denetim SI-8.a

#### <a name="spam-protection"></a>İstenmeyen posta korumasını

**SI 8.a** kuruluş kullanan istenmeyen posta koruma mekanizmalarını noktalarda algılamak ve istenmeyen iletileri eylemde için bilgi sistem giriş ve çıkış.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan posta sunucu yok. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-8b"></a>NIST 800 53 denetim SI-8.b

#### <a name="spam-protection"></a>İstenmeyen posta korumasını

**SI 8.b** yeni sürümler Kuruluş yapılandırması yönetim ilkesi ve yordamları uygun olarak kullanılabilir olduğunda kuruluş güncelleştirmeleri istenmeyen posta korumasını mekanizmaları.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan posta sunucu yok. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-8-1"></a>NIST 800 53 denetim SI-8 (1)

#### <a name="spam-protection--central-management"></a>Koruma istenmeyen | Merkezi Yönetim

**SI-8 (1)** kuruluş istenmeyen posta koruma mekanizmalarını merkezi olarak yönetir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan posta sunucu yok. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-si-8-2"></a>NIST 800 53 denetim SI-8 (2)

#### <a name="spam-protection--automatic-updates"></a>Koruma istenmeyen | Otomatik Güncelleştirmeler

**SI-8 (2)** bilgi sistemi istenmeyen posta koruma mekanizmalarını otomatik olarak güncelleştirir.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu ayrıntılı bir parçası olarak dağıtılan posta sunucu yok. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-10"></a>NIST 800 53 denetim SI-10

#### <a name="information-input-validation"></a>Bilgi giriş doğrulaması

**SI 10** bilgi sistemi geçerliliğini denetler [atama: kuruluş tarafından tanımlanan bilgileri girişleri].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynakların (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek) bilgi giriş doğrulaması sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-11a"></a>NIST 800 53 denetim SI-11.a

#### <a name="error-handling"></a>Hata işleme

**SI 11.a** bilgileri sistem tarafından rakiplerin yararlanılabilir bilgi göstermeden düzeltici eylemler için gerekli bilgileri sağlayın hata mesajları oluşturur.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan kaynak ticari işletim sistemleri ve yazılım uygulamalarını kullanın. Bu yazılım endüstrideki en iyi uygulamaları hassas bilgileri hata iletileri gösterilmez emin olmak için kullanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-11b"></a>NIST 800 53 denetim SI-11.b

#### <a name="error-handling"></a>Hata işleme

**SI 11.b** bilgi sistemi hata iletileri yalnızca ortaya çıkarır [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan kaynak ticari işletim sistemleri ve yazılım uygulamalarını kullanın. Bu yazılım endüstrideki en iyi uygulamaları iletiyi alan kullandığı bağlamında uygun hata iletileri sağlamak için kullanır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-12"></a>NIST 800 53 denetim SI-12

#### <a name="information-handling-and-retention"></a>Bilgi işleme ve bekletme

**SI-12** kuruluş bilgileri sistem ve ilgili federal yasalarına uygun olarak sistem bilgileri çıktısını içindeki bilgileri saklar ve işler Executive siparişleri, yönergeleri, ilkeleri, düzenlemeler, standartları ve işletimsel gereksinimleri.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, işleme ve müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) ve bu kaynakları bilgi çıktısını içindeki bilgileri korunuyor sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-si-16"></a>NIST 800 53 denetim SI-16

#### <a name="memory-protection"></a>Bellek koruması

**SI-16** bilgi sistemi uygulayan [atama: kuruluş tanımlanan güvenlik önlemlerinin] kendi bellek yetkisiz kod yürütülmesini korumak için.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Bu şeması tarafından dağıtılan sanal makineleri Windows işletim sistemlerinin çalıştırın. Windows kod yürütmeyi kısıtlı bellek konumlarda engellemek için yerinde korumaları sahiptir: yürütme yok (NX), adres alanı düzeni rastgele seçimini (ASLR) ve Veri Yürütme Engellemesi (DEP). |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |
