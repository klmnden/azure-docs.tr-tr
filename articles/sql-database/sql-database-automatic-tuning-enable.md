---
title: "Azure SQL veritabanı için otomatik olarak ayarlamayı etkinleştirmek | Microsoft Docs"
description: "Otomatik Azure SQL veritabanınızda kolayca ayarlama etkinleştirebilirsiniz."
services: sql-database
documentationcenter: 
author: veljko-msft
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/19/2016
ms.author: veljko-msft
ms.openlocfilehash: 82db5996c1ba1f224593e4eaa5b3b0067755db49
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure SQL veritabanı sürekli sorgularınızı izler ve İş yükünüzün performansını iyileştirmek için yapabileceğiniz eylemi tanımlayan bir otomatik olarak yönetilen veri hizmetidir. Önerileri gözden geçirin ve el ile uygulamadan veya için otomatik olarak düzeltme eylemleri uygulamak Azure SQL veritabanı sağlar - bu olarak bilinir **otomatik ayarlama modu**. Otomatik ayarlama sunucu veya veritabanı düzeyinde etkinleştirilebilir.

## <a name="enable-automatic-tuning-on-server"></a>Otomatik sunucu üzerinde ayarlama etkinleştir

Azure SQL veritabanı sunucusuna otomatik olarak ayarlamayı etkinleştirmek için Azure portalında sunucusuna gidin ve ardından **otomatik ayarlama** menüde. İstediğiniz etkinleştirmek ve seçmek için otomatik ayarlama seçenekleri seçin **Uygula**:

![Sunucu](./media/sql-database-automatic-tuning-enable/server.png)

Sunucuda otomatik ayarlama seçenekleri, sunucudaki tüm veritabanları için uygulanır. Varsayılan olarak, tüm veritabanları, kendi üst sunucusundan yapılandırmasını devralmak, ancak bu kullanılabilir geçersiz kılındı ve tek tek her veritabanı için belirtilen.

## <a name="configure-automatic-tuning-on-database"></a>Otomatik veritabanı ayarlama yapılandırın

Azure portalı ayrı ayrı her veritabanını ayarlama yapılandırmasını otomatik belirtmenize olanak sağlar.

> [!NOTE]
> Aynı yapılandırma ayarları her veritabanı üzerinde otomatik olarak uygulanabilir şekilde otomatik ayarlama yapılandırma sunucusu düzeyinde yönetmek için genel yüklenmemesi önerilir. Veritabanı farklı ise tek bir veritabanının üzerine otomatik ayarlama başkalarının aynı sunucuda yapılandırın.
>

Otomatik üzerinde tek bir veritabanı ayarlamayı etkinleştirmek için Azure portalında veritabanına gidin ve sonra hem de seçin **otomatik ayarlama**. Yapılandırma veritabanı için ayrı ayrı belirtin veya seçeneğini belirleyerek sunucudan ayarlarını devralmak için tek bir veritabanı yapılandırabilirsiniz.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Uygun yapılandırma seçtikten sonra tıklatın **Uygula**.

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [otomatik ayarlama makale](sql-database-automatic-tuning.md) nasıl performansınızı geliştirmenize yardımcı olabilir ve otomatik ayarlama hakkında daha fazla bilgi edinmek için.
* Bkz: [performans önerileri](sql-database-advisor.md) Azure SQL veritabanı performans önerileri genel bakış.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) üst sorgularınızı performans etkisini görüntüleme hakkında bilgi edinmek için.
