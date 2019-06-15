---
title: IBM Informix veritabanı - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: 6004c02f190bbfcf374b3b5d2a5c478f0e52c961
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60691010"
---
# <a name="get-started-with-the-informix-connector"></a>Informix Bağlayıcısı ile çalışmaya başlama
Informix için Microsoft Bağlayıcısı Logic Apps bir IBM Informix veritabanına depolanmış olan kaynaklara bağlar. Informix Bağlayıcısı uzak Informix sunucu bilgisayarları için bir TCP/IP ağı üzerinden iletişim kurmak için bir Microsoft client içerir. Bu Windows çalıştıran Azure sanallaştırma için IBM Informix gibi bulut veritabanlarını içerir ve şirket içi şirket içi veri ağ geçidini kullanarak veritabanlarını. Bkz: [liste desteklenen](connectors-create-api-informix.md#supported-informix-platforms-and-versions) IBM Informix platformlarını ve sürümlerini (Bu konudaki).

Bağlayıcı aşağıdaki veritabanı işlemleri destekler:

* Liste veritabanı tabloları
* SELECT kullanarak bir satırı okuyun
* Tüm Satırları Seç kullanarak okuma
* INSERT kullanarak bir satır Ekle
* Alter bir satır güncelleştirme kullanma
* DELETE kullanma bir satırı Kaldır

Bu konuda bir mantıksal uygulama işlemi veritabanı işlemleri için bağlayıcıyı kullanma gösterilmektedir.

Logic Apps hakkında daha fazla bilgi için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="available-actions"></a>Kullanılabilir eylemler
Bu bağlayıcı, aşağıdaki mantıksal uygulama eylemleri destekler:

* Getables
* GetRow
* Satır Al
* Insertrow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Listede tablolar
Herhangi bir işlem için bir mantıksal uygulama oluşturma, Microsoft Azure portal aracılığıyla gerçekleştirilen birçok adımlar oluşur.

Mantıksal uygulama içinde bir Informix veritabanındaki tablolar liste için bir eylem ekleyebilirsiniz. Bu eylem bir Informix schema deyiminin gibi işlemek için bağlayıcıyı bildirir `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixgetTables`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service Plan**. Seçin **panoya Sabitle**ve ardından **Oluştur**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve eylem ekleme
1. İçinde **Logic Apps Tasarımcısı'nda**seçin **boş mantıksal uygulama** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinden **yinelenme**. 
3. İçinde **yinelenme** tetikleyici seçme **Düzenle**seçin **sıklığı** seçmek için açılan **gün**ve ardından  **Aralığı** türüne **7**.  
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **eylemleri** listesinde, yazın **Informix** içinde **daha fazla eylem arayın** düzenleme kutusuna ve ardından **Informix - Get tabloları (Önizleme)** .
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. İçinde **Informix - Get tabloları** yapılandırma bölmesinde **onay kutusu** etkinleştirmek için **şirket içi veri ağ geçidi üzerinden Bağlan**. Ayarları buluttan şirket içine değiştirmek dikkat edin.
   
   * Tür için değer **sunucu**, adres veya diğer adı iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin `ibmserver01:9089`yazın.
   * Tür için değer **veritabanı**. Örneğin `nwind`yazın.
   * Değer seçin **kimlik doğrulaması**. Örneğin, **temel**.
   * Tür için değer **Username**. Örneğin `informix`yazın.
   * Tür için değer **parola**. Örneğin `Password1`yazın.
   * Değer seçin **ağ geçidi**. Örneğin, **datagateway01**.
7. Seçin **Oluştur**ve ardından **Kaydet**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. İçinde **InformixgetTables** dikey dahilinde **tüm çalıştırmalar** altında listesinde **özeti**, ilk olarak listelenen öğeyi (en son Çalıştır) seçin.
9. İçinde **mantıksal uygulama çalıştırması** dikey penceresinde **çalıştırma ayrıntıları**. İçinde **eylem** listesinden **Get_tables**. Değeri görmek **durumu**, gereken **başarılı**. Seçin **giriş bağlantısı** girişleri görüntülemek için. Seçin **çıkış bağlantısı**ve çıkışları görüntüleyin; Tablo listesini içermelidir.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Bağlantıları oluşturma
Bu bağlayıcı, aşağıdaki bağlantı özelliklerini kullanarak buluttaki ve şirket içi veritabanına bağlantıyı destekler. 

| Özellik | Açıklama |
| --- | --- |
| server |Gereklidir. TCP/IP adresi veya IPv4 veya IPv6 biçiminde, ardından (iki nokta üst üste bir TCP/IP bağlantı noktası numarası ile ayrılmış) diğer temsil eden bir dize değeri kabul eder. |
| database |Gereklidir. Bir DRDA ilişkisel veritabanı adı (RDBNAM) temsil eden bir dize değeri kabul eder. Informix 128 bayt dizesini kabul eder (veritabanı bir IBM Informix veritabanı adı (dbname) bilinir). |
| kimlik doğrulaması |İsteğe bağlı. Bir liste öğesi değeri, temel veya Windows (kerberos) kabul eder. |
| username |Gereklidir. Bir dize değeri kabul eder. |
| password |Gereklidir. Bir dize değeri kabul eder. |
| Ağ geçidi |Gereklidir. Logic Apps Depolama grubu içinde tanımlanan şirket içi veri ağ geçidini temsil eden bir liste öğesi değeri kabul eder. |

## <a name="create-the-on-premises-gateway-connection"></a>Şirket içi ağ geçidi bağlantısı oluşturma
Bu bağlayıcı, şirket içi veri ağ geçidini kullanarak şirket içi Informix veritabanına erişebilir. Ağ Geçidi daha fazla bilgi için bkz. 

1. İçinde **ağ geçitleri** yapılandırma bölmesinde **onay kutusu** etkinleştirmek için **ağ geçidi üzerinden Bağlan**. Buluttan şirket içine değiştirme ayarları bölümüne bakın.
2. Tür için değer **sunucu**, adres veya diğer adı iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin `ibmserver01:9089`yazın.
3. Tür için değer **veritabanı**. Örneğin `nwind`yazın.
4. Değer seçin **kimlik doğrulaması**. Örneğin, **temel**.
5. Tür için değer **Username**. Örneğin `informix`yazın.
6. Tür için değer **parola**. Örneğin `Password1`yazın.
7. Değer seçin **ağ geçidi**. Örneğin, **datagateway01**.
8. Seçin **Oluştur** devam etmek için. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Bulut bağlantı oluşturma
Bu bağlayıcı, Bulutu Informix veritabanına erişebilir. 

1. İçinde **ağ geçitleri** yapılandırma bölmesi bırakın **onay kutusu** (Tıklatılmamış) devre dışı **ağ geçidi üzerinden Bağlan**. 
2. Tür için değer **bağlantı adı**. Örneğin `hisdemo2`yazın.
3. Tür için değer **Informix sunucu adı**, adres veya diğer adı iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin `hisdemo2.cloudapp.net:9089`yazın.
4. Tür için değer **Informix veritabanı adı**. Örneğin `nwind`yazın.
5. Tür için değer **Username**. Örneğin `informix`yazın.
6. Tür için değer **parola**. Örneğin `Password1`yazın.
7. Seçin **Oluştur** devam etmek için. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Tüm Satırları Seç kullanarak getirir
Informix tablodaki tüm satırları getirmek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem bir Informix SELECT deyimi gibi işlemek için bağlayıcıyı bildirir `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı** (örn.) "**InformixgetRows**"), **abonelik**, **kaynak grubu**, **konumu**, ve **App Service planı**. Seçin **panoya Sabitle**ve ardından **Oluştur**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve eylem ekleme
1. İçinde **Logic Apps Tasarımcısı'nda**seçin **boş mantıksal uygulama** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinden **yinelenme**. 
3. İçinde **yinelenme** tetikleyici seçme **Düzenle**seçin **sıklığı** seçmek için açılan **gün**ve ardından  **Aralığı** türüne **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **eylemleri** listesinde, yazın **Informix** içinde **daha fazla eylem arayın** düzenleme kutusuna ve ardından **Informix - satırları Al (Önizleme)** .
6. İçinde **(Önizleme) satırları Al** seçme eylemini **Bağlantıyı Değiştir**.
7. İçinde **bağlantıları** yapılandırma bölmesinde **Yeni Oluştur**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. İçinde **ağ geçitleri** yapılandırma bölmesi bırakın **onay kutusu** (Tıklatılmamış) devre dışı **ağ geçidi üzerinden Bağlan**.
   
   * Tür için değer **bağlantı adı**. Örneğin `HISDEMO2`yazın.
   * Tür için değer **Informix sunucu adı**, adres veya diğer adı iki nokta üst üste bağlantı noktası numarası biçiminde. Örneğin `HISDEMO2.cloudapp.net:9089`yazın.
   * Tür için değer **Informix veritabanı adı**. Örneğin `NWIND`yazın.
   * Tür için değer **Username**. Örneğin `informix`yazın.
   * Tür için değer **parola**. Örneğin `Password1`yazın.
9. Seçin **Oluştur** devam etmek için.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. İçinde **tablo adı** listesinden **aşağı ok**ve ardından **alan**.
11. İsteğe bağlı olarak **Gelişmiş Seçenekleri Göster** sorgu seçeneklerini belirtmek için.
12. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. İçinde **InformixgetRows** dikey dahilinde **tüm çalıştırmalar** altında listesinde **özeti**, ilk olarak listelenen öğeyi (en son Çalıştır) seçin.
14. İçinde **mantıksal uygulama çalıştırması** dikey penceresinde **çalıştırma ayrıntıları**. İçinde **eylem** listesinden **Get_rows**. Değeri görmek **durumu**, gereken **başarılı**. Seçin **giriş bağlantısı** girişleri görüntülemek için. Seçin **çıkış bağlantısı**ve çıkışları görüntüleyin; satır listesini içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>INSERT kullanarak bir satır Ekle
Bir Informix tabloda bir satır eklemek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem bir Informix INSERT deyimi gibi işlemek için bağlayıcıyı bildirir `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixinsertRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service Plan**. Seçin **panoya Sabitle**ve ardından **Oluştur**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve eylem ekleme
1. İçinde **Logic Apps Tasarımcısı'nda**seçin **boş mantıksal uygulama** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinden **yinelenme**. 
3. İçinde **yinelenme** tetikleyici seçme **Düzenle**seçin **sıklığı** seçmek için açılan **gün**ve ardından  **Aralığı** türüne **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **eylemleri** listesinde, yazın **Informix** içinde **daha fazla eylem arayın** düzenleme kutusuna ve ardından **Informix - Satır Ekle (Önizleme)** .
6. İçinde **(Önizleme) satırları Al** seçme eylemini **Bağlantıyı Değiştir**. 
7. İçinde **bağlantıları** yapılandırma bölmesinde, bir bağlantıyı seçin. Örneğin, **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinden **aşağı ok**ve ardından **alan**.
9. Gerekli tüm sütunları (kırmızı yıldızlı bakın) için değerleri girin. Örneğin `99999` için **AREAID**, türü `Area 99999`ve türü `102` için **REGİONID**. 
10. **Kaydet**’i seçin.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. İçinde **InformixinsertRow** dikey dahilinde **tüm çalıştırmalar** altında listesinde **özeti**, ilk olarak listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **mantıksal uygulama çalıştırması** dikey penceresinde **çalıştırma ayrıntıları**. İçinde **eylem** listesinden **Get_rows**. Değeri görmek **durumu**, gereken **başarılı**. Seçin **giriş bağlantısı** girişleri görüntülemek için. Seçin **çıkış bağlantısı**ve çıkışları görüntüleyin; yeni satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>SELECT kullanarak bir satır getirilemedi
Informix tablosunda bir satıra getirmek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem bir Informix burada seçin deyimi gibi işlemek için bağlayıcıyı bildirir `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixgetRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service Plan**. Seçin **panoya Sabitle**ve ardından **Oluştur**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve eylem ekleme
1. İçinde **Logic Apps Tasarımcısı'nda**seçin **boş mantıksal uygulama** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinden **yinelenme**. 
3. İçinde **yinelenme** tetikleyici seçme **Düzenle**seçin **sıklığı** seçmek için açılan **gün**ve ardından  **Aralığı** türüne **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **eylemleri** listesinde, yazın **Informix** içinde **daha fazla eylem arayın** düzenleme kutusuna ve ardından **Informix - satırları Al (Önizleme)** .
6. İçinde **(Önizleme) satırları Al** seçme eylemini **Bağlantıyı Değiştir**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde mevcut bir bağlantıyı seçin. Örneğin, **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinden **aşağı ok**ve ardından **alan**.
9. Gerekli tüm sütunları (kırmızı yıldızlı bakın) için değerleri girin. Örneğin `99999` için **AREAID**. 
10. İsteğe bağlı olarak **Gelişmiş Seçenekleri Göster** sorgu seçeneklerini belirtmek için.
11. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. İçinde **InformixgetRow** dikey dahilinde **tüm çalıştırmalar** altında listesinde **özeti**, ilk olarak listelenen öğeyi (en son Çalıştır) seçin.
13. İçinde **mantıksal uygulama çalıştırması** dikey penceresinde **çalıştırma ayrıntıları**. İçinde **eylem** listesinden **Get_rows**. Değeri görmek **durumu**, gereken **başarılı**. Seçin **giriş bağlantısı** girişleri görüntülemek için. Seçin **çıkış bağlantısı**ve çıkışları görüntüleyin; satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Bir satır güncelleştirme kullanarak değiştirme
Informix tablosunda bir satıra değiştirmek için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem bir Informix güncelleştirme bildirimi gibi işlemek için bağlayıcıyı bildirir `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixupdateRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service Plan**. Seçin **panoya Sabitle**ve ardından **Oluştur**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve eylem ekleme
1. İçinde **Logic Apps Tasarımcısı'nda**seçin **boş mantıksal uygulama** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinden **yinelenme**. 
3. İçinde **yinelenme** tetikleyici seçme **Düzenle**seçin **sıklığı** seçmek için açılan **gün**ve ardından  **Aralığı** türüne **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **eylemleri** listesinde, yazın **Informix** içinde **daha fazla eylem arayın** düzenleme kutusuna ve ardından **Informix - satırı güncelleştir (Önizleme)** .
6. İçinde **(Önizleme) satırları Al** seçme eylemini **Bağlantıyı Değiştir**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde mevcut bir bağlantıyı seçin. Örneğin, **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinden **aşağı ok**ve ardından **alan**.
9. Gerekli tüm sütunları (kırmızı yıldızlı bakın) için değerleri girin. Örneğin `99999` için **AREAID**, türü `Updated 99999`ve türü `102` için **REGİONID**. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. İçinde **InformixupdateRow** dikey dahilinde **tüm çalıştırmalar** altında listesinde **özeti**, ilk olarak listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **mantıksal uygulama çalıştırması** dikey penceresinde **çalıştırma ayrıntıları**. İçinde **eylem** listesinden **Get_rows**. Değeri görmek **durumu**, gereken **başarılı**. Seçin **giriş bağlantısı** girişleri görüntülemek için. Seçin **çıkış bağlantısı**ve çıkışları görüntüleyin; yeni satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>DELETE kullanma bir satırı Kaldır
Bir Informix tablosunda bir satırı kaldırmak için bir mantıksal uygulama eylemi oluşturabilirsiniz. Bu eylem bir Informix DELETE deyimi gibi işlemek için bağlayıcıyı bildirir `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
1. İçinde **Azure başlangıç Panosu**seçin **+** (artı) **Web + mobil**, ardından **mantıksal uygulama**.
2. Girin **adı**, gibi `InformixdeleteRow`, **abonelik**, **kaynak grubu**, **konumu**, ve **App Service Plan**. Seçin **panoya Sabitle**ve ardından **Oluştur**.

### <a name="add-a-trigger-and-action"></a>Bir tetikleyici ve eylem ekleme
1. İçinde **Logic Apps Tasarımcısı'nda**seçin **boş mantıksal uygulama** içinde **şablonları** listesi.
2. İçinde **Tetikleyicileri** listesinden **yinelenme**. 
3. İçinde **yinelenme** tetikleyici seçme **Düzenle**seçin **sıklığı** seçmek için açılan **gün**ve ardından  **Aralığı** türüne **7**. 
4. Seçin **+ yeni adım** kutusuna ve ardından **Eylem Ekle**.
5. İçinde **eylemleri** listesinde, yazın **Informix** içinde **daha fazla eylem arayın** düzenleme kutusuna ve ardından **Informix - Satır Sil (Önizleme)** .
6. İçinde **(Önizleme) satırları Al** seçme eylemini **Bağlantıyı Değiştir**. 
7. İçinde **bağlantıları** yapılandırmaları bölmesinde, mevcut bir bağlantıyı seçin. Örneğin, **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. İçinde **tablo adı** listesinden **aşağı ok**ve ardından **alan**.
9. Gerekli tüm sütunları (kırmızı yıldızlı bakın) için değerleri girin. Örneğin `99999` için **AREAID**. 
10. **Kaydet**’i seçin. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. İçinde **InformixdeleteRow** dikey dahilinde **tüm çalıştırmalar** altında listesinde **özeti**, ilk olarak listelenen öğeyi (en son Çalıştır) seçin.
12. İçinde **mantıksal uygulama çalıştırması** dikey penceresinde **çalıştırma ayrıntıları**. İçinde **eylem** listesinden **Get_rows**. Değeri görmek **durumu**, gereken **başarılı**. Seçin **giriş bağlantısı** girişleri görüntülemek için. Seçin **çıkış bağlantısı**ve çıkışları görüntüleyin; silinen satır içermelidir.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Desteklenen Informix platformlara ve sürümlere
Bu bağlayıcı, dağıtılmış ilişkisel veritabanı mimarisi (DRDA) istemci bağlantılarını desteklemek için yapılandırıldığında IBM Informix aşağıdaki sürümleri destekler.

* IBM Informix 12.1
* IBM Informix 11.7

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/informix/). 

## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API listesi](apis-list.md).

