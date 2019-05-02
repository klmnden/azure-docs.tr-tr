---
title: Azure StorSimple’dan Azure Media Services hesabına dosya yükleme | Microsoft Docs
description: Bu makalede Azure StorSimple Veri Yöneticisi'ne ilişkin kısa bir genel bakış sunulmaktadır. Bu makale ayrıca StorSimple’dan verileri ayıklama ve bir Azure Media Services hesabına varlık olarak yükleme işlemini gösteren öğreticilerin bağlantılarını içerir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 1dd09328-262b-43ef-8099-73241b49a925
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: c77b700cab4afd411c3a2df824ee8335cb394cda
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64868307"
---
# <a name="upload-files-into-an-azure-media-services-account-from-azure-storsimple"></a>Azure StorSimple’dan Azure Media Services hesabına dosya yükleme  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)
>
> 
> Azure StorSimple Veri Yöneticisi şu anda özel önizleme aşamasındadır. 
> 

## <a name="overview"></a>Genel Bakış

Media Services’de dijital dosyalar bir varlığa yüklenir. Varlık; video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler) içerebilir. Dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanmış olur.

[Azure StorSimple](https://docs.microsoft.com/azure/storsimple/), şirket içi çözümün bir uzantısı olarak bulut depolama kullanır ve şirket içi depolama ile bulut depolama arasındaki verileri otomatik olarak katman haline getirir. StorSimple cihazı verilerinizi buluta göndermeden önce yinelenen verileri kaldırıp verileri sıkıştırır ve büyük dosyaları buluta göndermeyi çok verimli hale getirir. [StorSimple Veri Yöneticisi](../../storsimple/storsimple-data-manager-overview.md) hizmeti, StorSimple’dan verileri ayıklamanızı ve AMS varlığı olarak sunmanızı sağlayan API’ler sunar.

## <a name="get-started"></a>başlarken

1. Varlıkları aktarmak istediğiniz bir [Media Services hesabı oluşturun](media-services-portal-create-account.md).
2. [StorSimple Veri Yöneticisi](../../storsimple/storsimple-data-manager-overview.md) makalesinde açıklanan şekilde Veri Yöneticisi önizlemesine kaydolun.
3. Bir StorSimple Veri Yöneticisi hesabı oluşturun.
4. Çalıştığında bir StorSimple cihazından verileri ayıklayıp varlık olarak AMS hesabına aktaran bir veri dönüşüm işi oluşturun. 

    İş çalışmaya başladığında bir depolama kuyruğu oluşturulur. Bu kuyruk, hazır olduğunda dönüştürülen bloblar hakkında iletilerle doldurulur. Bu kuyruğun adı, iş tanımının adıyla aynıdır. Bir varlığın ne zaman hazır olduğunu belirlemek ve üzerinde çalıştırmak istediğiniz Media Services işlemini çağırmak için bu kuyruğu kullanabilirsiniz. Örneğin, bu kuyruğu kullanarak, üzerinde gerekli Media Services kodunun olduğu bir Azure İşlevi tetikleyebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

[Veri Yöneticisi'nde iş tetikleme .NET SDK'yı kullanın](../../storsimple/storsimple-data-manager-dotnet-jobs.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).
