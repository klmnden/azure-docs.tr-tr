---
title: Endpoint protection sorunlarını Azure Güvenlik Merkezi ile yönetme | Microsoft Docs
description: Azure Güvenlik Merkezi'nde bitiş noktası sorunları yönetmeyi öğrenin.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/23/2017
ms.author: terrylan
ms.openlocfilehash: abbcb0a8e0206d78ca94520dfa81ab92506c47af
ms.sourcegitcommit: 4d90200f49cc60d63015bada2f3fc4445b34d4cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
ms.locfileid: "23936297"
---
# <a name="manage-endpoint-protection-issues-with-azure-security-center"></a>Endpoint protection sorunlarını Azure Güvenlik Merkezi ile yönetme
Azure Güvenlik Merkezi, kötü amaçlı yazılım korumasını durumunu izler ve bu rapor, Endpoint protection sorunlarını dikey penceresinde altında. Güvenlik Merkezi tehdit algılanan ve sanal makineleri (VM'ler) ve bilgisayarları kötü amaçlı yazılım tehditlerine yapabilirsiniz yetersiz koruma gibi sorunlar vurgular. Altında verilen bilgileri kullanarak **bitiş noktası sorunları**, tanımlanan tüm sorunların yönelik bir plan tanımlayabilirsiniz.

Güvenlik Merkezi aşağıdaki bitiş noktası sorunları raporları:

- Azure Vm'lerinde – desteklenen kötü amaçlı yazılımdan koruma çözümünü yüklenmemiş endpoint protection bu Azure Vm'lerinde yüklü değil.
- Uç nokta koruma Azure olmayan bilgisayarlara – yüklenmemiş desteklenen kötü amaçlı yazılımdan koruma bu Azure olmayan bilgisayarlara yüklü değil.
- Uç nokta koruma durum:

   - İmza kötü amaçlı yazılımdan koruma çözümünü bu sanal makineleri ve bilgisayarlara yüklenir, ancak çözüm en son kötü amaçlı yazılımdan koruma imzaları yok güncel –.
   - Gerçek zamanlı koruma yok – kötü amaçlı yazılımdan koruma çözümünü bu sanal makineleri ve bilgisayarlara yüklenir, ancak gerçek zamanlı koruma için yapılandırılmamış.   Hizmet devre dışı veya Güvenlik Merkezi çözümü desteklenmediğinden durumu alınamıyor olabilir. Bkz: [ortak tümleştirme](security-center-partner-integration.md) desteklenen çözümleri listesi.
   - Raporlama – kötü amaçlı yazılımdan koruma çözümünü yüklenir, ancak raporlama verilerini değil.
   - Bilinmiyor – kötü amaçlı yazılımdan koruma çözümünü yüklendi, ancak bilinmeyen veya bilinmeyen bir hata raporlama durumundadır.

   > [!NOTE]
   > Bkz: [güvenlik çözümlerini tümleştirmenize](security-center-partner-integration.md#integrated-azure-security-solutions) Güvenlik Merkezi ile tümleşik uç nokta koruma güvenlik çözümleri listesi.
   >
   >

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
Bitiş noktası sorunları, Güvenlik Merkezi'nde bir öneri olarak sunulur.  Altında ortamınıza kötü amaçlı yazılım tehditlerine karşı savunmasız ise, bu öneri görüntülenir **önerileri** ve altında **işlem**. Görmek için **Endpoint protection sorunlarını Pano**, işlem iş akışı izlemeniz gerekir.

Bu örnekte, kullanacağız **işlem**.  Kötü amaçlı yazılımdan koruma Azure vm'lerinde ve Azure olmayan bilgisayarlara nasıl yükleneceği ele alacağız.

## <a name="install-antimalware-on-azure-vms"></a>Azure Vm'lerinde kötü amaçlı yazılımdan koruma yükleyin

1. Seçin **işlem** Güvenlik Merkezi ana menüsündeki veya **genel bakış**.

   ![İşlem seçin][1]

2. Altında **işlem**seçin **bitiş noktası sorunları**. **Bitiş noktası sorunları** panosu açılır.

   ![Bitiş noktası sorunları seçin][2]

   Pano üstündeki sağlar:

   - Endpoint protection sağlayıcıları - listeleri Güvenlik Merkezi tarafından tanımlanan farklı sağlayıcı yüklü.
   - Yüklü uç nokta koruma durumu - VMs ve yüklü bir uç nokta koruma çözümüne sahip bilgisayarların sistem durumunu gösterir. Grafik VM'ler ve iyi durumda olan bilgisayarların sayısını ve yetersiz koruma numarasıyla gösterir.
   - Güvenlik Merkezi yeri bildiren bilgisayarların kötü amaçlı yazılım algıladı ve kötü amaçlı yazılım algılanan – VM'lerin sayısını gösterir.
   - Saldırıya uğrayan bilgisayarlar – VM'ler ve Güvenlik Merkezi tarafından kötü amaçlı yazılımların saldırıları burada bildiriyor bilgisayarların sayısını gösterir.

   Pano alt kısmında, aşağıdaki bilgileri içeren koruma sorunları uç nokta listesi vardır:  

   - **Toplam** -VMs ve bilgisayarların sayısını sorundan etkilenen.
   - VM'ler ve bu sorundan etkilenen bilgisayarların sayısını toplayarak çubuğu A. Renkleri çubuğunda öncelik tanımlayın:

      - Kırmızı - yüksek öncelikli ve hemen ilgilenilmesi gerekir
      - Turuncu - Orta öncelikli ve hemen ilgilenilmesi gerekir

3. Seçin **Azure Vm'lerinde yüklenmemiş Endpoint protection**.

   ![Azure Vm'lerinde yüklenmemiş Endpoint protection seçin][3]

4. Altında **Azure Vm'lerinde yüklenmemiş Endpoint protection** Azure VM'ler, kötü amaçlı yazılımdan koruma yüklü olmayan bir listesi verilmiştir.  Listedeki tüm VM'ler kötü amaçlı yazılımdan koruma yükleyin veya kötü amaçlı yazılımdan koruma belirli VM'de tıklayarak yüklemek için tek tek sanal makineleri seçin seçebilirsiniz.
5. Altında **işaretleyin Endpoint protection**, kullanmak istediğiniz uç nokta koruma çözümü seçin. Bu örnekte seçin **Microsoft Antimalware**.
6. Uç nokta koruma çözümü hakkında ek bilgi görüntülenir. **Oluştur**’u seçin.

## <a name="install-antimalware-on-non-azure-computers"></a>Kötü amaçlı yazılımdan koruma Azure olmayan bilgisayarlara yükleme

1. Geri dönerek **bitiş noktası sorunları** seçip **Azure olmayan bilgisayarlarda yüklü olmayan Endpoint protection**.

   ![Azure olmayan bilgisayarlarda yüklü olmayan Endpoint protection seçin][4]

2. Altında **Azure olmayan bilgisayarlarda yüklü olmayan Endpoint protection**, bir çalışma alanı seçin. Çalışma alanına filtrelenmiş bir günlük analizi arama sorgusu açar ve kötü amaçlı yazılımdan koruma eksik bilgisayarları listeler. Daha fazla bilgi için listeden bir bilgisayar seçin.

   ![Günlük analizi arama][5]

Bu bilgisayar için yalnızca filtrelenen bilgilerle başka bir arama sonucu açar.

  ![Günlük analizi arama][6]

> [!NOTE]
> Endpoint protection tüm sanal makineleri ve bilgisayarlar için tanımlamak ve virüsler, casus yazılım ve diğer kötü amaçlı yazılımları kaldırmak hazırlanması öneririz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede "Endpoint Protection yükle." Güvenlik Merkezi öneriyi uygulamayı nasıl oluşturulacağını gösterir Microsoft Antimalware Azure etkinleştirme hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Bulut Hizmetleri ve sanal makineleri için Microsoft Antimalware](../security/azure-security-antimalware.md) --Microsoft Antimalware dağıtmayı öğrenin.

Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) --güvenlik ilkeleri yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/compute.png
[2]:./media/security-center-install-endpoint-protection/endpoint-protection-issues.png
[3]:./media/security-center-install-endpoint-protection/install-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/endpoint-protection-issues-computers.png
[5]:./media/security-center-install-endpoint-protection/log-search.png
[6]:./media/security-center-install-endpoint-protection/info-filtered-to-computer.png
