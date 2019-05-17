---
title: Visual Studio Code (Önizleme) ile Azure Stream Analytics işleri keşfedin
description: Bu makalede, yerel bir proje, listeleyemeyeceksiniz ve görünüm iş varlıkları için Azure Stream Analytics işi dışarı aktarma gösterir.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: 8674d478646c8f9be6b32521c6624752ac6df052
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827808"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Visual Studio Code (Önizleme) ile Azure Stream Analytics'i keşfedin

Visual Studio Code uzantısı için Azure Stream Analytics, geliştiricilerin Stream Analytics işlerini yönetmek için basit bir deneyim sunar. Windows, Mac ve Linux'ta kullanılabilir. Azure Stream Analytics uzantısı ile şunları yapabilirsiniz:

- [Oluşturma](quick-create-vs-code.md), başlatmak ve durdurmak işleri
- Mevcut dışarı aktarma işleri için yerel bir proje
- İşleri listelemek ve iş varlıkları görüntüleyin

## <a name="export-a-job-to-a-local-project"></a>Bir işi dışarı aktarma için yerel bir proje

Yerel bir proje için bir iş dışarı aktarmak için dışarı aktarmak istediğiniz işi bulun **Stream Analytics Gezgini'nde** Visual Studio code'da. Ardından, projeniz için bir klasör seçin. Proje, seçtiğiniz klasöre aktarılır ve Visual Studio Code'dan işi yönetmek devam edebilirsiniz. Stream Analytics işlerini yönetmek için Visual Studio Code kullanmayla ilgili daha fazla bilgi için bkz. Visual Studio Code [hızlı](quick-create-vs-code.md).

![Visual Studio code'da ASA işi Dışarı Aktar](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>İş listesi ve iş varlıkları görüntüleyin

Visual Studio'dan Azure Stream Analytics işleri ile etkileşim kurmak için iş görünümü kullanabilirsiniz.


1. Tıklayın **Azure** Visual Studio kod etkinlik çubuğundaki ve ardından **Stream Analytics düğümünü**. İşlerinizi aboneliklerinizi altında görünmelidir.

   ![Açık bir Stream Analytics Gezgini](./media/vscode-explore-jobs/open-explorer.png)

2. Proje düğümünü, açın ve iş sorgusu, yapılandırma, girişler, çıkışlar ve işlevleri görüntüleyin. 

3. Proje düğümünü sağ tıklatın ve seçin **iş görünümünde Aç portalı** Azure portalında iş görünümünü açmak için düğümü.

   ![Açık proje görünümü'nde portalı](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code'da (Önizleme) Azure Stream Analytics bulut işi oluşturma](quick-create-vs-code.md)
