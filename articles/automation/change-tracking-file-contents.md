---
title: Azure Otomasyonu ile dosya içeriği değişikliklerini görüntüle
description: Değişen bir dosyanın içeriğini görüntülemek için değişiklik izleme dosyası içerik değişikliği özelliğini kullanın.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: bobbytreed
ms.author: robreed
ms.date: 07/03/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6aef9a24e3337d1f5a5a6c9ac6b510cc7f9a66a5
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478640"
---
# <a name="view-contents-of-a-file-that-is-being-tracked-with-change-tracking"></a>Değişiklik izleme ile izlenmekte olan bir dosyanın içeriği görüntüle

Dosya içerik izleme önce ve sonra izlenmekte olan bir değişiklik değişiklik izleme ile bir dosyanın içeriğini görüntülemenizi sağlar. Her değişiklik gerçekleştikten sonra bunu yapmak için bu dosyanın içeriği bir depolama hesabına kaydeder.

## <a name="requirements"></a>Gereksinimler

* Resource Manager dağıtım modelini kullanarak bir standart depolama hesabı, dosya içeriğini depolamak için gereklidir. Premium ve klasik dağıtım modeli depolama hesapları kullanılmamalıdır. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md)

* Kullanılan depolama hesabı yalnızca 1 Otomasyon hesabına bağlı olabilir.

* [Değişiklik izleme](automation-change-tracking.md) Otomasyon hesabınızda etkinleştirilir.

## <a name="enable-file-content-tracking"></a>Dosya içerik izlemeyi etkinleştirme

1. Azure portalında Otomasyon hesabınızı açın ve ardından **değişiklik izleme**.
2. Üst menüden **ayarlarını Düzenle**.
3. Seçin **dosya içeriği** tıklatıp **bağlantı**. Bu açılır **değişiklik izleme için içerik konumu ekleme** bölmesi.

   ![Etkinleştirme](./media/change-tracking-file-contents/enable.png)

4. Dosya içeriği depolamak için kullanılacak depolama hesabı ve aboneliği seçin. Var olan tüm izlenen dosyaların dosya içerik izlemeyi etkinleştirmek isteyip istemediğinizi seçin **üzerinde** için **karşıya yükleme, dosya içeriğini tüm ayarları için**. Bunu her dosya yolu için daha sonra değiştirebilirsiniz.

   ![Depolama hesabını ayarlama](./media/change-tracking-file-contents/storage-account.png)

5. Etkinleştirildikten sonra depolama hesabı ve SAS URI'leriyle gösterilir. SAS URI'leriyle 365 gün sonra süresi dolacak ve tıklayarak yeniden **yeniden** düğmesi.

   ![Hesap anahtarlarını Listele](./media/change-tracking-file-contents/account-keys.png)

## <a name="add-a-file"></a>Bir dosya ekleyin

Aşağıdaki adımlar, değişiklik izleme için bir dosya çubuğunda kapatma yol:

1. Üzerinde **ayarlarını Düzenle** sayfasının **değişiklik izleme**, şunlardan birini seçin **Windows dosyaları** veya **Linux dosyaları** sekmesine ve tıklayın  **Ekleme**

1. Dosya yolu için bilgileri doldurun ve seçin **True** altında **karşıya yükleme, dosya içeriğini tüm ayarları için**. Bu ayar, yalnızca dosya yolu için izleme dosyasının içeriği sağlar.

   ![linux dosyası ekleme](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="viewing-the-contents-of-a-tracked-file"></a>İzlenen bir dosyanın içeriğini görüntüleme

1. Dosya veya yolda bir dosya için bir değişiklik algılandı. sonra portalda gösterir. Dosya değişikliği değişiklikleri listesinden seçin. **Değiştirme ayrıntıları** bölmesi görüntülenir.

   ![Listele](./media/change-tracking-file-contents/change-list.png)

1. Üzerinde **değiştirme ayrıntıları** sayfasında sol üst bilgi, dosyasına sağ tıklayıp standart önce ve sonra görürsünüz **dosya içeriği değişikliklerini görüntüle** dosyasının içeriğini görmek için.

   ![Değişiklik ayrıntıları](./media/change-tracking-file-contents/change-details.png)

1. Yeni sayfada dosya içeriğini bir yan yana görünümünde gösterilir. Belirleyebilirsiniz **satır içi** değişikliklerinin satıriçi görmek için.

   ![Dosya değişikliklerini görüntüleme](./media/change-tracking-file-contents/view-file-changes.png)

## <a name="next-steps"></a>Sonraki adımlar

Öğretici çözümünü kullanma hakkında daha fazla bilgi edinmek için değişiklik izleme'ı ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Kullanım [günlük aramaları Azure İzleyici günlüklerine](../log-analytics/log-analytics-log-searches.md) ayrıntılı değişiklik izleme verileri görüntülemek için.

