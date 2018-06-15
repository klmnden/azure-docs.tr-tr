---
title: Farklı durumları ya da BizTalk Services durumları izin görevleri | Microsoft Docs
description: 'Farklı MABS durumunda izin verilen eylemleri/operations: durdurmak, Başlat, yeniden başlatın, askıya alma, sürdürme, Sil, ölçeklendirme, Yukarı yapılandırma ve yedekleme güncelleştirme'
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: 05470e75fc7b46603c8fce3a98c66ac6a24758a8
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "24102751"
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>Ne yapıp BizTalk hizmeti durumu kullanarak olamaz

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk hizmeti geçerli durumuna bağlı olarak BizTalk hizmeti gerçekleştiremez veya işlem vardır.

Örneğin, yeni bir BizTalk hizmeti sağlayın. Başarıyla tamamlandığında, BizTalk hizmeti olarak `active` durumu. Etkin durumda durdurmak, askıya alma ve BizTalk hizmeti silin. BizTalk hizmetini durdurun ve durdurma başarısız olduysa BizTalk hizmeti gider bir `StopFailed` durumu. İçinde `StopFailed` durumunda, BizTalk hizmeti yeniden. Sürdür'gibi izin verilmeyen bir işlem denerseniz aşağıdaki hata oluşur:

`Operation not allowed`

## <a name="view-the-possible-states"></a>Olası durumlarını görüntülemek

Aşağıdaki tablolar, işlemler veya BizTalk hizmeti belirli bir durumda olduğunda yapılabilir eylemleri listeler. Bir ✔ durumundayken bu işleme izin anlamına gelir. Boş bir girdiyi durumundayken bu işlem gerçekleştirilemiyor anlamına gelir.

| Hizmet durumu | Başlatma | Durdur | Yeniden Başlatma | Askıya alma | Sürdür | Sil | Ölçek | Güncelleştirme <br/> Yapılandırma | Backup |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Etkin |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Devre dışı |  |  |  |  |  | ✔ | |  |  | 
| askıya alındı |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Durduruldu | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Hizmet güncelleştirmesi başarısız oldu |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Ayrıca Bkz.
* [BizTalk Services Pano, İzleyici ve ölçek sekmeleri yapabilecekleriniz](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services geliştirici, temel, standart ve Premium sürümlerinde ile Al](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Yedekleme ve BizTalk hizmeti geri yükleme](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services'da açıklandığı azaltma](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [BizTalk hizmetiniz için hizmet veri yolu ve erişim denetimi verenin adı ve verenin anahtar değerlerini alma](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](http://go.microsoft.com/fwlink/p/?LinkID=302335)

