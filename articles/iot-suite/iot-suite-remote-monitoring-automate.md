---
title: "Uzaktan izleme çözümünde - Azure cihaz sorunlarını algılamak | Microsoft Docs"
description: "Bu öğretici nasıl Uzaktan izleme çözümünde eşik tabanlı aygıt sorunları otomatik olarak algılamak için kurallar ve Eylemler kullanılacağını gösterir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 9d9fbefd81fed506bcc025fa0f44315ec831cf0d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="detect-issues-using-threshold-based-rules"></a>Eşik tabanlı kurallar kullanarak sorunlarını Algıla

Bu öğretici, Uzaktan izleme çözümünde kurallar altyapısı özelliklerini gösterir. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Contoso sahip baskısı tarafından bildirilen kritik uyarı oluşturur olan bir kural bir **Soğutucu** aygıt 250 PSI aşıyor. Bir operatör olarak tanımlamak istediğiniz **Soğutucu** sorunlu algılayıcılar bakarak olabilecek aygıtlar ilk baskısı ani. Bu cihazları tanımlamak için baskısı 150 PSI aştığında bir uyarı oluşturmak için bir kural oluşturun.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Çözümünüzde kuralları görüntülemek
> * Yeni bir kural oluşturun
> * Mevcut bir kuralı Düzenle
> * Kural silme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md) Öğreticisi.

## <a name="view-the-rules-in-your-solution"></a>Çözümünüzde kuralları görüntülemek

**Kurallar ve Eylemler** çözümü sayfasında geçerli olan tüm kuralların listesini görüntüler:

![Kurallar ve Eylemler sayfası](media/iot-suite-remote-monitoring-automate/rulesactions.png)

Geçerli kurallarını görüntülemek için **Soğutucu** aygıtlar, bir filtre uygula:

![Kurallar listesini filtreler](media/iot-suite-remote-monitoring-automate/rulesactionsfilter.png)

Bir kural hakkında daha fazla bilgi görüntülemek ve listede seçtiğinizde düzenleyin:

![Kuralın ayrıntılarını görüntüleme](media/iot-suite-remote-monitoring-automate/rulesactionsdetail.png)

Devre dışı bırakma, etkinleştirme veya bir veya daha fazla kural silmek için birden çok kural listeden seçin:

![Birden çok kural seçin](media/iot-suite-remote-monitoring-automate/rulesactionsmultiselect.png)

## <a name="create-a-new-rule"></a>Yeni bir kural oluşturun

Bir uyarı oluşturur Yeni bir kural eklemek için zaman baskısı bir **Soğutucu** aygıt 150 PSI aşıyor, seçin **yeni kural**:

![Kural oluşturma](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule.png)

Kuralı oluşturmak için aşağıdaki değerleri kullanın:

| Ayar          | Değer                                 |
| ---------------- | ------------------------------------- |
| Ad             | Soğutucu uyarı                       |
| Kaynak           | **Chillers** cihaz grubu             |
| Tetikleyici alan    | basınç                              |
| Tetikleyici işleci | Şu değerden fazla:                          |
| Tetikleyici değeri    | 150                                   |
| Önem derecesi   | Uyarı                               |
| Açıklama      | Soğutucu baskısı 150 PSI aştı |

Yeni Kural kaydetmek üzere seçim yapın **Uygula**.

Üzerinde kuralı tetiklendiğinde görüntüleyebileceğiniz **kurallar ve Eylemler** sayfa veya **Pano** sayfası.

## <a name="edit-an-existing-rule"></a>Mevcut bir kuralı Düzenle

Mevcut bir kuralı bir değişiklik yapmak için kurallar listesinde seçin. Ardından **kural ayrıntı** paneli seçin **düzenleme moduna**.

![Kuralını Düzenle](media/iot-suite-remote-monitoring-automate/rulesactionsedit.png)

## <a name="disable-a-rule"></a>Bir kural devre dışı bırak

Geçici olarak devre dışı bir kural geçmek için kurallar listesinde devre dışı bırakabilirsiniz. Devre dışı bırakabilir ve ardından kuralı seçme **devre dışı**. **Durum** listesinde kuralın kural belirtmek için değişiklikler artık devre dışı. Yordamın aynısını kullanarak daha önce devre bir kural yeniden etkinleştirebilirsiniz.

![Kuralı devre dışı bırak](media/iot-suite-remote-monitoring-automate/rulesactionsdisable.png)

Etkinleştirin ve listede birden çok kural seçin, aynı anda birden çok kural devre dışı.

## <a name="delete-a-rule"></a>Kural silme

Bir kural kalıcı olarak silmek için kurallar listesinde kuralı seçin ve ardından **silmek**.

Listede birden çok kural seçerseniz, aynı anda birden çok kural silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide gösterilen, nasıl için:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Çözümünüzde kuralları görüntülemek
> * Yeni bir kural oluşturun
> * Mevcut bir kuralı Düzenle
> * Kural silme

Eşik tabanlı kurallar kullanarak sorunlarını algılamak öğrendiniz, bilgi edinmek için önerilen sonraki adımlar olan nasıl yapılır:

* [Yönetmek ve cihazlarınızı yapılandırmak](./iot-suite-remote-monitoring-manage.md).
* [Aygıt sorunlarını gidermek ve](./iot-suite-remote-monitoring-maintain.md).
* [Sanal cihazlar ile çözümünüzü test](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->