---
title: Azure IOT Edge platformu desteği | Microsoft Docs
description: Azure IOT Edge tarafından desteklenen platformlar
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 91821d66ac0be265e6b66fd9eb2378169e337430
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42061749"
---
# <a name="azure-iot-edge-support"></a>Azure IOT Edge desteği
Bir Azure IOT Edge ürün desteği arama için çeşitli yollar vardır.

**Hata Raporlama** – Azure IOT Edge ürün şeklinde ulaştığı geliştirme çoğunu IOT Edge açık kaynaklı proje gerçekleşir. Hatalar rapor üzerinde [sorunlar sayfasında](https://github.com/azure/iotedge/issues) proje. Düzeltmeleri projesi ile aşamalarından geçerek ürün güncelleştirmelerini hızlı bir şekilde yapın.

**Microsoft müşteri destek ekibinin** – sahip kullanıcılar bir [destek planı](https://azure.microsoft.com/support/plans/) Microsoft müşteri destek ekibinin doğrudan bir destek bileti oluşturarak görüşebilirsiniz [Azure portalında]( https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Özellik istekleri** – Azure IOT Edge ürün ürünün özellik isteklerini izleyen [User Voice sayfa](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>İşletim sistemleri
Azure IOT Edge, kapsayıcıları çalıştırmak üzere çoğu işletim sistemi üzerinde çalışır; Ancak, tüm bunların eşit olarak desteklenmez. İşletim sistemleri, kullanıcıları bekleyebileceğiniz destek düzeyini temsil eden katmanlarda gruplandırılır.

### <a name="tier-1"></a>Katman 1
Katman 1 sistemleri gibi resmi olarak desteklenen zorlayıcı olabilir. Diğer bir deyişle Microsoft:
* Otomatikleştirilmiş testlerin bu işletim sistemi içerir
* bunların yükleme paketleri sağlar

Genel kullanıma sunuldu
| İşletim Sistemi | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Ubuntu Server 18.04 | Evet | Hayır |
| Ubuntu Server 16.04 | Evet | Hayır |
| Raspbian Uzat | Hayır | Evet|

Genel önizlemeye sunuldu
| İşletim Sistemi | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Windows 10 sunucu 1803 | Evet | Hayır |
| Windows 10 IOT Enterprise (Nisan 2018 güncelleştirmesi) | Evet | Hayır |
| Windows 10 IoT Core (Nisan 2018 güncelleştirmesi) | Evet | Hayır |

### <a name="tier-2"></a>Katman 2
Katman 2 sistemleri, Azure IOT Edge ile uyumlu olarak düşünülebilir ve görece bir kolayca kullanılabilir. Bunun anlamı:
* Microsoft platformlarda test geçici yaptı veya başarılı bir şekilde Azure IOT Edge platformunda çalışan bir iş ortağının bilir.
* Diğer platformlar için yükleme paketleri bu platformlarda çalışabilir

| İşletim Sistemi | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Ubuntu 18.04 | Evet | Hayır |
| Ubuntu 16.04 | Evet | Hayır |
| 8 Rüzgar Irmağı | Evet | Hayır |
| Yocto | Evet | Hayır |
| Debian | Evet | Hayır |
| Mac | Evet | Hayır |

## <a name="container-engines"></a>Kapsayıcı altyapıları
Azure IOT Edge modülleri, üzerinde çalıştığı işletim sisteminden bağımsız olarak başlatmak için bir kapsayıcı altyapısı gerekir. Microsoft, bu gereksinimi karşılamak için moby-altyapısı, bir kapsayıcı altyapısı sağlar. Bu, Moby açık kaynaklı proje ile ilgili temel alır. Docker CE ve Docker EE diğer popüler kapsayıcı motorlardır. Bunlar ayrıca Moby açılır kaynak projesini temel alıyor ve Azure IOT Edge ile uyumludur. Microsoft, bu kapsayıcı altyapıları kullanarak sistemleri için en iyi girişim desteği sağlar. Ancak Microsoft bunları sorunlarını giderir göndermesine imkan yok. Bu nedenle, üretim sistemlerine moby altyapısı kullanarak Microsoft önerir.


<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 
