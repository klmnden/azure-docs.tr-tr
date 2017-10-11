---
title: "Azure Analysis Services Excel ile bağlanma | Microsoft Docs"
description: "Excel kullanarak bir Azure Analysis Services sunucusuna bağlanmak öğrenin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: d51b6980120f1cf9bc8d053d463a73ac600b915f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Azure üzerinde bir sunucu oluşturulur ve bir tablo modeline dağıtılmış sonra bağlanmak ve veri araştırmaya başlamak hazırsınız.


## <a name="connect-in-excel"></a>Excel'de Bağlan

Excel'de sunucusuna bağlanan Excel 2016'da Veri Al kullanılarak desteklenir. Power Pivot Tablo Alma Sihirbazı'nı kullanarak bağlanması desteklenmiyor. 

**Excel 2016'da bağlanmak için**

1. 2016, üzerinde Excel **veri** Şerit, tıklatın **dış veri al** > **diğer kaynaklardan** > **Çözümleme Hizmetleri'nden** .

2. Veri Bağlantı Sihirbazı ' nda içinde **sunucu adı**, protokol ve URI'sini de dahil olmak üzere sunucu adı girin. Ardından **oturum açma kimlik bilgileri**seçin **aşağıdaki kullanıcı adını ve parolayı kullan**ve kuruluş kullanıcı adı, örneğin yazın nancy@adventureworks.comve parola.

    ![Excel oturumu açma bağlanma](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. İçinde **veritabanı ve Tablo Seç**, veritabanı ve model veya perspektif seçin ve ardından **son**.
   
    ![Excel select modelden Bağlan](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.
[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)     


