---
title: IBM Informix veritabanına - Azure Logic Apps | Microsoft Docs
description: IBM Informix REST API'leri ve Azure Logic Apps ile kaynakları yönetme
author: gplarsen
manager: jeconnoc
ms.author: plarsen
ms.date: 09/26/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: d0008c19ed96f731f7b57c5d8aa41cd9f128bc20
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296045"
---
# <a name="get-started-with-the-informix-connector"></a>Informix Bağlayıcısı ile çalışmaya başlama
Informix için Microsoft Bağlayıcısı Logic Apps bir IBM Informix veritabanında depolanan kaynakları bağlanır. Informix bağlayıcı uzak Informix sunucu bilgisayarlarına bir TCP/IP ağı üzerinden iletişim kurmak için Microsoft client içerir. Bu Windows Azure sanallaştırma çalıştıran için IBM Informix gibi bulut veritabanlarını içerir ve şirket içi veri ağ geçidi kullanarak veritabanlarını. Bkz: [liste desteklenen](connectors-create-api-informix.md#supported-informix-platforms-and-versions) IBM Informix platformları ve sürümleri (Bu konudaki).

Bağlayıcı aşağıdaki veritabanı işlemleri destekler:

* Liste veritabanı tabloları
* SELECT kullanarak bir satır okuma
* Tüm satırları seçin kullanarak okuyun
* INSERT kullanarak bir satır Ekle
* Alter bir satır güncelleştirme kullanma
* DELETE kullanma bir satırı Kaldır

Bu konuda işlem veritabanı işlemleri için bir mantıksal uygulama içinde Bağlayıcısı'nı kullanmayı gösterir.

Logic Apps hakkında daha fazla bilgi için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="available-actions"></a>Kullanılabilir eylemler
Bu bağlayıcı aşağıdaki mantıksal uygulama eylemleri destekler:

* Getables
* GetRow
* GetRows
* Insertrow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Tabloları listele
Microsoft Azure portalı üzerinden gerçekleştirilen birçok adımlar herhangi bir işlem için bir mantıksal uygulama oluşturma oluşur.

Mantıksal uygulama içinde eylem Informix veritabanına listesi tablolarda ekleyebilirsiniz. Bu eylem bir Informix schema deyiminin gibi işlemek için bağlayıcı bildirir `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixgetTables`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**.  
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **Informix** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **Informix - Get tabloları (Önizleme)**.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. İçinde **Informix - Get tablolar** yapılandırma bölmesinde, **onay kutusunu** etkinleştirmek için **Connect şirket içi veri ağ geçidi üzerinden**. Ayarları buluttan şirket içi değiştirmek dikkat edin.
   
   * Tür için değer **Server**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `ibmserver01:9089`.
   * Tür için değer **veritabanı**. Örneğin, `nwind`.
   * İçin değer seçin **kimlik doğrulama**. Örneğin, seçin **temel**.
   * Tür için değer **kullanıcıadı**. Örneğin, `informix`.
   * Tür için değer **parola**. Örneğin, `Password1`.
   * İçin değer seçin **ağ geçidi**. Örneğin, seçin **datagateway01**.
7. Seçin **oluşturma**ve ardından **kaydetmek**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. İçinde **InformixgetTables** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
9. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_tables**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; tabloların bir listesini içermelidir.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Bağlantıları oluşturma
Bu bağlayıcı için veritabanı şirket içi ve aşağıdaki bağlantı özelliklerini kullanarak bulutta bağlantılarını destekler. 

| Özellik | Açıklama |
| --- | --- |
| sunucu |Gereklidir. TCP/IP adresi veya IPv4 veya IPv6 biçiminde, ardından (iki nokta üst üste TCP/IP bağlantı noktası numarası ile ayrılmış) diğer temsil eden bir dize değeri kabul eder. |
| veritabanı |Gereklidir. Bir DRDA ilişkisel veritabanı adı (RDBNAM) temsil eden bir dize değeri kabul eder. Informix 128 bayt dizesi kabul eder (veritabanı bir IBM Informix veritabanı adı (dbname) olarak bilinir). |
| kimlik doğrulaması |İsteğe bağlı. Bir liste öğesi değeri, temel veya Windows (kerberos) kabul eder. |
| kullanıcı adı |Gereklidir. Bir dize değeri kabul eder. |
| password |Gereklidir. Bir dize değeri kabul eder. |
| ağ geçidi |Gereklidir. Logic Apps için depolama grubu içinde tanımlanan şirket içi veri ağ geçidi temsil eden bir liste öğesi değeri kabul eder. |

## <a name="create-the-on-premises-gateway-connection"></a>Şirket içi ağ geçidi bağlantısı oluşturma
Bu bağlayıcı, şirket içi veri ağ geçidi kullanarak şirket içi Informix veritabanına erişebilir. Daha fazla bilgi için ağ geçidi konularına bakın. 

1. İçinde **ağ geçitleri** yapılandırma bölmesinde, **onay kutusunu** etkinleştirmek için **Connect ağ geçidi üzerinden**. Buluttan şirket içi değiştirme ayarları bölümüne bakın.
2. Tür için değer **Server**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `ibmserver01:9089`.
3. Tür için değer **veritabanı**. Örneğin, `nwind`.
4. İçin değer seçin **kimlik doğrulama**. Örneğin, seçin **temel**.
5. Tür için değer **kullanıcıadı**. Örneğin, `informix`.
6. Tür için değer **parola**. Örneğin, `Password1`.
7. İçin değer seçin **ağ geçidi**. Örneğin, seçin **datagateway01**.
8. Seçin **oluşturma** devam etmek için. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Bulut bağlantı oluşturma
Bu bağlayıcı bir bulut Informix veritabanına erişebilir. 

1. İçinde **ağ geçitleri** yapılandırma bölmesinde, bırakın **onay kutusunu** (Tıklatılmamış) devre dışı **Connect ağ geçidi üzerinden**. 
2. Tür için değer **bağlantı adı**. Örneğin, `hisdemo2`.
3. Tür için değer **Informix sunucu adı**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `hisdemo2.cloudapp.net:9089`.
4. Tür için değer **Informix veritabanı adı**. Örneğin, `nwind`.
5. Tür için değer **kullanıcıadı**. Örneğin, `informix`.
6. Tür için değer **parola**. Örneğin, `Password1`.
7. Seçin **oluşturma** devam etmek için. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Tüm satırları seçin kullanarak getirme
Informix tablodaki tüm satırları getirmek için bir mantıksal uygulama eylem oluşturabilirsiniz. Bu eylem bir Informix SELECT deyimi gibi işlemek için bağlayıcı bildirir `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı** (örn. "**InformixgetRows**"), **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **Informix** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **Informix - Get satırları (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**.
7. İçinde **bağlantıları** yapılandırma bölmesinde, **Yeni Oluştur**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. İçinde **ağ geçitleri** yapılandırma bölmesinde, bırakın **onay kutusunu** (Tıklatılmamış) devre dışı **Connect ağ geçidi üzerinden**.
   
   * Tür için değer **bağlantı adı**. Örneğin, `HISDEMO2`.
   * Tür için değer **Informix sunucu adı**, adres veya diğer iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin, `HISDEMO2.cloudapp.net:9089`.
   * Tür için değer **Informix veritabanı adı**. Örneğin, `NWIND`.
   * Tür için değer **kullanıcıadı**. Örneğin, `informix`.
   * Tür için değer **parola**. Örneğin, `Password1`.
9. Seçin **oluşturma** devam etmek için.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
11. İsteğe bağlı olarak, seçin **Gelişmiş Seçenekleri Göster** sorgu seçeneklerini belirtmek için.
12. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. İçinde **InformixgetRows** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
14. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; satır listesini içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>INSERT kullanarak bir satır Ekle
Bir Informix tablodaki bir satır eklemek için bir mantıksal uygulama eylem oluşturabilirsiniz. Bu eylem bir Informix INSERT deyimi gibi işlemek için bağlayıcı bildirir `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixinsertRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **Informix** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **Informix - Satır Ekle (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırma bölmesinde, bir bağlantı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**, türü `Area 99999`ve türü `102` için **REGİONID**. 
10. **Kaydet**’i seçin.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. İçinde **InformixinsertRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; yeni satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>SELECT kullanarak bir satır getirme
Informix tablosunda bir satırı getirmek için bir mantıksal uygulama eylem oluşturabilirsiniz. Bu eylem, bir Informix yeri seçin deyimi gibi işlemek için bağlayıcı bildirir `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixgetRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **Informix** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **Informix - Get satırları (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, varolan bir bağlantıyı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**. 
10. İsteğe bağlı olarak, seçin **Gelişmiş Seçenekleri Göster** sorgu seçeneklerini belirtmek için.
11. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. İçinde **InformixgetRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
13. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Bir satır güncelleştirme kullanarak değiştirme
Informix tablosunda bir satırı değiştirmek için bir mantıksal uygulama eylem oluşturabilirsiniz. Bu eylem, bir Informix güncelleştirme deyimi gibi işlemek için bağlayıcı bildirir `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixupdateRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **Informix** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **Informix - güncelleştirme satır (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, varolan bir bağlantıyı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**, türü `Updated 99999`ve türü `102` için **REGİONID**. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. İçinde **InformixupdateRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; yeni satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>DELETE kullanma bir satırı Kaldır
Bir Informix tablosunda bir satırı kaldırmak için bir mantıksal uygulama eylem oluşturabilirsiniz. Bu eylem bir Informix DELETE deyimi gibi işlemek için bağlayıcı bildirir `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı işareti) **Web + mobil**ve ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixdeleteRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **oluşturma**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve Eylem Ekle
1. İçinde **Logic Apps Tasarımcısı**seçin **boş LogicApp** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinde **yineleme**. 
3. İçinde **yineleme** tetikleyici, select **Düzenle**seçin **sıklığı** seçmek için açılır **gün**ve ardından **aralığı** yazmak için **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **Eylemler** listesinde, yazın **Informix** içinde **diğer eylemler için arama** düzenleme kutusuna ve ardından **Informix - Satır Sil (Önizleme)**.
6. İçinde **alma satırları (Önizleme)** eylem, select **değiştirmek bağlantı**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, varolan bir bağlantı seçin. Örneğin, seçin **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinde **aşağı ok**ve ardından **alanı**.
9. Gerekli tüm sütunlar (kırmızı yıldız bakın) için değerleri girin. Örneğin, `99999` için **AREAID**. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. İçinde **InformixdeleteRow** dikey penceresinde, içinde **tüm metinler** altında listesinde **Özet**, ilk listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **çalıştırmak mantıksal uygulama** dikey penceresinde, select **çalıştırma ayrıntıları**. İçinde **eylem** listesinde **Get_rows**. Değeri bkz **durum**, gereken **başarılı**. Seçin **girişleri bağlantı** girişleri görüntülemek için. Seçin **çıkışları bağlantı**ve çıkışları görüntüleyin; silinen satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Desteklenen Informix platformları ve sürümleri
Bu bağlayıcı, dağıtılmış ilişkisel veritabanı mimarisi (DRDA) istemci bağlantılarını desteklemek için yapılandırıldığında aşağıdaki IBM Informix sürümlerini destekler.

* IBM Informix 12.1
* IBM Informix 11.7

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/informix/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).

