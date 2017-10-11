---
title: "Visual Studio ile Azure özel Bulutlar erişme | Microsoft Docs"
description: "Visual Studio kullanarak özel bulut kaynaklarına erişmek öğrenin."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Özel Azure bulut Visual Studio ile erişme
Varsayılan olarak, Visual Studio genel Azure bulut REST uç noktalarını destekler. Bu konuda, erişim - ve - ile etkileşim kurmak için özel bulutunuzun sertifika kullanmayı öğrenin Visual Studio'dan özel bulut.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Özel bir Azure erişmek için Visual Studio bulut
1. İçinde [Klasik Azure portalı](http://go.microsoft.com/fwlink/?LinkID=213885) özel bulut için yayımlama ayarları dosyanız indirin veya yayımlama ayarları dosyası için yöneticinize başvurun. Azure ortak sürümünde bu indirmek için bağlantıdır [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (İndirilen dosya uzantısına sahip olmalıdır `.publishsettings`)

1. Açık Visual Studio

1. İçinde **Sunucu Gezgini**, sağ **Azure** düğümü ve bağlam menüsünden seçin **yönetin ve filtre abonelikleri**.
   
    ![Abonelikleri komutu yönetme](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. İçinde **Microsoft Azure Aboneliklerini Yönet** iletişim kutusunda **sertifikaları** sekmesini tıklatın ve ardından **alma**.
   
    ![Azure sertifikaları alma](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. İçinde **alma Microsoft Azure abonelikleri** iletişim kutusunda **Gözat**.

    ![Gözat düğmesi alma Microsoft Azure abonelikleri iletişim](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. İçinde **açık** Seç iletişim kutusu, yayımlama ayarları dosyasını kaydettiğiniz dizine gidin dosya ve ardından **açık**.

    ![Yayımlama ayarları dosyasını seçin](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Döndürülen zaman **alma Microsoft Azure abonelikleri** iletişim kutusunda **alma**.

    ![Yayımlama ayarları dosyasını içeri aktar](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Sertifikaları yayımlama ayarları dosyasını Visual Studio'ya aktarılır ve şimdi, özel bulut kaynakları ile etkileşim kurabilirsiniz.
   
## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure bulut hizmeti Visual Studio'dan yayımlama](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Nasıl yapılır: indirme ve içeri aktarma yayımlama ayarları ve abonelik bilgileri](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
