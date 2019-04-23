---
title: Azure Güvenlik Merkezi ile uç nokta koruma sorunları yönetme | Microsoft Docs
description: Azure Güvenlik Merkezi'nde uç nokta koruma sorunları yönetmeyi öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: rkarlin
ms.openlocfilehash: 882d4e0592b74e8af30ff5bf110a41e403c3bf7d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59789660"
---
# <a name="manage-endpoint-protection-issues-with-azure-security-center"></a>Azure Güvenlik Merkezi ile uç nokta koruma sorunları yönetme
Azure Güvenlik Merkezi, kötü amaçlı yazılımdan korunma durumunu izler ve bu uç nokta koruma sorunları dikey penceresi bildirir. Güvenlik Merkezi, algılanan tehditlere ve sanal makinelerinizi (VM) ve bilgisayarları kötü amaçlı yazılım tehditlerine karşı savunmasız hale getirebilirsiniz yetersiz koruma gibi sorunlar vurgular. Altında verilen bilgileri kullanarak **uç nokta koruma sorunları**, tanımlanan tüm sorunların ele almak için bir plan belirleyebilirsiniz.

Güvenlik Merkezi şu uç nokta koruma sorunları bildirir:

- Bu Azure Vm'lere endpoint protection Azure Vm'lerinde – desteklenen kötü amaçlı yazılımdan koruma çözümü yüklenmedi yüklü değil.
- Endpoint protection yüklenmedi – Azure dışı bilgisayarların desteklenen bir kötü amaçlı yazılımdan koruma bu Azure dışı bilgisayarlara yüklü değil.
- Endpoint protection sistem durumu:

  - Kötü amaçlı yazılımdan koruma çözümünü bu Vm'lere ve bilgisayarlara yüklenir, ancak çözümüne sahip en son kötü amaçlı yazılımdan koruma imzaları güncel – imzası.
  - Gerçek zamanlı koruma yok – kötü amaçlı yazılımdan koruma çözümünü bu Vm'lere ve bilgisayarlara yüklenir, ancak gerçek zamanlı koruma için yapılandırılmamış.   Hizmet devre dışı bırakılabilir veya Güvenlik Merkezi çözüm desteklenmediğinden durumu alınamıyor olabilir. Bkz: [iş ortağı tümleştirmesi](security-center-os-coverage.md#supported-endpoint-protection-solutions) desteklenen çözümleri listesi.
  - Raporlama – kötü amaçlı yazılımdan koruma çözümünü yüklenir, ancak raporlama verilerini değil.
  - Bilinmiyor – kötü amaçlı yazılımdan koruma çözümünü yüklü ancak bilinmeyen veya bilinmeyen bir hata raporlama durumundadır.

    > [!NOTE]
    > Bkz: [güvenlik çözümlerini tümleştirme](security-center-os-coverage.md#supported-endpoint-protection-solutions) uç nokta koruma güvenlik çözümlerini Güvenlik Merkezi ile tümleşik bir listesi.
    >
    >

## <a name="implement-the-recommendation"></a>Önerisini uygulama
Uç nokta koruma sorunları, Güvenlik Merkezi'nde bir öneri olarak sunulur.  Altında ortamınıza kötü amaçlı yazılım tehditlerine karşı savunmasız ise, bu öneriyi görüntülenecektir **önerileri** altında **işlem**. Görmek için **uç nokta koruma Pano sorunları**, bilgi işlem iş akışını takip etmek gerekir.

Bu örnekte, kullanacağız **işlem**.  Azure sanal makinelerinde ve Azure harici bilgisayarları kötü amaçlı yazılımdan koruma yükleme atacağız.

## <a name="install-antimalware-on-azure-vms"></a>Azure Vm'lerinde kötü amaçlı yazılımdan koruma yükleyin

1. Seçin **işlem ve uygulamalar** altındaki Güvenlik Merkezi ana menüsünde veya **genel bakış**.

   ![İşlem seçin][1]

2. Altında **işlem**seçin **uç nokta koruma sorunları**. **Uç nokta koruma sorunları** panosu açılır.

   ![Uç nokta koruma sorunları seçin][2]

   Panonun üst tarafındaki sağlar:

   - Endpoint protection sağlayıcıları - Güvenlik Merkezi tarafından tanımlanan farklı sağlayıcıları listeler yüklü.
   - Yüklü endpoint protection sistem durumu - Vm'leri ve yüklü bir uç nokta koruma çözümüne sahip bilgisayarların sistem durumunu gösterir. Grafik, VM'ler ve iyi durumda olan bilgisayarların sayısını ve koruması yetersiz olan numarasını gösterir.
   - Kötü amaçlı yazılım algılanan – VM sayısını gösterir ve burada Güvenlik Merkezi raporlama bilgisayarları kötü amaçlı yazılım algıladı.
   - Saldırılan bilgisayarlar – Vm'leri ve Bilgisayarları Güvenlik Merkezi tarafından kötü amaçlı yazılım saldırılarına burada bildiriyor sayısını gösterir.

   Pano altındaki aşağıdaki bilgileri içeren koruma sorunları uç nokta listesi vardır:  

   - **Toplam** -VM'ler ve bilgisayarların sayısı, sorundan etkilenen.
   - VM'ler ve bu sorundan etkilenen bilgisayarların sayısını toplayarak çubuğu A. Çubuk renklerini öncelik tanımlayın:

      - Kırmızı - yüksek öncelikli ve hemen ilgilenilmesi gerekir
      - Turuncu - Orta önceliklidir ve hemen ilgilenilmesi gerekir

3. Seçin **Endpoint protection yüklenmedi Azure Vm'lerinde**.

   ![Azure Vm'leri üzerinde yüklü olmayan Endpoint protection seçin][3]

4. Altında **Endpoint protection yüklenmedi Azure Vm'lerinde** Azure Vm'leri, yüklü kötü amaçlı yazılımdan koruma sahip olmayan bir listesidir.  Listedeki tüm vm'lerde kötü amaçlı yazılımdan koruma yüklemeyi veya kötü amaçlı yazılımdan koruma belirli VM'ye tıklayarak yüklemek için tek tek sanal makineleri seçebilirsiniz.
5. Altında **işaretleyin Endpoint protection**, kullanmak istediğiniz uç nokta koruma çözümü seçin. Bu örnekte seçin **Microsoft Antimalware**.
6. Uç nokta koruma çözümü hakkında ek bilgiler görüntülenir. **Oluştur**’u seçin.

## <a name="install-antimalware-on-non-azure-computers"></a>Azure dışı bilgisayarlara kötü amaçlı yazılımdan koruma yükleyin

1. Geri Git **uç nokta koruma sorunları** seçip **Endpoint protection yüklenmedi Azure dışı bilgisayarlara**.

   ![Azure olmayan bilgisayarlarda yüklü olmayan Endpoint protection seçin][4]

2. Altında **Endpoint protection yüklenmedi Azure dışı bilgisayarlara**, bir çalışma alanı seçin. Çalışma alanına filtrelenmiş bir Azure İzleyici günlüklerine arama sorgusu açılır ve kötü amaçlı yazılımdan koruma eksik bilgisayarları listeler. Daha fazla bilgi için listeden bir bilgisayar seçin.

   ![Azure İzleyici arama günlüğe kaydeder.][5]

Bu bilgisayar için yalnızca filtrelenen bilgilerle başka bir arama sonucu açılır.

  ![Azure İzleyici arama günlüğe kaydeder.][6]

> [!NOTE]
> Tüm VM'ler ve bilgisayarlar için belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olmak uç nokta koruma sağlanması öneririz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nin önerisini "Endpoint Protection yükle." uygulama nasıl oluşturulacağını gösterir Azure'da Microsoft Antimalware etkinleştirme hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

* [Bulut Hizmetleri ve sanal makineler için Microsoft Antimalware](../security/azure-security-antimalware.md) --Microsoft Antimalware dağıtmayı öğrenin.

Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) --güvenlik ilkelerinin nasıl yapılandırılacağını öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/compute.png
[2]:./media/security-center-install-endpoint-protection/endpoint-protection-issues.png
[3]:./media/security-center-install-endpoint-protection/install-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/endpoint-protection-issues-computers.png
[5]:./media/security-center-install-endpoint-protection/log-search.png
[6]:./media/security-center-install-endpoint-protection/info-filtered-to-computer.png
