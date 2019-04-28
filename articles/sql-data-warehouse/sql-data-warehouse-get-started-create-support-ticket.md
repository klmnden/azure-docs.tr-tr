---
title: Azure SQL veri ambarı için bir destek bileti oluşturma | Microsoft Docs
description: Azure SQL Data Warehouse'da destek bileti oluşturma
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b3ffc9794967f44708d8330d4dc5d5de11b794d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61474564"
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>SQL Data Warehouse için destek bileti oluşturma
SQL Veri Ambarı’nız ile ilgili herhangi bir sorun yaşıyorsanız mühendislik destek ekibimizin size yardımcı olabilmesi için bir destek bileti oluşturun.

## <a name="create-a-support-ticket"></a>Destek bileti oluşturma
1. [Azure portalını][Azure portal] açın.
2. Giriş ekranında **Yardım + destek** sekmesine tıklayın.
   
    ![Yardım + destek](./media/sql-data-warehouse-get-started-create-support-ticket/MainPage.PNG)
3. Yardım + Destek dikey penceresinde **Yeni destek isteği**’ne tıklayın ve **Temel Bilgiler** dikey penceresini doldurun.

   [Azure destek planınızı][Azure support plan] seçin.
   
   * Tüm destek düzeylerinde **faturalama, kota ve abonelik yönetimi** desteği sunulmaktadır.
   * **Onarım** desteği [Geliştirici][Developer], [Standart][Standard], [Profesyonel Doğrudan][Professional Direct] veya [Premier][Premier] destek aracılığıyla sağlanır. Onarım sorunları, müşterilerin Azure'ı kullandığı sırada karşılaştıkları ve ilgili sorunun Microsoft'tan kaynaklandığına ilişkin makul bir olasılığın bulunduğu sorunlardır.
   * **Geliştirici rehberliği** ve **danışmanlık hizmetleri**, [Profesyonel Doğrudan][Professional Direct] ve [Premier][Premier] destek düzeylerinde kullanılabilir. 
     
     Bir Premier destek planınız varsa SQL Veri Ambarı ile ilgili sorunları [Microsoft Premier çevrimiçi portalı][Microsoft Premier online portal] üzerinden de bildirebilirsiniz.  Kapsam,yanıt süreleri ve fiyatlandırma dahil olmak üzere çeşitli destek planları hakkında daha fazla bilgi edinmek için bkz. [Azure destek planları][Azure support plan].  Azure desteği ile ilgili sık sorulan sorular için bkz. [Azure desteği ile ilgili SSS][Azure support FAQs].  
        
     ![Temel bilgiler dikey penceresi](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_1.PNG)
     ![Temel bilgiler dikey penceresi1](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_2.PNG)
4. **Sorun** dikey penceresini doldurun.
    ![Problem_blade](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_3.PNG)
   
   > [!NOTE]
   > Varsayılan olarak her SQL sunucusu (örn. myserver.database.windows.net) 45.000’lik **DTU Kotası**’na sahiptir. Bu kota yalnızca bir güvenlik sınırıdır. Bir destek bileti oluşturarak ve istek türü olarak *Kota* ’ yı seçerek kotanızı artırabilirsiniz. DTU gereksinimlerinizi hesaplamak için gereken toplam [DWU][DWU] değerini 7,5 ile çarpın. Örneğin, tek bir SQL sunucusu üzerinde iki DW6000 barındırmak istiyorsanız, 90.000’lik DTU kotası istemeniz gerekir.  Geçerli DTU tüketiminizi portaldaki SQL server dikey penceresinden görüntüleyebilirsiniz. DTU kotasında hem duraklatılmış hem de duraklatılmamış veritabanları sayılır. 
   > 
   > 
   
5. **İletişim bilgilerinizi** girin.
   ![Contact_information](./media/sql-data-warehouse-get-started-create-support-ticket/Create_ticket_4.PNG)

    
6. Destek isteğini göndermek için **Oluştur**'a tıklayın.

## <a name="monitor-a-support-ticket"></a>Destek biletini izleme
Destek isteğini gönderdikten sonra Azure destek ekibi sizinle iletişime geçer. İstek durumunuzu ve ayrıntılarını kontrol etmek için panoda bulunan **Tüm destek istekleri** kutucuğuna tıklayın.

![Durumu kontrol etme](./media/sql-data-warehouse-get-started-create-support-ticket/Monitor_ticket.PNG)

## <a name="other-resources"></a>Diğer kaynaklar
Ayrıca, [Stack Overflow][Stack Overflow] veya [Azure SQL Veri Ambarı MSDN forumu][Azure SQL Data Warehouse MSDN forum] üzerinden SQL Veri Ambarı topluluğuna bağlanabilirsiniz.

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

