---
title: Visual Studio ile Azure özel Bulutlar erişme | Microsoft Docs
description: Visual Studio kullanarak özel bulut kaynaklarına erişmek öğrenin.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/13/2017
ms.author: ghogen
ms.openlocfilehash: 0160ac6db2b92d5a30a19dd444d01a8558b3eed3
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "31793004"
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Özel Azure bulut Visual Studio ile erişme

Varsayılan olarak, Visual Studio Azure bulut REST uç noktalarını destekler. Bu makalede, özel bulutunuzun sertifika erişmek ve özel bulut Visual Studio'dan etkileşim için nasıl kullanılacağını öğrenin.

1. Özel bulut için Azure portalında, yayımlama ayarları dosyanız indirin veya yayımlama ayarları dosyası için yöneticinize başvurun. (Dosya uzantısına sahip `.publishsettings`.)

1. Visual Studio'da **Sunucu Gezgini**, sağ tıklatın **Azure** düğümü ve select **yönetin ve filtre abonelikleri**.

    ![Abonelikleri komutu yönetme](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. İçinde **Microsoft Azure Aboneliklerini Yönet** iletişim kutusunda **sertifikaları** sekmesini ve ardından **alma**.

    ![Azure sertifikaları alma](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. İçinde **alma Microsoft Azure abonelikleri** iletişim kutusunda **Gözat**.

    ![Gözat düğmesi alma Microsoft Azure abonelikleri iletişim](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. İçinde **açık** Seç iletişim kutusu, yayımlama ayarları dosyasını kaydettiğiniz dizine gidin dosya ve ardından **açık**.

    ![Yayımlama ayarları dosyasını seçin](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Döndürülen zaman **alma Microsoft Azure abonelikleri** iletişim kutusunda **alma**.

    ![Yayımlama ayarları dosyasını içeri aktar](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Sertifikaları yayımlama ayarları dosyasını Visual Studio'ya aktarılır ve şimdi, özel bulut kaynakları ile etkileşim kurabilirsiniz.

