---
title: Azure Analysis Services Excel ile bağlanma | Microsoft Docs
description: Excel kullanarak bir Azure Analysis Services sunucusuna bağlanmayı öğreneceksiniz.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 807496584acb3f93fccd3495de005792b769b37f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443011"
---
# <a name="connect-with-excel"></a>Excel ile bağlanma

Bir sunucu oluşturduğunuz ve tablosal bir model dağıttıktan sonra istemcileri bağlanabilir ve verileri araştırmaya başladıktan. 

## <a name="before-you-begin"></a>Başlamadan önce
Oturum açmada hesabı bir model veritabanı rolüyle en azından okuma izinlerinin ait olmalıdır. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Excel'de bağlanma

Excel'de bir sunucuya bağlanma, Excel 2016'da veri al seçeneğini kullanarak desteklenir. Power Pivot'ta Tablo Alma Sihirbazı'nı kullanarak bağlanma desteklenmiyor. 

**Excel 2016'da bağlanmak için**

1. 2016 üzerinde Excel **veri** Şerit, tıklayın **dış veri al** > **diğer kaynaklardan** > **gelen Analysis Services** .

2. Veri Bağlantı Sihirbazı ' nda, **sunucu adı**, protokolü ve URI gibi sunucu adını girin. Örneğin, asazure://westcentralus.asazure.windows.net/advworks. Ardından **oturum açma kimlik bilgilerini**seçin **aşağıdaki kullanıcı adını ve parolayı kullanın**ve ardından kuruluş kullanıcısına ad yazın örneğin nancy@adventureworks.comve parola.

    > [!IMPORTANT]
    > Bir Microsoft Account, Live ID, Yahoo, Gmail, vb. oturum oturum veya çok faktörlü kimlik doğrulaması ile oturum açmanız gereklidir, parola alanını boş bırakın. İleri'ye tıklama sonra bir parola istenir. 

    ![Excel oturum açma konumundan bağlanma](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. İçinde **veritabanı ve Tablo Seç**, veritabanı ve modeli veya perspektif seçin ve ardından **son**.
   
    ![Excel select modelden bağlanma](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Ayrıca bkz.
[İstemci kitaplıkları](analysis-services-data-providers.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)     


