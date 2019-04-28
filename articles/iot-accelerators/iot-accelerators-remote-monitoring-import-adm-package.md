---
title: Uzaktan izleme çözümü içeri aktarma paketini - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcısını bir otomatik cihaz Yönetim paketini içeri aktarma
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 8100914e9a1d1489cb80de55a689e17f6d28a941
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61443454"
---
# <a name="import-an-automatic-device-management-package-into-your-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm Hızlandırıcısını bir otomatik cihaz Yönetim paketini içeri aktarma

Bir otomatik cihaz Yönetim yapılandırması, bir cihaz grubuna dağıtmak için yapılandırma değişiklikleri tanımlar. Bu makalede, kuruluşunuzdaki bir geliştirici, zaten bir otomatik cihaz yönetimi yapılandırma oluşturdu varsayılır. Bir yapılandırma bir geliştirici nasıl oluşturduğunu hakkında bilgi edinmek için aşağıdaki IOT Hub ile ilgili nasıl yapılır makaleleri birine bakın:

- [Yapılandırma ve IOT cihazlarını uygun ölçekte Azure portalını kullanarak izleme](../iot-hub/iot-hub-auto-device-config.md)
- [Yapılandırma ve IOT cihazlarını uygun ölçekte Azure CLI kullanarak izleme](../iot-hub/iot-hub-auto-device-config-cli.md)

Bir geliştirici, oluşturur ve bir otomatik cihaz Yönetim yapılandırması bir geliştirme ortamında test eder. Hazır olduğunuzda, Uzaktan izleme çözüm Hızlandırıcısını yapılandırmasını içeri aktarabilirsiniz.

## <a name="export-a-configuration"></a>Bir yapılandırmayı Dışarı Aktar

Geliştirme ortamınızdan otomatik cihaz yönetim yapılandırmasını dışarı aktarmak için Azure portalını kullanın:

1. Azure portalında, geliştirme ve IOT cihazlarınızı test etmek için kullanmakta olduğunuz IOT hub'ına gidin. Tıklayın **IOT cihaz Yapılandırması**:

    [![IOT cihaz yapılandırması](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-expanded.png#lightbox)

1. Kullanmak istediğiniz yapılandırma öğesini tıklatın. **Cihaz yapılandırma ayrıntılarını** sayfası görüntülenir:

    [![IOT cihaz yapılandırma ayrıntısı](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-expanded.png#lightbox)
1. Tıklayın **yapılandırma dosyası indirme**:

    [![Yapılandırma dosyasını indirin](./media/iot-accelerators-remote-monitoring-import-adm-package/download-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-expanded.png#lightbox)

1. Adlı bir yerel dosya olarak JSON dosyasının kaydedileceği **configuration.json**.

Artık otomatik cihaz yönetim yapılandırmasını içeren bir dosya var. Sonraki bölümde, bu yapılandırma Uzaktan izleme çözümüne bir paket olarak alın.

## <a name="import-a-package"></a>Paketi içeri aktarma

Bir otomatik cihaz Yönetim yapılandırması, çözümünüze bir paket olarak içeri aktarmak için aşağıdaki adımları izleyin:

1. Gidin **paketleri** sayfa Uzaktan izleme Web kullanıcı Arabiriminde:  ![Paketleri sayfası](media/iot-accelerators-remote-monitoring-import-adm-package/packagepage.png)

1. Tıklayın **+ yeni paketi**, seçin **yapılandırma** tıklayın ve paket türü olarak **Gözat** seçilecek **configuration.json** dosyası önceki bölümde kaydettiğiniz:

    ![Yapılandırma seçin](media/iot-accelerators-remote-monitoring-import-adm-package/uploadpackage.png)

1. Tıklayın **karşıya** paketi Uzaktan izleme çözümünüze eklemek için:

    ![Karşıya yüklenen paket](media/iot-accelerators-remote-monitoring-import-adm-package/uploadedpackage.png)

Artık, bir otomatik cihaz yönetimi yapılandırma paketi olarak yüklediğiniz. Üzerinde **dağıtımları** sayfasında bu paket, bağlı cihazlara dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir yapılandırma paketini oluşturma ve Uzaktan izleme çözümü aktarmak öğrendiğinize göre sonraki adıma öğrenmektir nasıl [Uzaktan izleme için toplu olarak bağlı cihazları yönetmek](iot-accelerators-remote-monitoring-bulk-configuration-update.md).
