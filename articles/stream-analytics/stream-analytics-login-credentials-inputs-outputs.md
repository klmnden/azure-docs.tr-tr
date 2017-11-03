---
title: "Akış analizi: oturum açma kimlik bilgilerini girişleri ve çıkışları döndürme | Microsoft Docs"
description: "Stream Analytics girişleri ve çıkışları için kimlik bilgilerini güncelleştirme konusunda bilgi edinin."
keywords: "oturum açma kimlik bilgileri"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: a1a927fa9c34b38e54fdb22782e80fd13bf430c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Giriş ve çıkış akış analizi işleri, oturum açma kimlik bilgileri döndürün
## <a name="abstract"></a>Özet
Azure Stream Analytics işi devam ederken bir giriş/çıkış kimlik bilgilerini değiştirme bugün izin vermez.

Azure Stream Analytics işi son çıktısından desteklerken, tüm işlem durdurma arasındaki gecikmesini en aza indirmenizi ve işini başlatma ve oturum açma kimlik bilgilerini döndürme paylaşmak istedi.

## <a name="part-1---prepare-the-new-set-of-credentials"></a>Bölüm 1 - yeni kimlik bilgileri kümesi hazırlama:
Bu bölümü aşağıdaki girdi/çıktı geçerlidir:

* Blob Depolama
* Event Hubs
* SQL Veritabanı
* Tablo Depolama

Diğer giriş/çıkış için bölüm 2 ile devam edin.

### <a name="blob-storagetable-storage"></a>BLOB Depolama/tablo depolama
1. Depolama uzantı Azure yönetim portalında gidin:  
   ![graphic1][graphic1]
2. İş tarafından kullanılan depolama alanını bulun ve uygulamasına gidin:  
   ![graphic2][graphic2]
3. Erişim anahtarlarını Yönet komutuna tıklayın:  
   ![graphic3][graphic3]
4. Birincil erişim anahtarı ve ikincil erişim tuşunu arasında **işinizi tarafından kullanılmayan bir çekme**.
5. İsabet yeniden:  
   ![graphic4][graphic4]
6. Yeni oluşturulan anahtarı kopyalayın:  
   ![graphic5][graphic5]
7. Bölüm 2 devam edin.

### <a name="event-hubs"></a>Olay hub'ları
1. Hizmet veri yolu uzantısı Azure yönetim portalında gidin:  
   ![graphic6][graphic6]
2. Hizmet veri yolu, iş tarafından kullanılan Namespace bulun ve uygulamasına gidin:  
   ![graphic7][graphic7]
3. İşinizi üzerinde hizmet veri yolu Namespace bir paylaşılan erişim ilkesi kullanıyorsa, adım 6 atlama  
4. Olay hub'ları sekmesine gidin:  
   ![graphic8][graphic8]
5. İş tarafından kullanılan olay hub'ı bulun ve uygulamasına gidin:  
   ![graphic9][graphic9]
6. Yapılandırma sekmesine gidin:  
   ![graphic10][graphic10]
7. İlke adı açılan üzerinde iş tarafından kullanılan paylaşılan erişim ilkesi'ni bulun:  
   ![graphic11][graphic11]
8. Birincil anahtar ve ikincil anahtar arasındaki **işinizi tarafından kullanılmayan bir çekme**.  
9. İsabet yeniden:  
   ![graphic12][graphic12]
10. Yeni oluşturulan anahtarı kopyalayın:  
   ![graphic13][graphic13]
11. Bölüm 2 devam edin.  

### <a name="sql-database"></a>SQL Veritabanı
> [!NOTE]
> Not: SQL veritabanı hizmetine bağlanmak gerekir. Biz yönetim deneyimi üzerinde Azure Yönetim Portalı'nı kullanarak bunu göstermek için kullanacaksanız, ancak SQL Server Management Studio gibi bazı istemci-tarafı aracı da kullanmayı seçebilirsiniz.
>
> 

1. SQL veritabanları uzantısı Azure yönetim portalında gidin:  
   ![graphic14][graphic14]
2. İş tarafından kullanılan SQL veritabanını bulun ve **sunucusunda** aynı satırda bağlantı:  
   ![graphic15][graphic15]
3. Yönet komutuna tıklayın:  
   ![graphic16][graphic16]
4. Veritabanı Yöneticisi türü:  
   ![graphic17][graphic17]
5. Kullanıcı adınızı, parolanızı yazın ve günlük tıklayın:  
   ![graphic18][graphic18]
6. Yeni sorgu tıklatın:  
   ![graphic19][graphic19]
7. Aşağıdaki sorguda < login_name >, kullanıcı adıyla değiştirerek ve değiştirme türü <enterStrongPasswordHere> yeni parolanızla:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Çalıştır'ı tıklatın:  
   ![graphic20][graphic20]
9. Adım 2 ve bu süre tıklatın veritabanı dönün:  
   ![graphic21][graphic21]
10. Yönet komutuna tıklayın:  
   ![graphic22][graphic22]
11. Kullanıcı adınızı, parolanızı girin ve oturum açma'ı tıklatın:  
   ![graphic23][graphic23]
12. Yeni sorgu tıklatın:  
   ![graphic24][graphic24]
13. Bu oturum açma (örneğin verdiğiniz < login_name > için aynı değer sağlayabilir) Bu veritabanı bağlamında tanımlamak istediğiniz bir adla < Kullanıcı_adı > değiştiriliyor ve değiştirerek aşağıdaki sorguyla < login_name > Yeni yazın Kullanıcı adı:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Çalıştır'ı tıklatın:  
   ![graphic25][graphic25]
15. Şimdi yeni kullanıcı aynı rolleri ve ayrıcalıkları, özgün kullanıcıya sahip sağlaması gerekir.
16. Bölüm 2 devam edin.

## <a name="part-2-stopping-the-stream-analytics-job"></a>2. Kısım: akış analizi işi durdurma
1. Stream Analytics uzantısı Azure yönetim portalında gidin:  
   ![graphic26][graphic26]
2. İşinizi bulun ve uygulamasına gidin:  
   ![graphic27][graphic27]
3. Girişleri sekmesini veya olup bir giriş veya çıkış kimlik bilgileri döndürme dayalı çıkışları gidin.  
   ![graphic28][graphic28]
4. Stop komutu tıklayın ve iş durduruldu onaylayın:  
   ![graphic29][graphic29] işi durdurmak bekleyin.
5. Kimlik bilgileri üzerindeki döndürün ve içine gitmek için istediğiniz giriş/çıkış bulun:  
   ![graphic30][graphic30]
6. Bölüm 3 devam edin.

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>3. Kısım: akış analizi işi kimlik bilgilerine düzenleme
### <a name="blob-storagetable-storage"></a>BLOB Depolama/tablo depolama
1. Depolama hesabı anahtarı alanını bulun ve yeni oluşturulan anahtarınızı yapıştırın:  
   ![graphic31][graphic31]
2. Kaydet komutuna tıklayın ve Değişikliklerinizi kaydetmeden onaylayın:  
   ![graphic32][graphic32]
3. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatacak olan emin olun başarıyla geçti.
4. 4 bölümüne geçin.

### <a name="event-hubs"></a>Olay hub'ları
1. Olay hub'ı İlkesi anahtarı alanını bulun ve yeni oluşturulan anahtarınızı yapıştırın:  
   ![graphic33][graphic33]
2. Kaydet komutuna tıklayın ve Değişikliklerinizi kaydetmeden onaylayın:  
   ![graphic34][graphic34]
3. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak başarıyla geçti emin olun.
4. 4 bölümüne geçin.

### <a name="power-bi"></a>Power BI
1. Yenileme yetkilendirme tıklatın:  

   ![graphic35][graphic35]
2. Aşağıdaki onay iletisini alırsınız:  

   ![graphic36][graphic36]
3. Kaydet komutuna tıklayın ve Değişikliklerinizi kaydetmeden onaylayın:  
   ![graphic37][graphic37]
4. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak, emin olun, başarıyla geçti.
5. 4 bölümüne geçin.

### <a name="sql-database"></a>SQL Veritabanı
1. Kullanıcı adı ve parola alanlarına bulmak ve yeni oluşturulan kimlik bilgileri kümesini kopyalayıp yapıştırın:  
   ![graphic38][graphic38]
2. Kaydet komutuna tıklayın ve Değişikliklerinizi kaydetmeden onaylayın:  
   ![graphic39][graphic39]
3. Yaptığınız değişiklikleri kaydederken bir bağlantı testi otomatik olarak başlatılacak başarıyla geçti emin olun.  
4. 4 bölümüne geçin.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>4. Kısım: işinizi son durdurulma saatten başlayarak
1. Giriş/Çıkış uzağa doğru gidin:  
   ![graphic40][graphic40]
2. Başlat komutu tıklatın:  
   ![graphic41][graphic41]
3. Son durdurulma zamanı seçin ve Tamam'ı tıklatın:  
   ![graphic42][graphic42]
4. 5 bölümüne geçin.  

## <a name="part-5-removing-the-old-set-of-credentials"></a>5. Kısım: eski kimlik bilgileri kümesi kaldırılıyor
Bu bölümü aşağıdaki girdi/çıktı geçerlidir:

* Blob Depolama
* Event Hubs
* SQL Veritabanı
* Tablo Depolama

### <a name="blob-storagetable-storage"></a>BLOB Depolama/tablo depolama
1. Bölüm artık kullanılmayan erişim anahtarını yenilemek için iş tarafından daha önce kullanılmış erişim anahtarı için yineleyin.

### <a name="event-hubs"></a>Olay hub'ları
1. Bölüm artık kullanılmayan anahtarını yenilemek için iş tarafından daha önce kullanılmış anahtarı için yineleyin.

### <a name="sql-database"></a>SQL Veritabanı
1. Bölümü 1 adım 7 ve daha önce iş tarafından kullanılan kullanıcı adı < previous_login_name > yerine türü aşağıdaki sorguda, sorgu penceresine geri dönün:  
   `DROP LOGIN <previous_login_name>`  
2. Çalıştır'ı tıklatın:  
   ![graphic43][graphic43]  

Aşağıdaki onay almanız gerekir: 

    Command(s) completed successfully.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

