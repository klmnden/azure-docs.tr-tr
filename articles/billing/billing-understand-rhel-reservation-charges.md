---
title: Red Hat ayırma planı indirimi ve kullanımı - Azure'ı Anlama | Microsoft Docs
description: Red Hat planı indirimleri sanal makineler Red Hat yazılımı nasıl uygulanır öğrenin.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2019
ms.author: cwatson
ms.openlocfilehash: fe0d0f0baa2b3d1c08e871541dce1511e00f7f87
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370211"
---
# <a name="understand-how-the-red-hat-linux-enterprise-software-reservation-plan-discount-is-applied-for-azure"></a>Azure için Red Hat Linux Kurumsal Yazılım ayırma planı indirimi nasıl uygulanacağını anlama

Red Hat Linux planı satın sonra İndirim dağıtılan Red Hat sanal ayırma eşleşen makinelerine (VM'ler) otomatik olarak uygulanır. Red Hat Linux planı, bir Azure sanal makinesinde Red Hat yazılımı çalıştıran maliyetini kapsar.

Doğru Red Hat Linux planı satın almak için hangi Red Hat çalıştırdığınız VM'ler ve bu vm'lerdeki Vcpu sayısı anlamanız gerekir. Ne satın alma planı, kullanım CSV dosyasından belirlemenize yardımcı olması için aşağıdaki bölümleri kullanın.

## <a name="discount-applies-to-different-vm-sizes"></a>İndirimi, farklı VM boyutları için geçerlidir.

Ayrılmış VM örnekleri gibi Red Hat planı satın örneği boyutu esnekliği sunar. Başka bir deyişle, hatta farklı vCPU sayısı ile bir VM dağıtırken, indirim uygulanır. Yazılım planı içinde farklı VM boyutları indirimi geçerlidir.

İndirim tutarı aşağıdaki tablolarda listelenenler oranına bağlıdır. Bu gruptaki her bir ölçüm için göreli ayak izini oranı karşılaştırır. VM Vcpu oranına bağlıdır. Red Hat Linux planı indirim kaç VM örnekleri Al hesaplamak için oran değerini kullanın.

Örneğin, 3 veya 4 Vcpu ile bir VM için Red Hat Linux Enterprise Server için bir plan satın alırsanız, bu ayırma oranı 2'dir. İndirim Red Hat yazılımı maliyetini kapsar:

- 2 Vm'lerinde 1 veya 2 Vcpu ile dağıtılan,
- 1 ile 3 veya 4 Vcpu VM dağıtıldı,
- veya bir VM ile 5 veya daha fazla Vcpu 0.77 veya hakkında %77.

2\.6 5 veya daha fazla Vcpu için oranıdır. Bu nedenle Red Hat ile 5 veya daha fazla Vcpu ile bir VM için bir ayırma yaklaşık %77 yazılım maliyeti, yalnızca bir kısmını kapsar.

## <a name="understand-red-hat-vm-usage-before-you-buy"></a>Satın almadan önce Red Hat sanal makine kullanımını anlama

Aşağıdaki tablolarda her biri için yazılım planları için bir ayırma satın alabilir ve bunların ilişkili kullanım ölçümleri oranları gösterilmektedir.

### <a name="red-hat-enterprise-linux"></a>Red Hat Enterprise Linux

Azure portal Market adları:

- Red Hat Enterprise Linux 6.7
- Red Hat Enterprise Linux 6.8
- Red Hat Enterprise Linux 6.9
- Red Hat Enterprise Linux 6.10
- Red Hat Enterprise Linux 7.2
- Red Hat Enterprise Linux 7.3
- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.5
- Red Hat Enterprise Linux 7.6
- Red Hat Enterprise Linux 7 (latest lvm)

|Red Hat VM | MeterId| Oranı| Örnekte VM boyutu|
| -------| ------------------------| --- |--- |
|1-4 vCPU VM lisansı|077a07bb-20f8-4bc6-b596-ab7211a1e247|1|D4s_v3|
|1-4 vCPU VM lisansı|2f96d035-3bac-46d6-b2bc-c6daa0938536|1|D4s_v3|
|1-4 vCPU VM lisansı|4831a7b4-bdd4-48a2-8e95-18d053971ede|1|D4s_v3|
|5 + vCPU VM lisansı|291b2cbc-6c34-4e2b-a4e4-1ff8c106f672|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|3b6661c4-03dd-45e7-88c9-512fcb7906d5|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|037eddc0-fedd-4d73-b5d8-92fba9edb831|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|432cdeee-4034-4ddf-9ba4-9250a19b0d5f|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|794dcb90-0793-43e6-9909-70d29974e56d|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|86b5b0b4-3c19-4720-82e9-874f8c58b48e|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|86c35ec3-0a48-426a-9625-22d80e6ea55b|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|8b698c7a-47f1-4cba-8ae1-9853d5ad562d|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|a4daffb4-96f4-4fc5-b1e6-fd3a2cf3595e|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|a838cfb1-0bd3-4965-84f0-663f49afc2e2|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|99aed7b9-a0a9-4783-b90c-be7c2f3c7e30|2.166666667|D8s_v3|
|5 + vCPU VM lisansı|d09f877e-03b4-48b2-b11a-782b965cff19|2.166666667|D8s_v3|
|44 vCPU VM lisansı|6f44ae85-a70e-44be-83ec-153a0bc23979|2.166666667||
|60 vCPU VM lisansı|b9edcc5b-a429-4778-bc5a-82e7fa07fe55|2.166666667||

### <a name="red-hat-enterprise-linux-for-sap-with-ha"></a>Red Hat Enterprise Linux için yüksek kullanılabilirlik ile SAP

Azure portal Market adı:

|Red Hat VM | MeterId | Oranı|Örnekte VM boyutu|
| ------- | --- | ------------------------| --- | --- |
|1-4 vCPU VM lisansı |4d902611-eed7-4060-a33e-3c7fdbac6406|1|D4s_v3|
|5 + vCPU VM lisansı|6dfb482b-23ea-487f-810c-e66360f025de|2.333333333|D8s_v3|

### <a name="red-hat-enterprise-linux-with-ha"></a>HA özellikli Red Hat Enterprise Linux

Azure portal Market adları:

|Red Hat VM | MeterId | Oranı|Örnekte VM boyutu|
| ------- |------------------------| --- | --- |
|1-4 vCPU VM lisansı|e9711132-d9d9-450c-8203-25cfc4bce8de|1|D4s_v3|
|5 + vCPU VM lisansı|93954aa4-b55f-4b7b-844d-a119d6bf3c4e|2|D8s_v3|

### <a name="rhel-for-sap-business-applications"></a>RHEL for SAP Business Applications

Azure portal Market adları:

- Red Hat Enterprise Linux 6,8 SAP iş uygulamaları için
- Red Hat Enterprise Linux 7.3 SAP iş uygulamaları için
- SAP için Red Hat Enterprise Linux 7.4
- SAP için Red Hat Enterprise Linux 7.5

|Red Hat VM | MeterId | Oranı|Örnekte VM boyutu|
| ------- |------------------------| --- |--- |
|1 vCPU VM lisansı|25889e91-c740-42ac-bc52-6b8f73b98575|1|D2s_v3|
|2 vCPU VM lisansı|2a0c92c8-23a7-4dc9-a39c-c4a73a85b5da|1|D2s_v3|
|4 vCPU VM lisansı|875898d3-3639-423c-82c1-38846281b7e8|1|D4s_v3|
|6 vCPU VM lisansı|69a140fa-e08e-415c-85f2-48158e4c73a0|2.166666667||
|8 vCPU VM lisansı|777a5a74-22d6-48c9-9705-ac38fe05a278|2.166666667|D8s_v3|
|12 vCPU VM lisansı|d6b8917a-5127-497a-9f48-1e959df98812|2.166666667||
|16 vCPU VM lisansı|03667e82-e009-425a-83f7-8ebddbca5af4|2.166666667|D16s_v3|
|20 vCPU VM lisansı|bbd65e5b-35f1-42be-b86d-6625fbc1f1a4|2.166666667||
|24 vCPU VM lisansı|c2c07d3e-a7d0-400b-8832-b532bfd0be25|2.166666667|ND24s|
|32 vCPU VM lisansı|633d1494-5ec1-46f0-a742-eaf58eeaec7e|2.166666667|D32s_v3|
|40 vCPU VM lisansı|737142c3-8e4f-4fc1-aa41-05b1661edff8|2.166666667||
|44 vCPU VM lisansı|722bda73-a8c8-4d04-b96b-541f0bb6c0c4|2.166666667||
|60 vCPU VM lisansı|a22bb342-ba9a-4529-a178-39a92ce770b6|2.166666667||
|64 vCPU VM lisansı|d37c8e17-e5f2-4060-881b-080dd4a8c4ce|2.166666667|64s_v3|
|72 vCPU VM lisansı|14341b96-e92c-4dca-ba66-322c88a79aa6|2.166666667|F72s_v2|
|96 vCPU VM lisansı|8b2e5cb8-0362-4cbf-a30a-115e8d6dbc49|2.166666667||
|128 Vcpu'lu VM lisansı|9b198a68-974a-47a7-9013-49169ac0f2e9|2.166666667| M128ms|

### <a name="rhel-for-sap-hana"></a>RHEL for SAP HANA

Azure portal Market adları:

- SAP HANA için Red Hat Enterprise Linux 6.7
- SAP HANA için Red Hat Enterprise Linux 7.2
- SAP HANA için Red Hat Enterprise Linux 7.3

|Red Hat VM | MeterId | Oranı|Örnekte VM boyutu|
| ------- |------------------------| --- |--- |
|1 vCPU VM lisansı|be0a59d1-eed7-47ec-becd-453267753793|1|D2s_v3|
|2 vCPU VM lisansı|3b97c9f5-f5d5-4fd3-a421-b78fca32a656|1|D2s_v3|
|4 vCPU VM lisansı|b39feb58-57bf-40f2-8193-f4fe9ac3dda3|1|D4s_v3|
|6 vCPU VM lisansı|a5963812-0f5a-4053-8ace-2b5babd15ed8|2.166666667||
|8 vCPU VM lisansı|5460ab4d-ce9a-46af-8ad5-ca5e53d715b5|2.166666667|D8s_v3|
|12 vCPU VM lisansı|0e3bc72d-a888-4bcf-8437-119f763a3215|2.166666667||
|16 vCPU VM lisansı|b40e95d8-3176-42f0-967c-497785c031b2|2.166666667|D16s_v3|
|20 vCPU VM lisansı|81f34277-499d-40a3-a634-99adc08e2d45|2.166666667||
|24 vCPU VM lisansı|e03f1906-d35d-4084-b2cd-63281869c8ee|2.166666667|ND24s|
|32 vCPU VM lisansı|0a58c082-ceb8-4327-9b64-887c30dddb23|2.166666667|D32s_v3|
|40 vCPU VM lisansı|a14225c0-04e6-4669-974f-e2ddd61a9c5b|2.166666667||
|44 vCPU VM lisansı|378b8125-d8a5-4e09-99bc-c1462534ffb0|2.166666667||
|60 vCPU VM lisansı|5d7db11a-54e9-404e-aaa8-509fac7c0638|2.166666667||
|64 vCPU VM lisansı|3c8157b2-a57d-45ce-ba02-bd86e9209795|2.166666667|64s_v3|
|72 vCPU VM lisansı|5e87a3ee-7afb-4040-b8d9-b109ddb38f31|2.166666667|F72s_v2|
|96 vCPU VM lisansı|b13895fc-0d06-4de9-b860-627c471cd247|2.166666667||
|128 Vcpu'lu VM lisansı|6e67ac0b-19d3-4289-96df-05d0093d4b3b|2.166666667| M128ms|

## <a name="next-steps"></a>Sonraki adımlar

Rezervasyonlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure için ayırmaları nelerdir](billing-save-compute-costs-reservations.md)
- [Red Hat yazılımı planlarıyla Azure ayırmalar için ön ödeme](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure için ayırmaları yönetme](billing-manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Kurumsal kayıt için ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
