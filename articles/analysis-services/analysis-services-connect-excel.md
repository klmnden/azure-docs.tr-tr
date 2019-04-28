---
title: Azure Analysis Services Excel ile bağlanma | Microsoft Docs
description: Excel kullanarak bir Azure Analysis Services sunucusuna bağlanmayı öğreneceksiniz.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5c46d4e4d23744cf07ccf7857a33990bf405a6a1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023281"
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Bir sunucu oluşturduğunuz ve tablosal bir model dağıttıktan sonra istemcileri bağlanabilir ve verileri araştırmaya başladıktan. 

## <a name="before-you-begin"></a>Başlamadan önce

Hesabı ile bir model veritabanı rolüyle en azından okuma izinlerinin ait olmalıdır. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Excel'de bağlanma

Excel'de bir sunucuya bağlanma, Excel 2016 ve daha sonra Veri Al'ı kullanarak desteklenir. Power Pivot'ta Tablo Alma Sihirbazı'nı kullanarak bağlanma desteklenmiyor. 

1. Excel'de üzerinde **veri** Şerit, tıklayın **dış veri al** > **diğer kaynaklardan** > **gelen Analysis Services**.

2. Veri Bağlantı Sihirbazı ' nda, **sunucu adı**, protokolü ve URI gibi sunucu adını girin. Örneğin, asazure://westcentralus.asazure.windows.net/advworks. Ardından **oturum açma kimlik bilgilerini**seçin **aşağıdaki kullanıcı adını ve parolayı kullanın**ve ardından kuruluş kullanıcısına ad yazın örneğin nancy@adventureworks.comve parola.

    > [!IMPORTANT]
    > Bir Microsoft Account, Live ID, Yahoo, Gmail, vb. oturum oturum veya çok faktörlü kimlik doğrulaması ile oturum açmanız gereklidir, parola alanını boş bırakın. İleri'ye tıklama sonra bir parola istenir. 

    ![Excel oturum açma konumundan bağlanma](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. İçinde **veritabanı ve Tablo Seç**, veritabanı ve modeli veya perspektif seçin ve ardından **son**.
   
    ![Excel select modelden bağlanma](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.

[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)     


