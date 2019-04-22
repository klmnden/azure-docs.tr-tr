---
title: Azure sanal Makineler'de TmaxSoft OpenFrame kullanmaya başlama
description: Azure sanal makinelerinde (VM'ler) TmaxSoft OpenFrame ortamı kullanarak IBM z/OS ana iş yüklerinizi yeniden barındırın.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 408e0166e52af9efd3d4c64f1b29bddcfc1cca4c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58896476"
---
# <a name="get-started-with-tmaxsoft-openframe-on-azure"></a>Azure'da TmaxSoft OpenFrame kullanmaya başlama

Mevcut ana bilgisayar varlıklarınızı alın ve bunları TmaxSoft OpenFrame kullanarak Microsoft Azure'a taşıyın. Bu popüler rehosting çözüm, böylece uygulamaları hızlı bir şekilde geçirmek, Azure üzerinde bir öykünmesi ortamı oluşturur. Hiçbir biçimlendirme gereklidir.

## <a name="openframe-rehosting-environment"></a>OpenFrame yeniden barındırma ortamı

Geliştirme, tanıtımlar, test veya üretim iş yükleri için azure'da OpenFrame ortamı ayarlayın. Aşağıdaki şekilde gösterildiği gibi Azure üzerinde anabilgisayar öykünmesi ortam oluşturma birden çok bileşen OpenFrame içerir. Örneğin, Çevrimiçi Hizmetler OpenFrame anabilgisayar ara yazılım IBM müşteri bilgileri denetim sistemi (CICS) gibi değiştirin. OpenFrame Batch, kendi TJES bileşeni ile IBM anabilgisayar kişinin iş giriş alt sistem (JES) değiştirir. 

![OpenFrame yeniden barındırma işlemi](media/openframe-01.png)

> [!NOTE]
> OpenFrame ortamın Azure üzerinde çalıştırmak için geçerli bir ürün lisansı veya TmaxSoft deneme lisansı olmalıdır.

## <a name="openframe-components"></a>OpenFrame bileşenleri

Aşağıdaki bileşenler OpenFrame ortamı azure'da bir parçasıdır:

- **Geçiş Araçları** OFMiner, ana bilgisayarlar varlıkları analiz eder ve ardından bunları Azure'a geçirir. bir çözüm de dahil olmak üzere.
- **Derleyiciler**, OFCOBOL, ana bilgisayar'ın COBOL programları yorumlar bir derleyici dahil olmak üzere OFPLI, ana bilgisayar'ın PL yorumlar / ı programları; ve OFASM, ana bilgisayar'ın assembler programlar yorumlar bir derleyici.
- **Ön uç** bileşenlerine Java Enterprise kullanıcı çözümü (JEUS) ve Java Enterprise Edition 6.OFGW için sertifikalı bir web uygulaması sunucusunun 3270 dinleyici sağlayan OpenFrame ağ geçidi bileşeni dahil.
- **Uygulama** ortam. Tüm sistemin yöneten bir ara yazılım OpenFrame tabanıdır. Ana bilgisayar'ın Ara yazılım ve IBM CICS OpenFrame sunucu türü C (OSC) değiştirir.
- **İlişkisel veritabanı**Tibero (görünür), Oracle veritabanı, Microsoft SQL Server, IBM Db2 veya MySQL gibi. OpenFrame uygulamaları, veritabanı ile iletişim kurmak için açık veritabanı bağlantısı (ODBC) protokolünü kullanır.
- **Güvenlik** TACF, sistemleri ve kaynaklara kullanıcı erişimini denetleyen bir hizmeti modülü aracılığıyla. 
- **OFManager** web ortamında OpenFrame'nın işlem ve yönetim işlevleri sağlayan bir çözümdür.

![OpenFrame mimarisi](media/openframe-02.png)

## <a name="next-steps"></a>Sonraki adımlar

- [TmaxSoft OpenFrame Azure'a yükleme](./install-openframe-azure.md)
