---
title: Oturum açma kimlik bilgileri, Azure Stream Analytics işlerini Döndür
description: Bu makalede, girdi kimlik bilgilerini güncelleştirmeniz açıklar ve çıktı Azure akış analizi işleri havuzlarını.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: b49b4fecb6be70987e7e6736d78f224c03f719bf
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-of-a-stream-analytics-job"></a>Giriş ve çıkış akış analizi işi, oturum açma kimlik bilgileri döndürün

Bir giriş veya çıkış akış analizi işi için kimlik bilgilerini yeniden her iş yeni kimlik bilgileriyle güncelleştirmeniz gerekir. Kimlik bilgileri güncelleştirmeden önce iş durdurmanız gerekir, iş çalışırken kimlik bilgilerini değiştirilemiyor. İş durdurup arasındaki gecikme azaltmak için akış analizi, son çıktısını işten sürdürme destekler. Bu konuda, oturum açma kimlik bilgilerini döndürme ve iş yeni kimlik bilgileri ile yeniden başlatma işlemi açıklanmaktadır.

## <a name="regenerate-new-credentials-and-update-your-job-with-the-new-credentials"></a>Yeni kimlik bilgilerini yeniden oluşturun ve işinizi yeni kimlik bilgileriyle güncelleştirin 

Bu bölümde, biz yeniden oluşturuluyor kimlik bilgileri üzerinden Blob Storage, olay hub'ları, SQL veritabanı ve tablo depolaması için anlatılır. 

### <a name="blob-storagetable-storage"></a>BLOB Depolama/tablo depolama
1. Azure portalında oturum açın > Stream Analytics işi için giriş/çıkış kullanılan depolama hesabı göz atın.    
2. Ayarları bölümünden açmak **erişim anahtarları**. İki varsayılan anahtarlar (key1, key2), arasındaki, iş tarafından kullanılmayan bir çekme ve onu yeniden:  
   ![Depolama hesabı anahtarlarını yeniden oluştur](media/stream-analytics-login-credentials-inputs-outputs/image1.png)
3. Yeni oluşturulan anahtarı kopyalayın.    
4. Stream Analytics işi Azure Portal'da Gözat > seçin **durdurmak** ve işi durdurmak bekleyin.    
5. Blob/tablosunu bulun depolama giriş/çıkış kimlik bilgilerini güncelleştirmek istediğiniz.    
6. Bul **depolama hesabı anahtarı** alan ve yeni oluşturulan anahtarınızı yapıştırma > tıklatın **kaydetmek**.    
7. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak, bildirimler sekmesinden görüntüleyebilirsiniz. İki bildirimleri-bir güncelleştirme kaydetmek için karşılık gelen vardır ve diğer bağlantı sınama için karşılık gelir:  
   ![Anahtar düzenleme sonrasında bildirimleri](media/stream-analytics-login-credentials-inputs-outputs/image4.png)
8. [Son durdurulma saati işinizi başlatmak için] devam (#start-your-job-from-the-last-stopped-time) bölümü.

### <a name="event-hubs"></a>Event Hubs

1. Azure portalında oturum açın > Stream Analytics işi için giriş/çıkış kullanılan olay hub'ı bulun.    
2. Ayarları bölümünden açmak **paylaşılan erişim ilkeleri** ve gerekli erişim ilkesini seçin. Arasında **birincil anahtar** ve **ikincil anahtar**, işinizi tarafından kullanılmayan bir çekme ve onu yeniden oluştur:  
   ![Olay hub'ı için anahtarlarını yeniden oluştur](media/stream-analytics-login-credentials-inputs-outputs/image2.png)
3. Yeni oluşturulan anahtarı kopyalayın.    
4. Stream Analytics işi Azure Portal'da Gözat > seçin **durdurmak** ve işi durdurmak bekleyin.    
5. Hub giriş/çıkış kimlik bilgilerini güncelleştirmek istediğiniz olayı bulun.    
6. Bul **olay hub'ı İlkesi anahtarı** alan ve yeni oluşturulan anahtarınızı yapıştırın > tıklatın **kaydetmek**.    
7. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak başarıyla geçti emin olun.    
8. İle devam [işinizi son durdurulma saati başlangıç](#start-your-job-from-the-last-stopped-time) bölümü.

### <a name="sql-database"></a>SQL Database

Mevcut bir kullanıcının oturum açma kimlik bilgilerini güncelleştirmek için SQL veritabanına bağlanmak için gerekir. Azure portal veya SQL Server Management Studio gibi bir istemci-tarafı aracını kullanarak kimlik bilgilerini güncelleştirebilirsiniz. Bu bölümde Azure portalı kullanarak kimlik bilgilerini güncelleştirme işlemini gösterir.

1. Azure portalında oturum açın > Stream Analytics işi çıkış olarak kullanılan SQL veritabanı göz atın.    
2. Gelen **Veri Gezgini**, oturum açma/bağlantısı veritabanınıza > Yetkilendirme türü olarak seçin **SQL server kimlik doğrulaması** > yazın, **oturum açma** ve  **Parola** ayrıntıları > seçin **Tamam**.  
   ![SQL veritabanı için kimlik bilgilerini yeniden oluşturun](media/stream-analytics-login-credentials-inputs-outputs/image3.png)

3. Sorgu sekmesinde, aşağıdaki sorguyu çalıştırarak, kullanıcının parolasını alter (değiştirdiğinizden emin olun `<user_name>` , username ve `<new_password>` yeni parolanızla):  

   ```SQL
   Alter user `<user_name>` WITH PASSWORD = '<new_password>'
   Alter role db_owner Add member `<user_name>`
   ```

4. Yeni parolayı not edin.    
5. Stream Analytics işi Azure Portal'da Gözat > seçin **durdurmak** ve işi durdurmak bekleyin.    
6. Kimlik bilgileri döndürmek istediğiniz SQL veritabanı çıkış bulun. Parolayı güncelleştirmek ve değişiklikleri kaydedin.    
7. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak başarıyla geçti emin olun.    
8. İle devam [işinizi son durdurulma saati başlangıç](#start-your-job-from-the-last-stopped-time) bölümü.

### <a name="power-bi"></a>Power BI
1. Azure portalında oturum açın > Stream Analytics işiniz Gözat > seçin **durdurmak** ve işi durdurmak bekleyin.    
2. Kimlik bilgilerini yenilemek istediğiniz Power BI çıkışı bulun > tıklatın **yetkilendirmeyi yenileyin** (bir başarı iletisi görmeniz gerekir) > **kaydetmek** değişiklikleri.    
3. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak, emin olun, başarıyla geçti.    
4. İle devam [işinizi son durdurulma saati başlangıç](#start-your-job-from-the-last-stopped-time) bölümü.

## <a name="start-your-job-from-the-last-stopped-time"></a>Son durdurulma saati işinizi Başlat

1. İşin gidin **genel bakış** bölmesinde > seçin **Başlat** işini başlatmak için.    
2. Seçin **son durduğunda** > tıklatın **Başlat**. Daha önce işi çalıştırılmış ve bazı çıktı vardı "son durduğunda" seçeneği yalnızca görünür açtıysanız oluşturulur. İş tabanlı son çıktı değerinin saat yeniden başlatılır.
   ![İşi Başlat](media/stream-analytics-login-credentials-inputs-outputs/image5.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
