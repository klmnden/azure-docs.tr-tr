---
title: Azure Güvenlik Merkezi'nde, sanal makineleri koruma | Microsoft Docs
description: Yardımcı olacak öneriler Azure Güvenlik Merkezi'nde, sanal makinelerin korunmasına ve güvenlik ilkeleriyle uyumlu olmak bu belge adresleri.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: terrylan
ms.openlocfilehash: 54375f6f98b4989a7af8bcde649d967f77c6c862
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
ms.locfileid: "27623507"
---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde, sanal makineleri koruma
Azure Güvenlik Merkezi, ayrıca Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması sürecinde size rehberlik öneriler oluşturur.  Önerileri Azure kaynak türleri için geçerlidir: sanal ağ, SQL ve uygulamaları makineleri (VM'ler).

Bu makalede Vm'lere uygulamak önerileri giderir.  VM önerileri merkezi veri toplama, kötü amaçlı yazılımdan koruma, sağlama sistemi güncelleştirmelerini uygulama etrafında VM disklerinizi ve daha fazlasını şifreleme.  Aşağıdaki tabloda kullanılabilir VM önerileri ve onu uygularsanız her birini ne yapacağını anlamanıza yardımcı olması için bir başvuru olarak kullanın.

## <a name="available-vm-recommendations"></a>Kullanılabilir VM önerileri
| Öneri | Açıklama |
| --- | --- |
| [Abonelikler için veri toplamayı etkinleştirin](security-center-enable-data-collection.md) |Her bir aboneliğiniz ve aboneliklerinizdeki tüm sanal makinelerine (VM) için güvenlik ilkesinde veri toplamayı etkinleştirmenizi önerir. |
| [Azure depolama hesabı için şifrelemeyi etkinleştir](security-center-enable-encryption-for-storage-account.md) | Rest verileri için Azure depolama hizmeti şifrelemesi etkinleştirmenizi önerir. Azure depolama alanına yazılır ve alma önce şifresini çözer verileri şifreleyerek depolama hizmeti şifreleme (SSE) çalışır. SSE yalnızca Azure Blob hizmeti için şu anda kullanılabilir değil ve blok blobları, sayfa bloblarını kullanılabilir ve ekleme blobları. Daha fazla bilgi için bkz: [bekleyen veri için depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).</br>SSE yalnızca Resource Manager depolama hesaplarında desteklenir. Klasik depolama hesapları şu anda desteklenmemektedir. Klasik ve Resource Manager dağıtım modellerinde anlamak için bkz: [Azure dağıtım modelleri](../azure-classic-rm.md). |
| [Güvenlik yapılandırmalarını Düzelt](security-center-remediate-os-vulnerabilities.md) |Önerilen güvenlik yapılandırma kuralları ile işletim sistemi yapılandırmalarını Hizala önerir örneğin izin verme Parolaların kaydedilmesine. |
| [Sistem güncelleştirmelerini uygulayın](security-center-apply-system-updates.md) |VM’lerinize eksik sistem güvenliği güncelleştirmelerini ve kritik güncelleştirmeleri dağıtmanızı önerir. |
| [Bir sadece zaman içinde geçerli ağ erişim denetimi](security-center-just-in-time.md) | Yalnızca süresi VM erişimi uygulamanızı önerir. Özellik önizlemededir zaman yalnızca ve Güvenlik Merkezi'nin standart katmanında kullanılabilir. Bkz: [fiyatlandırma](security-center-pricing.md) Güvenlik Merkezi hakkında daha fazla katmanları fiyatlandırma öğrenin. |
| [Sistem güncelleştirmelerinden sonra yeniden başlatın](security-center-apply-system-updates.md#reboot-after-system-updates) |Sistem güncelleştirmelerini uygulama işlemini tamamlamak için VM’yi yeniden başlatmanızı önerir. |
| [Endpoint Protection’ı yükleyin](security-center-install-endpoint-protection.md) |VM’lere (yalnızca Windows VM’leri) kötü amaçlı yazılımdan koruma programları sağlamanızı önerir. |
| [VM Aracısını etkinleştirin](security-center-enable-vm-agent.md) |Hangi VM’lerin VM Aracısı gerektirdiğini görmenizi sağlar. Düzeltme eki tarama, temel tarama ve kötü amaçlı yazılımdan koruma programları sağlamak üzere VM’lere VM Aracısı yüklenmelidir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir. |
| [Disk şifrelemesi uygulayın](security-center-apply-disk-encryption.md) |Azure Disk Şifrelemesi kullanarak VM’nizi şifrelemenizi önerir (Windows ve Linux VM’leri). Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir. |
| [İşletim sistemi sürümünü güncelleştirme](security-center-update-os-version.md) |İşletim sistemi (OS) sürümü kullanılabilir en son sürümüne bulut hizmetiniz için işletim sistemi ailesi için güncelleştirme önerir.  Bulut hizmetleri hakkında daha fazla bilgi için bkz: [bulut hizmetlerine genel bakış](../cloud-services/cloud-services-choose-me.md). |
| [Güvenlik açığı değerlendirmesi yüklü değil](security-center-vulnerability-assessment-recommendations.md) |Sanal makinenize bir güvenlik açığı değerlendirme çözümü yüklemenizi önerir. |
| [Güvenlik açıklarını düzeltin](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |VM’nize yüklü güvenlik açığı değerlendirme çözümü tarafından algılanan sistem ve uygulama güvenlik açıklarını görmenizi sağlar. |

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türleri için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Uygulamalarınızı Azure Güvenlik Merkezi'nde koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi, ağınızda koruma](security-center-network-recommendations.md)
* [Azure SQL hizmetinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
