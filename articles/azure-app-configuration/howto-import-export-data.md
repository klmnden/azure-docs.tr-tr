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
ms.openlocfilehash: 851a8705b3cfa9c88e369af7b961a653dee8fd7a
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959859"
---
# <a name="import-or-export-configuration-data"></a>Yapılandırma verilerini içeri veya dışarı aktarma

Azure uygulama yapılandırmasını destekleyen veri alma ve verme işlemleri. Bu, yapılandırma verilerini de uygulama yapılandırmanız arasında exchange veri olarak depolar ve proje kodu toplu ile çalışmanıza olanak sağlar. Örneğin, verileri iki kez girmeniz gerekmez, test etmek için tek bir uygulama yapılandırma deposu, diğeri üretim ve kopya uygulama ayarları aralarında aracılığıyla bir dosya için ayarlayabilirsiniz.

Bu makalede, içeri ve dışarı aktarma ile uygulama yapılandırma verileri için bir kılavuz sağlar.

## <a name="import-data"></a>Veri içeri aktarma

İçeri aktarma, bunları el ile girmek yerine var olan bir kaynaktan uygulama yapılandırma veri deposu yapılandırması getirir. İçeri aktarma işlevi, bir uygulama yapılandırma deposu veya toplama verilerinin birden çok kaynaktan alınan verileri geçirmek için kullanabilirsiniz. Uygulama yapılandırmasını destekleyen JSON veya YAML özellikleri dosyadan içeri aktarabilirsiniz.

Kullanarak verileri içeri aktarabilirsiniz [Azure portalında](https://aka.ms/azconfig/portal) veya [Azure CLI](./scripts/cli-import.md). Azure portalında aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deponuza bulun ve tıklayın **içeri/dışarı aktarma**.

2. İçinde **alma** sekmesini, **kaynak hizmet** ve **yapılandırma dosyası**.

3. Seçin **dilin** ve **dosya türü**.

4. Tıklayarak **klasör** simgesi ve içeri aktarmak için bu dosyaya göz atın.

    ![İçeri aktarma dosyası](./media/import-file.png)

5. Seçin bir **ayırıcı** ve isteğe bağlı olarak girin bir **önek** içeri aktarılan anahtar adları için kullanılacak.

6. İsteğe bağlı olarak bir **etiket**.

7. Tıklayın **Uygula** alma işlemini tamamlamak için.

    ![Tam içeri aktarma dosyası](./media/import-file-complete.png)

## <a name="export-data"></a>Verileri dışarı aktarma

Dışarı aktarma için başka bir hedef uygulama yapılandırmasında depolanan yapılandırma verilerini yazar. Örneğin, uygulama kodunuz, dağıtım sırasında gömülecektir bir dosyaya bir uygulama yapılandırma deposundaki verileri kaydetmek için dışarı aktarma işlevini kullanabilirsiniz.

Kullanarak verileri dışarı aktarabilirsiniz [Azure portalında](https://aka.ms/azconfig/portal) veya [Azure CLI](./scripts/cli-export.md). Azure portalında aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deponuza bulun ve tıklayın **içeri/dışarı aktarma**.

2. İçinde **dışarı** sekmesini, **hedef hizmet** ve **yapılandırma dosyası**.

3. İsteğe bağlı olarak girin bir **önek** ve bir **etiket** ve bir-belirli bir noktaya anahtarlarının dışa aktarılacak.

4. Seçin bir **dosya türü** ve **ayırıcı**.

5. Tıklayın **Uygula** dışa aktarmayı tamamlamak için.

    ![Dışarı aktarma dosyası tamamlandı](./media/export-file-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: ASP.NET web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
