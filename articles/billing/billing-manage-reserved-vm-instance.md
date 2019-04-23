---
title: Azure ayırmalarını yönetme | Microsoft Docs
description: Abonelik kapsamını değiştirmek ve erişimi yönetmek için Azure ayırmaları nasıl öğrenin.
ms.service: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 9a5b200ffb9441b90875c7764786004ff5f1e8a1
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994972"
---
# <a name="manage-reservations-for-azure-resources"></a>Rezervasyonlar Azure kaynaklarını yönetme

Azure için bir ayırma satın sonra gerekebilir ayırma farklı bir aboneliğe uygulamak için kimin ayırma yönetmek veya ayırma kapsamını değiştirmek değiştirin. Ayrıca, bir ayırma satın aldığınız örnekleri bazı başka bir aboneliğe uygulamak için iki ayırmalar halinde bölebilirsiniz.

Azure ayrılmış sanal makine örnekleri satın aldıysanız, ayırma için İyileştir ayarı değiştirebilirsiniz. Ayırma indirimi, aynı dizide Vm'lere uygulayabilir veya belirli bir VM boyutu için veri merkezi kapasite ayırabilirsiniz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reservation-order-and-reservation"></a>Rezervasyon siparişi ve ayırma

Bir rezervasyon satın alma işlemi yaptığınızda, iki nesne oluşturulur: **Rezervasyon siparişi** ve **ayırma**.

Satın alım zamanında rezervasyon siparişi altındaki bir ayırma vardır. Bölünmüş, birleştirme, kısmi para iadesi veya exchange gibi eylemleri altında yeni ayırmaları oluşturma **rezervasyon siparişi**.

Rezervasyon siparişi görüntülemek için Git **ayırmaları** > Rezervasyon seçin ve ardından **rezervasyon sipariş kimliği**.

![Rezervasyon sipariş ayrıntılarını gösteren rezervasyon sipariş kimliği örneği ](./media/billing-manage-reserved-vm-instance/reservation-order-details.png)

Ayırma, rezervasyon siparişini izinleri devralır.

## <a name="change-the-reservation-scope"></a>Ayırma Kapsamı Değiştir

 Ayırma indirimi, sanal makineler, SQL veritabanları, Azure Cosmos DB veya rezervasyonunuzun eşleşen ve ayırma kapsamı içinde çalıştırılan diğer kaynaklar için geçerlidir. Fatura bağlamı, rezervasyon satın almak için kullanılan abonelik üzerinde bağlıdır.

Rezervasyon kapsamı güncelleştirilemedi:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayırma seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Kapsamını değiştirin.

Gelen tek bir kapsam için paylaşılan değiştirirseniz, yalnızca sahibi olduğu abonelikleri seçebilirsiniz. Yalnızca rezervasyonla aynı fatura bağlamında bulunan abonelikleri seçebilirsiniz.

Kapsam yalnızca MS-AZR-0003P veya MS-AZR-0023P kodlu Kullandıkça Öde teklifi, MS-AZR-0017P veya MS-AZR-0148P kodlu Kurumsal teklif ya da CSP abonelikleri için geçerlidir.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Rezervasyonu yönetebilecek kullanıcıları ekleme veya değiştirme

Ayırma yönetim rolleri rezervasyon siparişi veya ayırma kişiler ekleyerek devredebilirsiniz. Varsayılan olarak, rezervasyon siparişi ve Hesap Yöneticisi yerleştirir kişi rezervasyon siparişi ve ayırma sahip rolüne sahiptir.

Rezervasyon siparişleri ve ayırmaları bağımsız olarak ayırma indirimi alma aboneliklerinden gelen erişimi yönetebilir. Birisi bir rezervasyon siparişi veya ayırma yönetme izni verdiğinizde, bunları aboneliği yönetme izni vermek değil. Birisi bir abonelikte bir ayırma'nın kapsamı yönetme izni verirseniz, benzer şekilde, bu bunları rezervasyon siparişi veya ayırma yönetme hakkı verin değil.

Bir exchange ya da para iadesinin gerçekleştirmek için kullanıcı rezervasyon siparişi erişimi olmalıdır. Birisi izin verirken, rezervasyon siparişi ayırma izinler en iyisidir.


Erişim yönetimi için bir ayırma temsilci atamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırma** erişiminiz listesi ayırmalar için.
3. Diğer kullanıcılara erişim vermek istediğiniz ayırma seçin.
4. **Erişim denetimi (IAM)** öğesini seçin.
5. Seçin **rol ataması Ekle** > **rol** > **sahibi**. Sınırlı erişim vermek istiyorsanız farklı bir rol seçebilirsiniz.
6. Sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın.
7. Kullanıcıyı ve ardından **Kaydet**'i seçin.

## <a name="split-a-single-reservation-into-two-reservations"></a>Tek bir ayırma iki ayırmalar halinde bölme

 Birden fazla kaynak örneği içindeki bir ayırma satın sonra bu ayırma örneklerinden farklı aboneliklere atamak isteyebilirsiniz. Varsayılan olarak, tüm örnekleri bir kapsamı - ya da tek bir abonelik veya paylaşılan. Örneğin, 10 ayırma örnekleri satın ve belirtilen abonelik a için kapsamı Artık abonelik A için 7 ayırmaları kapsamını değiştirmek isteyebilirsiniz ve ayrıntılı kapsam yönetimi için örnek dağıtmak kalan abonelik b bölme bir ayırma 3 sağlar. Paylaşılan kapsam seçerek aboneliklere ayırma basitleştirebilir. Ancak, maliyet Yönetimi'ni veya ödemeyle amaçları için özel abonelikler miktarlar ayırabilirsiniz.

 PowerShell, CLI, ancak iki ayırmalar halinde ayırma bölebilir veya API üzerinden.

### <a name="split-a-reservation-by-using-powershell"></a>PowerShell kullanarak bir ayırma Böl

1. Rezervasyon sipariş kimliği, aşağıdaki komutu çalıştırarak alın:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzReservationOrder
    ```

2. Bir ayırmanın ayrıntılarını alın:

    ```powershell
    Get-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Ayırma iki halinde bölme ve örnekleri dağıtabilirsiniz:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Aşağıdaki komutu çalıştırarak kapsamı güncelleştirebilirsiniz:

    ```powershell
    Update-AzReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancellations-and-exchanges"></a>İptalleri ve değişimler

Ayırma türüne bağlı olarak, iptal etme veya rezervasyon exchange mümkün olabilir. Daha fazla bilgi için İptal ve aşağıdaki konulardaki değişimleri bölümlere bakın:

- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](..//virtual-machines/windows/prepay-reserved-vm-instances.md#cancellations-and-exchanges)
- [Azure Ayırmaları ile SUSE yazılım planları için ön ödeme yapma](../virtual-machines/linux/prepay-suse-software-charges.md#cancellation-and-exchanges-not-allowed)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md#cancellations-and-exchanges)

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Ayrılmış VM örnekleri için ayar değişiklik iyileştirin

 Ayrılmış VM örneği satın aldığınızda, örneği boyutu esnekliği veya kapasite önceliği seçin. Örnek boyutu esneklik, aynı diğer VM'ler için ayırma indirimi geçerlidir [VM boyutu grubu](https://aka.ms/RIVMGroups). Kapasite önceliği dağıtımlarınız için veri merkezi kapasitenizi önceliklendirir. Bu seçenek, ihtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneğinizi ek güvence sunar.

Rezervasyon kapsamı paylaşıldığında, varsayılan olarak, örnek boyutu esneklik açıktır. Veri Merkezi kapasitenizi VM dağıtımları için öncelik değildir.

Kapsamı tek olduğu için ayırmaları, sanal makine örneği boyutu esnekliği yerine kapasite önceliği ayırmasını iyileştirebilirsiniz.

Ayırma için İyileştir ayarını güncelleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayırma seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Değişiklik **için en iyi duruma getirme** ayarı.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure için ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)

Bir hizmet planı satın alın:
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Cosmos DB ayrılmış kapasite ile Azure Cosmos DB kaynaklarını için ön ödeme](../cosmos-db/cosmos-db-reserved-capacity.md)

Yazılım planı satın alın:
- [Red Hat yazılımı planlarından Azure ayırmalar için ön ödeme](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Azure Ayırmaları ile SUSE yazılım planları için ön ödeme yapma](../virtual-machines/linux/prepay-suse-software-charges.md)

İndirim ve kullanımını anlayın:
- [VM ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Red Hat Enterprise Linux yazılım planı indirim nasıl uygulanacağını anlama](../billing/billing-understand-rhel-reservation-charges.md)
- [SUSE Linux Enterprise yazılım planı indirim nasıl uygulanacağını anlama](../billing/billing-understand-suse-reservation-charges.md)
- [Nasıl diğer ayırma indirimi uygulanmaz anlama](billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
