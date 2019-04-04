---
title: Azure Stream Analytics işi Azure Data Lake depolama Gen1 çıkışı için kimlik doğrulaması
description: Bu makalede, Azure Stream Analytics işinizi Azure Data Lake depolama Gen1 çıkış için kimlik doğrulaması için yönetilen kimlikleri kullanmayı açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 994ccf292a4215624d4222fe13ca9ac25c863368
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895875"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities"></a>Stream Analytics yönetilen kimlik kullanarak Azure Data Lake depolama Gen1 için kimlik doğrulaması

Azure Stream Analytics, Azure Data Lake Storage (ADLS) Gen1 çıktıyla yönetilen kimlik doğrulama destekler. Kimlik Azure Active Directory içinde belirli bir Stream Analytics işi temsil eden kayıtlı bir yönetilen uygulama ve hedeflenen kaynak kimliğini doğrulamak için kullanılabilir. Yönetilen kimlik parola değişikliklerini veya 90 günde gerçekleşen kullanıcı belirteci süre sonu nedeniyle yeniden kimlik doğrulamaya zorlayabilir gerek gibi kullanıcı tabanlı kimlik doğrulama yöntemlerini sınırlamaları ortadan kaldırır. Ayrıca, yönetilen kimlikleri, Azure Data Lake depolama Gen1 output Stream Analytics işi dağıtımlarının Otomasyonu ile yardımcı olur.

Bu makalede yönetilen kimlik veren bir Azure Data Lake depolama Gen1 Azure portalı, Azure Resource Manager şablon dağıtımı ve Azure Stream Analytics araçları için Visual Studio için Azure Stream Analytics işi için etkinleştirmek için üç yol gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-portal"></a>Azure portal

1. Yeni bir Stream Analytics işi oluşturma veya var olan bir işi, Azure portalında açarak başlatın. Ekranın sol tarafında bulunan menü çubuğundan seçin **yönetilen kimliği** altında bulunan **yapılandırma**.

   ![Stream Analytics yönetilen kimlik yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Seçin **yönetilen kimliği kullanma sistem tarafından atanan** penceresinden sağ tarafta görüntülenir. Tıklayın **Kaydet** kimliğini Azure Active Directory'de Stream Analytics işi için bir hizmet sorumlusu için. Yeni oluşturulan kimlik yaşam döngüsünü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (diğer bir deyişle, hizmet sorumlusu), Azure tarafından otomatik olarak silinir.

   Yapılandırma kaydedilirken, hizmet sorumlusu nesne kimliği (OID) asıl aşağıda gösterildiği gibi kimlik olarak listelenir:

   ![Stream Analytics Hizmet sorumlusu kimliği](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Hizmet sorumlusu, Stream Analytics işiyle aynı ada sahip. Örneğin, işinizin adı ise **MyASAJob**, oluşturulan hizmet sorumlusu adını da olduğu **MyASAJob**.

3. Kimlik doğrulama modu açılır ve select ADLS Gen1 çıkış havuzu, çıkış Özellikler penceresinde tıklayın ** yönetilen kimliği **.

4. Kalan özellikleri doldurun. ADLS çıktı oluşturma hakkında daha fazla bilgi edinmek için [stream analytics ile Data lake Store çıktı oluşturma](../data-lake-store/data-lake-store-stream-analytics.md). İşlemi tamamladığınızda, tıklayın **Kaydet**.

   ![Azure Data Lake depolama yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. ADLS Gen1 Genel Bakış sayfasına gidin ve tıklayarak **Veri Gezgini**.

   ![Data Lake depolamaya genel bakış yapılandırın](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Veri Gezgini bölmesinde seçin **erişim** tıklatıp **Ekle** erişim bölmesinde.

   ![Data Lake depolama erişimi yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Metin kutusuna **kullanıcı veya grup seçin** bölmesinde, hizmet sorumlusu adını yazın. Hizmet sorumlusu adını karşılık gelen Stream Analytics işinin adı olduğunu unutmayın. Asıl adını yazmaya başladığınızda, bu metin kutusunun altında görüntülenir. İstenen hizmet asıl adı'nı seçin ve tıklayın **seçin**.

   ![Bir hizmet asıl adı seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. İçinde **izinleri** bölmesinde, onay **yazma** ve **yürütme** izinleri atayabilirsiniz **bu klasör ve tüm alt öğeleri**. Ardından **Tamam**.

   ![Yazma ve Yürütme izinleri seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Hizmet sorumlusu altında listelenen **atanan izinler** üzerinde **erişim** aşağıda gösterildiği gibi bölmesi. Şimdi, geri dönün ve Stream Analytics işinizi başlatın.

   ![Stream Analytics erişim Portalı'nda listesi](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Data Lake depolama Gen1 dosya sistemi izinleri hakkında daha fazla bilgi için bkz: [Azure Data Lake depolama Gen1 erişim denetimi](../data-lake-store/data-lake-store-access-control.md).

## <a name="stream-analytics-tools-for-visual-studio"></a>Visual Studio için Stream Analytics araçları

1. JobConfig.json içinde ayarlamak **kullanım sistem tarafından atanan kimlik** için **True**.

   ![Stream Analytics işi yapılandırma yönetilen kimlikleri](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. Kimlik doğrulama modu açılır ve select ADLS Gen1 çıkış havuzu, çıkış Özellikler penceresinde tıklayın **yönetilen kimliği (Önizleme)**.

   ![ADLS yönetilen kimlik çıkışı](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Kalan özelliklerini doldurun ve tıklayın **Kaydet**.

4. Tıklayın **azure'a Gönder** sorgu Düzenleyicisi'nde.

   İşi gönderdiğinizde, araçları, iki işlem yapmanız gerekir:

   * Otomatik olarak bir hizmet sorumlusu kimliğinin Stream Analytics işi Azure Active Directory'de oluşturur. Yeni oluşturulan kimlik yaşam döngüsünü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (diğer bir deyişle, hizmet sorumlusu), Azure tarafından otomatik olarak silinir.

   * Otomatik olarak **yazma** ve **yürütme** ADLS Gen1 izinlerini önek işlemde kullanılan yolu ve bu klasör ve tüm alt öğeleri için atayın.

5. Resource Manager şablonlarını kullanarak aşağıdaki özellik ile oluşturabileceğiniz [Stream Analytics CI. CD Nuget paketini](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/) 1.5.0 sürümü veya üzeri bir yapı makinesinde (dışında Visual Studio). Resource Manager şablon dağıtımı, hizmet sorumlusu almak ve hizmet sorumlusu PowerShell erişim vermek için sonraki bölümde adımlarını izleyin.

## <a name="resource-manager-template-deployment"></a>Resource Manager şablon dağıtımı

1. Oluşturabileceğiniz bir *Microsoft.StreamAnalytics/streamingjobs* kaynak Resource Manager şablonunuzu kaynak bölümünde aşağıdaki özellik ekleyerek bir yönetilen kimlik ile:

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Bu özellik, Azure Resource Manager'ı oluşturma ve Azure Stream Analytics işiniz için Kimlik Yönetimi söyler.

   **Örnek Proje**

    ```json
    {
      "Name": "AsaJobWithIdentity",
      "Type": "Microsoft.StreamAnalytics/streamingjobs",
      "Location": "West US",
      "Identity": {
        "Type": "SystemAssigned",
      },
      "properties": {
        "sku": {
          "name": "standard"
        },
        "outputs": [
          {
            "name": "string",
            "properties":{
              "datasource": {
                "type": "Microsoft.DataLake/Accounts",
                "properties": {
                  "accountName": "myDataLakeAccountName",
                  "filePathPrefix": "cluster1/logs/{date}/{time}",
                  "dateFormat": "YYYY/MM/DD",
                  "timeFormat": "HH",
                  "authenticationMode": "Msi"
                }
              }
   ```
  
   **Örnek Proje yanıtı**

   ```json
   {
    "Name": "mySAJob",
    "Type": "Microsoft.StreamAnalytics/streamingjobs",
    "Location": "West US",
    "Identity": {
      "Type": "SystemAssigned",
        "principalId": "GUID",
        "tenantId": "GUID",
      },
      "properties": {
        "sku": {
          "name": "standard"
        },
      }
   ```

   Asıl gerekli ADLS kaynağına erişim izni vermek için kimlik iş yanıtından not alın.

   **Kiracı kimliği** hizmet sorumlusu oluşturulduğu Azure Active Directory kiracısı kimliğidir. Hizmet sorumlusu abonelik tarafından güvenilen bir Azure kiracısı oluşturulur.

   **Türü** yönetilen kimlikleri türlerinde açıklandığı gibi yönetilen kimlik türünü belirtir. Yalnızca sistem atanan türü desteklenmiyor.

2. PowerShell kullanarak hizmet sorumlusuna erişim sağlar. Hizmet sorumlusu PowerShell erişim vermek için aşağıdaki komutu yürütün:

   ```powershell
   Set-AzDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   **Principalıd** hizmet sorumlusu nesne kimliği ve hizmet sorumlusu oluşturulduktan sonra portal ekranında listelenir. Resource Manager şablon dağıtımı kullanarak işi oluşturduysanız, nesne kimliği proje yanıtı kimlik özelliğinde listelenir.

   **Örnek**

   ```powershell
   PS > Set-AzDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Yukarıdaki PowerShell komutu hakkında daha fazla bilgi için bkz [kümesi AzDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/az.datalakestore/set-azdatalakestoreitemaclentry) belgeleri.

## <a name="limitations"></a>Sınırlamalar
Bu özellik, aşağıdakileri desteklemez:

1.  **Çok kiracılı erişim**: Belirli bir Stream Analytics iş için oluşturulan hizmet sorumlusu, Azure Active Directory kiracısı üzerinde iş oluşturuldu ve farklı bir Azure Active Directory kiracısı üzerinde bulunan bir kaynağa karşı kullanılamaz yer alacaktır. Bu nedenle, Azure Stream Analytics işinizi olarak aynı Azure Active Directory kiracısı içinde ADLS Gen 1 kaynaklar üzerinde yalnızca MSI kullanabilirsiniz. 

2.  **[Atanan kullanıcı kimlik](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)**: Desteklenmeyen bu kullanıcı, Stream Analytics işi tarafından kullanılmak üzere kendi hizmet sorumlusu girmeniz mümkün değil anlamına gelir. Hizmet sorumlusunu Azure Stream Analytics tarafından oluşturulur. 


## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics ile Data lake Store çıkış oluşturma](../data-lake-store/data-lake-store-stream-analytics.md)
* [Stream Analytics sorguları Visual Studio ile yerel olarak test etme](stream-analytics-vs-tools-local-run.md)
* [Visual Studio için Azure Stream Analytics araçları kullanarak yerel olarak test canlı verileri](stream-analytics-live-data-local-testing.md) 
