---
title: Azure Güvenlik Merkezi için IOT güvenlik modül ikizlerini Önizleme anlama | Microsoft Docs
description: Kavramını güvenlik modül ikizlerini ve IOT için Azure Güvenlik Merkezi'nde nasıl kullanıldığı hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 8f964fd2b121c6232081673666f5babb160e1721
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618304"
---
# <a name="security-module"></a>Güvenlik modülü

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, IOT için Azure Güvenlik Merkezi (ASC) cihaz ikizleri ve modüllerini nasıl kullandığı açıklanmaktadır. 

## <a name="device-twins"></a>Cihaz ikizlerini

Azure'da yerleşik IOT çözümleri için cihaz ikizlerini hem süreç otomasyonu, hem de cihaz Yönetimi önemli bir rol oynar.  

ASC IOT için cihaz güvenlik durumunuzu yönetmenize olanak sağlayan tam tümleştirmesi var olan IOT cihaz Yönetimi platformunuz ile sunuyor olun yanı sıra mevcut cihaz denetim özelliklerini kullanın. Tümleştirme, IOT hub'ını kullanın sağlayarak gerçekleştirilir ikizi mekanizması.  

Kavramı hakkında daha fazla bilgi [cihaz ikizlerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins) Azure IOT hub. 

## <a name="security-module-twins"></a>Modül ikizlerini güvenlik

IOT için ASC hizmetindeki her cihaz için bir güvenlik modül ikizi tutar.
Güvenlik modül ikizi tüm bilgileri çözümünüzde belirli her cihaz için cihaz güvenliği ile ilgili tutar.
Cihaz güvenlik özellikleri, bir adanmış güvenlik modül ikizi güncelleştirmeleri ve daha az kaynak gerektiren Bakımı etkinleştirmek için ve güvenli iletişim için korunur.  

Bkz [Oluştur güvenlik modül ikizi](quickstart-create-security-twin.md) ve [güvenlik Denetleyicisilerinin](how-to-agent-configuration.md) nasıl oluşturulacağını öğrenmek için özelleştirme ve ikizi yapılandırın. Bkz: [modül ikizlerini anlama](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) modül ikizlerini IOT hub'ında kavramı hakkında daha fazla bilgi için. 
 

## <a name="see-also"></a>Ayrıca bkz.
- [ASC IOT Önizleme](overview.md)
- [Güvenlik aracılarını dağıtma](how-to-deploy-agent.md)
- [Güvenlik aracı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)