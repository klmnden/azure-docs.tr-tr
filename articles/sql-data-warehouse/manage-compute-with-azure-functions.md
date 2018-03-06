---
title: "Azure SQL Veri Ambarı - Azure İşlevleri'ni kullanarak SQL Veri Deposu işlem düzeylerini otomatikleştirme | Microsoft Docs"
description: "Veri ambarınızın işlemini yönetmek için Azure işlevlerini kullanma."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 11/06/2017
ms.author: elbutter
ms.openlocfilehash: 8947da9d34261be46ad9aea961b6020141484172
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="use-azure-functions-to-automate-sql-dw-compute-levels"></a>Azure İşlevleri'ni kullanarak SQL DW işlem düzeylerini otomatikleştirme

Bu öğreticide, Azure İşlevleri'ni kullanarak Azure SQL Veri Ambarınızın işlem düzeylerini nasıl yönetebileceğiniz gösterilir. Bu mimarilerin SQL Veri Ambarı [Esneklik için İyileştirilmiş][Performance Tiers] katmanında kullanılması önerilir.

Azure İşlevi Uygulamasını SQL Veri Ambarı'yla kullanmak için, veri ambarı örneğinizle aynı abonelik kapsamında katılımcı erişimine sahip bir [Hizmet Sorumlusu Hesabı](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) oluşturmalısınız. 

## <a name="deploy-timer-based-scaler-with-an-azure-resource-manager-template"></a>Azure Resource Manager Şablonuyla Zamanlayıcı tabanlı skaler dağıtma

Şablonu dağıtmak için, aşağıdaki bilgilere ihtiyacınız vardır:

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

Şablonu dağıttıktan sonra, üç yeni kaynak bulmalısınız: ücretsiz bir Azure App Service Planı, kullanıma dayalı bir İşlev Uygulaması planı, günlüğü ve işlem kuyruğunu işleyecek bir depolama hesabı. Dağıtılan işlevlerin gereksinimlerinize uyacak şekilde nasıl değiştirileceğini görmek için okumaya devam edin.

### <a name="change-the-scale-up-or-scale-down-compute-level"></a>İşlem düzeyinin ölçeğini artırarak veya azaltarak değiştirme

1. İşlev Uygulaması hizmetinize gidin. Şablonu varsayılan değerlerle dağıttıysanız, bu hizmetin adı *DWOperations* olacaktır. İşlev Uygulamanız açıldığında, İşlev Uygulaması Hizmetinize beş işlevin dağıtılmış olduğunu görürsünüz. 

   ![Şablonla dağıtılan işlevler](media/manage-compute-with-azure-functions/five-functions.png)

2. Zaman ölçeğini artırmak mı yoksa azaltmak mı istediğinize bağlı olarak, *DWScaleDownTrigger*'ı veya *DWScaleUpTrigger*'ı seçin. Açılan listede Tümleştir'i seçin.

   ![İşlev için Tümleştir'i seçme](media/manage-compute-with-azure-functions/select-integrate.png)

3. Şu anda görüntülenen değer *%ScaleDownTime%* veya *%ScaleUpTime%* olmalıdır. Bu değerler, zamanlamanın [Uygulama Ayarları][Application Settings] altında tanımlanmış değerleri temel alacağını gösterir. Şimdilik bunu yoksayabilir ve sonraki adımlara göre zamanlamayı tercih ettiğiniz şekilde değiştirebilirsiniz.

4. Zamanlama alanında, SQL Veri Ambarı'nın ölçeğinin ne sıklıkta artırılmasını istediğinizi yansıtan zaman CRON ifadesini ekleyin. 

  ![İşlev zamanlamasını değiştirme](media/manage-compute-with-azure-functions/change-schedule.png)

  `schedule` değeri, şu altı alanı içeren bir [CRON ifadesidir](http://en.wikipedia.org/wiki/Cron#CRON_expression): 
  ```json
  {second} {minute} {hour} {day} {month} {day-of-week}
  ```

  Örneğin *"0 30 9 * * 1-5"*, hafta içi her gün 09:30'daki tetikleyiciyi yansıtır. Daha fazla bilgi için Azure İşlevleri[zamanlama örnekleri][schedule examples] sayfasını ziyaret edin.


### <a name="change-the-scale-up-or-scale-down-time"></a>Zaman ölçeğini artırarak veya azaltarak değiştirme

1. İşlev Uygulaması hizmetinize gidin. Şablonu varsayılan değerlerle dağıttıysanız, bu hizmetin adı *DWOperations* olacaktır. İşlev Uygulamanız açıldığında, İşlev Uygulaması Hizmetinize beş işlevin dağıtılmış olduğunu görürsünüz. 

2. İşlem değeri ölçeğini artırmak mı yoksa azaltmak mı istediğinize bağlı olarak, *DWScaleDownTrigger*'ı veya *DWScaleUpTrigger*'ı seçin. İşlevleri seçtikten sonra, bölmenizde *index.js* dosyası gösterilmelidir.

   ![İşlev tetikleyicisi işlem düzeyini değiştirme](media/manage-compute-with-azure-functions/index-js.png)

3. *ServiceLevelObjective*'in değerini istediğiniz düzeyle değiştirin ve Kaydet'e tıklayın. Bu, Tümleştir bölümünde tanımlanan zamanlama temelinde veri ambarı örneğinizin ölçeklendirileceği işlem düzeyidir.

### <a name="use-pause-or-resume-instead-of-scale"></a>Ölçek yerine duraklatma veya sürdürme kullanma 

Şu anda, varsayılan olarak açık olan işlevler *DWScaleDownTrigger* ve *DWScaleUpTrigger*'dır. Bunun yerine duraklatma ve sürdürme işlevselliğini kullanmak isterseniz, *DWPauseTrigger* veya *DWResumeTrigger*'ı etkinleştirebilirsiniz.

1. İşlevler bölmesine gidin.

   ![İşlevler bölmesi](media/manage-compute-with-azure-functions/functions-pane.png)



2. Etkinleştirmek istediğiniz tetikleyicilere karşılık gelen kayan düğmeye tıklayın.

3. Zamanlamalarını değiştirmek için ilgili tetikleyicilerin *Tümleştir* sekmelerine gidin.

   [!NOTE]: The functional difference between the scaling triggers and the pause/resume triggers is the message that is sent to the queue. See [Add a new trigger function][Add a new trigger function] for more information.



### <a name="add-a-new-trigger-function"></a>Yeni tetikleyici işlevi ekleme

Şu anda, şablona dahil edilmiş yalnızca iki ölçeklendirme işlevi vardır. Bu, gün boyunca yalnızca bir kez ölçeği azaltabileceğiniz ve bir kez de artırabileceğiniz anlamına gelin. Daha ayrıntılı bir denetim elde etmek, örneğin ölçeği bir günde birkaç kez azaltmak veya hafta sonralı farklı bir ölçeklendirme davranışına sahip olmak için, başka bir tetikleyici eklemeniz gerekir.

1. Yeni boş bir işlev oluşturun. İşlev şablonu bölmesini açmak için İşlevler konumunuzun yakınındaki *+* düğmesini seçin.

   ![Yeni işlev oluşturma](media/manage-compute-with-azure-functions/create-new-function.png)

2. Dil'de *Javascript*'i seçin ve sonra da *TimerTrigger*'ı seçin.

   ![Yeni işlev oluşturma](media/manage-compute-with-azure-functions/timertrigger-js.png)

3. İşlevinizi adlandırın ve zamanlamanızı ayarlayın. Resimde, işlevin her Cumartesi gece yarısı (Cuma gecesi) nasıl tetiklenebileceği gösterilir.

   ![Cumartesi ölçeği azaltma](media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Diğer tetikleyici işlevlerinden birinin *index.js* içeriğini kopyalayın.

   ![Index.js'yi kopyalama](media/manage-compute-with-azure-functions/index-js.png)

5. İşlem değişkeninizi aşağıda gösterildiği gibi istediğiniz davranışa ayarlayın:

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


### <a name="complex-scheduling"></a>Karmaşık zamanlama

Bu bölümde, duraklatma, sürdürme ve ölçeklendirme özelliklerinin daha karmaşık zamanlaması için gerekenler kısaca gösterilir.

#### <a name="example-1"></a>Örnek 1:

Gündelik olarak 08:00'da DW600'a ölçeği artırma ve 20:00'da DW200'e ölçeği azaltma.

| İşlev  | Zamanlama     | İşlem                                |
| :-------- | :----------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW600"}` |
| İşlev2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-2"></a>Örnek 2: 

Gündelik olarak 08:00'da DW1000'e ölçeği artırma ve 16:00'da DW600'e, sonra da 22:00'da DW200'e ölçeği azaltma.

| İşlev  | Zamanlama     | İşlem                                |
| :-------- | :----------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW1000"}` |
| İşlev2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| İşlev3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-3"></a>Örnek 3: 

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

[Performance Tiers]: performance-tiers.md
