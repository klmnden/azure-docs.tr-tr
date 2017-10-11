---
title: "Logic Apps içinde DB2 Bağlayıcısı'nı ekleme | Microsoft Docs"
description: "DB2 Bağlayıcısı REST API parametrelerle genel bakış"
services: 
documentationcenter: 
author: gplarsen
manager: erikre
editor: 
tags: connectors
ms.assetid: 1c6b010c-beee-496d-943a-a99e168c99aa
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: 4501b3d9a2fdc00582596cb907f7130591e4782e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-db2-connector"></a>DB2 Bağlayıcısı ile çalışmaya başlama
DB2 için Microsoft Bağlayıcısı Logic Apps bir IBM DB2 veritabanında depolanan kaynakları bağlanır. Bu bağlayıcı bir TCP/IP ağı üzerinden uzak DB2 sunucu bilgisayarlarla iletişim kurmak için Microsoft client içerir. Bu bulut veritabanları, IBM Bluemix dashDB veya Azure sanallaştırma çalışan Windows için IBM DB2 gibi içerir ve şirket içi veri ağ geçidi kullanarak veritabanlarını. Bkz: [liste desteklenen](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2 platformları ve sürümleri (Bu konudaki).

DB2 Bağlayıcısı'nı aşağıdaki veritabanı işlemleri destekler:

* Liste veritabanı tabloları
* SELECT kullanarak bir satır okuma
* Tüm satırları seçin kullanarak okuyun
* INSERT kullanarak bir satır Ekle
* Alter bir satır güncelleştirme kullanma
* DELETE kullanma bir satırı Kaldır

Bu konuda işlem veritabanı işlemleri için bir mantıksal uygulama içinde Bağlayıcısı'nı kullanmayı gösterir.

Logic Apps hakkında daha fazla bilgi için bkz: [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Kullanılabilir eylemler
DB2 Bağlayıcısı'nı aşağıdaki mantıksal uygulama eylemleri destekler:

* GetTables
* GetRow
* GetRows
* Insertrow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Listede tablolar
Microsoft Azure portalı üzerinden gerçekleştirilen birçok adımlar herhangi bir işlem için bir mantıksal uygulama oluşturma oluşur.

Mantıksal uygulama içinde bir DB2 veritabanında listede tablolar için bir eylem ekleyebilirsiniz. Bir DB2 schema deyiminin gibi işlemek için bağlayıcı eylemi bildirir `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin  **+**  (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `Db2getTables`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**.  
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın `db2` içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **DB2 - Get tabloları (Önizleme)**.
   
   ![](./media/connectors-create-api-db2/Db2connectorActions.png)  
6. İçinde **DB2 - Get tablolar** yapılandırma bölmesinde, **onay kutusunu** etkinleştirmek için **Connect şirket içi veri ağ geçidi üzerinden**. Ayarları buluttan şirket içi değiştirmek dikkat edin.
   
   * Tür için değer **Server**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `ibmserver01:50000`.
   * Tür için değer **veritabanı**. Örneğin, `nwind`.
   * İçin değer seçin **kimlik doğrulama**. Örneğin, seçin **temel**.
   * Tür için değer **kullanıcıadı**. Örneğin, `db2admin`.
   * Tür için değer **parola**. Örneğin, `Password1`.
   * İçin değer seçin **ağ geçidi**. Örneğin, seçin **datagateway01**.
7. Seçin **oluşturma**ve ardından **kaydetmek**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)
8. İçinde **Db2getTables** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
9. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_tables**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; tabloların bir listesini içermelidir.
   
   ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Bağlantıları oluşturma
Bu bağlayıcı barındırılan veritabanları şirket içi ve aşağıdaki bağlantı özelliklerini kullanarak bulutta bağlantılarını destekler. 

| Özellik | Açıklama |
| --- | --- |
| sunucu |Gerekli. TCP/IP adresi veya izlenen IPv4 veya IPv6 biçiminde (virgülle ayrılmış) diğer bir TCP/IP bağlantı noktası numarası ile temsil eden bir dize değeri kabul eder. |
| Veritabanı |Gerekli. Bir DRDA ilişkisel veritabanı adı (RDBNAM) temsil eden bir dize değeri kabul eder. DB2 z/OS için 16 bayt dizesini kabul eder (bir IBM DB2 z/OS konumu olarak veritabanı bilinir). DB2 i5/OS için kabul 18-bayt dizisi (veritabanı için bir IBM DB2 olarak bilinir i ilişkisel veritabanı). DB2 LUW için 8-bayt dizisi kabul eder. |
| Kimlik doğrulaması |İsteğe bağlı. Bir liste öğesi değeri, temel veya Windows (kerberos) kabul eder. |
| kullanıcı adı |Gerekli. Bir dize değeri kabul eder. DB2 z/OS için 8-bayt dizisi kabul eder. DB2 için i 10 bayt dizesini kabul eder. DB2 Linux veya UNIX için 8-bayt dizisi kabul eder. DB2 için Windows 30-bayt dizesini kabul eder. |
| password |Gerekli. Bir dize değeri kabul eder. |
| Ağ geçidi |Gerekli. Logic Apps için depolama grubu içinde tanımlanan şirket içi veri ağ geçidi temsil eden bir liste öğesi değeri kabul eder. |

## <a name="create-the-on-premises-gateway-connection"></a>Şirket içi ağ geçidi bağlantısı oluşturma
Bu bağlayıcıyı şirket içi ağ geçidi'ni kullanarak bir şirket içi DB2 veritabanına erişebilir. Daha fazla bilgi için ağ geçidi konularına bakın. 

1. İçinde **ağ geçitleri** yapılandırma bölmesinde, **onay kutusunu** etkinleştirmek için **Connect ağ geçidi üzerinden**. Ayarları buluttan şirket içi değiştirmek dikkat edin.
2. Tür için değer **Server**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `ibmserver01:50000`.
3. Tür için değer **veritabanı**. Örneğin, `nwind`.
4. İçin değer seçin **kimlik doğrulama**. Örneğin, seçin **temel**.
5. Tür için değer **kullanıcıadı**. Örneğin, `db2admin`.
6. Tür için değer **parola**. Örneğin, `Password1`.
7. İçin değer seçin **ağ geçidi**. Örneğin, seçin **datagateway01**.
8. Seçin **oluşturma** devam etmek için. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Bulut bağlantı oluşturma
Bu bağlayıcı bir bulut DB2 veritabanına erişebilir. 

1. İçinde **ağ geçitleri** yapılandırma bölmesinde, bırakın **onay kutusunu** (Tıklatılmamış) devre dışı **Connect ağ geçidi üzerinden**. 
2. Tür için değer **bağlantı adı**. Örneğin, `hisdemo2`.
3. Tür için değer **DB2 sunucu adı**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `hisdemo2.cloudapp.net:50000`.
4. Tür için değer **DB2 veritabanı adı**. Örneğin, `nwind`.
5. Tür için değer **kullanıcıadı**. Örneğin, `db2admin`.
6. Tür için değer **parola**. Örneğin, `Password1`.
7. Seçin **oluşturma** devam etmek için. 
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Tüm satırları seçin kullanarak getirme
DB2 tablodaki tüm satırları getirmek için bir mantıksal uygulama eylemi tanımlayabilirsiniz. Bu bağlayıcıyı bir DB2 SELECT deyimi gibi işlem bildirir `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin  **+**  (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `Db2getRows`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın `db2` içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **DB2 - Get satırları (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**.
7. İçinde **bağlantıları** yapılandırma bölmesinde, **Yeni Oluştur**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
8. İçinde **ağ geçitleri** yapılandırma bölmesinde, bırakın **onay kutusunu** (Tıklatılmamış) devre dışı **Connect ağ geçidi üzerinden**.
   
   * Tür için değer **bağlantı adı**. Örneğin, `HISDEMO2`.
   * Tür için değer **DB2 sunucu adı**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `HISDEMO2.cloudapp.net:50000`.
   * Tür için değer **DB2 veritabanı adı**. Örneğin, `NWIND`.
   * Tür için değer **kullanıcıadı**. Örneğin, `db2admin`.
   * Tür için değer **parola**. Örneğin, `Password1`.
9. Seçin **oluşturma** devam etmek için.
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)
10. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
11. İsteğe bağlı olarak, seçin **Gelişmiş Seçenekleri Göster** sorgu seçeneklerini belirtmek için.
12. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)
13. İçinde **Db2getRows** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
14. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; satır listesini içermelidir.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>INSERT kullanarak bir satır Ekle
Bir DB2 tablosunda bir satır eklemek için bir mantıksal uygulama eylemi tanımlayabilirsiniz. Bu eylem bir DB2 INSERT deyimi gibi işlemek için bağlayıcı bildirir `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin  **+**  (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `Db2insertRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **db2** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **DB2 - Satır Ekle (Önizleme)**.
6. İçinde **DB2 - Satır Ekle (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırma bölmesinde, bir bağlantı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**, türü `Area 99999`ve türü `102` için **REGİONID**. 
10. **Kaydet**’i seçin.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
11. İçinde **Db2insertRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; yeni satır içermelidir.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>SELECT kullanarak bir satır getirme
DB2 tablosunda bir satırı getirmek için bir mantıksal uygulama eylemi tanımlayabilirsiniz. Bu eylem, bir DB2 yeri seçin deyimi gibi işlemek için bağlayıcı bildirir `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin  **+**  (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı** (örn. "**Db2getRow**"), **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi. 
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **db2** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **DB2 - Get satırları (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, varolan bir bağlantı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**. 
10. İsteğe bağlı olarak, seçin **Gelişmiş Seçenekleri Göster** sorgu seçeneklerini belirtmek için.
11. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)
12. İçinde **Db2getRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
13. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; satır içermelidir.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Bir satır güncelleştirme kullanarak değiştirme
DB2 tablosunda bir satırı değiştirmek için bir mantıksal uygulama eylemi tanımlayabilirsiniz. Bu eylem, bir DB2 güncelleştirme deyimi gibi işlemek için bağlayıcı bildirir `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin  **+**  (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `Db2updateRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **db2** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **DB2 - güncelleştirme satır (Önizleme)**.
6. İçinde **DB2 - güncelleştirme satır (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, varolan bir bağlantıyı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**, türü `Updated 99999`ve türü `102` için **REGİONID**. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)
11. İçinde **Db2updateRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; yeni satır içermelidir.
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>DELETE kullanma bir satırı Kaldır
Bir DB2 tablosunda bir satırı kaldırmak için bir mantıksal uygulama eylemi tanımlayabilirsiniz. Bu eylem bir DB2 DELETE deyimi gibi işlemek için bağlayıcı bildirir `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin  **+**  (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `Db2deleteRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi. 
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde **db2** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **DB2 - Satır Sil (Önizleme)**.
6. İçinde **DB2 - Satır Sil (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, varolan bir bağlantı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)
11. İçinde **Db2deleteRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; silinen satır içermelidir.
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="supported-db2-platforms-and-versions"></a>Desteklenen DB2 platformları ve sürümleri
Bu bağlayıcı aşağıdaki IBM DB2 platformları ve sürümleri, yanı sıra dağıtılmış ilişkisel veritabanı mimarisi (DRDA) SQL Erişim Yöneticisi (SQLAM) sürüm 10 ve 11 desteği IBM DB2 uyumlu ürünler (örneğin IBM Bluemix dashDB) destekler:

* IBM DB2 z/OS 11.1 için
* IBM DB2 z/OS 10.1 için
* IBM DB2 için i 7.3
* IBM DB2 için i 7.2
* IBM DB2 için i 7.1
* IBM DB2 LUW 11
* IBM DB2 LUW 10.5 için

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/db2/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).

