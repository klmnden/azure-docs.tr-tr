---
title: Uzaktan izleme çözümü içeri aktarma Edge paketi - Azure | Microsoft Docs
description: Bu makalede bir IOT Edge paketi Uzaktan izleme çözüm Hızlandırıcısını içeri aktarma
author: dominicbetts
manager: timlt
ms.author: v-yiso
ms.service: iot-accelerators
services: iot-accelerators
origin.date: 10/10/2018
ms.date: 12/17/2018
ms.topic: conceptual
ms.openlocfilehash: 34222f396ed3c43932371aa9f64a459bb2a5dd0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61443020"
---
# <a name="import-an-iot-edge-package-into-your-remote-monitoring-solution-accelerator"></a>Bir IOT Edge paketi Uzaktan izleme çözüm Hızlandırıcısını alma

Bir dağıtım bildirimi IOT Edge cihazına dağıtmak için modülleri tanımlar. Bu makalede, kuruluşunuzdaki bir geliştirici dağıtım bildirimi zaten oluşturduğu varsayılır. Bir geliştirici bir bildirime nasıl oluşturduğunu hakkında bilgi edinmek için aşağıdaki IOT Edge ile ilgili nasıl yapılır makaleleri birine bakın:

- [Azure portalından Azure IOT Edge modüllerini dağıtmak](../iot-edge/how-to-deploy-modules-portal.md)
- [Azure CLI ile Azure IOT Edge modüllerini dağıtmak](../iot-edge/how-to-deploy-modules-cli.md)
- [Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak](../iot-edge/how-to-deploy-modules-vscode.md)

Bir geliştirici, oluşturur ve bir dağıtım bildirimini bir geliştirme ortamında test eder. Hazır olduğunuzda bildirimi Uzaktan izleme çözüm Hızlandırıcısını aktarabilirsiniz.

## <a name="export-a-manifest"></a>Bir bildirim dışarı aktarma

Dağıtım bildirimi geliştirme ortamınızdan dışarı aktarmak için Azure portalını kullanın:

1. Azure portalında geliştirip IOT Edge cihazlarınıza test için kullanmakta olduğunuz IOT hub'ına gidin. Tıklayın **IOT Edge** ardından **IOT Edge dağıtımları**: ![IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/iotedge.png)

1. Kullanmak istediğiniz dağıtım yapılandırması olan dağıtım'a tıklayın. **Dağıtım ayrıntıları** sayfası görüntülenir: ![IOT Edge dağıtımı ayrıntıları](media/iot-accelerators-remote-monitoring-import-edge-package/deploymentdetails.png)

1. Tıklayın **indirme IOT Edge bildirimi**:  ![Dağıtım bildirimi indir](media/iot-accelerators-remote-monitoring-import-edge-package/download.png)

1. Adlı bir yerel dosya olarak JSON dosyasının kaydedileceği **deploymentmanifest.json**.

Artık dağıtım bildirimi içeren bir dosya var. Sonraki bölümde, bu bildirimi Uzaktan izleme çözüm paketi olarak alın.

## <a name="import-a-package"></a>Paketi içeri aktarma

Bir Edge dağıtım bildirimi, çözümünüze bir paket olarak içeri aktarmak için aşağıdaki adımları izleyin:

1. Gidin **paketleri** sayfa Uzaktan izleme Web kullanıcı Arabiriminde:  ![Paketleri sayfası](media/iot-accelerators-remote-monitoring-import-edge-package/packagespage.png)

1. Tıklayın **+ yeni paketi**, seçin **Edge bildirim** tıklayın ve paket türü olarak **Gözat** seçilecek **deploymentmanifest.json** dosya önceki bölümde kaydettiğiniz:  ![Bildirimi seçin](media/iot-accelerators-remote-monitoring-import-edge-package/selectmanifest.png)

1. Tıklayın **karşıya** paketi Uzaktan izleme çözümünüze eklemek için:  ![Karşıya yüklenen paket](media/iot-accelerators-remote-monitoring-import-edge-package/uploadedpackage.png)

Artık, bir IOT Edge dağıtım bildirimini bir paket olarak yüklediğiniz. Üzerinde **dağıtımları** sayfasında bu paketin bağlı IOT Edge cihazlarınıza dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Modüller, Uzaktan izleme çözüm IOT Edge cihazına dağıtmak öğrendiniz, hakkında daha fazla bilgi edinmek için sonraki adım olan [IOT Edge](../iot-edge/about-iot-edge.md).
