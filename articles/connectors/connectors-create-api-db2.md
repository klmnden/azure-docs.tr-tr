---
title: IBM DB2 - Azure Logic Apps'i bağlama
description: IBM DB2 REST API'leri ve Azure Logic Apps ile kaynakları yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: plarsen, LADocs
ms.topic: article
ms.date: 08/23/2018
tags: connectors
ms.openlocfilehash: 7785d1788e8d5e9b432a8189345f293ebf05ef7c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58878409"
---
# <a name="manage-ibm-db2-resources-with-azure-logic-apps"></a>Azure Logic Apps ile IBM DB2 kaynaklarını yönetme

Azure Logic Apps ve IBM DB2 Bağlayıcısı ile otomatik görevler ve iş akışları, DB2 veritabanında depolanan kaynaklara göre oluşturabilirsiniz. İş akışlarınızı veritabanı kaynaklarına bağlanmak, okuma ve, veritabanı tablolarını listelemek, satır ekleme, satır, satır silme ve daha fazlasını değiştirin. Çıkış diğer eylemler için kullanılabilir hale getirmek ve veritabanınızdan yanıtları alma logic apps eylemleri dahil edebilirsiniz.

Bu makalede, çeşitli veritabanı işlemlerini gerçekleştiren bir mantıksal uygulamayı nasıl oluşturacağınızı gösterir. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md).

## <a name="supported-platforms-and-versions"></a>Desteklenen platformlara ve sürümlere

DB2 Bağlayıcısı DB2 uzak sunucularla iletişim kuran bir TCP/IP ağı üzerinden bir Microsoft client içerir. Bu bağlayıcı, IBM Bluemix dashDB veya Windows çalıştıran Azure sanallaştırma için IBM DB2 gibi bulut veritabanlarında erişmek için kullanabilirsiniz. Aynı zamanda şirket içi DB2 veritabanları sonra erişebilirsiniz [yükleyin ve şirket içi veri ağ geçidi ayarlama](../logic-apps/logic-apps-gateway-connection.md).

IBM DB2 Bağlayıcısı'nı bu IBM DB2 platformlara ve sürümlere dağıtılmış ilişkisel veritabanı mimarisi (DRDA) SQL Erişim Yöneticisi (SQLAM) sürüm 10 ve 11 desteği IBM DB2 uyumlu ürünler, IBM Bluemix dashDB gibi birlikte destekler:

| Platform | Sürüm | 
|----------|---------|
| IBM DB2 z/OS için | 11.1, 10.1 |
| IBM DB2 için ediyorum | 7.3, 7.2, 7.1 |
| IBM DB2 LUW için | 11, 10.5 |
|||

## <a name="supported-database-operations"></a>Desteklenen bir veritabanı işlemleri

IBM DB2 Bağlayıcısı bağlayıcı ilgili eylemler için harita veritabanı işlemlerini destekler:

| Veritabanı işlemi | Bağlayıcı eylemi |
|--------------------|------------------|
| Liste veritabanı tabloları | Tabloları al |
| SELECT kullanarak bir satırı okuyun | Satırı al |
| Tüm Satırları Seç kullanarak okuma | Satırları al |
| INSERT kullanarak bir satır Ekle | Satır ekle |
| Güncelleştirme kullanarak bir satırı düzenleyin | Satırı güncelleştir |
| DELETE kullanma bir satırı Kaldır | Satırı sil |
|||

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Bir IBM DB2 veritabanı, ya da bulut tabanlı veya şirket içi

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* DB2 veritabanınıza erişim sağlamak istediğiniz mantıksal uygulaması. Bu bağlayıcı, bu nedenle, mantıksal uygulamanızı başlatmak için yalnızca eylem seçebilir ayrı bir tetikleyici örneğin sağlar, **yinelenme** tetikleyici.
Bu örneklerde makale kullanım **yinelenme** tetikleyici.

<a name="add-db2-action"></a>

## <a name="add-db2-action---get-tables"></a>DB2 eylem - Get tabloları Ekle

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Tetikleyici altında seçin **yeni adım**.

1. Arama kutusuna filtreniz olarak "db2" girin. Bu örnekte, eylemler listesi altında şu eylemi seçin: **Tablolar'ı (Önizleme)**

   ![Eylem seçin](./media/connectors-create-api-db2/select-db2-action.png)

   Şimdi, DB2 veritabanı bağlantı ayrıntıları sağlayın istenir.

1. İçin bağlantılar oluşturmaya yönelik adımları [bulut veritabanlarını](#cloud-connection) veya [şirket içi veritabanlarını](#on-premises-connection).

<a name="cloud-connection"></a>

## <a name="connect-to-cloud-db2"></a>DB2 buluta bağlayın

Bağlantınızı kurmak için istendiğinde bu bağlantı ayrıntılarını sağlayın, **Oluştur**ve ardından mantıksal uygulamanızı kaydedin:

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **Şirket içi ağ geçidi üzerinden Bağlan** | Hayır | Yalnızca şirket içi bağlantıları için geçerlidir. |
| **Bağlantı Adı** | Evet | Örneğin, "MyLogicApp-DB2-connection" bağlantınız için bir ad |
| **Sunucu** | Evet | DB2 sunucunuz, örneğin, "myDB2server.cloudapp.net:50000" için adres veya diğer adı iki nokta üst üste bağlantı noktası numarası <p><p>**Not**: Bu değer bir TCP/IP adresi temsil eden bir dize ya da IPv4 veya IPv6 biçiminde ya da diğer adını ardından bir iki nokta üst üste ve bir TCP/IP bağlantı noktası numarası. |
| **Veritabanı** | Evet | Veritabanı adı <p><p>**Not**: Bir DRDA ilişkisel veritabanı adı (RDBNAM) temsil eden bir dize değeridir: <p>-DB2 z/OS için burada veritabanı bir "Z/OS için IBM DB2" konumu olarak bilinen bir 16 bayt dizesini kabul eder. <br>-DB2 i burada veritabanı olarak bilinen bir 18 bayt dizesini kabul eder için bir "için IBM DB2 miyim" ilişkisel veritabanı. <br>-LUW DB2 8 baytlık dizisi kabul eder. |
| **Kullanıcı Adı** | Evet | Veritabanı kullanıcı adı <p><p>**Not**: Bu değer, uzunluğu belirli bir veritabanını temel alan bir dizedir: <p><p>-Z/OS için DB2 8 baytlık dizisi kabul eder. <br>-DB2 için i bir 10 bayt dizesini kabul eder. <br>-Linux veya UNIX için DB2 8 baytlık dizisi kabul eder. <br>-Windows için DB2 30-bayt dizesini kabul eder. |
| **Parola** | Evet | Veritabanı parolası |
||||

Örneğin:

![Bulut tabanlı veritabanı bağlantı ayrıntıları](./media/connectors-create-api-db2/create-db2-cloud-connection.png)

<a name="on-premises-connection"></a>

## <a name="connect-to-on-premises-db2"></a>Şirket içi DB2'ye bağlanın

Bağlantınızı oluşturmadan önce zaten yüklü, şirket içi veri ağ geçidi olması gerekir. Aksi takdirde, bağlantınızı ayarı tamamlayamıyor. Ağ geçidi yüklemeniz varsa, bu bağlantı ayrıntılarını sağlamaya devam edin ve ardından **Oluştur**.

| Özellik | Gerekli | Açıklama |
|----------|----------|-------------|
| **Şirket içi ağ geçidi üzerinden Bağlan** | Evet | Bir şirket içi bağlantı istediğinizde uygular ve şirket içi bağlantı özelliklerini gösterir. |
| **Bağlantı Adı** | Evet | Örneğin, "MyLogicApp-DB2-connection" bağlantınız için bir ad | 
| **Sunucu** | Evet | DB2 sunucunuz, örneğin, "myDB2server:50000" için adres veya diğer adı iki nokta üst üste bağlantı noktası numarası <p><p>**Not**: Bu değer bir TCP/IP adresi temsil eden bir dize ya da IPv4 veya IPv6 biçiminde ya da diğer adını ardından bir iki nokta üst üste ve bir TCP/IP bağlantı noktası numarası. |
| **Veritabanı** | Evet | Veritabanı adı <p><p>**Not**: Bir DRDA ilişkisel veritabanı adı (RDBNAM) temsil eden bir dize değeridir: <p>-DB2 z/OS için burada veritabanı bir "Z/OS için IBM DB2" konumu olarak bilinen bir 16 bayt dizesini kabul eder. <br>-DB2 i burada veritabanı olarak bilinen bir 18 bayt dizesini kabul eder için bir "için IBM DB2 miyim" ilişkisel veritabanı. <br>-LUW DB2 8 baytlık dizisi kabul eder. |
| **Kimlik doğrulaması** | Evet | Bağlantınız için örneğin, "Temel" kimlik doğrulaması türü <p><p>**Not**: Bu değer, temel veya Windows (Kerberos) içeren listeden seçin. |
| **Kullanıcı Adı** | Evet | Veritabanı kullanıcı adı <p><p>**Not**: Bu değer, uzunluğu belirli bir veritabanını temel alan bir dizedir: <p><p>-Z/OS için DB2 8 baytlık dizisi kabul eder. <br>-DB2 için i bir 10 bayt dizesini kabul eder. <br>-Linux veya UNIX için DB2 8 baytlık dizisi kabul eder. <br>-Windows için DB2 30-bayt dizesini kabul eder. |
| **Parola** | Evet | Veritabanı parolası |
| **Ağ geçidi** | Evet | Yüklü şirket içi veri ağ geçidi adı <p><p>**Not**: Listeden Azure aboneliğinizi ve kaynak grubu içindeki tüm yüklenen veri ağ geçitlerini içerir. Bu değeri seçin. |
||||

Örneğin:

![Şirket içi veritabanları için bağlantı ayrıntıları](./media/connectors-create-api-db2/create-db2-on-premises-connection.png)

### <a name="view-output-tables"></a>Görünüm çıkış tabloları

Mantıksal Uygulama Tasarımcısı araç çubuğunda el ile çalıştırmayı tercih **çalıştırma**. Mantıksal uygulamanızı çalışmayı tamamladıktan sonra çalışma çıktısını görüntüleyebilirsiniz.

1. Mantıksal uygulama menüsünde seçin **genel bakış**.

1. Altında **özeti**, **çalıştırma geçmişi** bölümünde, listedeki ilk öğe en son çalıştırmayı seçin.

   ![Çalıştırma geçmişini görüntüleme](./media/connectors-create-api-db2/run-history.png)

1. Altında **mantıksal uygulama çalıştırması**, durum girdileri, şimdi gözden geçirebilirsiniz ve çıktılar her mantıksal uygulamanızda adım.
Genişletin **tabloları Al** eylem.

   ![Eylem genişletin](./media/connectors-create-api-db2/expand-action-step.png)

1. Girişleri görüntülemek için seçin **ham girişleri görüntüleyin**.

1. Çıktıları görüntülemek için seçin **ham çıkışları görüntüleyin**.

   Çıktıları tabloların bir listesini içerir.

   ![Görünüm çıkış tabloları](./media/connectors-create-api-db2/db2-connector-get-tables-outputs.png)

## <a name="get-row"></a>Satırı al

Bir DB2 veritabanı tablosundaki tek bir kayıtta getirilecek kullanın **Get satır** mantıksal uygulamanızda eylem. Bu eylem bir DB2 çalıştırır `SELECT WHERE` deyimi, örneğin, `SELECT FROM AREA WHERE AREAID = '99999'`.

1. Mantıksal uygulamanızda DB2 eylemleri önce hiç kullanmadıysanız adımları gözden [ekleme DB2 eylem - Get tabloları](#add-db2-action) bölümünde, ancak ekleme **Get satır** eylem bunun yerine ve devam etmek için buraya dönün.

   Siz ekledikten sonra **Get satır** eylem, örneği mantıksal uygulamanızı nasıl göründüğü aşağıda gösterilmiştir:

   ![Satır eylemini Al](./media/connectors-create-api-db2/db2-get-row-action.png)

1. Tüm gerekli özellikleri (*) için değerleri belirtin. Bir tablo seçin sonra eylemi bu tablodaki kayıtlara özel ilgili özellikleri gösterir.

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | Kayıt içeren tabloda, bu örnekte "alanı" gibi istediğiniz |
   | **Alan kimliği** | Evet | Kayıt kimliği, bu örnekte "99999" gibi istediğiniz |
   ||||

   ![Tablo seçin](./media/connectors-create-api-db2/db2-get-row-action-select-table.png)

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

### <a name="view-output-row"></a>Çıkış satır görünümü

Mantıksal Uygulama Tasarımcısı araç çubuğunda el ile çalıştırmayı tercih **çalıştırma**. Mantıksal uygulamanızı çalışmayı tamamladıktan sonra çalışma çıktısını görüntüleyebilirsiniz.

1. Mantıksal uygulama menüsünde seçin **genel bakış**.

1. Altında **özeti**, **çalıştırma geçmişi** bölümünde, listedeki ilk öğe en son çalıştırmayı seçin.

1. Altında **mantıksal uygulama çalıştırması**, durum girdileri, şimdi gözden geçirebilirsiniz ve çıktılar her mantıksal uygulamanızda adım.
Genişletin **Get satır** eylem.

1. Girişleri görüntülemek için seçin **ham girişleri görüntüleyin**.

1. Çıktıları görüntülemek için seçin **ham çıkışları görüntüleyin**.

   Çıktıları, belirtilen satır içerir.

   ![Çıkış satır görünümü](./media/connectors-create-api-db2/db2-connector-get-row-outputs.png)

## <a name="get-rows"></a>Satırları al

Bir DB2 veritabanı tablosundaki tüm kayıtlar getirilecek kullanın **satırları Al** mantıksal uygulamanızda eylem. Bu eylem bir DB2 çalıştırır `SELECT` deyimi, örneğin, `SELECT * FROM AREA`.

1. Mantıksal uygulamanızda DB2 eylemleri önce hiç kullanmadıysanız adımları gözden [ekleme DB2 eylem - Get tabloları](#add-db2-action) bölümünde, ancak ekleme **satırları Al** eylem bunun yerine ve devam etmek için buraya dönün.

   Siz ekledikten sonra **satırları Al** eylem, örneği mantıksal uygulamanızı nasıl göründüğü aşağıda gösterilmiştir:

   ![Satırları eylemini Al](./media/connectors-create-api-db2/db2-get-rows-action.png)

1. Açık **tablo adı** listelemek ve sonra bu örnekte "Alanı" olan istediğiniz tabloyu seçin:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-get-rows-action-select-table.png)

1. Bir filtre veya sorgu sonuçları için belirtmek için seçin **Gelişmiş Seçenekleri Göster**.

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

### <a name="view-output-rows"></a>Çıktı satırları görüntüleyin

Mantıksal Uygulama Tasarımcısı araç çubuğunda el ile çalıştırmayı tercih **çalıştırma**. Mantıksal uygulamanızı çalışmayı tamamladıktan sonra çalışma çıktısını görüntüleyebilirsiniz.

1. Mantıksal uygulama menüsünde seçin **genel bakış**.

1. Altında **özeti**, **çalıştırma geçmişi** bölümünde, listedeki ilk öğe en son çalıştırmayı seçin.

1. Altında **mantıksal uygulama çalıştırması**, durum girdileri, şimdi gözden geçirebilirsiniz ve çıktılar her mantıksal uygulamanızda adım.
Genişletin **satırları Al** eylem.

1. Girişleri görüntülemek için seçin **ham girişleri görüntüleyin**.

1. Çıktıları görüntülemek için seçin **ham çıkışları görüntüleyin**.

   Çıktı belirtilen tablonuzdaki tüm kayıtları içerir.

   ![Çıktı satırları görüntüleyin](./media/connectors-create-api-db2/db2-connector-get-rows-outputs.png)

## <a name="insert-row"></a>Satır ekle

DB2 veritabanından tek bir kayıt eklemek için **Satır Ekle** mantıksal uygulamanızda eylem. Bu eylem bir DB2 çalıştırır `INSERT` deyimi, örneğin, `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

1. Mantıksal uygulamanızda DB2 eylemleri önce hiç kullanmadıysanız adımları gözden [ekleme DB2 eylem - Get tabloları](#add-db2-action) bölümünde, ancak ekleme **Satır Ekle** eylem bunun yerine ve devam etmek için buraya dönün.

   Siz ekledikten sonra **Satır Ekle** eylem, örneği mantıksal uygulamanızı nasıl göründüğü aşağıda gösterilmiştir:

   ![Satır Eylemi ekleme](./media/connectors-create-api-db2/db2-insert-row-action.png)

1. Tüm gerekli özellikleri (*) için değerleri belirtin. Bir tablo seçin sonra eylemi bu tablodaki kayıtlara özel ilgili özellikleri gösterir.

   Bu örnekte, özellikleri şunlardır:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | Tablo where "Alanı" gibi bir kayıt eklemek |
   | **Alan kimliği** | Evet | Eklenecek "gibi 99999" alanı kimliği |
   | **Alan açıklaması** | Evet | Ekleme, "Area gibi 99999" alanı için açıklama |
   | **Bölge kodu** | Evet | Eklenecek "gibi 102" bölge kimliği |
   |||| 

   Örneğin:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-insert-row-action-select-table.png)

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

### <a name="view-insert-row-outputs"></a>Görünüm satır çıkışları Ekle

Mantıksal Uygulama Tasarımcısı araç çubuğunda el ile çalıştırmayı tercih **çalıştırma**. Mantıksal uygulamanızı çalışmayı tamamladıktan sonra çalışma çıktısını görüntüleyebilirsiniz.

1. Mantıksal uygulama menüsünde seçin **genel bakış**.

1. Altında **özeti**, **çalıştırma geçmişi** bölümünde, listedeki ilk öğe en son çalıştırmayı seçin.

1. Altında **mantıksal uygulama çalıştırması**, durum girdileri, şimdi gözden geçirebilirsiniz ve çıktılar her mantıksal uygulamanızda adım.
Genişletin **Satır Ekle** eylem.

1. Girişleri görüntülemek için seçin **ham girişleri görüntüleyin**.

1. Çıktıları görüntülemek için seçin **ham çıkışları görüntüleyin**.

   Çıktıları, belirtilen tabloya eklenen kaydın içerir.

   ![Eklenen satır görünümü çıkış](./media/connectors-create-api-db2/db2-connector-insert-row-outputs.png)

## <a name="update-row"></a>Satırı güncelleştir

Bir DB2 veritabanı tablosundaki tek bir kaydı güncelleştirmeye **satırı Güncelleştir** mantıksal uygulamanızda eylem. Bu eylem bir DB2 çalıştırır `UPDATE` deyimi, örneğin, `UPDATE AREA SET AREAID = '99999', AREADESC = 'Updated 99999', REGIONID = 102)`.

1. Mantıksal uygulamanızda DB2 eylemleri önce hiç kullanmadıysanız adımları gözden [ekleme DB2 eylem - Get tabloları](#add-db2-action) bölümünde, ancak ekleme **satırı Güncelleştir** eylem bunun yerine ve devam etmek için buraya dönün.

   Siz ekledikten sonra **satırı Güncelleştir** eylem, örneği mantıksal uygulamanızı nasıl göründüğü aşağıda gösterilmiştir:

   ![Satır Eylemi güncelleştir](./media/connectors-create-api-db2/db2-update-row-action.png)

1. Tüm gerekli özellikleri (*) için değerleri belirtin. Bir tablo seçin sonra eylemi bu tablodaki kayıtlara özel ilgili özellikleri gösterir.

   Bu örnekte, özellikleri şunlardır:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | Tablo where "Alanı" gibi bir kaydı güncelleştirmek |
   | **Satır kimliği** | Evet | Güncelleştirilecek "gibi 99999" kayıt kimliği |
   | **Alan kimliği** | Evet | "99999" gibi yeni alan kimliği |
   | **Alan açıklaması** | Evet | "Güncelleştirilmiş 99999" gibi yeni alan açıklaması |
   | **Bölge kodu** | Evet | "102" gibi yeni bölge kimliği |
   ||||

   Örneğin:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-update-row-action-select-table.png)

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

### <a name="view-update-row-outputs"></a>Görünümü güncelleştirme satırı çıkışı

Mantıksal Uygulama Tasarımcısı araç çubuğunda el ile çalıştırmayı tercih **çalıştırma**. Mantıksal uygulamanızı çalışmayı tamamladıktan sonra çalışma çıktısını görüntüleyebilirsiniz.

1. Mantıksal uygulama menüsünde seçin **genel bakış**.

1. Altında **özeti**, **çalıştırma geçmişi** bölümünde, listedeki ilk öğe en son çalıştırmayı seçin.

1. Altında **mantıksal uygulama çalıştırması**, durum girdileri, şimdi gözden geçirebilirsiniz ve çıktılar her mantıksal uygulamanızda adım.
Genişletin **satırı Güncelleştir** eylem.

1. Girişleri görüntülemek için seçin **ham girişleri görüntüleyin**.

1. Çıktıları görüntülemek için seçin **ham çıkışları görüntüleyin**.

   Çıktı belirtilen tablonuzda güncelleştirilen kaydı içerir.

   ![Güncelleştirilen satır görünümü çıkış](./media/connectors-create-api-db2/db2-connector-update-row-outputs.png)

## <a name="delete-row"></a>Satırı sil

DB2 veritabanı tablosundan tek bir kaydı silmek için kullanın **Satır Sil** mantıksal uygulamanızda eylem. Bu eylem bir DB2 çalıştırır `DELETE` deyimi, örneğin, `DELETE FROM AREA WHERE AREAID = '99999'`.

1. Mantıksal uygulamanızda DB2 eylemleri önce hiç kullanmadıysanız adımları gözden [ekleme DB2 eylem - Get tabloları](#add-db2-action) bölümünde, ancak ekleme **Satır Sil** eylem bunun yerine ve devam etmek için buraya dönün.

   Siz ekledikten sonra **Satır Sil** eylem, örneği mantıksal uygulamanızı nasıl göründüğü aşağıda gösterilmiştir:

   ![Satır Eylemi Sil](./media/connectors-create-api-db2/db2-delete-row-action.png)

1. Tüm gerekli özellikleri (*) için değerleri belirtin. Bir tablo seçin sonra eylemi bu tablodaki kayıtlara özel ilgili özellikleri gösterir.

   Bu örnekte, özellikleri şunlardır:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Tablo adı** | Evet | Tablo where "Alanı" gibi bir kaydı silmek |
   | **Satır kimliği** | Evet | Kayıt Kimliği "gibi 99999" silinemedi |
   ||||

   Örneğin:

   ![Tablo seçin](./media/connectors-create-api-db2/db2-delete-row-action-select-table.png)

1. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

### <a name="view-delete-row-outputs"></a>Satır çıkışları görünümünü Sil

Mantıksal Uygulama Tasarımcısı araç çubuğunda el ile çalıştırmayı tercih **çalıştırma**. Mantıksal uygulamanızı çalışmayı tamamladıktan sonra çalışma çıktısını görüntüleyebilirsiniz.

1. Mantıksal uygulama menüsünde seçin **genel bakış**.

1. Altında **özeti**, **çalıştırma geçmişi** bölümünde, listedeki ilk öğe en son çalıştırmayı seçin.

1. Altında **mantıksal uygulama çalıştırması**, durum girdileri, şimdi gözden geçirebilirsiniz ve çıktılar her mantıksal uygulamanızda adım.
Genişletin **Satır Sil** eylem.

1. Girişleri görüntülemek için seçin **ham girişleri görüntüleyin**.

1. Çıktıları görüntülemek için seçin **ham çıkışları görüntüleyin**.

   Çıktı artık, belirtilen tablosundan silinen kaydı içerir.

   ![Silinen satır olmadan görünümü çıkış](./media/connectors-create-api-db2/db2-connector-delete-row-outputs.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/db2/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
