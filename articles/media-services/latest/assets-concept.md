---
title: Azure Media Services varlıkları | Microsoft Docs
description: Bu makalede varlıklar nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları hakkında bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 928d88d51503350abe7df847ce58ff066b987c8e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="assets"></a>Varlıklar

Bir **varlık** dijital dosyaları (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta verileri içerir. Dijital dosyalar bir varlığa karşıya yükledikten sonra medya kodlama ve iş akışları akış Hizmetleri'nde kullanılabilir.

Bir varlık, bir blob kapsayıcısında eşlenmiş [Azure depolama hesabı](storage-account-concept.md) ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Depolama SDK istemcileri kullanarak kapsayıcılarında varlık dosyaları ile etkileşim kurabilir.

Azure Media Services hesabı genel amaçlı v2 kullandığında Blob katmanları destekler (GPv2) depolama. GPv2 ile soğutma dosyaları veya soğuk depolama taşıyabilirsiniz. Soğuk depolama (örneğin, bunlar kodlanmıştır.) sonra artık gerekmediğinde mezzanine dosyaları arşivleme için uygundur

Media Services v3 iş giriş varlıklar veya http (s) URL'leri oluşturulabilir. İşiniz için bir giriş olarak kullanılabilen bir varlık oluşturmak için bkz: [yerel bir dosyadan bir iş girişi oluşturma](job-input-from-local-file-how-to.md).

Ayrıca, bilgiyi [Media Services depolama hesaplarında](storage-account-concept.md) ve [dönüşümler ve işleri](transform-concept.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosya akışı](stream-files-dotnet-quickstart.md)
