---
title: Azure ayrılmış VM örnekleri yönetme | Microsoft Docs
description: Abonelik kapsamını değiştirmek ve Azure ayrılmış VM örnekleri için erişim yönetmek nasıl öğrenin.
services: billing
documentationcenter: ''
author: vikramdesai01
manager: vikramdesai01
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: vikdesai
ms.openlocfilehash: ddb9d46dc2689b0dbcd8734e276916f7cd9d2728
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064131"
---
# <a name="manage-reserved-instances-in-azure"></a>Azure'da ayrılmış örnekler yönetme

Bir Azure ayrılmış VM örnek satın sonra farklı bir abonelik satın alma sırasında belirtilenden ayrılmış örnek uygulamak isteyebilirsiniz. Alternatif olarak, eşleşen sanal makinelerinizin içinde birden çok abonelik çalıştırıyorsanız, paylaşılan ayrılmış örnek kapsamını değiştirmek isteyebilirsiniz. Ayrılmış örnek indirim en üst düzeye çıkarmak için satın aldığınız örnek sayısı öznitelikleri ve sahip olduğunuz sanal makine sayısı eşleştiğinden emin olun çalışıyor. Azure ayrılmış örnekler hakkında daha fazla bilgi için bkz: [Azure sanal makineler için önceden ödeme tarafından paradan tasarruf](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Ayrılmış örnek için kapsamı değiştirin
 Ayrılmış örnek eşleşen ve ayrılmış örnek kapsamı içinde çalıştırılan sanal makineler için ayrılmış örnek indirim uygulanır. Ayrılmış örnek kapsamı tek bir abonelik veya tüm abonelikler, faturalama bağlamda olabilir. Tek bir abonelik için kapsam ayarlarsanız, ayrılmış örnek seçilen abonelikte çalışan sanal makinelere eşleştirilir. Fatura bağlamında tüm abonelikler çalışan sanal makineleri için ayrılmış örnek için paylaşılan, Azure eşleşmeleri kapsamı ayarlarsanız. Ayrılmış örnek satın almak için kullanılan abonelikte fatura bağlam bağlıdır. Daha fazla bilgi için bkz: [ayrılmış örnekler olan VM'ler için ön ödeme](https://go.microsoft.com/fwlink/?linkid=861721).

Ayrılmış örnek kapsamını güncelleştirmek için: 
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayrılmış örnek seçin.
4. Seçin **ayarları** > **yapılandırma**.
5. Kapsamı değiştirin. Gelen tek bir kapsam için paylaşılan değiştirirseniz, yalnızca sahibi olduğunuz abonelikleri seçebilirsiniz. Yalnızca ayrılmış örnek olarak aynı fatura bağlamında abonelikleri seçilebilir. Fatura bağlam ayrılmış örnek satın zaman seçtiğiniz abonelik tarafından belirlenir. Kapsam yalnızca teklifinden Kullandıkça Öde teklifine MS-AZR - 0003 P abonelikleri ve kurumsal teklif MS-AZR - 0017 P abonelikler için geçerlidir. Kurumsal Anlaşma için geliştirme ve test abonelikleri ayrılmış örnek indirim almak uygun değildir.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>İki ayrılmış örnekler tek ayrılmış bir örneğine bölme
 Birden fazla örneği satın sonra farklı aboneliklere ayrılmış örnek içindeki örnekleri atamak isteyebilirsiniz. Varsayılan olarak, tüm örnekler (satın alma sırasında belirtilen miktarı) sahip bir kapsam - ya da tek bir abonelik veya paylaşılan. Örneğin, 10 standart D2 VM'ler satın alınan ve belirtilen abonelik A. olmasını kapsamı Şimdi bir abonelik için yedi ayrılmış örnekler kapsamını değiştirmek isteyebilirsiniz ve abonelik B. bölme ayrılmış örnek için kalan 3 parçalı kapsam yönetimi için örnek dağıtmak sağlar. Paylaşılan kapsam seçerek aboneliklere ayırma basitleştirebilirsiniz. Ancak maliyet Yönetimi veya bütçe amacıyla belirli aboneliklere miktarları tahsis edebilirsiniz.

 PowerShell'i, CLI, ancak iki ayrılmış örnekler ayrılmış bir örneğine bölebilirsiniz veya API'si aracılığıyla.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Ayrılmış örnek PowerShell kullanarak Böl
1. Ayrılmış örnek düzeni kimliği, aşağıdaki komutu çalıştırarak alın:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```
2. Ayrılmış örnek ayrıntıları:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. İki ayrılmış örneğine bölme ve örnekleri dağıtabilirsiniz:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Aşağıdaki komutu çalıştırarak kapsamı güncelleştirebilirsiniz:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Ekleme veya ayrılmış bir örnek yöneten kullanıcılar değiştirme
Ayrılmış örnek rollerinde kişiler ekleyerek ayrılmış örnek Yönetimi devredebilirsiniz. Varsayılan olarak, ayrılmış örnek satın kişi ve Hesap Yöneticisi ayrılmış örneğinde sahip rolünü sahip. 

Erişim için ayrılmış örnekler bağımsız olarak ayrılmış örnek indirim almak aboneliklerden yönetebilirsiniz. Ne zaman, birisi bunları aboneliği yönetme hakkı vermediğinin ayrılmış bir örnek, yönetme izni verin. Ve birisi ayrılmış örneğinin kapsamdaki bir aboneliği yönetme izni verirseniz, bunları ayrılmış örnek yönetme hakkı verin değil.
 
Erişim Yönetimi ayrılmış örnek için temsilci atamak için: 
1.  [Azure Portal](https://portal.azure.com)’da oturum açın.
2.  Seçin **tüm hizmetleri** > **ayırma** erişiminiz ayrılmış örneklerinin listesi.
3.  Diğer kullanıcılara erişim vermek istediğiniz ayrılmış örnek seçin.
4.  Seçin **erişim denetimi (IAM)** menüde.
5.  Seçin **Ekle** > **rol** > **sahibi** (veya sınırlı erişim vermek istiyorsanız, farklı bir rol). 
6. Sahibi olarak eklemek istediğiniz kullanıcının e-posta adresini yazın. 
7. Kullanıcıyı seçin ve ardından **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar
Azure ayrılmış örnekler hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayrılmış VM örnekleri nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Ayrılmış örnek indirim nasıl uygulandığını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için ayrılmış örnek kullanımını anlamak](billing-understand-reserved-instance-usage.md)
- [İşletme kaydı için ayrılmış örnek kullanım anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayrılmış örnekler dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
