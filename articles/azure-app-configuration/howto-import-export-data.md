---
title: Azure uygulama yapılandırma verilerini dışarı veya içeri aktarın | Microsoft Docs
description: İçeri aktarma veya için veya Azure uygulama yapılandırma verileri dışarı aktarma hakkında bilgi edinin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 377c5088d39821e87412c517540b3190b0a14a00
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393281"
---
# <a name="import-or-export-configuration-data"></a>Yapılandırma verilerini içeri veya dışarı aktarma

Azure uygulama yapılandırmasını destekleyen veri alma ve verme işlemleri. Uygulama yapılandırma deposu ve kod arasında toplu ve exchange verilerini yapılandırma verilerle çalışmayı bu işlemleri kullanmanız proje. Örneğin, test etmek için tek bir uygulama yapılandırma deposu, diğeri üretim için ayarlayabilirsiniz. Böylece verileri iki kez girmesi gerekmez, uygulama ayarları aralarında aracılığıyla bir dosyayı kopyalayabilirsiniz.

Bu makalede, içeri ve dışarı aktarma ile uygulama yapılandırma verileri için bir kılavuz sağlar.

## <a name="import-data"></a>Veri içeri aktarma

İçeri aktarma el ile girmek yerine var olan bir kaynaktan bir uygulama yapılandırma veri deposu yapılandırması getirir. Bir uygulama yapılandırma deposu veya toplama verilerinin birden çok kaynaktan alınan verileri geçirmek için içeri aktarma işlevini kullanın. JSON veya YAML özellikleri dosyadan içeri aktarabilirsiniz, uygulama yapılandırmasını desteklemez.

Kullanarak verileri içeri aktarma [Azure portalında](https://portal.azure.com) veya [Azure CLI](./scripts/cli-import.md). Azure portalında aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deposuna Gözat ve Seç **içeri/dışarı aktarma**.

2. Üzerinde **alma** sekmesinde **kaynak hizmet** > **yapılandırma dosyası**.

3. Seçin **dilin** > **dosya türü**.

4. Seçin **klasör** simgesi ve içeri aktarmak için bu dosyaya göz atın.

    ![İçeri aktarma dosyası](./media/import-file.png)

5. Seçin bir **ayırıcı**ve isteğe bağlı olarak girin bir **önek** içeri aktarılan anahtar adları için kullanılacak.

6. İsteğe bağlı olarak bir **etiket**.

7. Seçin **Uygula** almayı tamamlamak için.

    ![Dosya içeri aktarma tamamlandı](./media/import-file-complete.png)

## <a name="export-data"></a>Verileri dışarı aktarma

Dışarı aktarma için başka bir hedef uygulama yapılandırmasında depolanan yapılandırma verilerini yazar. Dışarı aktarma işlevi, örneğin, uygulama kodunuz, dağıtım sırasında katıştırılmış bir dosyaya bir uygulama yapılandırma deposundaki verileri kaydetmek için kullanın.

Kullanarak verileri dışarı aktarma [Azure portalında](https://portal.azure.com) veya [Azure CLI](./scripts/cli-export.md). Azure portalında aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deposuna Gözat ve Seç **içeri/dışarı aktarma**.

2. Üzerinde **dışarı** sekmesinde **hedef hizmet** > **yapılandırma dosyası**.

3. İsteğe bağlı olarak girin bir **önek** seçip bir **etiket** ve bir-belirli bir noktaya anahtarlarının dışa aktarılacak.

4. Seçin bir **dosya türü** > **ayırıcı**.

5. Seçin **Uygula** dışarı aktarma tamamlanması.

    ![Dışarı aktarma dosyası tamamlandı](./media/export-file-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir ASP.NET Core web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
