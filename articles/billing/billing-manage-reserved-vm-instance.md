---
title: Azure ayırmalarını yönetme | Microsoft Docs
description: Abonelik kapsamını değiştirmek ve erişimi yönetmek için Azure ayırmaları nasıl öğrenin.
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
ms.date: 08/08/2018
ms.author: yashesvi
ms.openlocfilehash: 2283a12845a3b334e29e3f48cbadb99ad508e459
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301590"
---
# <a name="manage-reservations-for-resources-in-azure"></a>Azure'daki kaynakları ayırmalarını yönetme

Azure rezervasyon satın alma sonra gerekebilir ayırma farklı bir aboneliğe uygulamak için kimin ayırma yönetmek veya ayırma kapsamını değiştirmek değiştirin. Ayrıca, bir ayırma satın aldığınız örnekleri bazı başka bir aboneliğe uygulamak için iki ayırmalar halinde bölebilirsiniz.

Azure ayrılmış sanal makine örnekleri satın aldıysanız, ayırma için İyileştir ayarı değiştirebilirsiniz. Ayırma indirimi, aynı dizide Vm'lere uygulayabilir veya belirli bir VM boyutu için veri merkezi kapasite ayırabilirsiniz.

## <a name="change-the-scope-for-a-reservation"></a>Rezervasyon için kapsam değiştirme

 Ayırma indirimi, sanal makineler, SQL veritabanları veya rezervasyonunuzun eşleşen ve ayırma kapsamı içinde çalıştırılan diğer kaynaklar için geçerlidir. Ayırma kapsamı tek bir abonelik veya tüm abonelikler, faturalama bağlamında olabilir. Tek bir abonelik için kapsamı ayarlarsanız, seçili Abonelikteki kaynaklar çalışan ayırma eşleştirilir. Paylaşılan kapsam ayarlarsanız, Azure faturalandırma bağlam içinde tüm abonelikleri çalışan kaynaklar ayırmaya eşleşir. Fatura bağlamı, rezervasyon satın almak için kullanılan abonelik üzerinde bağlıdır.

Rezervasyon kapsamı güncelleştirilemedi:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayırma seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Kapsamını değiştirin. 

Gelen tek bir kapsam için paylaşılan değiştirirseniz, yalnızca sahibi olduğu abonelikleri seçebilirsiniz. Yalnızca abonelikleri ayırma olarak fatura aynı bağlam içinde seçilebilir.

Kapsamı yalnızca Kullandıkça Öde teklifi MS-AZR - 0003 P, MS-AZR - 0017 P Kurumsal teklif veya CSP aboneliği türleri için geçerlidir. Kurumsal anlaşmalar için geliştirme ve test abonelikleri ayırma indirim almak uygun değildir.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Ekleme veya ayırma yönetebilen kullanıcılar değiştirme

Bir ayırmanın yönetim rollerine rezervasyonu kişiler ekleyerek devredebilirsiniz. Varsayılan olarak, kişi, rezervasyon satın ve Hesap Yöneticisi, üzerinde ayırma sahip rolüne sahip.

Erişim ayırmaları bağımsız olarak ayırma indirimi alma aboneliklerden yönetebilirsiniz. Birisi bir ayırma yönetme izni verdiğinizde, bunları aboneliğinizi yönetmek için hakları verin değil. Ve birisi ayırma'nın kapsamı içinde bir aboneliği yönetme izni verirseniz, bunları ayırma yönetme hakkı vermez.

Erişim yönetimi için bir ayırma temsilci atamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırma** erişiminiz listesi ayırmalar için.
3. Diğer kullanıcılara erişim vermek istediğiniz ayırma seçin.
4. Seçin **erişim denetimi (IAM)**.
5. Seçin **ekleme** > **rol** > **sahibi**. Veya sınırlı erişim vermek istiyorsanız, farklı bir rol seçin.
6. Sahip olarak eklemek istediğiniz kullanıcının e-posta adresini yazın.
7. Kullanıcıyı ve ardından **Kaydet**'i seçin.

## <a name="split-a-single-reservation-into-two-reservations"></a>Tek bir ayırma iki ayırmalar halinde bölme

 Birden fazla kaynak örneği içindeki bir ayırma satın sonra bu ayırma örneklerinden farklı aboneliklere atamak isteyebilirsiniz. Varsayılan olarak, tüm örnekleri bir kapsamı - ya da tek bir abonelik veya paylaşılan. Örneğin, 10 standart D2 VM satın alınan ve belirtilen abonelik a için kapsamı Artık abonelik A için 7 ayırmaları kapsamını değiştirmek isteyebilirsiniz ve ayrıntılı kapsam yönetimi için örnek dağıtmak kalan abonelik b bölme bir ayırma 3 sağlar. Paylaşılan kapsam seçerek aboneliklere ayırma basitleştirebilir. Ancak, maliyet Yönetimi'ni veya ödemeyle amaçları için özel abonelikler miktarlar ayırabilirsiniz.

 PowerShell, CLI, ancak iki ayırmalar halinde ayırma bölebilir veya API üzerinden.

### <a name="split-a-reservation-by-using-powershell"></a>PowerShell kullanarak bir ayırma Böl

1. Rezervasyon sipariş kimliği, aşağıdaki komutu çalıştırarak alın:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```

2. Bir ayırmanın ayrıntılarını alın:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Ayırma iki halinde bölme ve örnekleri dağıtabilirsiniz:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Aşağıdaki komutu çalıştırarak kapsamı güncelleştirebilirsiniz:
    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Ayrılmış VM örnekleri için ayar değişiklik iyileştirin

 Ayrılmış VM örneği satın aldığınızda, örneği boyutu esnekliği veya kapasite önceliği seçin. Örnek boyutu esneklik, aynı diğer VM'ler için ayırma indirimi geçerlidir [VM boyutu grubu](https://aka.ms/RIVMGroups). Kapasite önceliği dağıtımlarınız için veri merkezi kapasitenizi ayırır. Bu seçenek, ihtiyaç duyduğunuzda sanal makine örneklerini başlatma yeteneğinizi ek güvence sunar.

Rezervasyon kapsamı paylaşıldığında, varsayılan olarak, örnek boyutu esneklik açıktır. Veri Merkezi kapasitenizi VM dağıtımları için öncelik değildir.

Kapsamı tek olduğu için ayırmaları, sanal makine örneği boyutu esnekliği yerine kapasite önceliği ayırmasını iyileştirebilirsiniz.

Ayırma için İyileştir ayarını güncelleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **ayırmaları**.
3. Ayırma seçin.
4. **Ayarlar** > **Yapılandırma**'yı seçin.
5. Değişiklik **için en iyi duruma getirme** ayarı.

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure ayrılmış VM örnekleri ile sanal makineler için ön ödeme](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL veritabanı'nın ayrılmış kapasite ile SQL veritabanı bilgi işlem kaynakları için ön ödeme](../sql-database/sql-database-reserved-capacity.md)
- [VM ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Nasıl diğer ayırma indirimi uygulanmaz anlama](billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
