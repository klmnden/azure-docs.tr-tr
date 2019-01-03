---
title: Desteklenen işletim sistemleri, kapsayıcı altyapıları - Azure IOT Edge | Microsoft Docs
description: Azure IOT Edge arka plan programı ve çalışma zamanı ve üretim cihazlarınız için desteklenen kapsayıcı altyapıları hangi işletim sistemini çalıştırabileceğiniz öğrenin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6443260de0a8bd8531edb303fa581d281034fef3
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555617"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IOT Edge desteklenen sistemleri

Bir Azure IOT Edge ürün desteği arama için çeşitli yollar vardır.

**Hata Raporlama** – Azure IOT Edge ürün şeklinde ulaştığı geliştirme çoğunu IOT Edge açık kaynaklı proje gerçekleşir. Hatalar rapor üzerinde [sorunlar sayfasında](https://github.com/azure/iotedge/issues) proje. Düzeltmeleri projesi ile aşamalarından geçerek ürün güncelleştirmelerini hızlı bir şekilde yapın.

**Microsoft müşteri destek ekibinin** – sahip kullanıcılar bir [destek planı](https://azure.microsoft.com/support/plans/) Microsoft müşteri destek ekibinin doğrudan bir destek bileti oluşturarak görüşebilirsiniz [Azure portalında](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

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
| Raspbian Uzat | Hayır | Evet|
| Ubuntu Server 16.04 | Evet | Hayır |
| Ubuntu Server 18.04 | Evet | Hayır |

Genel önizleme
| İşletim Sistemi | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core derleme 17763 | Evet | Hayır |
| Windows 10, Windows kapsayıcıları için 17763 oluşturun<br><br>Windows 10 derleme 14393 ya da Linux kapsayıcıları için daha yeni\* | Evet | Hayır |
| Windows Server 2019 Windows kapsayıcıları için<br><br>Windows Server 2016 veya daha yeni Linux kapsayıcıları için\* | Evet | Hayır |

\* Microsoft, yalnızca geliştirme ve test için Windows cihazlarda Linux kapsayıcıları için yükleme paketleri sağlar. Bu, üretim kullanımı için desteklenen bir yapılandırma değildir. 

### <a name="tier-2"></a>Katman 2
Katman 2 sistemleri, Azure IOT Edge ile uyumlu olarak düşünülebilir ve görece bir kolayca kullanılabilir. Bunun anlamı:
* Microsoft platformlarda test geçici yaptı veya başarılı bir şekilde Azure IOT Edge platformunda çalışan bir iş ortağının bilir.
* Diğer platformlar için yükleme paketleri bu platformlarda çalışabilir

| İşletim Sistemi | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Evet | Evet |
| Debian 8 | Evet | Evet |
| Debian 9 | Evet | Evet |
| RHEL 7.5 | Evet | Evet |
| Ubuntu 18.04 | Evet | Evet |
| Ubuntu 16.04 | Evet | Evet |
| 8 Rüzgar Irmağı | Evet | Hayır |
| Yocto | Evet | Hayır |

## <a name="container-engines"></a>Kapsayıcı altyapıları
Azure IOT Edge modülleri, üzerinde çalıştığı işletim sisteminden bağımsız olarak başlatmak için bir kapsayıcı altyapısı gerekir. Microsoft, bu gereksinimi karşılamak için moby-altyapısı, bir kapsayıcı altyapısı sağlar. Bu, Moby açık kaynaklı proje ile ilgili temel alır. Docker CE ve Docker EE diğer popüler kapsayıcı motorlardır. Bunlar ayrıca Moby açılır kaynak projesini temel alıyor ve Azure IOT Edge ile uyumludur. Microsoft, bu kapsayıcı altyapıları kullanarak sistemleri için en iyi girişim desteği sağlar. Ancak Microsoft bunları sorunlarını giderir göndermesine imkan yok. Bu nedenle, üretim sistemlerine moby altyapısı kullanarak Microsoft önerir.

