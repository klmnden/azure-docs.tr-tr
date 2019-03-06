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
ms.openlocfilehash: 7e97983e1316476848eb4a051cd636339b8caff7
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57440973"
---
# <a name="azure-iot-edge-supported-systems"></a>Azure IOT Edge desteklenen sistemleri

Bir Azure IOT Edge ürün desteği arama için çeşitli yollar vardır.

**Hata Raporlama** – Azure IOT Edge ürün şeklinde ulaştığı geliştirme çoğunu IOT Edge açık kaynaklı proje gerçekleşir. Hatalar rapor üzerinde [sorunlar sayfasında](https://github.com/azure/iotedge/issues) proje. Düzeltmeleri projesi ile aşamalarından geçerek ürün güncelleştirmelerini hızlı bir şekilde yapın.

**Microsoft müşteri destek ekibinin** – sahip kullanıcılar bir [destek planı](https://azure.microsoft.com/support/plans/) Microsoft müşteri destek ekibinin doğrudan bir destek bileti oluşturarak görüşebilirsiniz [Azure portalında](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Özellik istekleri** – Azure IOT Edge ürün ürünün özellik isteklerini izleyen [User Voice sayfa](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="container-engines"></a>Kapsayıcı altyapıları
Azure IOT Edge modülleri kapsayıcılar uygulanan bu yana başlatmak için bir kapsayıcı altyapısı gerekir. Microsoft, bu gereksinimi karşılamak için moby-altyapısı, bir kapsayıcı altyapısı sağlar. Bu, Moby açık kaynaklı proje ile ilgili temel alır. Docker CE ve Docker EE diğer popüler kapsayıcı motorlardır. Bunlar ayrıca Moby açık kaynak projesini temel alıyor ve Azure IOT Edge ile uyumludur. Microsoft, bu kapsayıcı altyapıları kullanarak sistemleri için en iyi girişim desteği sağlar. Ancak Microsoft bunları sorunlarını giderir göndermesine imkan yok. Bu nedenle, üretim sistemlerine moby altyapısı kullanarak Microsoft önerir.

<br>
<center>
![Moby kapsayıcı çalışma zamanı](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>İşletim sistemleri
Azure IOT Edge, kapsayıcıları çalıştırmak üzere çoğu işletim sistemi üzerinde çalışır; Ancak, tüm bunların eşit olarak desteklenmez. İşletim sistemleri, kullanıcıları bekleyebileceğiniz destek düzeyini temsil eden katmanlarda gruplandırılır.
* Katman 1 sistemleri gibi resmi olarak desteklenen zorlayıcı olabilir. Diğer bir deyişle Microsoft:
    * Otomatikleştirilmiş testlerin bu işletim sistemi içerir
    * bunların yükleme paketleri sağlar
* Katman 2 sistemleri, Azure IOT Edge ile uyumlu olarak düşünülebilir ve görece bir kolayca kullanılabilir. Bunun anlamı:
    * Microsoft platformlarında ad hoc testi işlemi yapana veya başarılı bir şekilde Azure IOT Edge platformunda çalışan bir iş ortağının bilir
    * Diğer platformlar için yükleme paketleri bu platformlarda çalışabilir
    
Konak işletim sistemi ailesi, konuk bir modülün kapsayıcısının içinde kullanılan işletim sistemi ailesi her zaman eşleşmelidir. Diğer bir deyişle, yalnızca Linux kapsayıcıları üzerinde Linux ve Windows kapsayıcıları Windows üzerinde kullanabilirsiniz. Windows kullanırken, yalnızca yalıtılmış işlem desteklenen, olmayan hyper-v yalıtılmış kapsayıcılar kapsayıcılardır.  

<br>
<center>
![Ana bilgisayar işletim sistemi konuk işletim sistemi ile eşleşir.](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Katman 1
Genel kullanıma sunuldu
| İşletim Sistemi | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Raspbian Uzat | Hayır | Evet|
| Ubuntu Server 16.04 | Evet | Hayır |
| Ubuntu Server 18.04 | Evet | Hayır |

Genel önizleme
| İşletim Sistemi | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core derleme 17763 | Evet | Hayır |
| Windows 10 IOT Enterprise derleme 17763 | Evet | Hayır |
| Windows Server 2019 | Evet | Hayır |

Yukarıdaki Windows işletim sistemleri, Windows üzerinde Windows kapsayıcılarını çalıştırmaya aygıtları için gereksinimleridir. Üretim için yalnızca desteklenen yapılandırma budur. Windows için Azure IOT Edge yükleme paketleri, Windows üzerinde Linux kapsayıcıları kullanılmasına izin verin; Ancak, yalnızca geliştirme ve test için budur. Windows üzerinde Linux kapsayıcıları kullanımını üretim için desteklenen bir yapılandırma değildir. Bu geliştirme senaryosu için herhangi bir Windows 10 derleme 14393 ya da daha yeni ve Windows Server 2016 veya daha yeni sürümü kullanılabilir.

### <a name="tier-2"></a>Katman 2

| İşletim Sistemi | AMD64 | ARM32v7 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Evet | Evet |
| Debian 8 | Evet | Evet |
| Debian 9 | Evet | Evet |
| RHEL 7.5 | Evet | Evet |
| Ubuntu 18.04 | Evet | Evet |
| Ubuntu 16.04 | Evet | Evet |
| 8 Rüzgar Irmağı | Evet | Hayır |
| Yocto | Evet | Hayır |


## <a name="virtual-machines"></a>Virtual Machines
Azure IOT Edge, sanal makinelerde çalıştırılabilir. Müşteriler ile uç zekası mevcut altyapınızı genişletmek istediğinizde bu yaygındır. Ana VM'nin işletim sistemi ailesi, konuk bir modülün kapsayıcısının içinde kullanılan işletim sistemi ailesi ile eşleşmesi gerekir. Bu, bir cihazda doğrudan Azure IOT Edge çalıştırıldığında aynı gereksinimdir. Azure IOT Edge, temel alınan sanallaştırma teknolojisini bağımsızdır ve Hyper-V ve vSphere gibi platformları tarafından desteklenen Vm'leri çalışır.

<br>
<center>
![Bir sanal makinede Azure IOT Edge](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>En düşük sistem gereksinimleri
Azure IOT Edge cihazları bir Raspberry Pi3 küçük sınıf donanımını harika çalışır. Senaryonuz için doğru donanımı seçme, çalıştırmak istediğiniz iş yükleri üzerinde çok bağlıdır. Son cihaz kararı karmaşık olabilir. Ancak, kolayca prototip oluşturma bir çözüm geleneksel dizüstü bilgisayarlar veya masaüstünde başlatabilirsiniz.

Prototip oluşturma, son cihaz seçiminizi Kılavuzu yardımcı olur ancak deneyimi. Göz önünde bulundurmalıdır sorular şunlardır: kaç modülleri oluşturan modülleri kapsayıcıları paylaşımı hangi dilde yazılmış, veri miktarını modüllerinizi işleniyor olabilir, modüllerinizi olan kaç katmanları iş yükünüz modülleri gereksiniminizi herhangi özel yapın iş yüklerini hızlandırmanız için donanım, donanım bütçenizi nedir, çözümünüzün istenen performans özellikleri nelerdir?
