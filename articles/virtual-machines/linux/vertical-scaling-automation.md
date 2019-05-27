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
ms.openlocfilehash: 3d9bf155f24c947f8a27a38af01aedcf0b041b94
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966039"
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

> [!NOTE]
> İlk sanal makine boyutları için ölçeklendirilebilir boyutu nedeniyle geçerli sanal makine dağıtılır kullanılabilirlik kümesindeki diğer boyutlarının nedeniyle sınırlanabilir. Bu makalede kullanılan yayımlanan Otomasyon runbook'ları biz ilgileniriz bu durumda ve yalnızca içinde ölçeklendirme VM boyutu çiftleri aşağıda. Bu, bir Standard_D1v2 sanal makine değil aniden işler için standart_g5 için yukarı ölçeklendirilemez veya kaldırılacak Basic_A0 için ölçeği, anlamına gelir. Ayrıca kısıtlı sanal makine boyutları ölçeğini artırmanızı/azaltmanızı desteklenmiyor. Aşağıdaki boyutları çiftleri arasında ölçeklendirme seçebilirsiniz:
> 
> | Ölçeklendirme çifti VM boyutları |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_A1_v2 |Standard_A8_v2 |
> | Standard_A2m_v2 |Standard_A8m_v2  |
> | Standard_B1s |Standard_B2s |
> | Standard_B1ms |Standard_B8ms |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1_v2 |Standard_D5_v2 |
> | Standard_D11_v2 |Standard_D14_v2 |
> | Standard_DS1_v2 |Standard_DS5_v2 |
> | Standard_DS11_v2 |Standard_DS14_v2 |
> | Standard_D2_v3 |Standard_D64_v3 |
> | Standard_D2s_v3 |Standard_D64s_v3 |
> | Standard_DC2s |Standard_DC4s |
> | Standard_E2v3 |Standard_E64v3 |
> | Standard_E2sv3 |Standard_E64sv3 |
> | Standard_F1 |Standard_F16 |
> | Standard_F1s |Standard_F16s |
> | Standard_F2sv2 |Standard_F72sv2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> | Standard_H8 |Standard_H16 |
> | Standard_H8m |Standard_H16m |
> | Standart_L4s |Standart_L32s |
> | Standard_L8s_v2 |Standard_L80s_v2 |
> | İşler için Standard_M8ms  |İşler için standart_m128ms |
> | Standard_M32ls  |İşler için standart_m64ls |
> | İşler için standart_m64s  |İşler için standart_m128s |
> | Standard_M64  |Standard_M128 |
> | Standard_M64m  |Standard_M128m |
> | Standard_NC6 |Standard_NC24 |
> | Standard_NC6s_v2 |Standard_NC24s_v2 |
> | Standard_NC6s_v3 |Standard_NC24s_v3 |
> | Standard_ND6s |Standard_ND24s |
> | Standard_NV6 |Standard_NV24 |
> | Standard_NV6s_v2 |Standard_NV24s_v2 |

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

