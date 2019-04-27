---
title: Azure IoT hub'ında elle yük devretme | Microsoft Docs
description: Bir Azure IoT hub'ı için elle yük devretme gerçekleştirmeyi gösterme
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 40a7bba99068ebc2368e413199cf966bd2e4f25c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60650548"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub-public-preview"></a>Öğretici: El ile yük devretme gerçekleştirmek için IOT hub (genel Önizleme)

Elle yük devretme, müşterilerin hub işlemleri için birincil bölgeden coğrafi olarak eşleştirilen ilgili Azure coğrafi eşleme bölgesine [yük devretme](https://en.wikipedia.org/wiki/Failover) gerçekleştirmesini sağlayan bir IoT Hub hizmeti özelliğidir. Elle yük devretme, bölgesel olağanüstü durum veya uzun süreli hizmet kesintisi durumunda gerçekleştirilebilir. Ayrıca sisteminizin olağanüstü durum kurtarma özelliklerini test etmek için de planlı bir yük devretme gerçekleştirebilirsiniz. Bunun için üretim ortamı yerine test amaçlı bir IoT hub kullanmanız önerilir. Elle yük devretme özelliği, müşterilere ek maliyet olmadan sunulur.

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure portalı kullanarak bir IoT hub'ı oluşturun. 
> * Yük devretme gerçekleştirin. 
> * Hub'ın ikincil konumda çalıştığını görün.
> * IoT hub'ın işlemlerini birincil konuma geri almak için yeniden çalışma gerçekleştirin. 
> * Hub'ın doğru konumda düzgün biçimde çalıştığını onaylayın.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın. 

2. **+ Kaynak oluştur**'a tıklayıp **Nesnelerin İnterneti**'ni ve ardından **IoT Hub** öğesini seçin.

   ![IoT hub'ı oluşturmayı gösteren ekran görüntüsü](./media/tutorial-manual-failover/create-hub-01.png)

3. **Temel Bilgiler** sekmesini seçin. Aşağıdaki alanları doldurun.

    **Abonelik**: Kullanmak istediğiniz Azure aboneliğini seçin.

    **Kaynak Grubu**: **Yeni oluştur**'a tıklayın ve kaynak grubu adı olarak **ManlFailRG** girin.

    **Bölge**: Önizleme kapsamındaki bölgelerden size yakın olanı seçin. Bu öğreticide `westus2` kullanılır. Yük devretme yalnızca coğrafi olarak eşleştirilmiş Azure bölgeleri arasında gerçekleştirilebilir. westus2 ile coğrafi olarak eşleştirilmiş bölge WestCentralUS bölgesidir.
    
   **IoT Hub'ı Adı**: IoT hub'ınıza bir ad verin. Hub adının genel olarak benzersiz olması gerekir. 

   ![IoT hub'ı oluşturmak için Temel Bilgiler bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/create-hub-02-basics.png)

   **Gözden geçir ve oluştur**’a tıklayın. (Boyut ve ölçek için varsayılan değerler kullanılır.) 

4. Bilgileri gözden geçirin ve **Oluştur**'a tıklayarak IoT hub'ını oluşturun. 

   ![IoT hub'ı oluşturmak için son adımı gösteren ekran görüntüsü](./media/tutorial-manual-failover/create-hub-03-create.png)

## <a name="perform-a-manual-failover"></a>Elle yük devretme gerçekleştirme

Bir IoT hub'ı için günlük iki yük devretme ve iki yeniden çalışma sınırı vardır.

1. **Kaynak grupları**'na tıklayın ve **ManlFailRG** adlı kaynak grubunu seçin. Kaynak listesinde hub'ınıza tıklayın. 

2. IoT Hub'ı bölmesinin **Dayanıklılık** bölümünde **Elle yük devretme (önizleme)** seçeneğini belirleyin. Hub'ınız geçerli bir bölgede oluşturulmadıysa elle yük devretme seçeneği devre dışı olur.

   ![IoT Hub'ı özellikler bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-01.png)

3. Elle yük devretme bölmesinde **IoT Hub'ı Birincil Konumu** ve **IoT Hub'ı İkincil Konumu** bilgileri gösterilir. Birincil konum, IoT hub'ını oluştururken belirlediğiniz konumdur ve her zaman hub'ın etkin olduğu konumu gösterir. İkincil konum, birincil konumla eşleştirilmiş olan standart [coğrafi olarak eşleştirilmiş Azure bölgesi](../best-practices-availability-paired-regions.md) olur. Konum değerlerini değiştiremezsiniz. Bu öğreticide birincil konum `westus2`, ikincil konum ise `WestCentralUS` olarak belirlenmiştir.

   ![Elle Yük Devretme bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-02.png)

3. Elle yük devretme bölmesinin en üstündeki **Yük devretmeyi başlat**'a tıklayın. **Elle yük devretmeyi onaylayın** bölmesi açılır. Yük devretmek istediğiniz IoT hub'ını onaylamak için adını yazın. Ardından yük devretmeyi başlatmak için **Tamam**'a tıklayın.

   Elle yük devretme gerçekleştirmek için gereken süre, hub'ınıza kayıtlı olan cihaz sayısına bağlıdır. Örneğin 100.000 cihazınız varsa 15 dakika, beş milyon cihazınız varsa bir saat veya daha uzun sürebilir.

4. **Elle yük devretmeyi onaylayın** bölmesinde yük devretmek istediğiniz IoT hub'ını onaylamak için adını yazın. Ardından yük devretmeyi başlatmak için Tamam'a tıklayın. 

   ![Elle Yük Devretme bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Elle yük devretme işlemi çalışırken Elle Yük Devretme bölmesinde devam eden bir elle yük devretme işlemi olduğunu gösteren bir başlık görüntülenir. 

   ![Devam eden Elle Yük Devretme işlemini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   IoT Hub'ı bölmesini kapatıp Kaynak Grubu bölmesinde tıklayarak yeniden açmanız halinde hub'ın etkin olmadığını gösteren bir başlık görünür. 

   ![IoT Hub'ının devre dışı olduğunu gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   İşlem tamamlandıktan sonra Elle Yük Devretme sayfasındaki birincil ve ikincil bölgeler yer değiştirir ve hub yeniden etkin hale gelir. Bu örnekte birincil konum `WestCentralUS`, ikincil konum da `westus2` olmuştur. 

   ![Yük devretme işleminin tamamlandığını gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

## <a name="perform-a-failback"></a>Yeniden çalışma gerçekleştirme 

Elle yük devretme gerçekleştirdikten sonra hub işlemlerini tekrar birincil bölgeye geçirebilirsiniz. Bu işlem yeniden çalışma olarak adlandırılır. Yük devretme işlemini kısa bir süre önce gerçekleştirdiyseniz yeniden çalışma isteğinde bulunmak için yaklaşık bir saat beklemeniz gerekir. Daha kısa bir süre içinde yeniden çalışma isteğinde bulunursanız bir hata iletisi görüntülenir.

Yeniden çalışma işlemi, elle yük devretme işlemiyle aynı şekilde gerçekleştirilir. Bu adımlar şunlardır: 

1. Yeniden çalışma gerçekleştirmek için IoT hub'ınızın IoT Hub'ı bölmesine dönün.

2. IoT Hub'ı bölmesinin **Dayanıklılık** bölümünde **Elle yük devretme (önizleme)** seçeneğini belirleyin. 

3. Elle yük devretme bölmesinin en üstündeki **Yük devretmeyi başlat**'a tıklayın. **Elle yük devretmeyi onaylayın** bölmesi açılır. 

4. **Elle yük devretmeyi onaylayın** bölmesinde yeniden çalışma gerçekleştirmek istediğiniz IoT hub'ını onaylamak için adını yazın. Ardından yeniden çalışmayı başlatmak için Tamam'a tıklayın. 

   ![Elle yeniden çalışma isteğinin ekran görüntüsü](./media/tutorial-manual-failover/trigger-failback-01-regions.png)

   Başlıklar, gerçekleştirme yük devretme bölümünde açıklandığı gibi görüntülenir. Yeniden çalışma tamamlandıktan sonra birincil konum olarak `westus2`, ikincil konum olarak da `WestCentralUS` özgün değerleri gösterilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu öğretici için oluşturduğunuz kaynakları kaldırmak için kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda, IoT hub'ı ve kaynak grubu kaldırılır. 

1. **Kaynak Grupları**'na tıklayın. 

2. **ManlFailRG** adlı kaynak grubunu bulun ve seçin. Açmak için tıklayın. 

3. **Kaynak grubunu sil**'e tıklayın. İstendiğinde kaynak grubunun adını girin ve **Sil**'e tıklayarak onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki görevleri gerçekleştirerek elle yük devretme yapılandırma ve gerçekleştirmenin yanı sıra yeniden çalışma isteğinde bulunmayı öğrendiniz:

> [!div class="checklist"]
> * Azure portalı kullanarak bir IoT hub'ı oluşturun. 
> * Yük devretme gerçekleştirin. 
> * Hub'ın ikincil konumda çalıştığını görün.
> * IoT hub'ın işlemlerini birincil konuma geri almak için yeniden çalışma gerçekleştirin. 
> * Hub'ın doğru konumda düzgün biçimde çalıştığını onaylayın.

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [IoT cihazı durumunu yönetme](tutorial-device-twins.md)
