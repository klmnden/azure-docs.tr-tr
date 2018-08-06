---
title: Azure ayrılmış VM örneklerini yönetme | Microsoft Docs
description: Abonelik kapsamını değiştirmek ve erişimi Azure ayrılmış VM örnekleriyle ilgili yönetmek nasıl öğrenin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2018
ms.author: yashesvi
ms.openlocfilehash: 589afc736faaa210fdcd5cf6c79d10af4af3c0e9
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502534"
---
# <a name="manage-reserved-instances-in-azure"></a>Azure ayrılmış örnekler yönetme

Azure ayrılmış VM örneği satın aldığınız sonra ayrılmış örnek satın alma sırasında belirtilenden farklı bir aboneliğe uygulamak isteyebilirsiniz. Alternatif olarak, eşleşen sanal makinelerinizin içinde birden çok abonelik çalıştırıyorsanız, paylaşılan ayrılmış örnek kapsamını değiştirmek isteyebilirsiniz. Ayrılmış örnek indirimi en üst düzeye çıkarmak için satın aldığınız örneklerinin sayısını, öznitelikleri ve sahip olduğunuz sanal makine sayısı eşleştiğinden emin olun çalışıyor. Azure ayrılmış örnekleri hakkında daha fazla bilgi için bkz: [Azure sanal makineler için önceden ödeme yaparak tasarruf](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Ayrılmış örnek için kapsamı değiştirin
 Ayrılmış örneğiniz eşleşen ve ayrılmış örnek kapsamı içinde çalıştırılan sanal makineler, ayrılmış örnek indirimi geçerlidir. Ayrılmış örnek kapsamını, tek bir abonelik veya tüm abonelikler, faturalama bağlamında olabilir. Tek bir abonelik için kapsamı ayarlarsanız, seçili abonelikte çalışan sanal makineler için ayrılmış örnek eşleştirilir. Ayrılmış örnek için fatura bağlamı içinde tüm abonelikler çalışan sanal makineleri paylaşılan, Azure eşleşmeler kapsamı ayarlarsanız. Ayrılmış örnek satın almak için kullandığınız aboneliğe fatura bağlamı bağlıdır. Daha fazla bilgi için bkz. [ayrılmış örneklerle VM'ler için önceden ödeme](https://go.microsoft.com/fwlink/?linkid=861721).

Ayrılmış örnek kapsamını güncelleştirmek için: 
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayrılmış örnek seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Kapsamını değiştirin. Gelen tek bir kapsam için paylaşılan değiştirirseniz, yalnızca sahibi olduğu abonelikleri seçebilirsiniz. Yalnızca abonelikleri ayrılmış örnek olarak fatura aynı bağlam içinde seçilebilir. Fatura bağlamı ayrılmış örnek krem seçtiğiniz abonelik tarafından belirlenir. Kapsam, yalnızca Kullandıkça Öde teklifimize MS-AZR - 0003 P abonelikleri ve kurumsal teklif MS-AZR - 0017 P abonelikler için geçerlidir. Kurumsal anlaşmalar için geliştirme ve test abonelikleri ayrılmış örnek indirim almak uygun değildir.

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Ekleme veya ayrılmış örnek yönetebilen kullanıcılar değiştirme
Ayrılmış örnek Yönetimi kişilerin rollerine ayrılmış örnek ekleyerek devredebilirsiniz. Varsayılan olarak, kişi, ayrılmış örnek satın ve Hesap Yöneticisi ayrılmış bir örneğinde sahip rolüne sahip. 

Erişim için ayrılmış örnekler bağımsız olarak ayrılmış örnek indirimi alma aboneliklerden yönetebilirsiniz. Birisi bir ayrılmış örnek yönetme izni verdiğinizde, bunları aboneliğinizi yönetmek için hakları verin değil. Ve birisi ayrılmış örneğin kapsam içinde bir aboneliği yönetme izni verirseniz, bunları ayrılmış örnek yönetme hakkı vermez.
 
Ayrılmış örnek için erişim yönetimini temsilci atamak için: 
1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Seçin **tüm hizmetleri** > **ayırma** erişiminiz ayrılmış örnekleri listesi.
3.  Diğer kullanıcılara erişim vermek istediğiniz ayrılmış örnek seçin.
4.  Seçin **erişim denetimi (IAM)** menüsünde.
5.  Seçin **Ekle** > **rol** > **sahibi** (veya sınırlı erişim vermek istiyorsanız farklı bir rol). 
6. Sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın. 
7. Kullanıcıyı seçin ve ardından **Kaydet**.

## <a name="optimize-reserved-vm-instance-for-vm-size-flexibility-or-capacity-priority"></a>Ayrılmış VM örneği için VM boyutu esneklik veya kapasite önceliği en iyi duruma getirme
 VM örneği esnekliği, aynı diğer VM'ler için ayırma indirimi geçerlidir [VM boyutu grubu](https://aka.ms/RIVMGroups). Varsayılan olarak, rezervasyon kapsamı paylaşıldığında örneği boyutu esnekliği açıktır ve veri merkezinizin kapasitesini VM dağıtımları için öncelik değildir. Kapsamı tek olduğu için ayırmaları, sanal makine örneği boyutu esnekliği yerine kapasite önceliği ayırmasını iyileştirebilirsiniz. Kapasite önceliği, ihtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneğinizi ek güvenilirlik sunan dağıtımlarınız için veri merkezi kapasitenizi ayırır.

Ayrılmış örnek kapsamını güncelleştirmek için: 
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayrılmış örnek seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Ayar için iyileştirme değiştirin.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>İki ayrılmış örnekler tek bir ayrılmış örneğine Böl
 Birden fazla örneği satın aldığınız sonra farklı aboneliklere ayrılmış örnek örneklerinden atamak isteyebilirsiniz. Varsayılan olarak, tüm örnekleri (satın alma sırasında belirtilen miktarı) bir kapsamı - ya da tek bir abonelik veya paylaşılan. Örneğin, 10 standart D2 VM satın alınan ve belirtilen abonelik a için kapsamı Şimdi kapsam yedi ayrılmış örnekler için bir aboneliğe dönüştürmek isteyebilirsiniz ve ayrıntılı kapsam yönetimi için örnek dağıtmak kalan abonelik b bölme ayrılmış örnek 3 sağlar. Paylaşılan kapsam seçerek aboneliklere ayırma basitleştirebilir. Ancak, maliyet Yönetimi'ni veya ödemeyle amaçları için özel abonelikler miktarlar ayırabilirsiniz.

 PowerShell, CLI, ancak iki ayrılmış örnekleri ayrılmış bir örneğine bölebilir veya API üzerinden.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Ayrılmış örnek PowerShell kullanarak Böl
1. Ayrılmış örnek sipariş kimliği, aşağıdaki komutu çalıştırarak alın:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```

2. Ayrılmış örnek ayrıntılarını alın:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Ayrılmış örnek iki halinde bölme ve örnekleri dağıtabilirsiniz:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```

1. Aşağıdaki komutu çalıştırarak kapsamı güncelleştirebilirsiniz:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="next-steps"></a>Sonraki adımlar
Azure ayrılmış örnekleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ayrılmış örnek indiriminin nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için ayrılmış örnek kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Ayrılmış örneklere dahil olmayan Windows yazılım maliyetleri](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
