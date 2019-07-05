---
title: Azure Otomasyonu ile Azure sanal makine dikey olarak ölçeklendirme | Microsoft Docs
description: Bir Linux sanal makinesini izleme uyarıları Azure Otomasyonu ile yanıt dikey olarak ölçeklendirme
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e7190f13f9d41afffcbbc1104f533b0d0586a0d6
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486024"
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a>Azure Otomasyonu ile Azure Linux sanal makine dikey olarak ölçeklendirme
Dikey ölçeklendirme artan veya azalan bir makine yanıt iş yükü olarak kaynaklarını işlemidir. Azure'da bu sanal makinenin boyutunu değiştirerek gerçekleştirilebilir. Bu, aşağıdaki senaryolarda yardımcı olabilir

* Sanal makine sık kullanılmayan, aylık maliyetlerinizi azaltmak için daha küçük bir boyut boyutlandırabilirsiniz
* Sanal makinenin en yoğun yük gördüğünü, bu kapasitesini artırmak için daha büyük boyutlara yeniden boyutlandırılabilir

Bunu başarmak adımlar için ana hat gibidir aşağıda

1. Azure Otomasyonu, sanal makinelerinizi erişmek için Kurulum
2. Aboneliğinize Azure Otomasyon dikey ölçeklendirme runbook'ları alma
3. Runbook'unuza bir Web kancası Ekle
4. Sanal makinenize bir uyarı Ekle

## <a name="scale-limitations"></a>Ölçek sınırlamaları

İlk sanal makine boyutları için ölçeklendirilebilir boyutu nedeniyle geçerli sanal makine dağıtılır kullanılabilirlik kümesindeki diğer boyutlarının nedeniyle sınırlanabilir. Bu makalede kullanılan yayımlanan Otomasyon runbook'ları biz ilgileniriz bu durumda ve yalnızca içinde ölçeklendirme VM boyutu çiftleri aşağıda. Bu, bir Standard_D1v2 sanal makine değil aniden işler için standart_g5 için yukarı ölçeklendirilemez veya kaldırılacak Basic_A0 için ölçeği, anlamına gelir. Ayrıca kısıtlı sanal makine boyutları ölçeğini artırmanızı/azaltmanızı desteklenmiyor. 

Aşağıdaki boyutları çiftleri arasında ölçeklendirme seçebilirsiniz:

* [A serisi](#a-series)
* [B serisi](#b-series)
* [D serisi](#d-series)
* [E serisi](#e-series)
* [F serisi](#f-series)
* [G serisi](#g-series)
* [H serisi](#h-series)
* [L serisi](#l-series)
* [M serisi](#m-series)
* [N serisi](#n-series)

### <a name="a-series"></a>A Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Basic_A0 | Basic_A1 |
| Basic_A1 | Basic_A2 |
| Basic_A2 | Basic_A3 |
| Basic_A3 | Basic_A4 |
| Standard_A0 | Standard_A1 |
| Standard_A1 | Standard_A2 |
| Standard_A2 | Standard_A3 |
| Standard_A3 | Standard_A4 |
| Standard_A5 | Standard_A6 |
| Standard_A6 | Standard_A7 |
| Standard_A8 | Standard_A9 |
| Standard_A10 | Standard_A11 |
| Standard_A1_v2 | Standard_A2_v2 |
| Standard_A2_v2 | Standard_A4_v2 |
| Standard_A4_v2 | Standard_A8_v2 |
| Standard_A2m_v2 | Standard_A4m_v2 |
| Standard_A4m_v2 | Standard_A8m_v2 |

### <a name="b-series"></a>B Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_B1s | Standard_B2s |
| Standard_B1ms | Standard_B2ms |
| Standard_B2ms | Standard_B4ms |
| Standard_B4ms | Standard_B8ms |

### <a name="d-series"></a>D Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_D1 | Standard_D2 |
| Standard_D2 | Standard_D3 |
| Standard_D3 | Standard_D4 |
| Standard_D11 | Standard_D12 |
| Standard_D12 | Standard_D13 |
| Standard_D13 | Standard_D14 |
| Standard_DS1 | Standard_DS2 |
| Standard_DS2 | Standard_DS3 |
| Standard_DS3 | Standard_DS4 |
| Standard_DS11 | Standard_DS12 |
| Standard_DS12 | Standard_DS13 |
| Standard_DS13 | Standard_DS14 |
| Standard_D1_v2 | Standard_D2_v2 |
| Standard_D2_v2 | Standard_D3_v2 |
| Standard_D3_v2 | Standard_D4_v2 |
| Standard_D4_v2 | Standard_D5_v2 |
| Standard_D11_v2 | Standard_D12_v2 |
| Standard_D12_v2 | Standard_D13_v2 |
| Standard_D13_v2 | Standard_D14_v2 |
| Standard_DS1_v2 | Standard_DS2_v2 |
| Standard_DS2_v2 | Standard_DS3_v2 |
| Standard_DS3_v2 | Standard_DS4_v2 |
| Standard_DS4_v2 | Standard_DS5_v2 |
| Standard_DS11_v2 | Standard_DS12_v2 |
| Standard_DS12_v2 | Standard_DS13_v2 |
| Standard_DS13_v2 | Standard_DS14_v2 |
| Standard_D2_v3 | Standard_D4_v3 |
| Standard_D4_v3 | Standard_D8_v3 |
| Standard_D8_v3 | Standard_D16_v3 |
| Standard_D16_v3 | Standard_D32_v3 |
| Standard_D32_v3 | Standard_D64_v3 |
| Standard_D2s_v3 | Standard_D4s_v3 |
| Standard_D4s_v3 | Standard_D8s_v3 |
| Standard_D8s_v3 | Standard_D16s_v3 |
| Standard_D16s_v3 | Standard_D32s_v3 |
| Standard_D32s_v3 | Standard_D64s_v3 |
| Standard_DC2s | Standard_DC4s |

### <a name="e-series"></a>E Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_E2_v3 | Standard_E4_v3 |
| Standard_E4_v3 | Standard_E8_v3 |
| Standard_E8_v3 | Standard_E16_v3 |
| Standard_E16_v3 | Standard_E20_v3 |
| Standard_E20_v3 | Standard_E32_v3 |
| Standard_E32_v3 | Standard_E64_v3 |
| Standard_E2s_v3 | Standard_E4s_v3 |
| Standard_E4s_v3 | Standard_E8s_v3 |
| Standard_E8s_v3 | Standard_E16s_v3 |
| Standard_E16s_v3 | Standard_E20s_v3 |
| Standard_E20s_v3 | Standard_E32s_v3 |
| Standard_E32s_v3 | Standard_E64s_v3 |

### <a name="f-series"></a>F Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_F1 | Standard_F2 |
| Standard_F2 | Standard_F4 |
| Standard_F4 | Standard_F8 |
| Standard_F8 | Standard_F16 |
| Standard_F1s | Standard_F2s |
| Standard_F2s | Standard_F4s |
| Standard_F4s | Standard_F8s |
| Standard_F8s | Standard_F16s |
| Standard_F2s_v2 | Standard_F4s_v2 |
| Standard_F4s_v2 | Standard_F8s_v2 |
| Standard_F8s_v2 | Standard_F16s_v2 |
| Standard_F16s_v2 | Standard_F32s_v2 |
| Standard_F32s_v2 | Standard_F64s_v2 |
| Standard_F64s_v2 | Standard_F7s_v2 |

### <a name="g-series"></a>G Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_G1 | Standard_G2 |
| Standard_G2 | Standard_G3 |
| Standard_G3 | Standard_G4 |
| Standard_G4 | Standard_G5 |
| Standard_GS1 | Standard_GS2 |
| Standard_GS2 | Standard_GS3 |
| Standard_GS3 | Standard_GS4 |
| Standard_GS4 | Standard_GS5 |

### <a name="h-series"></a>H serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_H8 | Standard_H16 |
| Standard_H8m | Standard_H16m |

### <a name="l-series"></a>L Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standart_L4s | Standart_L8s |
| Standart_L8s | Standart_L16s |
| Standart_L16s | Standart_L32s |
| Standard_L8s_v2 | Standard_L16s_v2 |
| Standard_L16s_v2 | Standard_L32s_v2 |
| Standard_L32s_v2 | Standard_L64s_v2 |
| Standard_L64s_v2 | Standard_L80s_v2 |

### <a name="m-series"></a>M Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| İşler için Standard_M8ms | İşler için Standard_M16ms |
| İşler için Standard_M16ms | Standard_M32ms |
| Standard_M32ms | Standard_M64ms |
| Standard_M64ms | İşler için standart_m128ms |
| Standard_M32ls | İşler için standart_m64ls |
| İşler için standart_m64s | İşler için standart_m128s |
| Standard_M64 | Standard_M128 |
| Standard_M64m | Standard_M128m |

### <a name="n-series"></a>N Serisi

| Başlangıç boyutu | Boyutu ölçeği | 
| --- | --- |
| Standard_NC6 | Standard_NC12 |
| Standard_NC12 | Standard_NC24 |
| Standard_NC6s_v2 | Standard_NC12s_v2 |
| Standard_NC12s_v2 | Standard_NC24s_v2 |
| Standard_NC6s_v3 | Standard_NC12s_v3 |
| Standard_NC12s_v3 | Standard_NC24s_v3 |
| Standart_nd6 | Standart_nd12 |
| Standart_nd12 | Standart_nd24 |
| Standard_NV6 | Standard_NV12 |
| Standard_NV12 | Standard_NV24 |
| Standard_NV6s_v2 | Standard_NV12s_v2 |
| Standard_NV12s_v2 | Standard_NV24s_v2 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Azure Otomasyonu, sanal makinelerinizi erişmek için Kurulum
Yapmanız gereken ilk şey, Ölçek VM ölçek kümesi örneklerine kullanılan runbook'ları barındıracak bir Azure Otomasyonu hesabı oluşturmaktır. Yakın zamanda Otomasyon hizmetini ayarı oluşturan hizmet sorumlusunu otomatik olarak runbook'ları kullanıcı adına çok kolay çalıştırmak için getiren "Farklı Çalıştır hesabı" özelliğini kullanıma sunduk. Daha fazla bilgiyi bu konuda aşağıdaki makalede:

* [Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Aboneliğinize Azure Otomasyon dikey ölçeklendirme runbook'ları alma
Sanal makinenizi dikey yönde ölçeklendirmek için gerekli olan runbook'ları, Azure Otomasyonu Runbook Galerisi'nde zaten yayımlanır. Aboneliğinize aktarmak gerekir. Makaleyi okuyarak, runbook'ları içeri aktarma öğrenebilirsiniz.

* [Azure Otomasyonu için runbook ve modül galerileri](../../automation/automation-runbook-gallery.md)

Aşağıdaki resimde gösterilen içeri aktarılması gereken runbook'ları

![Runbook'ları içeri aktarma](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Runbook'unuza bir Web kancası Ekle
Aktardıktan sonra bir sanal makineden bir uyarı tetiklenebilir için runbook'ları gerekir bir Web kancası runbook'a ekleyin. Runbook için bir Web kancası oluşturma ayrıntıları burada okuyabilirsiniz

* [Azure Otomasyonu Web kancaları](../../automation/automation-webhooks.md)

Bu sonraki bölümde ihtiyacınız olacak şekilde, Web kancası iletişim kutusunu kapatmadan önce Web kancası kopyaladığınızdan emin olun.

## <a name="add-an-alert-to-your-virtual-machine"></a>Sanal makinenize bir uyarı Ekle
1. Sanal makine ayarlarını seçin
2. "Uyarı kuralları" seçin
3. "Uyarı" Ekle
4. Uyarı üzerinde harekete geçirmek için bir ölçüm seçin
5. Bir koşul Seç olan yerine olacak uyarı ateşlenmesine neden
6. Koşul için bir eşik 5. Adım'ı seçin. yerine getirilmesi için
7. İzleme hizmeti üzerinde denetleyecek bir dönem için koşulu ve eşik adımları 5 ve 6'seçin
8. Önceki bölümde kopyaladığınız Web kancası yapıştırın.

![Sanal makine için 1 uyarı Ekle](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![2 sanal makine için uyarı Ekle](./media/vertical-scaling-automation/add-alert-webhook-2.png)

