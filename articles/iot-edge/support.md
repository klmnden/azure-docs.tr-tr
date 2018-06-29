---
title: Azure IOT uç Platform desteği | Microsoft Docs
description: Azure IOT kenar tarafından desteklenen platformlar
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a5cbabf8080efd1ae25ba151a1be339e8f5cad2
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081763"
---
# <a name="azure-iot-edge-support"></a>Azure IOT uç desteği
Bir Azure IOT kenar ürün desteğini arama için çeşitli yollar vardır.

**Hata Raporlama** – Azure IOT kenar üründe gider geliştirme çoğunluğu IOT kenar açık kaynaklı proje olur. Bildirilen hatalar üzerinde [sorunlar sayfası](https://github.com/azure/iot-edge/issues) projenin. Düzeltmeleri hızlı bir şekilde kendi şekilde projesi ile ürün güncelleştirmeleri yapın.

**Microsoft müşteri destek ekibinin** – sahip kullanıcılar bir [destek planı](https://azure.microsoft.com/support/plans/) Microsoft müşteri destek ekibi doğrudan bir destek bileti oluşturarak devreye [Azure portal]( https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Özellik istekleri** – Azure IOT Kenar Ürün ürünün özellik istekleri izler [kullanıcı sesi sayfa](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>İşletim Sistemleri
Azure IOT kenar kapsayıcıları çalıştırabilirsiniz çoğu işletim sistemlerinde çalışır; Ancak, bunların tümü eşit olarak desteklenmez. İşletim sistemleri, kullanıcıları bekleyebilirsiniz destek düzeyini temsil eden katmanlara gruplandırılır.

### <a name="tier-1"></a>Katman 1
Katman 1 sistemleri, olarak resmi olarak desteklenen değerlendirilebilir. Bunun anlamı Microsoft:
* Bu işletim sistemi otomatikleştirilmiş testleri içerir
* Bunlar için yükleme paketleri sağlar

Genel olarak kullanılabilir
* Ubuntu Server 18.04
* Ubuntu Server 16.04
* Raspbian Uzat

Genel Önizleme
* Windows 10 sunucu 1803
* Windows 10 IOT Enterprise (Nisan 2018 ile güncelleştirmesi)
* Windows 10 IOT Core (Nisan 2018 ile güncelleştirmesi)

### <a name="tier-2"></a>Katman 2
Katman 2 sistemleri, Azure IOT Edge ile uyumlu olarak değerlendirilebilir ve görece kolayca kullanılabilir. Bunun anlamı:
* Microsoft platformlarında sınama geçici tamamladıktan veya başarılı bir şekilde Azure IOT kenar platformu üzerinde çalışan bir iş ortağının bilir
* Diğer platformlar için kurulum paketleri bu platformlarda çalışabilir

Ubuntu 18.04

Ubuntu 16.04

Rüzgar Akarsu 8

Yocto

Debian

Mac

## <a name="container-engines"></a>Kapsayıcı motorları
Azure IOT kenar modülleri, üzerinde çalıştığı işletim sistemi bağımsız olarak başlatmak için bir kapsayıcı altyapısı gerekir. Microsoft, bu gereksinimi karşılamak için moby-altyapısı, bir kapsayıcı altyapısı sağlar. Moby açık kaynaklı proje ile ilgili temel alır. Docker CE ve Docker EE diğer popüler kapsayıcı motorları markalarıdır. Bunlar ayrıca Moby açılır kaynaklı proje ile ilgili temel alır ve Azure IOT Edge ile uyumludur. Microsoft, bu kapsayıcı altyapılarını kullanarak sistemler için en iyi çaba desteği sağlar. Ancak, Microsoft bunların sorunlarını giderir sevk yeteneği yok. Bu nedenle, Microsoft, üretim sistemlerine moby Altyapısı kullanılmasını önerir.


<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 