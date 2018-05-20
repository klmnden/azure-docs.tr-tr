---
title: Azure Analysis Services Excel ile bağlanma | Microsoft Docs
description: Excel kullanarak bir Azure Analysis Services sunucusuna bağlanmak öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b3ab749b4610f57e19ad1af0fc74d41340d7555b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Bir sunucu oluşturulur ve bir tablo modeline dağıtılmış sonra istemcileri bağlanabilir ve verileri araştırmaya başlamak.


## <a name="connect-in-excel"></a>Excel'de Bağlan

Excel'de sunucusuna bağlanan Excel 2016'da Veri Al kullanılarak desteklenir. Power Pivot Tablo Alma Sihirbazı'nı kullanarak bağlanması desteklenmiyor. 

**Excel 2016'da bağlanmak için**

1. 2016, üzerinde Excel **veri** Şerit, tıklatın **dış veri al** > **diğer kaynaklardan** > **Çözümleme Hizmetleri'nden** .

2. Veri Bağlantı Sihirbazı ' nda içinde **sunucu adı**, protokol ve URI'sini de dahil olmak üzere sunucu adı girin. Örneğin, asazure://westcentralus.asazure.windows.net/advworks. Ardından **oturum açma kimlik bilgileri**seçin **aşağıdaki kullanıcı adını ve parolayı kullan**ve kuruluş kullanıcı adı, örneğin yazın nancy@adventureworks.comve parola.

    > [!IMPORTANT]
    > Bir Microsoft Account, Live ID, Yahoo, Gmail, vb. ile oturum ya da çok faktörlü kimlik doğrulaması ile oturum imzalamak için gereklidir, parola alanı boş bırakın. İleri'yi tıklatmadan sonra için bir parola istenir.

    ![Excel oturumu açma bağlanma](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. İçinde **veritabanı ve Tablo Seç**, veritabanı ve model veya perspektif seçin ve ardından **son**.
   
    ![Excel select modelden Bağlan](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.
[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)     


