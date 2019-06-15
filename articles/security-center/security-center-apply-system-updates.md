---
title: Azure Güvenlik Merkezi'nde sistem güncelleştirmelerini uygulayın | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerilerinin uygulanması gösterilmektedir **sistem güncelleştirmelerini** ve **sistem güncelleştirmelerinden sonra yeniden**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: ebd9939128d1f2b870541e82710792d13b69728e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62095464"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde sistem güncelleştirmelerini uygulayın
Azure Güvenlik Merkezi günlük Windows ve Linux sanal makineleri (VM'ler) ve işletim sistemi güncelleştirmeleri eksik bilgisayarlar izler. Güvenlik Merkezi bir Windows bilgisayarda yapılandırılmış hizmet bağlı olarak Windows Update veya Windows Server Update Services (WSUS) kullanılabilir güvenlik güncelleştirmeleri ve kritik güncelleştirmeler listesini alır. Güvenlik Merkezi, ayrıca Linux sistemlerinde en son güncelleştirmeleri denetler. Sistem Güncelleştirmesi VM'de veya bilgisayarda bulunmuyorsa, Güvenlik Merkezi sistem güncelleştirmelerini uygulayın önerir.

## <a name="implement-the-recommendation"></a>Önerisini uygulama
Geçerli sistem güncelleştirmeleri, Güvenlik Merkezi'nde bir öneri olarak sunulur. Altında sanal makine veya bilgisayar sistem güncelleştirmesi eksik, bu öneriyi görüntülenir **önerileri** altında **işlem**.  Öneriyi seçtiğinizde açılır **sistem güncelleştirmelerini** Pano.

Bu örnekte, kullanacağız **işlem**.

1. Seçin **işlem** altındaki Güvenlik Merkezi ana menüsünde.

   ![İşlem seçin][1]

2. Altında **işlem**seçin **eksik sistem güncelleştirmeleri**. **Sistem güncelleştirmelerini** panosu açılır.

   ![Sistem güncelleştirmeleri Panosu Uygula][2]

   Panonun üst tarafındaki sağlar:

    - Windows ve Linux sanal makinelerini ve eksik sistem güncelleştirmeleri bilgisayarların toplam sayısı.
    - Kullanarak Vm'lerinizdeki ve bilgisayarlarınızdaki eksik olan kritik güncelleştirme sayısı.
    - Kullanarak Vm'lerinizdeki ve bilgisayarlarınızdaki arasında eksik güvenlik güncelleştirmeleri toplam sayısı.

   Pano altındaki tüm eksik güncelleştirmeler Vm'lerinizi ve bilgisayarlar ve eksik güncelleştirmenin önem arasında listelenir.  Liste aşağıdakileri içerir:

    - ADI: Eksik güncelleştirmenin adıdır.
    - HAYIR VM ve bilgisayarlar: VM'ler ve bu güncelleştirmenin eksik olduğu bilgisayarların toplam sayısı.
    - DURUM: Önerinin geçerli durumu:

      - Açık: Öneri henüz ele alınmadı.
      - Devam eden: Öneri şu anda bu kaynaklara uygulanıyor ve herhangi bir işlem yapmanıza gerek yoktur.
      - Çözümlendi: Öneri zaten tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).

    - ÖNEM DERECESİ: Belirli bir önerinin önem açıklanmaktadır:

      - Yüksek: Bir güvenlik açığı, anlamlı bir kaynakta (uygulama, sanal makine veya ağ güvenlik grubu) var ve ilgilenilmesi gerekiyor.
      - Orta: Bir işlemin tamamlanması veya bir güvenlik açığını ortadan kaldırmak için kritik olmayan veya ek adımlar gerekir.
      - Düşük: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

3. Eksik güncelleştirme ayrıntılarını görüntülemek için listeden seçin.

   ![Eksik güvenlik güncelleştirmesi][3]

4. Seçin **arama** üstteki Şeritte simgesi.  Azure İzleyici günlüklerine arama sorgusuyla güncelleştirmenin eksik olan bilgisayarlar için filtrelenmiş açılır.

   ![Azure İzleyici arama günlüğe kaydeder.][4]

5. Daha fazla bilgi için listeden bir bilgisayar seçin. Bu bilgisayar için yalnızca filtrelenen bilgilerle başka bir arama sonucu açılır.

    ![Azure İzleyici arama günlüğe kaydeder.][5]

## <a name="reboot-after-system-updates"></a>Sistem güncelleştirmelerinden sonra yeniden başlatın
1. Geri dönüp **önerileri** dikey penceresi. Sistem güncelleştirmeleri, adlı uyguladıktan sonra yeni bir girişin üretildiği **sistem güncelleştirmelerinden sonra yeniden**. Bu giriş, sistemi güncelleştirmelerini uygulama işlemini tamamlamak için VM yeniden başlatma yapmanız bilmenizi sağlar.

   ![Sistem güncelleştirmelerinden sonra yeniden başlatın][6]
2. Seçin **sistem güncelleştirmelerinden sonra yeniden**. Bu açılır **sistem güncelleştirmelerini tamamlamak için yeniden başlatma beklemede** Uygula sistem tamamlamak için yeniden başlatmanız gereken VM'lerin listesini görüntüleyen dikey penceresinde, işlem güncelleştirir.

   ![Yeniden başlatma bekleniyor][7]

İşlemi tamamlamak için Azure VM'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
[6]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[7]: ./media/security-center-apply-system-updates/restart-pending.png
