---
title: Yazılım planı indirim - Azure | Microsoft Docs
description: Yazılım planı indirimleri sanal makinelerde yazılım nasıl uygulanır öğrenin.
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2019
ms.author: banders
ms.openlocfilehash: bcbf5ab48f3476a911fc4ade1eb0c395fb335d43
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370240"
---
# <a name="azure-software-plan-discount"></a>Azure yazılım planı indirim

Azure yazılım planları SUSE ve RedHat, dağıtılan sanal makinelere uygulanmasını ayırmaları oluşturulabilir. Yazılım planı indirimi, eşleşen ayırma dağıtılan VM'ler yazılım kullanımını uygulanır.

Bir VM'yi kapatın, indirim varsa otomatik olarak başka bir eşleşen VM'lere uygulanır. Yazılım planı yazılımı çalıştıran bir VM maliyetini kapsar. İşlem, depolama ve ağ gibi diğer ücretleri ayrı olarak ücretlendirilir.

Doğru planı satın almak için VM kullanımının ve bu vm'lerdeki Vcpu sayısı anlamanız gerekir. Kullanım verilerinizi temel alan ne satın almayı planladığınız belirlemenize yardımcı olması için aşağıdaki bölümleri kullanın.

## <a name="how-reservation-discount-is-applied"></a>Ayırma indirimi nasıl uygulanır

Ayırma indirimi olan "*kullanın-BT-veya-kaybetmek-BT*". Her saat için eşleşen kaynak yoksa, bu nedenle, daha sonra ayırma miktarını bu saat için kaybedersiniz. Yerine getirilemiyor kullanılmayan ayrılmış saat iletin.

Bir kaynak kapattığınızda, ayırma indirimini Belirtilen kapsam içinde başka bir eşleşen kaynak otomatik olarak uygular. Belirtilen kapsamda bulunan eşleşen kaynak yok sonra ayrılmış saatleri *kayıp*.

## <a name="review-redhat-vm-usage-before-you-buy"></a>Satın almadan önce RedHat VM kullanımını gözden geçirin.

Ürün adı, kullanım verilerinizi nereden alacağınızı ve aynı türde ve boyutu ile RedHat plan satın alın.

Örneğin, kullanımınızı ürün varsa **Red Hat Enterprise Linux - 1-4 vCPU VM lisans**, satın almalıdır **Red Hat Enterprise Linux** için **1-4 vCPU VM**.

<!--ADD RHEL SCREENSHOT -->

## <a name="review-suse-vm-usage-before-you-buy"></a>Satın almadan önce SUSE VM kullanımını gözden geçirin.

Ürün adı, kullanım verilerinizi nereden alacağınızı ve aynı türde ve boyutu ile SUSE plan satın alın.

Örneğin, kullanımınız için ürün ise **SUSE Linux Enterprise Server öncelik - 2-4 vCPU VM desteği**, satın almalıyım **SUSE Linux Enterprise Server öncelik** için **2-4 vCPU**.

![Satın almak için bir ürün seçme örneği](./media/billing-understand-suse-reservation-charges/select-suse-linux-enterprise-server-priority-2-4-vcpu.png)

## <a name="discount-applies-to-different-vm-sizes-for-suse-plans"></a>İndirimi, farklı VM boyutları SUSE planlar için geçerlidir.

Ayrılmış VM örnekleri, SUSE planlama gibi satın alma işlemleri örneği boyutu esnekliği sunar. Başka bir deyişle, hatta farklı vCPU sayısı ile bir VM dağıtırken, indirim uygulanır. Yazılım planı içinde farklı VM boyutları indirimi geçerlidir.

İndirim tutarı aşağıdaki tablolarda listelenenler oranına bağlıdır. Bu gruptaki her bir ölçüm için göreli ayak izini oranı karşılaştırır. VM Vcpu oranına bağlıdır. Kaç tane sanal makine örnekleri SUSE Linux planı indirim almak hesaplamak için oran değerini kullanın.

Örneğin, bir plan için SUSE Linux Enterprise Server HPC önceliğini 3 veya 4 Vcpu ile bir VM için satın alırsanız, bu ayırma oranı 2'dir. İndirim SUSE yazılım maliyetini kapsar:

- 2 Vm'lerinde 1 veya 2 Vcpu ile dağıtılan,
- 1 ile 3 veya 4 Vcpu VM dağıtıldı,
- veya bir VM ile 5 veya daha fazla Vcpu 0.77 veya hakkında %77.

2.6 5 veya daha fazla Vcpu için oranıdır. Bu nedenle yaklaşık %77 yazılım maliyeti, yalnızca bir kısmı 5 veya daha fazla Vcpu ile bir VM ile SUSE için bir ayırma kapsar.

Aşağıdaki tablolarda her biri için yazılım planları için bir ayırma satın alabilir ve bunların ilişkili kullanım ölçümleri oranları gösterilmektedir.

### <a name="suse-linux-enterprise-server-for-hpc-priority"></a>SUSE Linux Enterprise Server HPC önceliği

Azure portal Market adı:

- SLES 12 SP3 HPC (öncelik) için

|SUSE VM | Ölçüm kimliği| Oranı| Örnekte VM boyutu|
| -------| ------------------------| --- |--- |
|SLES HPC 1-2 Vcpu için|e275a668-ce79-44e2-a659-f43443265e98|1|D2s_v3|
|SLES HPC 3-4 Vcpu için|e531e1c0-09c9-4d83-b7d0-a2c6741faa22|2|D4s_v3|
|SLES for HPC 5 + Vcpu|4edcd5a5-8510-49a8-a9fc-c9721f501913|2.6|D8s_v3|

### <a name="suse-linux-enterprise-server-for-hpc-standard"></a>HPC Standard SUSE Linux Enterprise Server

Azure portal Market adı:

- HPC için SLES 12 SP3

|SUSE VM | Ölçüm kimliği | Oranı|Örnekte VM boyutu|
| ------- | --- | ------------------------| --- |
|SLES HPC 1-2 Vcpu için |8c94ad45-b93b-4772-aab1-ff92fcec6610|1|D2s_v3|
|SLES HPC 3-4 Vcpu için|4ed70d2d-e2bb-4dcd-b6fa-42da71861a1c|1.92308|D4s_v3|
|SLES for HPC 5 + Vcpu |907a85de-024f-4dd6-969c-347d47a1bdff|2.92308|D8s_v3|

### <a name="suse-linux-enterprise-server-for-sap-priority"></a>SUSE Linux Enterprise Server SAP önceliği

Azure portal Market adları:

- SLES for SAP (öncelik) 15
- SLES for SAP 12 SP3 (öncelik)
- İçin SLES 12 SP2 SAP (öncelik)

|SUSE VM | Ölçüm kimliği | Oranı|Örnekte VM boyutu|
| ------- |------------------------| --- | --- |
|SLES SAP Priority 1-2 Vcpu için|497fe0b6-fa3c-4e3d-a66b-836097244142|1|D2s_v3|
|SLES SAP Priority 3-4 Vcpu için |847887de-68ce-4adc-8a33-7a3f4133312f|2|D4s_v3|
|SLES SAP Priority 5 + Vcpu için |18ae79cd-dfce-48c9-897b-ebd3053c6058|2.41176|D8s_v3|

### <a name="suse-linux-enterprise-server-priority"></a>SUSE Linux Enterprise Server önceliği

Azure portal Market adları:

- SLES (ÖNCELİK) 15
- SLES 12 SP3 (öncelik)
- SLES 11 SP4 (öncelik)

|SUSE VM | Ölçüm kimliği | Oranı|Örnekte VM boyutu|
| ------- |------------------------| --- |--- |
|SLES 1 vCPU|462cd632-ec6b-4663-b79f-39715f4e8b38|1|B1ms|
|SLES 2-4 Vcpu |924bee71-5eb8-424f-83ed-a58823c33908|2|D4s_v3|
|SLES 2-4 Vcpu |60b3ae9d-e77a-46b2-9cdf-92fa87407969|2|D4s_v3|
|SLES 6 Vcpu |e8862232-6131-4dbe-bde4-e2ae383afc6f|3||
|SLES 8 Vcpu |e11331a8-fd32-4e71-b60e-4de2a818c67a|3.2|D8s_v3|
|SLES 12 core Vcpu |a5afd00d-d3ef-4bcd-8b42-f158b2799782|3.2||
|SLES 16 Vcpu |bb21066f-fe46-46d3-8006-b326b1663e52|3.2| D16s_v3|
|SLES 20 Vcpu |c5228804-1de6-4BD4-a61c-501d9003acc8|3.2| |
|SLES 24 çekirdek Vcpu |-005d-4075-ac11-822ccde9e8f6|3.2| ND24s|
|SLES 32 Vcpu |180c1a0a-b0a5-4de3-a032-f92925a4bf90|3.2| D32s_v3|
|SLES 40 çekirdek Vcpu |a161d3d3-0592-4956-9b64-6829678b6506|3.2||
|SLES 64 Vcpu |7f5a36ed-d5b5-4732-b6bb-837dbf0fb9d8|3.2| D64s_v3|
|SLES 72 çekirdek Vcpu |93329a72-24d7-4faa-93d9-203f367ed334|3.2|F72s_v2|
|SLES 96 çekirdek Vcpu |2018c3a8-ff13-41f8-b64d-9558c5206547|3.2||
|SLES 128 çekirdek vCPUss |ac27e4d7-44b5-4fee-bc1a-78ac5b4abaf7|3.2| M128ms|

### <a name="suse-linux-enterprise-server-standard"></a>SUSE Linux Enterprise Server standart

Azure portal Market adları:

- SLES 15
- SLES 15 (standart)
- SLES 12 SP3 (standart)

|SUSE VM | Ölçüm kimliği | Oranı|Örnekte VM boyutu|
| ------- |------------------------| --- |--- |
|SLES 1-2 Çekirdek Vcpu |4b2fecfc-b110-4312-8f9d-807db1cb79ae|1|D2s_v3|
|SLES 3-4 çekirdek Vcpu |0c3ebb4c-db7d-4125-b45a-0534764d4bda|1.92308|D4s_v3|
|SLES 5 + Vcpu |7b349b65-d906-42e5-833f-b2af38513468|2.30769| D8s_v3|

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyonlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [SUSE yazılım planları ile Azure ayırmalar için ön ödeme](../virtual-machines/linux/prepay-suse-software-charges.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
