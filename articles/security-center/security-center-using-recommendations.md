---
title: Güvenliği artırmak için Azure Güvenlik Merkezi'ni kullanarak önerileri | Microsoft Docs
description: " Güvenlik ilkeleri ve öneriler, Azure Güvenlik Merkezi'nde güvenlik saldırısını önlemeye yardımcı olmak için kullanmayı öğrenin. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/26/2019
ms.author: monhaber
ms.openlocfilehash: 82a46ae9523c4c2778f86c445e35d0bec961307f
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517700"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Güvenliği artırmak için Azure Güvenlik Merkezi'ni kullanarak önerileri
Güvenlik İlkesi yapılandırma ve sonra Azure Güvenlik Merkezi tarafından sağlanan öneriler uygulayarak bir önemli güvenlik olayı olasılığını azaltabilirsiniz. Bu makalede güvenlik ilkeleri ve öneriler Güvenlik Merkezi'nde güvenlik saldırısını önlemeye yardımcı olmak için nasıl kullanılacağını gösterir. 

Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz etmek için sürekli taramaları otomatik olarak çalıştırılır. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli güvenlik denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur. Güvenlik Merkezi önerilerini, aşağıdaki istisnalarla birlikte 24 saat içinde güncelleştirir:

- İşletim sistemi güvenlik yapılandırması önerileri 48 saat içinde güncelleştirilir.
- Endpoint Protection sorunlarını önerileri 8 saat içinde güncelleştirilir.

## <a name="scenario"></a>Senaryo
Bu senaryo izleme Güvenlik Merkezi önerileri ve eylemi gerçekleştirmeden bir güvenlik olayı olasılığını azaltmaya yardımcı olmak için Güvenlik Merkezi'ni kullanmayı gösterir. Senaryo adlı kurgusal şirketin, Contoso ve Güvenlik Merkezi tarafından sunulan rollerini kullanır [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). Bu senaryoda, biz aşağıdaki kişilerin rollerine odaklandığınız:

![Senaryo rolleri](./media/security-center-using-recommendations/scenario-roles.png)

Contoso kısa süre önce şirket içi kaynaklarından bazılarını Azure'a taşımıştır. Contoso, kendi kaynaklarını koruyabilir ve bulut kaynaklarından bazılarını güvenlik açığını azaltmak istemektedir.

## <a name="use-azure-security-center"></a>Azure Güvenlik Merkezi’ni kullanma
David, şubeden BT güvenlik kullanıcının, yerleşik Güvenlik Merkezi önleme ve güvenlik açıklarını algılama için Azure Güvenlik Merkezi'ne Contoso'nun Aboneliklerde zaten seçmiştir. 

Güvenlik Merkezi otomatik olarak Contoso'nun Azure kaynaklarınızın güvenlik durumunu analiz eder ve varsayılan güvenlik ilkelerini uygular. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, oluşturduğu **önerileri** güvenlik ilkesi ayarlayın denetimleri göre. 

David, Azure güvenlik standart katmanı, önerileri ve güvenlik özellikleri içeren tam paketini almak için tüm kendi abonelikler arasında çalışır. Ayrıca tüm kendi mevcut şirket içi kendisinin Güvenlik Merkezi'nin karma yararlanabilir, böylece buluta henüz geçirilmeyen sunucuları ekleme desteği HIS arasında Jeff [Windows](quick-onboard-windows-computer.md) ve [Linux](quick-onboard-linux-computer.md) sunucuları.

Jeff, bir bulut iş yükü sahibi değil. Jeff, Contoso'nun güvenlik ilkelerine uygun güvenlik denetimleri uygulamak için sorumludur. 

Jeff aşağıdaki görevleri gerçekleştirir:

- Güvenlik Merkezi tarafından sağlanan güvenlik önerilerini izleyin
- Güvenlik önerileri değerlendirin ve o uygulama kapatmak veya seçemeyeceğini
- Güvenlik önerilerini uygulama

### <a name="remediate-threats-using-recommendations"></a>Öneriler kullanarak tehditleri gidermeleri
Jeff, kendi günlük izleme etkinliklerin bir parçası olarak Azure'a açar ve Güvenlik Merkezi açılır. 

1. Jeff, kendi iş yükünün abonelikleri seçer.

2. Jeff denetler HIS **puanı güvenli** almak için genel bir resim nasıl güvenli abonelikleri olan ve kendi puanı 548 olup olmadığını görür.

3. Jeff ilk işlemek için hangi önerilerin karar vermeniz gerekir. Jeff güvenli puanı tıklar ve önerileri ne kadar himself artırır üzerinde işlemeye başlar bu nedenle [puanı etkisi güvenli](security-center-secure-score.md).

4. Jeff bağlı Vm'leri ve sunuculardan çok sayıda olduğundan, Jeff odaklanmak karar **işlem ve uygulamaları**.

5. Jeff tıkladığında **işlem ve uygulamaları**, kendisinin öneriler listesini görür ve onları güvenli göre işler puan etkisi.

6. Jeff Vm'lere birçok Internet'e vardır ve bağlantı noktalarının açık olduğundan, kendisinin kaygılı bir saldırgan sunucular üzerinde denetim geçirebilir. Jeff seçti şekilde (**tam zamanında VM erişimi**) [güvenlik-center-just-içinde-time.md].

Jeff, Orta öncelikli öneriler ve yüksek öncelikli geçmeye devam eder ve mantığınız kararlarını verir. Güvenlik Merkezi tarafından ın hangi kaynakların etkilendiğini anlamak için ne güvenli puanı etkisi olması koşuluyla, hangi her öneri anlamına gelir ve düzeltme adımları nasıl her sorunu gidermek için her bir öneri Jeff ayrıntılı bilgilerine bakar.

## <a name="conclusion"></a>Sonuç
Güvenlik Merkezi'nde öneriler izleme, bir saldırı gerçekleşmeden önce güvenlik açıklarını ortadan kaldırmanıza yardımcı olur. Düzeltme önerileri, güvenli, Puanlama ve iş yüklerinizi güvenlik duruşunu geliştirin. Güvenlik Merkezi, dağıtma, güvenlik ilkeniz göre bunları değerlendirir ve bunların güvenliğini sağlamak için yeni öneriler yeni kaynaklar otomatik olarak bulur.


## <a name="next-steps"></a>Sonraki adımlar
Böylece zaman içinde kaynaklarınızın güvenliğini emin olun, düzenli olarak Güvenlik Merkezi önerileri kontrol yerinde izleme işlemi olduğundan emin olun.

Bu senaryoda güvenlik ilkeleri ve öneriler Güvenlik Merkezi'nde güvenlik saldırısını önlemeye yardımcı olmak için nasıl kullanılacağını gösterdi. Bkz: [olay yanıtı senaryosu](security-center-incident-response.md) olay yanıtı bir saldırı gerçekleşmeden önce planınızın olması hakkında bilgi edinmek için.

İle tehditlere yanıt verin öğrenin [olay yanıtı](security-center-incident-response.md).
