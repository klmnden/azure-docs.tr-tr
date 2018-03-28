---
title: Sprint 3 Ocak 2018 Azure ML çalışma ekranı sürüm notları
description: Bu belgede Azure ML sprint 3 sürümü için güncelleştirmeler ayrıntıları
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: fa209ba2259ae82796556fc1229cd6944c7a2ae1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2018
---
# <a name="sprint-3---january-2018"></a>Sprint 3 - Ocak 2018 

#### <a name="version-number-01171218263"></a>Sürüm numarası: 0.1.1712.18263

>İşte nasıl [sürüm numarasını bulun](known-issues-and-troubleshooting-guide.md).

Azure Machine Learning çalışma ekranı dördüncü güncelleştirmeye Hoş Geldiniz. Güncelleştirmeleri ve bu sprint yenilikleri şunlardır: Bu güncelleştirmeler çoğunu doğrudan kullanıcı geri bildirim sonucu olarak yapılır. 

## <a name="notable-new-features-and-changes"></a>Önemli yeni özellikleri ve değişiklikleri
- Kimlik doğrulama yığınını güncelleştirmeleri zorlar başlangıçta oturum açma ve hesap seçimi

## <a name="detailed-updates"></a>Ayrıntılı güncelleştirmeleri
Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmelerin bir listesi aşağıda verilmiştir.

### <a name="workbench"></a>Workbench
- Program Ekle/Kaldır'ndan uygulamanın yükleme/kaldırma yeteneği
- Kimlik doğrulama yığınını güncelleştirmeleri zorlar başlangıç oturum açma ve hesap seçimi
- Windows geliştirilmiş çoklu oturum açma (SSO) deneyimi
- Farklı kimlik bilgileriyle birden çok kiracıya ait olan kullanıcıların artık çalışma ekranına aktardığınız oturum mümkün olacaktır

#### <a name="ui"></a>UI
- Genel geliştirmeler ve hata düzeltmeleri

### <a name="notebooks"></a>Dizüstü bilgisayarlar
- Genel geliştirmeler ve hata düzeltmeleri

### <a name="data-preparation"></a>Veri hazırlama 
- Örnekle dönüşümleri gerçekleştirirken geliştirilmiş otomatik-önerileri
- Desen sıklığı denetçisi için geliştirilmiş algoritması
- Örnekle dönüşümleri gerçekleştirilirken örnek veriler ve geri bildirim gönderme olanağı ![gönderme geri bildirim bağlantısı üzerinde görüntüsü türetilen sütun dönüştürme](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Spark çalışma zamanı geliştirmeleri
- Scala Pyspark değiştirilmiştir
- Zaman serisi denetleyici için veri uygulanamaz kapatmaya sabit kullanamama 
- Sabit veri hazırlığı yürütme HDI için yanıt vermemesine saat

### <a name="model-management-cli-updates"></a>Model yönetim CLI güncelleştirir 
  - Aboneliğin sahipliğini, artık kaynakların sağlamak için gereklidir. Katkıda bulunan erişim kaynak grubuna dağıtım ortamı ayarlamak yeterli olacaktır.
  - Etkin yerel ortamı Kurulumu ücretsiz abonelikler 
