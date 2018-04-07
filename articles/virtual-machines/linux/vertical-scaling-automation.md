---
title: Azure Otomasyonu ile Azure sanal makine dikey olarak ölçeklendirmek | Microsoft Docs
description: Azure Otomasyonu uyarılarla izleme yanıt dikey olarak Linux sanal makine ölçeklendirme
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
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 683348c907484ccd9394eb4aae18e9006ecb5c48
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a>Dikey olarak Azure Automation ile Azure Linux sanal makine ölçekleme
Dikey ölçekleme artan veya azalan bir makine yanıt iş yükü olarak kaynakları işlemidir. Azure'da bu sanal makine boyutunu değiştirerek gerçekleştirilebilir. Bu aşağıdaki senaryolarda yardımcı olabilir

* Sanal makine sık kullanılmayan, aylık, maliyetleri azaltmak için daha küçük bir boyuta aşağıya doğru boyutlandırabilirsiniz
* Sanal makine yoğun yük görmesini, onu kapasitesini artırmak için daha büyük bir boyuta yeniden boyutlandırılabilir

Bunu gerçekleştirmek için gereken adımları anahat gibidir aşağıda

1. Sanal makinelerinize erişmek için Kurulum Azure Otomasyonu
2. Aboneliğiniz Azure Otomasyon dikey ölçek runbook'ları içe
3. Bir Web kancası runbook'a ekleyin
4. Sanal makinenize bir uyarı Ekle

> [!NOTE]
> İlk sanal makine için Genişletilebilir boyutları boyutu nedeniyle geçerli sanal makine olarak dağıtılan kümedeki diğer boyutlara kullanılabilirliği nedeniyle sınırlanabilir. Bu makalede kullanılan yayımlanan Otomasyon runbook'ları Biz bu durumda dikkatli ve içinde yalnızca ölçeklendirme VM boyutu çiftleri aşağıda. Bu bir Standard_D1v2 sanal makine değil aniden Standard_G5 için yukarı ölçeklendirilemez ya da Basic_A0 için genişletilmiş olduğunu anlamına gelir.
> 
> | VM boyutları çifti ölçeklendirme |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Sanal makinelerinize erişmek için Kurulum Azure Otomasyonu
Yapmanız gereken ilk şey VM ölçek kümesi örneklerinin ölçeklendirmek için kullanılan runbook'ları barındıracak bir Azure Otomasyonu hesabı oluşturmaktır. Yakın zamanda Otomasyon hizmeti otomatik olarak çalışan runbook'ları kullanıcı adınıza çok kolay için ayar yukarı hizmet sorumlusu getirir "Farklı Çalıştır hesabı" özelliğini kullanıma sunmuştur. Daha fazla bilgiyi bu konuda aşağıdaki makalede:

* [Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Aboneliğiniz Azure Otomasyon dikey ölçek runbook'ları içe
Sanal makineniz dikey ölçekleme için gerekli olan runbook'ları zaten Azure Otomasyonu Runbook Galerisi yayımlanır. Aboneliğinizi aktarmak gerekir. Runbook'ları makalesini okuyarak içeri öğrenebilirsiniz.

* [Azure Otomasyonu Runbook ve modül galerileri](../../automation/automation-runbook-gallery.md)

Aşağıdaki resimde gösterilen içeri aktarılması gereken runbook'ları

![Runbook'ları alma](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Bir Web kancası runbook'a ekleyin
Alınan sonra gerekir runbook'ları bir sanal makineden bir uyarı tarafından tetiklenebilir için bir Web kancası runbook'a ekleyin. Runbook için bir Web kancası oluşturma ayrıntılarını burada okuyabilir

* [Azure Otomasyonu Web kancası](../../automation/automation-webhooks.md)

Bu sonraki bölümde ihtiyaç duyacağınız Web kancası iletişim kutusunu kapatmadan önce Web kancası kopyaladığınızdan emin olun.

## <a name="add-an-alert-to-your-virtual-machine"></a>Sanal makinenize bir uyarı Ekle
1. Sanal makine ayarlarını seçin
2. "Uyarı kuralları" seçeneğini belirleyin
3. "Uyarı Ekle" seçin
4. Hakkında uyarı tetiklenecek bir ölçümü seçin
5. Bir koşul Seç hangi olduğunda yerine tetiklenecek uyarının nedeni
6. Koşul için bir eşik adım 5'te seçin. yerine getirilmesi için
7. Üzerinde izleme hizmeti denetleyecek bir dönem için koşul ve eşik adımları 5 ve 6'seçin
8. Önceki bölümünde kopyaladığınız Web kancası yapıştırın.

![Sanal makine 1 uyarısı ekleme](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Sanal makine 2 uyarısı ekleme](./media/vertical-scaling-automation/add-alert-webhook-2.png)

