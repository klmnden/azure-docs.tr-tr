---
title: Azure Site Recovery ile Azure'a çoğaltma VMware için kaynak ortamı ayarlama | Microsoft Docs
description: Bu makalede, azure'da Azure Site Recovery ile VMware Vm'lerini çoğaltmak için şirket içi ortamınızı ayarlama açıklar.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/14/2019
ms.author: ramamill
ms.openlocfilehash: 075f86b24e2915d9689db8097889a830bade74c5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723435"
---
# <a name="set-up-the-source-environment-for-vmware-to-azure-replication"></a>Azure'a çoğaltma için VMware için kaynak ortamı ayarlama

Bu makalede, VMware Vm'lerini Azure'a çoğaltma için kaynak şirket içi ortamı ayarlamak açıklar. Otomatik olarak keşfetme şirket içi Vm'leri ve bir şirket içi makine Site Recovery yapılandırma sunucusu olarak ayarlama, çoğaltma senaryosu seçmeye yönelik adımlar içerir. 

## <a name="prerequisites"></a>Önkoşullar

Sahip olduğunuz varsayılır:

- Dağıtımınızı yardımıyla planlı [Azure Site Recovery dağıtım Planlayıcısı](site-recovery-deployment-planner.md). Bu, istenen kurtarma noktası hedefini (RPO) karşılayacak günlük veri değişikliği hızınıza, göre yeterli bant genişliğini, size yardımcı olur.
- [Kaynaklarını ayarlama](tutorial-prepare-azure.md) içinde [Azure portalında](https://portal.azure.com).
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
3. [İndirme](vmware-azure-deploy-configuration-server.md#download-the-template) ve [alma](vmware-azure-deploy-configuration-server.md#import-the-template-in-vmware) OVA şablonu şirket içi VMware yapılandırma sunucusu çalıştıran bir VM kümesi. Şablonu ile sağlanan lisans bir deneme lisansı olduğunu ve 180 gün boyunca geçerli olur. POST bu dönem, tedarik edilen lisansa sahip windows etkinleştirmek müşteri gerekir.
4. VMware VM kapatma ve [kaydetmek](vmware-azure-deploy-configuration-server.md#register-the-configuration-server-with-azure-site-recovery-services) kurtarma Hizmetleri kasası.

## <a name="azure-site-recovery-folder-exclusions-from-antivirus-program"></a>Virüsten koruma programı'ndaki Azure Site Recovery klasör dışlamaları

### <a name="if-antivirus-software-is-active-on-source-machine"></a>Virüsten koruma yazılımı etkin kaynak makinede ise

Yükleme klasörü, kaynak makinede bir virüsten koruma yazılımı etkin varsa, hariç tutulması gerekir. Bu nedenle, klasörü dışlamak *C:\ProgramData\ASR\agent* kesintisiz çoğaltma.

### <a name="if-antivirus-software-is-active-on-configuration-server"></a>Virüsten koruma yazılımını yapılandırma sunucusunda etkin olması durumunda

Kesintisiz çoğaltma ve bağlantı sorunları önlemek için virüsten koruma yazılımı klasörlerinden aşağıdaki Dışla

- C:\Program Files\Microsoft Azure kurtarma Hizmetleri Aracısı.
- C:\Program Files\Microsoft Azure Site Recovery sağlayıcısı
- C:\Program Files\Microsoft Azure Site Recovery Yapılandırma Yöneticisi 
- C:\Program Files\Microsoft Azure Site Recovery hata koleksiyon aracı 
  - C:\thirdparty
  - C:\Temp
  - C:\strawberry
  - C:\ProgramData\MySQL
  - C:\Program Files (x86)\MySQL
  - C:\ProgramData\ASR
  - C:\ProgramData\Microsoft Azure Site kurtarma
  - C:\ProgramData\ASRLogs
  - C:\ProgramData\ASRSetupLogs
  - C:\ProgramData\LogUploadServiceLogs
  - C:\inetpub
  - ASR sunucusu yükleme dizininde. Örneğin: E:\Program dosyaları (x86) \Microsoft Azure Site kurtarma

### <a name="if-antivirus-software-is-active-on-scale-out-process-servermaster-target"></a>Virüsten koruma yazılımı üzerinde genişleme etkinse sunucusu/ana hedef işleme

Aşağıdaki klasörleri virüsten koruma yazılımından Dışla

1. C:\Program Files\Microsoft Azure Recovery Services Agent
2. C:\ProgramData\ASR
3. C:\ProgramData\ASRLogs
4. C:\ProgramData\ASRSetupLogs
5. C:\ProgramData\LogUploadServiceLogs
6. C:\ProgramData\Microsoft Azure Site kurtarma
7. ASR yük dengeli işlem sunucusu yükleme dizininde, örneğin: C:\Program dosyaları (x86) \Microsoft Azure Site kurtarma


## <a name="next-steps"></a>Sonraki adımlar
[Hedef ortamı ayarlama](./vmware-azure-set-up-target.md) 
