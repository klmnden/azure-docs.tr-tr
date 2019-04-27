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
ms.openlocfilehash: ec7033719316bb186408ea78f6dabac43c383491
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60519301"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Oluşturma ve Azure IOT Central uygulamanızda bir işi çalıştırma

Bağlı cihazlarınızın uygun ölçekte işleri kullanarak yönetmek için Microsoft Azure IOT Central kullanabilirsiniz. İşler sağlar, cihaz özelliklerini, ayarlarını ve komutları güncelleştirmeleri toplu. Bu makalede, kendi uygulamanıza işler kullanmaya başlama konusunda yol göstermektedir.

## <a name="create-and-run-a-job"></a>Oluşturma ve bir işi çalıştırma

Bu bölümde, oluşturup bir iş çalıştırmak gösterilir. Birden çok refrigerated satış makineler için fanı hızını artırmak nasıl gösterir.

1. Gezinti bölmesinden işlerini gidin.

1. Seçin **+ yeni** yeni bir proje oluşturmak için.

    ![Yeni proje oluşturma](./media/howto-run-a-job/createnewjob.png)

1. Bir ad ve oluşturmakta olduğunuz işi belirlemek için bir açıklama girin.

1. İşinizi uygulamak istediğiniz cihaz kümesini seçin. Cihaz seçilerek ayarladıktan sonra cihaz kümesindeki aygıtlar ile doldurmak sağ tarafı bakın. Bozuk bir cihaz kümesi seçerseniz, cihaz görüntülemek ve cihaz kümenizi bozuk bir ileti görürsünüz.

1. Ardından, (bir ayar, özelliği veya komutu) tanımlamak için proje türünü seçin. Seçin **+** yanındaki iş türünü seçtiğinizde ve işlemlerinizi ekleyin.

    ![Proje yapılandırma](./media/howto-run-a-job/configurejob.png)

1. Sağ tarafta, işin üzerinde çalıştırılacağı istediğiniz cihazları seçin. Üstteki onay kutusunu seçerek, tüm cihazlar cihazın tamamını kümesinde seçilir. Yanında onay kutusunu seçerek **adı**, geçerli sayfadaki tüm aygıt seçilmedi.

1. Cihazlarınızı seçtikten sonra seçin **çalıştırma** veya **Kaydet**. İş üzerinde ana görünür **işleri** sayfası. Bu görünümü, çalışmakta olan iş ve tüm daha önce işleri çalıştırma geçmişini görebilirsiniz. Çalışan iş her zaman listenin en üstünde gösterilir. İstediğiniz zaman yeniden düzenlemeye devam etmek veya çalıştırmak için kaydedilen işinizi açılabilir.

    ![İşi görüntüle](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > En çok 30 gün önceden çalıştırılmış işlerinizi geçmişini görüntüleyebilirsiniz.

1. İşinizi genel bakışını almak için listeden görüntülemek için işi seçin. Bu genel bakış, iş ayrıntılarını, cihazları ve cihaz durum değerleri içeriyor. Bu genel bakış'tan, de seçebilirsiniz **indirme işi ayrıntıları** cihazların ve durum değerleri dahil olmak üzere, iş ayrıntılarını bir .csv dosyasını indirmek için. Bu bilgiler sorun giderme için yararlı olabilir.

    ![Cihaz durumunu görüntüle](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Bir çalışan işi Durdur

Bir çalışan işi durdurmak için onu seçip **Durdur** panelde. İş durumu işi durduruldu yansıtacak şekilde değişir.

   ![İşi Durdur](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Durdurulan bir işi çalıştırma

Şu anda durdurulan bir işi çalıştırmak için durdurulmuş iş'i seçin. Seçin **çalıştırma** panelde. İş yansıtacak şekilde iş durumu değişiklikleri şimdi çalışıyor yeniden.

   ![Sürdürülen işi](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Bir işi Kopyala

Oluşturduğunuz var olan bir işi kopyalamak için ana işleri sayfasında seçin ve seçin **kopyalama**. İş Yapılandırması'nın yeni bir kopyasını düzenleme için açar. Yeni iş ya da kaydedin. Seçili cihaz kümenize herhangi bir değişiklik yapılmadıysa düzenlemek, bu kopyalanan iş yansıtılan.

   ![Kopyalama işi](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>İş durumunu görüntüleme

Bir işi oluşturulduktan sonra **durumu** sütun güncelleştirmeleriyle işin en son durum iletisi. Aşağıdaki tabloda olası durum değerleri listelenmektedir:

| Durum iletisi       | Durum anlama                                          |
| -------------------- | ------------------------------------------------------- |
| Tamamlandı            | Tüm cihazlarda bu işlemi çalıştırıldı.              |
| Başarısız               | Bu iş, başarısız oldu ve cihazlarda tam olarak yürütülür.  |
| Beklemede              | Bu proje, henüz cihazlarda yürütme başlamış edilmemiş.         |
| Çalışıyor              | Bu iş şu anda cihazlarda yürütüyor.             |
| Durduruldu              | Bu işlem, bir kullanıcı tarafından el ile durduruldu.           |

Durum iletisi işinde cihazlar için genel bir bakış tarafından izlenir. Aşağıdaki tabloda olası cihaz durum değerleri listelenmektedir:

| Durum iletisi       | Durum anlama                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Başarılı oldu            | İş üzerinde başarıyla yürütüldü. cihazların sayısı.       |
| Başarısız               | Proje, yürütmek için başarısız olan cihazların sayısı.       |

### <a name="view-the-device-status"></a>Cihaz durumunu görüntüleme

İş ve etkilenen tüm cihazların durumunu görüntülemek için işi seçin. Cihazların ve durum değerleri listesi dahil olmak üzere iş ayrıntılarını içeren bir .csv dosyasını indirmek için seçin **indirme iş ayrıntılarını**. Her bir cihaz adının yanında, aşağıdaki durum iletileri birine bakın:

| Durum iletisi       | Durum anlama                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Tamamlandı            | Bu cihaz üzerinde işlemi çalıştırıldı.                                     |
| Başarısız               | Bu cihaz üzerinde çalıştırmak işi başarısız oldu. Hata iletisi, daha fazla bilgi gösterir.  |
| Beklemede              | İşi henüz bu cihaz üzerinde yürütülen edilmemiş.                                   |

> [!NOTE]
> Cihaz silinmişse, cihaz seçemezsiniz ve cihaz kimliği ile silinmiş görüntüler

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, uygulamanızda işler oluşturulacağını öğrendiniz, sonraki adımlardan birkaçı şunlardır:

- [Cihaz kümelerini kullanma](howto-use-device-sets.md)
- [Cihazlarınızı yönetme](howto-manage-devices.md)
- [Sürümü, cihaz şablonu](howto-version-devicetemplate.md)
