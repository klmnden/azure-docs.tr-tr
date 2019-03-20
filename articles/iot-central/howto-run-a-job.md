---
title: Oluşturma ve Azure IOT Central, uygulamanızda işler çalıştırma | Microsoft Docs
description: Azure IOT Central işleri gibi bir cihaz özelliğinin güncelleştirilmesi, ayarı veya bir komut yürütülürken toplu cihaz yönetimi özellikleri sağlar.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/18/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: bb5eb2904ae969c3ffcd4ee8e365a51853add2fd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182243"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Oluşturma ve Azure IOT Central uygulamanızda bir işi çalıştırma

Bağlı cihazlarınızın uygun ölçekte işleri kullanarak yönetmek için Microsoft Azure IOT Central kullanabilirsiniz. İşleri işlevi cihaz özelliklerini, ayarlarını ve komutları toplu güncelleştirmeler gerçekleştirmek için etkinleştirir. Bu makalede, kendi uygulamanıza işler kullanmaya başlama konusunda yol göstermektedir.

## <a name="create-and-run-a-job"></a>Oluşturma ve bir işi çalıştırma

Bu bölümde, oluşturup bir iş çalıştırmak gösterilir. Her adım fanı sunma hızını artırdık gerek refrigerated satış makine cihazlar için bir işinin nasıl çalıştırılacağını gösteren bir örnek geçer.

1. Gezinti bölmesinden işlerini gidin.

1. Seçin **+ yeni** yeni bir proje oluşturmaya başlamak için.

    ![Yeni proje oluşturma](./media/howto-run-a-job/createnewjob.png)

1. Bir ad girin ve oluşturmakta olduğunuz iş tanımlamanıza yardımcı olan açıklama.

1. İşinizi uygulanmasını istediğiniz cihaz kümesini seçin. Cihaz seçilerek ayarladıktan sonra seçili cihaz kümesi içinde cihazlarla doldurma sağ tarafı bakın. Bozuk bir cihaz kümesi seçerseniz, cihaz görüntülemek ve cihaz kümenizi bozuk olduğunu açıklayan bir ileti görürsünüz.

1. Ardından, (bir ayar, özelliği veya komutu) tanımlamak için proje türünü seçin. Seçin **+** işin türü yanındaki seçilen ve istenen işlemlerinizi ekleyin.

    ![Proje yapılandırma](./media/howto-run-a-job/configurejob.png)

1. Sağ tarafta seçin ve işi çalıştırmak istediğiniz cihazları seçin. Üstteki onay kutusunu seçerek, tüm cihazlar cihazın tamamını kümesinde seçilir. Yanında onay kutusunu seçerek **adı**, geçerli sayfadaki tüm aygıt seçilmedi.

1. Cihazlarınızı seçtikten sonra seçin **çalıştırma** veya **Kaydet**. İş üzerinde ana görünür **işleri** sayfası. Bu görünümü, çalışmakta olan iş ve tüm daha önce işleri çalıştırma geçmişini görebilirsiniz. Çalışan iş her zaman listenin en üstünde gösterilir. İstediğiniz zaman yeniden düzenlemeye devam etmek veya çalıştırmak için kaydedilen işinizi açılabilir.

    ![İşi görüntüle](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > En çok 30 gün önceden çalıştırılmış işlerinizi geçmişini görüntüleyebilirsiniz.

1. İşinizi genel bakışını almak için listeden görüntülemek istediğiniz iş adı seçin. Bu genel bakış, iş ayrıntılarını, cihazları ve cihaz durum değerleri içeriyor. Bu genel bakış'tan, de seçebilirsiniz **indirme işi ayrıntıları** cihazların ve durum değerleri dahil olmak üzere, iş ayrıntılarını bir .csv dosyasını indirmek için. Bu bilgiler sorun giderme için yararlı olabilir.

    ![Cihaz durumunu görüntüle](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Bir çalışan işi Durdur

Çalışmakta olan bir işi durdurmak istiyorsanız, durdurmak istediğiniz bir çalışan işi adını seçin. Seçin **Durdur** panelinde düğmesini. İş durumu, iş durduruldu yansıtacak şekilde değişti görürsünüz.

   ![İşi Durdur](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Durdurulan bir işi çalıştırma

Şu anda durdurulan bir işi çalıştırmak için çalıştırmak istediğiniz durdurulmuş iş adını seçin. Seçin **çalıştırma** panelinde düğmesini. İşi yeniden çalıştığını yansıtacak şekilde iş durumu değişir.

   ![Sürdürülen işi](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Bir işi Kopyala

Oluşturduğunuz var olan bir işi kopyalamak için ana işleri sayfasında seçin ve seçin **kopyalama**. Bu iş yapılandırmasını düzenlemek, yeni bir kopyasını açar. Yeni iş ya da kaydedin. Seçili cihaz kümenize herhangi bir değişiklik yapılmadıysa düzenleyebilmeniz için kopyalanan bu işteki yansıtılır.

   ![Kopyalama işi](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>İş durumunu görüntüleme

Bir işi oluşturulduktan sonra **durumu** sütun güncelleştirmeleriyle işin en son durum iletisi. Durum iletileri şu anlama gelir:

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

İşin içinde her cihazın durumunu görüntülemek için iş adını seçin. İş ve bir parçası olan cihazların tüm ayrıntılarını görürsünüz. Seçebileceğiniz **indirme iş ayrıntılarını** iş ayrıntılarını, cihazların ve durum değerlerini içeren bir .csv dosyası indirilemedi. Her bir cihaz adının yanında, aşağıdaki durum iletileri birine bakın:

| Durum iletisi       | Durum anlama                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | Bu cihaz üzerinde işlemi çalıştırıldı.                                     |
| Başarısız               | Bu cihaz üzerinde çalıştırmak işi başarısız oldu. Daha fazla bilgi belirten ekteki hata iletisine gösterir.  |
| Beklemede              | İşi henüz bu cihaz üzerinde yürüttüğünü değil.                                  |

> [!NOTE]
> Cihaz silinmişse, cihaz seçemezsiniz ve cihaz kimliği ile silinmiş görüntüler

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, uygulamanızda işler oluşturulacağını öğrendiniz, sonraki adımlardan birkaçı şunlardır:

- [Cihaz kümelerini kullanma](howto-use-device-sets.md)
- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Sürümü, cihaz şablonu](howto-version-devicetemplate.md)
