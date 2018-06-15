---
title: Azure Güvenlik Merkezi'nde sistem güncelleştirmeleri uygulamak | Microsoft Docs
description: Bu belgede Azure Güvenlik Merkezi önerileri uygulamak gösterilmiştir ** sistem güncelleştirmeleri ** uygulamak ve ** sistem güncelleştirmeleri ** sonra yeniden başlatın.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 9f7924f3f0975dc32fdf5b8e1b89a1fb8e9b7d57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866432"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde sistem güncelleştirmeleri uygulama
Azure Güvenlik Merkezi günlük Windows ve Linux sanal makineleri (VM'ler) ve işletim sistemi güncelleştirmeleri eksik bilgisayarlar izler. Güvenlik Merkezi bir Windows bilgisayarda yapılandırılmış hizmet bağlı olarak Windows Update veya Windows Server Update Services (WSUS) kullanılabilir güvenlik ve kritik güncelleştirmeler listesini alır. Güvenlik Merkezi, ayrıca Linux sistemleri için en son güncelleştirmeleri denetler. VM veya bilgisayar sistem güncelleştirmesi eksikse, Güvenlik Merkezi sistem güncelleştirmeleri uygulamanızı öneririz.

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
Sistem geçerli güncelleştirmeler, Güvenlik Merkezi'nde bir öneri olarak sunulur. VM veya bilgisayar sistem güncelleştirmesi eksik, bu öneri altında görüntülenir **önerileri** ve altında **işlem**.  Öneri seçme açılır **sistem güncelleştirmelerini** Pano.

Bu örnekte, kullanacağız **işlem**.

1. Seçin **işlem** Güvenlik Merkezi ana menü altında.

   ![İşlem seçin][1]

2. Altında **işlem**seçin **eksik sistem güncelleştirmeleri**. **Sistem güncelleştirmelerini** panosu açılır.

   ![Sistem güncelleştirmeleri Panosu Uygula][2]

   Pano üstündeki sağlar:

    - Windows ve Linux VM'ler ve eksik sistem güncelleştirmeleri bilgisayarları toplam sayısı.
    - Kritik güncelleştirmeleri eksik VM'ler ve bilgisayarlar arasında toplam sayısı.
    - VM'ler ve bilgisayarlar arasında eksik güvenlik güncelleştirmelerini toplam sayısı.

  Pano altındaki tüm eksik güncelleştirmeleri Vm'leriniz ve bilgisayarlar ve eksik güncelleştirmenin önem arasında listeler.  Liste aşağıdakileri içerir:

    - ADI: Eksik güncelleştirme adı.
    - HAYIR. Sanal makineleri &AMP; bilgisayarlar: VM'ler ve bu güncelleştirmeyi eksik olan bilgisayarlar toplam sayısı.
    - DURUMU: Önerinin geçerli durumu:

      - Açık: Öneri henüz ele alınmadı.
      - Devam eden: Öneri şu anda bu kaynaklara uygulanıyor ve herhangi bir işlem yapmanız gerekmez.
      - Çözümlendi: Öneri zaten tamamlandı neden. (Sorun çözüldüğünde girdi soluklaşır).

    - Önem DERECESİ: belirli bir önerinin önem açıklanmaktadır:

      - Yüksek: Bir güvenlik açığı anlamlı bir kaynakta (uygulama, sanal makine ya da ağ güvenlik grubu) var ve dikkat gerektiriyor.
      - Orta: Bir işlemin tamamlanması veya bir güvenlik açığının ortadan kaldırılması için kritik olmayan veya ek adımlar gereklidir.
      - Düşük: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

3. Eksik güncelleştirme ayrıntılarını görüntülemek için listeden seçin.

   ![Eksik güvenlik güncelleştirmesi][3]

4. Seçin **arama** üst Şerit simgesi.  Günlük analizi arama sorgusu güncelleştirmeyi eksik olan bilgisayarlar için filtrelenmiş açar.

   ![Günlük analizi arama][4]

5. Daha fazla bilgi için listeden bir bilgisayar seçin. Bu bilgisayar için yalnızca filtrelenen bilgilerle başka bir arama sonucu açar.

    ![Günlük analizi arama][5]

## <a name="reboot-after-system-updates"></a>Sistem güncelleştirmelerinden sonra yeniden başlatın
1. Geri dönüp **önerileri** dikey. Adlı Sistem güncelleştirmeleri uygulandıktan sonra yeni bir giriş oluşturulan **sonra sistem güncelleştirmelerini yeniden**. Bu giriş, VM sistemi güncelleştirmelerini uygulama işlemini tamamlamak için bilgisayarınızı yeniden başlatmanız gerektiğini bilmenizi sağlar.

   ![Sistem güncelleştirmelerinden sonra yeniden başlatın][6]
2. Seçin **sonra sistem güncelleştirmelerini yeniden**. Bu açılır **sistem güncelleştirmelerini tamamlamak için yeniden başlatma bekleniyor** Uygula sistem tamamlamak için yeniden başlatmanız gerekir VM'ler listesini görüntüleyen dikey işlem güncelleştirir.

   ![Beklemedeki yeniden başlatma][7]

İşlemi tamamlamak için Azure VM'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
[6]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[7]: ./media/security-center-apply-system-updates/restart-pending.png
