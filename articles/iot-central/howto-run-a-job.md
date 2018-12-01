---
title: Oluşturma ve Azure IOT Central, uygulamanızda işler çalıştırma | Microsoft Docs
description: Azure IOT Central işleri gibi bir cihaz özelliğinin güncelleştirilmesi, ayarı veya bir komut yürütülürken toplu cihaz yönetimi özellikleri sağlar.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/15/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: ac5accc72369d811c0d36c4ef64cd8d523a061f3
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724511"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Oluşturma ve Azure IOT Central uygulamanızda bir işi çalıştırma

Bağlı cihazlarınızın uygun ölçekte işleri kullanarak yönetmek için Microsoft Azure IOT Central kullanabilirsiniz. İşleri işlevi cihaz özelliklerini, ayarlarını ve komutları toplu güncelleştirmeler gerçekleştirmek için etkinleştirir. Bu makale, kendi uygulamanıza işler kullanmaya başlama konusunda size yol gösterir.

## <a name="create-and-run-a-job"></a>Oluşturma ve bir işi çalıştırma

Bu bölümde, oluşturup bir iş çalıştırmak gösterilir. Her adım fanı sunma hızını artırdık gerek refrigerated satış makine cihazlar için bir işinin nasıl çalıştırılacağını gösteren bir örnek geçer.

1. Gezinti bölmesinden işlerini gidin.

1. Tıklayın **+ yeni** yeni bir proje oluşturmaya başlamak için.

    ![Yeni proje oluşturma](./media/howto-run-a-job/createnewjob.png)

1. Bir ad girin ve oluşturmakta olduğunuz iş tanımlamanıza yardımcı olan açıklama.

1. İşinizi uygulanmasını istediğiniz cihaz kümesini seçin. Cihaz seçilerek ayarladıktan sonra seçili cihaz kümesi içinde cihazlarla doldurma sağ tarafı görürsünüz. Bozuk bir cihaz kümesi seçerseniz, hiçbir cihazlar olarak görüntülenir ve cihaz kümenizi bozuk olduğunu açıklayan bir ileti görürsünüz.

1. Ardından, olacak iş türünü (bir ayar, özelliği veya komutu) tanımlanan seçin. Tıklayın **+** işin türü yanındaki seçilen ve istenen işlemlerinizi ekleyin.

    ![Proje yapılandırma](./media/howto-run-a-job/configurejob.png)

1. Sağ tarafta seçin ve işi çalıştırmak istediğiniz cihazları seçin. Tüm cihazlar üst onay kutusuna tıklayarak tüm cihaz kümesinde seçilir. Adının yanında onay kutusunu tıklatarak, geçerli sayfadaki tüm cihazlar seçilir.

1. İstenen cihazlarınızı seçtikten sonra Seç **çalıştırma**. İş üzerinde ana artık görünecek **işleri** sayfası. Bu görünümü, çalışmakta olan iş ve tüm daha önce işleri çalıştırma geçmişini görebilirsiniz. Çalışan iş her zaman listenin en üstünde gösterilir.

    ![İşi çalıştırma](./media/howto-run-a-job/runjob.png)

    ![İşi görüntüle](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > En çok 30 gün önceden çalıştırılmış işlerinizi geçmişini mümkün olacaktır.

1. İşinizi genel bakışını almak için listeden görüntülemek istediğiniz iş adına tıklayın. Bu genel bakışta, iş ayrıntılarını, cihazları ve cihaz durumları içerir.

    ![Cihaz durumunu görüntüle](./media/howto-run-a-job/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Bir çalışan işi Durdur

Çalışmakta olan bir işi durdurmak istiyorsanız, durdurmak istediğiniz çalışan iş adına tıklayın. Seçin **Durdur** panelinde düğmesini. İş durumu, iş durduruldu yansıtacak şekilde değişti görürsünüz.

   ![İşi Durdur](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Durdurulan bir işi çalıştırma

Durdurulmuş bir iş çalıştırmak istiyorsanız, çalıştırmak istediğiniz durdurulmuş iş adına tıklayın. Seçin **çalıştırma** panelinde düğmesini. İş durumu işi yeniden çalıştığını yansıtacak şekilde değiştiğini görürsünüz.

   ![Sürdürülen işi](./media/howto-run-a-job/resumejob.png)

## <a name="view-the-job-status"></a>İş durumunu görüntüleme

Bir işi oluşturulduktan sonra **durumu** sütununu işin en son durum iletisi ile güncelleştirin. Durum iletileri şu anlama gelir:

| Durum iletisi       | Durum anlama                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Tüm cihazlarda bu işlemi çalıştırıldı.              |
| Başarısız               | Bu iş, başarısız oldu ve cihazlarda tam olarak yürütülür.  |
| Beklemede              | Bu iş, cihazlarda yürütme henüz başlatılmamıştır.        |
| Çalışıyor              | Bu iş şu anda cihazlarda yürütüyor.             |
| Durduruldu              | Bu işlem, bir kullanıcı tarafından el ile durduruldu.           |

Durum iletisi iş içindeki cihazlar için genel bir bakış tarafından izlenir. Bu cihaz durumları şu anlama gelir:

| Durum iletisi       | Durum anlama                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Başarılı oldu            | İş üzerinde başarıyla yürütüldü, cihazların sayısı.  |
| Başarısız               | Proje, yürütmek için başarısız oldu, cihazların sayısı.      |

### <a name="view-the-device-status"></a>Cihaz durumunu görüntüleme

İşin içinde her cihazın durumunu görüntülemek için iş adına tıklayın. Burada, iş ve bu belirli bir işin bir parçası olan cihazların tüm ayrıntılarını görürsünüz. Her bir cihaz adının yanında, aşağıdaki durum iletileri birini görürsünüz:

| Durum iletisi       | Durum anlama                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | Bu cihaz üzerinde işlemi çalıştırıldı.                                     |
| Başarısız               | Bu cihaz üzerinde çalıştırmak işi başarısız oldu. Daha fazla bilgi belirten ekteki hata iletisine gösterir.  |
| Beklemede              | İşi henüz bu cihaz üzerinde yürüttüğünü değil.                                  |

> [!NOTE]
> Cihaz silinmişse, cihaz seçmek mümkün olmayacaktır ve cihaz kimliği ile silinmiş olarak görüntülenir

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, uygulamanızda işler oluşturulacağını öğrendiniz, sonraki adımlardan birkaçı şunlardır:

- [Cihaz kümelerini kullanma](howto-use-device-sets.md)
- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Sürümü, cihaz şablonu](howto-version-devicetemplate.md)
