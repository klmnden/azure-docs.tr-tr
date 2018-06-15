---
title: Azure ödeme işleme şeması - üst düzey genel bakış
description: Azure ödeme işleme şeması - müşteri sorumluluk matrisi (genel bakış)
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 23cf68d8-bebd-4ac4-a194-39e052281c0e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 4c36f50b5c4ceba911003ec7a633dcab8724c6e0
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33896063"
---
# <a name="pci-dss-requirements---high-level-overview"></a>PCI DSS gereksinimleri - üst düzey genel bakış

Ödeme Kartı sektör veri güvenliği standardı (PCI DSS) teşvik edin ve kart sahibi veri güvenliğini artırmak ve tutarlı verileri güvenlik önlemleri geniş benimsenmesi genel kolaylaştırmak için geliştirilmiştir. PCI DSS hesap verilerini korumak üzere tasarlanmış teknik ve işletimsel gereksinimleri için bir temel sağlar. PCI DSS ödeme kartı işleme tüccarların, işlemciler, acquirers, verenler ve hizmet sağlayıcılarını da dahil olmak üzere ilgili tüm varlıklar için geçerlidir. PCI DSS depolamak, işlem veya kart sahibi verileri (CHD) ve/veya hassas kimlik doğrulama verileri (SAD) iletme tüm diğer varlıklar için de geçerlidir. 12 PCI DSS gereksinimleri üst düzey bir özeti aşağıdadır.

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

|   |   |
|---|---|
| **Derleme ve güvenli sürdürmek<br/>ağ ve sistem** | 1. [Yüklemek ve kart sahibi verileri korumak için bir güvenlik duvarı yapılandırmasını yönetmek](pci-dss-requirement-1-firewall.md)<br/><br/> 2. [Satıcı tarafından sağlanan varsayılan sistem parolalar ve diğer güvenlik parametreleri için kullanmayın](pci-dss-requirement-2-password.md) |  
| **Kart sahibi verileri koruma** | 3. [Saklı kart sahibi verileri koruma](pci-dss-requirement-3-chd.md)<br/><br/> 4. [Kart sahibi veri iletimini açık, ortak ağlarda şifrele](pci-dss-requirement-4-encryption.md) |
| **Bir güvenlik açığı korumak<br/>yönetim programı** | 5. [Tüm sistemler kötü amaçlı yazılımlardan korunmasına ve virüsten koruma yazılımı veya programları düzenli olarak güncelleştirin](pci-dss-requirement-5-malware.md)<br/><br/> 6. [Geliştirme ve güvenli sistemleri ve uygulamalar bakımının](pci-dss-requirement-6-secure-system.md) |
| **Uygulama güçlü erişim<br/>denetim ölçümleri** | 7. [İş gerekenler tarafından kart sahibi verilere erişimi kısıtlama](pci-dss-requirement-7-access.md)<br/><br/> 8. [Tanımlamak ve sistem bileşenlerine erişimi kimlik doğrulaması](pci-dss-requirement-8-identity.md) <br/><br/> 9. [Kart sahibi veri fiziksel erişimi kısıtlama](pci-dss-requirement-9-physical-access.md) |
| **Düzenli olarak izleyin ve<br/>Test ağları** | 10. [İzleme ve tüm ağ kaynaklarına erişim ve kart sahibi veri izleme](pci-dss-requirement-10-monitoring.md) <br/><br/> 11. [Güvenlik sistemleri ve işlemleri düzenli olarak test](pci-dss-requirement-11-testing.md) |
| **Bir bilgi koruma<br/>güvenlik ilkesi** | 12. [Bilgi güvenliği için tüm personel adresleri bir ilke koru](pci-dss-requirement-12-policy.md) |

