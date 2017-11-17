---
title: "Azure ayrılmış sanal makine örnekleri yönetme | Microsoft Docs"
description: "Abonelik kapsamını değiştirmek ve Azure ayrılmış VM örnekleri için erişim yönetmek nasıl öğrenin."
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikramdesai01
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/06/2017
ms.author: vikdesai
ms.openlocfilehash: e23eea52ff5d27beacf938a1ef153172e24f1aee
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="manage-reserved-virtual-machine-instances"></a>Ayrılmış sanal makine örneklerini yönetme

Bir Azure ayrılmış VM örnek satın sonra farklı bir abonelik satın alma sırasında belirtilenden ayırma uygulamak isteyebilirsiniz. Alternatif olarak, eşleşen sanal makinelerinizin içinde birden çok abonelik çalıştırıyorsanız, paylaşılan ayırma kapsamını değiştirmek isteyebilirsiniz. Ayırma indirim en üst düzeye çıkarmak için satın aldığınız örnek sayısı öznitelikleri ve sahip olduğunuz sanal makine sayısı eşleştiğinden emin olun çalışıyor. Ayrılmış sanal makine örnekleri hakkında daha fazla bilgi için bkz: [Azure sanal makineler için önceden ödeme tarafından paradan tasarruf](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reservation"></a>Bir ayırma için kapsamı değiştirin
 Ayırma indirim, ayırma eşleşen ve ayırma kapsamı içinde çalıştırılan sanal makineler için geçerlidir. Ayırma kapsamı tek bir abonelik veya tüm abonelikler, faturalama bağlamda olabilir. Tek bir abonelik için kapsam ayarlarsanız, seçilen abonelikte çalışan sanal makinelere ayırma eşleştirilir. Fatura bağlamında tüm abonelikler çalışan sanal makineleri ayırmaya paylaşılan, Azure eşleşmeler kapsamı ayarlarsanız. Fatura içerik ayırma satın almak için kullanılan abonelikte bağlıdır. Daha fazla bilgi için bkz: [ayrılmış VM örnekleri olan VM'ler için ön ödeme](https://go.microsoft.com/fwlink/?linkid=861721).

Ayırma kapsamı güncelleştirmek için: 
1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Seçin **daha fazla hizmet** > **ayırmaları**.
3. Ayırma seçin.
4. Seçin **ayarları** > **yapılandırma**.
5. Kapsamı değiştirin. Gelen tek bir kapsam için paylaşılan değiştirirseniz, yalnızca sahibi olduğunuz abonelikleri seçebilirsiniz. Yalnızca abonelikleri ayırma ile aynı fatura bağlamında seçilebilir. Fatura içerik ayırma satın zaman seçtiğiniz abonelik tarafından belirlenir. Kapsam yalnızca teklifinden Kullandıkça Öde teklifine MS-AZR - 0003 P abonelikleri ve kurumsal teklif MS-AZR - 0017 P abonelikler için geçerlidir. Kurumsal Anlaşma için geliştirme ve test abonelikleri ayırma indirim almak uygun değildir.

## <a name="split-a-single-reservation-into-two-reservations"></a>Tek bir ayırma iki ayırmaları içine bölme
 Birden fazla örneği satın sonra bir ayırma içindeki örnekleri farklı aboneliklere atamak isteyebilirsiniz. Varsayılan olarak, tüm örnekler (satın alma sırasında belirtilen miktarı) sahip bir kapsam - ya da tek bir abonelik veya paylaşılan. Örneğin, 10 standart D2 VM'ler satın alınan ve belirtilen abonelik A. olmasını kapsamı Şimdi kapsam 7 ayrılmış VM örnekleri için bir abonelik için değiştirmek isteyebilirsiniz ve kalan 3 aboneliğine B. bölme bir ayırma ayrıntılı kapsam yönetimi için örnek dağıtmanızı sağlar. Paylaşılan kapsam seçerek aboneliklere ayırma basitleştirebilirsiniz. Ancak maliyet Yönetimi veya bütçe amacıyla belirli aboneliklere miktarları tahsis edebilirsiniz.

 PowerShell'i, CLI, ancak iki ayırmaları içine bir ayırma bölebilirsiniz veya API'si aracılığıyla.

### <a name="split-a-reservation-by-using-powershell"></a>PowerShell kullanarak bir ayırma bölme
1. Aşağıdaki komutu çalıştırarak ayırma düzeni kimliği alın:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```
2. Bir ayırması ayrıntıları:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. İki içine ayırma bölme ve örnekleri dağıtabilirsiniz:

    ```powershell
    # Split the reservation. The sum of the Reserved VM instances, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Aşağıdaki komutu çalıştırarak kapsamı güncelleştirebilirsiniz:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Ekleme veya ayırma yöneten kullanıcılar değiştirme
Ayırma rollerinde kişiler ekleyerek bir ayırma yönetim hizmeti temsilci seçebilirsiniz. Varsayılan olarak, ayırma satın kişi ve Hesap Yöneticisi üzerinde ayırma sahip rolünü sahip. 

Erişim ayırmaları bağımsız olarak ayırma indirim almak aboneliklerden yönetebilirsiniz. Birisi bir ayırma yönetme izni verdiğinizde, bunları aboneliğinizi yönetmek için hakları verin değil. Ve birisi ayırma 's kapsamdaki bir aboneliği yönetme izni verirseniz, bunları ayırma yönetme hakkı verin değil.
 
Erişim yönetimi bir ayırma için temsilci atamak için: 
1.  [Azure Portal](https://portal.azure.com)’da oturum açın.
2.  Seçin **daha Hizmetleri** > **ayırma** erişiminiz listesi ayırmaları için.
3.  Diğer kullanıcılara erişim vermek istediğiniz ayırma seçin.
4.  Seçin **erişim denetimi (IAM)** menüde.
5.  Seçin **Ekle** > **rol** > **sahibi** (veya sınırlı erişim vermek istiyorsanız, farklı bir rol). 
6. Sahibi olarak eklemek istediğiniz kullanıcının e-posta adresini yazın. 
7. Kullanıcıyı seçin ve ardından **kaydetmek**.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala daha fazla, sorularınız varsa [desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) hızla çözümlenen sorunu almak için.
