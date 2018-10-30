---
title: Azure'da Azure Site Recovery ile VMware vm'lerinin olağanüstü durum kurtarma için kaynak ortamı ayarlama | Microsoft Docs
description: Bu makalede, Azure'da Azure Site Recovery ile VMware vm'lerinin olağanüstü durum kurtarma için şirket içi ortamınızı ayarlama açıklar.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: 44b1cac461fa49b4bea19b7cf98f6038eb264492
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230735"
---
# <a name="set-up-the-source-environment-for-disaster-recovery-of-vmware-vms-to-azure"></a>Azure'a VMware vm'lerinin olağanüstü durum kurtarma için kaynak ortamı ayarlama

Bu makalede, VMware Vm'lerini Azure'a çoğaltma için kaynak şirket içi ortamı ayarlamak açıklar. Otomatik olarak keşfetme şirket içi Vm'leri ve bir şirket içi makine Site Recovery yapılandırma sunucusu olarak ayarlama, çoğaltma senaryosu seçmeye yönelik adımlar içerir. 

## <a name="prerequisites"></a>Önkoşullar

Sahip olduğunuz varsayılır:

- Dağıtımınızı yardımıyla planlı [Azure Site Recovery dağıtım Planlayıcısı](site-recovery-deployment-planner.md). Bu, istenen kurtarma noktası hedefini (RPO) karşılayacak günlük veri değişikliği hızınıza, göre yeterli bant genişliğini, size yardımcı olur.
- [Kaynaklarını ayarlama](tutorial-prepare-azure.md) içinde [Azure portalında](http://portal.azure.com).
- [Şirket içi VMware ayarlama](vmware-azure-tutorial-prepare-on-premises.md), otomatik bulma için adanmış bir hesap da dahil olmak üzere.

## <a name="choose-your-protection-goals"></a>Koruma hedeflerinizi seçme

1. **Kurtarma Hizmetleri kasaları** bölümünde kasa adını seçin. Bu senaryo için**ContosoVMVault**’u kullanıyoruz.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Ardından **Altyapıyı Hazırla** seçeneğini belirleyin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.

## <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarlayın

Yapılandırma sunucusunu bir açık sanallaştırma uygulama (OVA) şablonu aracılığıyla şirket içi VMware VM ayarlayabilirsiniz. [Daha fazla bilgi edinin](concepts-vmware-to-azure-architecture.md) VMware VM'de yüklü bileşenler hakkında.

1. Hakkında bilgi edinin [önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites) yapılandırma sunucusu dağıtımı için.
2. [Kapasite rakamlarının denetleyin](vmware-azure-deploy-configuration-server.md#capacity-planning) dağıtımı için.
3. [İndirme](vmware-azure-deploy-configuration-server.md#download-the-template) ve [alma](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) OVA şablonu şirket içi VMware yapılandırma sunucusu çalıştıran bir VM kümesi. Şablonu ile sağlanan lisans, bir değerlendirme lisans olduğunu ve 180 gün boyunca geçerli olur. POST bu dönem, tedarik edilen lisans ile windows etkinleştirmek müşteri gerekir.
4. VMware VM kapatma ve [kaydetmek](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) kurtarma Hizmetleri kasası.

## <a name="azure-site-recovery-folder-exclusions-from-antivirus-program"></a>Virüsten koruma programı'ndaki Azure Site Recovery klasör dışlamaları

### <a name="if-antivirus-software-is-active-on-source-machine"></a>Virüsten koruma yazılımı etkin kaynak makinede ise

Yükleme klasörü, kaynak makinede bir virüsten koruma yazılımı etkin varsa, hariç tutulması gerekir. Bu nedenle, klasörü dışlamak *C:\ProgramData\ASR\agent* kesintisiz çoğaltma.

### <a name="if-antivirus-software-is-active-on-configuration-server"></a>Virüsten koruma yazılımını yapılandırma sunucusunda etkin olması durumunda

Kesintisiz çoğaltma ve bağlantı sorunları önlemek için virüsten koruma yazılımı klasörlerinden aşağıdaki Dışla

 1. C:\Program Files\Microsoft Azure kurtarma Hizmetleri Aracısı.
 2. C:\Program Files\Microsoft Azure Site Recovery sağlayıcısı
 3. C:\Program Files\Microsoft Azure Site Recovery Yapılandırma Yöneticisi 
 4. C:\Program Files\Microsoft Azure Site Recovery hata koleksiyon aracı 
 5. C:\thirdparty
 6. C:\Temp
 7. C:\strawberry
 8. C:\ProgramData\MySQL
 9. C:\Program dosyaları (x86) \MySQL
 10. C:\ProgramData\ASR
 11. C:\ProgramData\Microsoft Azure Site kurtarma
 12. C:\ProgramData\ASRLogs
 13. C:\ProgramData\ASRSetupLogs
 14. C:\ProgramData\LogUploadServiceLogs
 15. C:\inetpub
 16. ASR sunucusu yükleme dizininde. Örneğin: E:\Program dosyaları (x86) \Microsoft Azure Site Recovery

### <a name="if-antivirus-software-is-active-on-scale-out-process-servermaster-target"></a>Virüsten koruma yazılımı üzerinde genişleme etkinse sunucusu/ana hedef işleme

Aşağıdaki klasörleri virüsten koruma yazılımından Dışla

1. C:\Program Files\Microsoft Azure Recovery Services Agent
2. C:\ProgramData\ASR
3. C:\ProgramData\ASRLogs
4. C:\ProgramData\ASRSetupLogs
5. C:\ProgramData\LogUploadServiceLogs
6. C:\ProgramData\Microsoft Azure Site kurtarma
7. ASR yük dengeli işlem sunucusu yükleme dizininde, örnek: C:\Program Files (x86) \Microsoft Azure Site Recovery

## <a name="common-issues"></a>Genel sorunlar
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamınızı](./vmware-azure-set-up-target.md) azure'da.
