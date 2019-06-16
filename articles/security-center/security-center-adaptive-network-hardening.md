---
title: Azure Güvenlik Merkezi'ndeki Uyarlamalı ağ sağlamlaştırma | Microsoft Docs
description: " Azure Güvenlik Merkezi'ndeki Uyarlamalı ağ sağlamlaştırma etkinleştirmeyi öğrenin. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: 09d62d23-ab32-41f0-a5cf-8d80578181dd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2019
ms.author: monhaber
ms.openlocfilehash: f35f410ddc039ee264fa1de317e152cb03f391b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66241491"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki Uyarlamalı ağ sağlamlaştırma
Azure Güvenlik Merkezi'ndeki Uyarlamalı ağ sağlamlaştırma yapılandırmayı öğrenin.

## <a name="what-is-adaptive-network-hardening"></a>Uyarlamalı ağ sağlamlaştırma nedir?
Uygulama [ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) kaynakları, gelen ve giden trafiği filtrelemek için ağ güvenlik duruşunuzu artırır. Ancak, yine de olabilir NSG'de fiili trafiği tanımlı NSG kuralları bir alt kümesi olan bazı durumlarda. Bu durumlarda, daha fazla güvenlik duruşunu geliştirme sağlamlaştırma gerçek trafik düzenlerini esas alarak NSG kuralları tarafından gerçekleştirilebilir.

Uyarlamalı ağ sağlamlaştırma NSG kuralları daha da güçlendirmek için öneriler sağlar. Bir makine öğrenme algoritmasına gerçek trafiği güvenilir yapılandırma, tehdit zekası ve diğer göstergeleri tehlike, bilinen hesaba katan kullanır ve ardından yalnızca belirli bir IP/bağlantı noktası diziler gelen trafiğe izin vermek için öneriler sağlar.

Örneğin, varolan bir NSG kuralı 140.20.30.10/24 22 numaralı bağlantı noktasında gelen trafiğe izin vermek diyelim. Analize dayalı, Uyarlamalı ağ sağlamlaştırma ın öneri aralığını daraltın ve daha dar bir IP aralığı olan 140.23.30.10/29 – gelen trafiğe izin vermek için olması ve ancak bu bağlantı noktasına diğer tüm trafiği reddetmeye.

![Ağ sağlamlaştırma görünümü](./media/security-center-adaptive-network-hardening/traffic-hardening.png)

> [!NOTE]
> Uyarlamalı ağ sağlamlaştırma önerileri aşağıdaki bağlantı noktalarında desteklenir: 22, 3389, 21, 23, 445, 4333, 3306, 1433, 1434, 53, 20, 5985, 5986, 5432, 139, 66, 1128

## <a name="view-adaptive-network-hardening-alerts-and-rules"></a>Uyarlamalı ağ sağlamlaştırma uyarılar ve kuralları görüntüleyin

1. Güvenlik Merkezi'nde seçin **ağ** -> **Uyarlamalı ağ sağlamlaştırma**. Ağ sanal makinelerin üç ayrı sekmeler altında listelenir:
   * **İyi durumda olmayan kaynaklar**: Öneriler ve Uyarlamalı ağ sağlamlaştırma algoritması çalıştırarak tarafından tetiklenen uyarılar şu anda sahip VM'ler. 
   * **İyi durumdaki kaynaklar**: Uyarıları ve öneriler olmayan VM'ler.
   * **Taranmayan kaynaklar**: Uyarlamalı ağ sağlamlaştırma algoritması aşağıdakilerden biri nedeniyle çalıştırılamaz Vm'leri:
      * **Sanal makineleri Klasik Vm'si olan**: Yalnızca Azure Resource Manager sanal makineleri desteklenir.
      * **Yeterli veri kullanılabilir**: Doğru trafiği sağlamlaştırma önerileri oluşturmak için en az 30 gün trafik verileri Güvenlik Merkezi gerektirir.
      * **VM'yi ASC standart tarafından değil korumalı**: Güvenlik Merkezi'nin standart fiyatlandırma katmanına ayarlanmış olan Vm'leri bu özellik için uygundur.

     ![iyi durumda olmayan kaynaklar](./media/security-center-adaptive-network-hardening/unhealthy-resources.png)

2. Gelen **iyi durumda olmayan kaynaklar** sekmesinde, onun uyarıları ve önerilen sağlamlaştırma kuralları uygulamak görüntülemek için bir VM seçin.

    ![sağlamlaştırma uyarıları](./media/security-center-adaptive-network-hardening/hardening-alerts.png)


## <a name="review-and-apply-adaptive-network-hardening-recommended-rules"></a>Gözden geçirin ve Uyarlamalı ağ sağlamlaştırma önerilen kurallar uygulayın

1. Gelen **iyi durumda olmayan kaynaklar** sekmesinde, bir VM seçin. Uyarıları ve önerilen sağlamlaştırma kuralları listelenir.

     ![sağlamlaştırma kuralları](./media/security-center-adaptive-network-hardening/hardening-alerts.png)

   > [!NOTE]
   > **Kuralları** eklediğiniz önerir Uyarlamalı ağ sağlamlaştırma kuralları sekmesinde listelenir. **Uyarılar** nedeniyle önerilen kurallarda izin verilen IP aralığı içinde değil kaynağa akan trafiği, oluşturulan uyarıların sekmesinde listelenir.

2. Bazı parametreler bir kuralın değiştirmek istiyorsanız, bunu, açıklandığı gibi değiştirebilirsiniz [bir kuralı değiştirmek](#modify-rule).
   > [!NOTE]
   > Ayrıca [Sil](#delete-rule) veya [ekleme](#add-rule) bir kural.

3. NSG üzerinde uygulamak ve istediğiniz kuralları seçin **zorla**.

      > [!NOTE]
      > VM koruma nsg'ler uygulanan kuralları eklenir. (Bir VM bir NSG, NIC'ye ilişkili veya sanal Makinenin bulunduğu alt ağ veya her ikisi tarafından korunabilecek)

    ![kuralları zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule2.png)


### Bir kuralı değiştirin  <a name ="modify-rule"> </a>

Önerilen bir kuralı parametreleri değiştirmek isteyebilirsiniz. Örneğin, önerilen IP aralıklarını değiştirmek isteyebilirsiniz.

Önemli bazı yönergeler Uyarlamalı ağ sağlamlaştırma bir kuralı değiştirmek için:

* Yalnızca "izin ver" kuralları parametrelerini değiştirebilirsiniz. 
* "" Reddet kurallarının"olacak kuralları izin ver" değiştiremezsiniz. 

  > [!NOTE]
  > Oluşturma ve değiştirme "Reddet" kuralları yapılır NSG için doğrudan hakkında daha fazla ayrıntı için bkz: [oluşturma, değiştirme veya silme bir ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

* A **tüm trafiği reddetmeye** kural burada listelenir ve "Reddet" kuralının tek tür ve değiştirilemez. Ancak, bunu silebilirsiniz (bkz [kural silme](#delete-rule)).
  > [!NOTE]
  > A **tüm trafiği reddetmeye** kuralı olması önerilir, sonuç olarak algoritma çalışmakta olan Güvenlik Merkezi, mevcut bir NSG yapılandırmasını temel alan izin verilmesi trafiği tanımlamaz. Bu nedenle, belirtilen bağlantı noktası için tüm trafiği reddetmeye yönelik önerilen kuralı gereklidir. Bu kural türü adı olarak gösterilen "*sistem oluşturulan*". Bu kural zorlama sonra gerçek adını NSG protokolü, trafik yönü, "REDDET" ve rastgele bir sayı oluşan bir dize olacaktır.

*Uyarlamalı ağ sağlamlaştırma bir kuralı değiştirmek için:*

1. Bazı parametreler bir kuralı değiştirmek için **kuralları** sekme, kuralın satırın sonundaki üç noktaya (...) tıklayın ve tıklayın **Düzenle**.

   ![Kuralını Düzenle](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. İçinde **düzenleme kuralı** penceresinde güncelleştirme değiştirin ve istediğiniz ayrıntıları **Kaydet**.

   > [!NOTE]
   > ' I tıklattıktan sonra **Kaydet**, kural başarıyla değiştirildi. *Ancak, bu NSG için uygulanmamış.* Bunu uygulamak için bir kural listeden seçin ve gerekir tıklayın **zorla** (sonraki adımda açıklandığı gibi).

   ![Kuralını Düzenle](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Güncelleştirilmiş bir kural listeden uygulamak için güncelleştirilmiş kuralı seçin ve **zorla**.

    ![Kural zorlama](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

### Yeni bir kural ekleyin <a name ="add-rule"> </a>

Güvenlik Merkezi tarafından önerilen bir "izin ver" kuralı ekleyebilirsiniz.

> [!NOTE]
> Yalnızca "izin ver" kuralları burada eklenebilir. "Reddet kurallarının" eklemek istiyorsanız, bunu doğrudan NSG üzerinde bunu yapabilirsiniz. Daha fazla ayrıntı için [oluşturma, değiştirme veya silme bir ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

*Uyarlamalı ağ sağlamlaştırma bir kural eklemek için:*

1. Tıklayın **Kuralı Ekle** (sol üst köşede bulunur).

   ![Kural Ekle](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. İçinde **yeni kural** penceresinde ayrıntılarını girin ve tıklatın **Ekle**.

   > [!NOTE]
   > ' I tıklattıktan sonra **Ekle**kuralı başarıyla eklendi ve diğer önerilen kurallara listelenir. Ancak, bu NSG üzerinde uygulanmamış. Etkinleştirmek için bir kural listeden seçin ve gerekir tıklayın **zorla** (sonraki adımda açıklandığı gibi).

3. Yeni bir kural listeden uygulamak için yeni bir kural seçin ve **zorla**.

    ![Kural zorlama](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


### Kural silme <a name ="delete-rule"> </a>

Gerektiğinde, önerilen bir kuralı silebilirsiniz. Örneğin, önerilen kuralının uygulanması yasal trafiği engelleyebilecek belirleyebilir.

*Uyarlamalı ağ sağlamlaştırma bir kuralı silmek için:*

1. İçinde **kuralları** sekme, kuralın satırın sonundaki üç noktaya (...) tıklayın ve tıklayın **Sil**.  

    ![sağlamlaştırma kuralları](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)







 

