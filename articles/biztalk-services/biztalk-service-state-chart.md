---
title: Farklı durumlar ya da BizTalk Hizmetleri'nde durumları izin verilen görevleri | Microsoft Docs
description: 'Farklı MABS durumu izin verilen eylemleri/işlemleri: durdurmak, Başlat, yeniden, askıya alma, sürdürme, Sil, ölçeklendirme, yapılandırma ve yedekleme ayarlama güncelleştirme'
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
ms.openlocfilehash: bbe1288a42db307001ac778394ac410206f1df21
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228207"
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>Ve yapabileceklerinizi BizTalk hizmeti durumunu kullanarak yapamazsınız

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

BizTalk hizmeti geçerli durumuna bağlı olarak, BizTalk hizmeti üzerinde gerçekleştiremez veya işlem vardır.

Örneğin, yeni bir BizTalk hizmeti sağlayın. BizTalk hizmeti başarıyla tamamlandığında, aşamasındadır `active` durumu. Etkin durumda durdurmak, askıya alabilir ve BizTalk hizmeti silin. BizTalk Hizmeti Durdur ve durdurma başarısız durumunda BizTalk hizmeti gittiği bir `StopFailed` durumu. İçinde `StopFailed` durumunda, BizTalk hizmeti yeniden. Sürdür'gibi izin verilmeyen bir işlem denerseniz aşağıdaki hata oluşur:

`Operation not allowed`

## <a name="view-the-possible-states"></a>Olası durumlar görüntüleyin

Aşağıdaki tablolarda, işlemler veya BizTalk hizmeti belirli bir durumda olduğunda yapılabilir eylemleri listeleyin. Bir ✔ işlemi o durumda izin anlamına gelir. Boş bir girdiyi o durumda işlemi gerçekleştirilemiyor anlamına gelir.

| Hizmet durumu | Başlatma | Durdur | Yeniden Başlatma | Erişimi askıya al | Sürdür | Sil | Ölçek | Güncelleştirme <br/> Yapılandırma | Backup |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Etkin |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Devre dışı |  |  |  |  |  | ✔ | |  |  | 
| Askıya Alındı |  |  |  |  | ✔ | ✔ | |  | ✔ |
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
* [BizTalk Services Pano, İzleyici ve ölçek sekmeleri yapabilecekleriniz](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Hizmetleri geliştirici, temel, standart ve Premium sürümlerinde ile elde edecekleriniz](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Yedekleme ve BizTalk hizmeti geri yükleme](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services hizmetinde açıklanan azaltma](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [BizTalk hizmetiniz için Service Bus ve erişim denetimi verenin adı ve verenin anahtarı değerleri alma](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Azure BizTalk Services SDK'sını Kullanmaya Nasıl Başlarım](https://go.microsoft.com/fwlink/p/?LinkID=302335)

