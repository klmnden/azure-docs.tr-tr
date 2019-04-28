---
title: "Öğretici: Azure SQL veri ambarı'nda Azure işlevleri ile işlemleri yönetme | Microsoft Docs"
description: Veri ambarınızın işlemini yönetmek için Azure işlevlerini kullanma.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: consume
ms.date: 04/27/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: b94e4c6f178119d6205c302cf35a9effaf2aa885
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61083948"
---
# <a name="use-azure-functions-to-manage-compute-resources-in-azure-sql-data-warehouse"></a>İşlem kaynakları Azure SQL veri ambarı'nda yönetmek için Azure işlevlerini kullanın

Bu öğreticide, Azure SQL veri ambarı'nda bir veri ambarı için işlem kaynaklarını yönetmek için Azure işlevleri kullanır.

Azure İşlevi Uygulamasını SQL Veri Ambarı'yla kullanmak için, veri ambarı örneğinizle aynı abonelik kapsamında katılımcı erişimine sahip bir [Hizmet Sorumlusu Hesabı](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) oluşturmalısınız. 

## <a name="deploy-timer-based-scaling-with-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonuyla Zamanlayıcı tabanlı ölçeklendirme dağıtma

Şablonu dağıtmak için aşağıdaki bilgiler gereklidir:

- SQL DW örneğinizin içinde bulunduğu kaynak grubunun adı
- SQL DW örneğinizin içinde bulunduğu mantıksal sunucunun adı
- SQL DW örneğinizin adı
- Azure Active Directory'nizin Kiracı Kimliği (Dizin Kimliği)
- Abonelik Kimliği 
- Hizmet Sorumlusu Uygulama Kimliği
- Hizmet Sorumlusu Gizli Anahtarı

Yukarıdaki bilgileri aldıktan sonra şu şablonu dağıtın:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Şablonu dağıttıktan sonra üç yeni kaynak bulmalısınız: ücretsiz bir Azure App Service planı, tüketim temelli bir işlev uygulaması planı ve günlüğe kaydetme ve işlem kuyruğunu işleyecek bir depolama hesabı. Dağıtılan işlevlerin gereksinimlerinize uyacak şekilde nasıl değiştirileceğini görmek için okumaya devam edin.

## <a name="change-the-compute-level"></a>İşlem düzeyini değiştirme

1. İşlev Uygulaması hizmetinize gidin. Şablonu varsayılan değerlerle dağıttıysanız, bu hizmetin adı *DWOperations* olacaktır. İşlev Uygulamanız açıldığında, İşlev Uygulaması Hizmetinize beş işlevin dağıtılmış olduğunu görürsünüz. 

   ![Şablonla dağıtılan işlevler](media/manage-compute-with-azure-functions/five-functions.png)

2. Zaman ölçeğini artırmak mı yoksa azaltmak mı istediğinize bağlı olarak, *DWScaleDownTrigger*'ı veya *DWScaleUpTrigger*'ı seçin. Açılan menüde, tümleştir'i seçin.

   ![İşlev için Tümleştir'i seçme](media/manage-compute-with-azure-functions/select-integrate.png)

3. Şu anda görüntülenen değer *%ScaleDownTime%* veya *%ScaleUpTime%* olmalıdır. Bu değerler, zamanlamanın [Uygulama Ayarları][Application Settings] altında tanımlanmış değerleri temel alacağını gösterir. Şimdilik bu değeri yoksayar ve tercih edilen zaman sonraki adımlara göre zamanlamayı değiştirebilirsiniz.

4. Zamanlama alanında, SQL Veri Ambarı'nın ölçeğinin ne sıklıkta artırılmasını istediğinizi yansıtan zaman CRON ifadesini ekleyin. 

   ![İşlev zamanlamasını değiştirme](media/manage-compute-with-azure-functions/change-schedule.png)

   `schedule` değeri, şu altı alanı içeren bir [CRON ifadesidir](https://en.wikipedia.org/wiki/Cron#CRON_expression): 
   ```json
   {second} {minute} {hour} {day} {month} {day-of-week}
   ```

   Örneğin, *"0 30 9 ** 1-5"* bir tetikleyici hafta içi her gün saat 09: 30'da yansıtır. Daha fazla bilgi için Azure İşlevleri[zamanlama örnekleri][schedule examples] sayfasını ziyaret edin.


## <a name="change-the-time-of-the-scale-operation"></a>Ölçeklendirme işleminin saatini değiştir

1. İşlev Uygulaması hizmetinize gidin. Şablonu varsayılan değerlerle dağıttıysanız, bu hizmetin adı *DWOperations* olacaktır. İşlev Uygulamanız açıldığında, İşlev Uygulaması Hizmetinize beş işlevin dağıtılmış olduğunu görürsünüz. 

2. İşlem değeri ölçeğini artırmak mı yoksa azaltmak mı istediğinize bağlı olarak, *DWScaleDownTrigger*'ı veya *DWScaleUpTrigger*'ı seçin. İşlevleri seçtikten sonra, bölmenizde *index.js* dosyası gösterilmelidir.

   ![İşlev tetikleyicisi işlem düzeyini değiştirme](media/manage-compute-with-azure-functions/index-js.png)

3. *ServiceLevelObjective*'in değerini istediğiniz düzeyle değiştirin ve Kaydet'e tıklayın. Bu değer tümleştir bölümünde tanımlanan zamanlama dayalı olarak veri ambarı örneğinizi ölçeklendirilebilen işlem düzeyidir.

## <a name="use-pause-or-resume-instead-of-scale"></a>Ölçek yerine duraklatma veya sürdürme kullanma 

Şu anda, varsayılan olarak açık olan işlevler *DWScaleDownTrigger* ve *DWScaleUpTrigger*'dır. Bunun yerine duraklatma ve sürdürme işlevselliğini kullanmak isterseniz, *DWPauseTrigger* veya *DWResumeTrigger*'ı etkinleştirebilirsiniz.

1. İşlevler bölmesine gidin.

   ![İşlevler bölmesi](media/manage-compute-with-azure-functions/functions-pane.png)



2. Etkinleştirmek istediğiniz tetikleyicilere karşılık gelen kayan düğmeye tıklayın.

3. Zamanlamalarını değiştirmek için ilgili tetikleyicilerin *Tümleştir* sekmelerine gidin.

   > [!NOTE]
   > Ölçeklendirme tetikleyiciler ve VM'yi duraklatabilir/sürdürebilirsiniz Tetikleyiciler arasında işlevsel bir fark, kuyruğa gönderilen iletisidir. Daha fazla bilgi için [Yeni Tetikleyici işlevi ekleme][Add a new trigger function].


## <a name="add-a-new-trigger-function"></a>Yeni tetikleyici işlevi ekleme

Şu anda, şablona dahil edilmiş yalnızca iki ölçeklendirme işlevi vardır. Sahip bu işlevlerin, bir günün Kurs sırasında yalnızca bir kez aşağı ve defa ölçeklendirebilirsiniz. Günde birkaç kez ölçeği veya sonralı farklı bir ölçeklendirme davranışına sahip gibi daha ayrıntılı denetim için başka bir tetikleyici eklemeniz gerekir.

1. Yeni boş bir işlev oluşturun. Seçin *+* işlev şablonu bölmesini görüntülemek için İşlevler konumunuzun yakınındaki düğmesine.

   ![Yeni işlev oluşturma](media/manage-compute-with-azure-functions/create-new-function.png)

2. Dil'de *Javascript*'i seçin ve sonra da *TimerTrigger*'ı seçin.

   ![Yeni işlev oluşturma](media/manage-compute-with-azure-functions/timertrigger-js.png)

3. İşlevinizi adlandırın ve zamanlamanızı ayarlayın. Resimde, işlevin her Cumartesi gece yarısı (Cuma gecesi) nasıl tetiklenebileceği gösterilir.

   ![Cumartesi ölçeği azaltma](media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Diğer tetikleyici işlevlerinden birinin *index.js* içeriğini kopyalayın.

   ![Index.js'yi kopyalama](media/manage-compute-with-azure-functions/index-js.png)

5. İşlem Değişkeninizi için istenen davranışı aşağıdaki gibi ayarlayın:

   ```javascript
   // Resume the data warehouse instance
   var operation = {
       "operationType": "ResumeDw"
   }

   // Pause the data warehouse instance
   var operation = {
       "operationType": "PauseDw"
   }

   // Scale the data warehouse instance to DW600
   var operation = {
       "operationType": "ScaleDw",
       "ServiceLevelObjective": "DW600"
   }
   ```


## <a name="complex-scheduling"></a>Karmaşık zamanlama

Bu bölümde kısaca gerekenler gösterilmektedir daha karmaşık zamanlaması duraklatma, sürdürme ve ölçeklendirme özelliklerinin.

### <a name="example-1"></a>Örnek 1:

Gündelik olarak 08:00'da DW600'a ölçeği artırma ve 20:00'da DW200'e ölçeği azaltma.

| İşlev  | Zamanlama     | İşlem                                |
| :-------- | :----------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW600"}` |
| İşlev2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-2"></a>Örnek 2: 

Günlük ölçeği artırma dw1000, sabah 8 saat 4'te bir kez DW600 ölçeğini ve 10 pm da dw200'e ölçeği.

| İşlev  | Zamanlama     | İşlem                                |
| :-------- | :----------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW1000"}` |
| İşlev2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| İşlev3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-3"></a>Örnek 3: 

Hafta içi günlerinde 08:00'da DW1000'e ölçeği artırma ve 16:00'da bir kez DW600'e ölçeği azaltma. Cuma 23:00'da duraklatılır, Pazartesi sabahı 07:00'da sürdürülür.

| İşlev  | Zamanlama       | İşlem                                |
| :-------- | :------------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * 1-5  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| İşlev2 | 0 0 16 * * 1-5 | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| İşlev3 | 0 0 23 * * 5   | `var operation = {"operationType": "PauseDw"}` |
| İşlev4 | 0 0 7 * * 0    | `var operation = {"operationType": "ResumeDw"}` |



## <a name="next-steps"></a>Sonraki adımlar

[Zamanlayıcı tetikleyicisi](../azure-functions/functions-create-scheduled-function.md) Azure işlevleri hakkında daha fazla bilgi edinin.

SQL Veri Ambarı [örnek deposunu](https://github.com/Microsoft/sql-data-warehouse-samples) gözden geçirin.



[schedule examples]: ../azure-functions/functions-bindings-timer.md#example

[Application Settings]: ../azure-functions/functions-how-to-use-azure-function-app-settings.md
[Add a new trigger function]: manage-compute-with-azure-functions.md#add-a-new-trigger-function
