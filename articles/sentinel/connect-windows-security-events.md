---
title: Gözcü Azure Önizleme için Windows Güvenlik Olay verileri bağlayın | Microsoft Docs
description: Azure Gözcü için Windows Güvenlik Olay verileri bağlanmayı öğreneceksiniz.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 188febf090ddb3f685f9d3c3b94d822f15bbcfcb
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673773"
---
# <a name="connect-windows-security-events"></a>Windows güvenlik olaylarını bağlama 

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tüm güvenlik olayları Azure Gözcü çalışma alanınıza bağlı Windows sunucularından akışını yapabilirsiniz. Bu bağlantı, panoları görüntülemesine, özel uyarıları oluşturma ve araştırma geliştirmek sağlar. Bu, kuruluşunuzun ağ daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerinizi geliştirir.  Hangi olayların akışı seçebilirsiniz:

- **Tüm olaylar** -tüm Windows Güvenlik ve AppLocker olayları.
- **Ortak** -olayları denetim amacıyla standart bir dizi. Tam kullanıcı denetim kaydı bu kümesindeki dahil edilir. Örneğin, bu, hem kullanıcı oturum açma ve kullanıcı oturumunuzu olayları (olay kimliği 4634) içerir. Güvenlik grubu değişikliklerini, anahtar etki alanı denetleyicisi Kerberos işlemleri ve sektör kuruluşlar tarafından önerilen diğer olaylar gibi eylemleri denetimi ekliyoruz.

Tüm olayları üzerine miktarının azaltılmasını ve belirli olay filtre için olduğunu seçin için ana motivasyon olarak ortak çok düşük bir birime sahip olayları dahil edilmişti.
- **En az** -küçük bir olayların olası tehditleri gösterebilir. Bu seçenek etkinleştirildiğinde, tam denetim kaydı mümkün olmayacaktır.  Bu, başarılı bir ihlal işaret eden olayları ve çok düşük bir birime sahip önemli olayları kapsar. Örneğin, bu kullanıcı başarılı ve başarısız oturum açma (olay kimliği 4624 4625) içerir, ancak oturum denetimi için önemlidir, ancak algılama için anlamlı ve görece yüksek hacimli olan bilgi içermiyor. Çoğu veri hacmi bu kümenin oturum açma olayları ve işlem oluşturma olayı (olay kimliği 4688) olur.
- **Hiçbiri** -hiçbir güvenlik veya AppLocker olayı.

> [!NOTE]
> 
> - Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

Aşağıdaki listede, her küme için olay kimlikleri güvenlik ve App Locker tam bir dökümünü sağlar:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En az | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

## <a name="set-up-the-windows-security-events-connector"></a>Windows Güvenlik olayları Bağlayıcısı'nı Ayarla

Tam olarak Windows Güvenlik olaylarınız Gözcü Azure ile tümleştirmek için:

1. Gözcü Azure portalında **veri bağlayıcıları** ve ardından **Windows Güvenlik olayları** Döşe. 
1. Akışı yapmak istediğiniz veri türlerini seçin.
1. Tıklayın **güncelleştirme**.
6. İlgili şema için Windows Güvenlik olaylarını Log Analytics'e kullanmak için arama **SecurityEvent**.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Bu, Log Analytics'te görünmesini günlüklerinizi başlatana kadar yaklaşık 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Windows Güvenlik olayları bağlanma öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

