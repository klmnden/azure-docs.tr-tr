---
title: Azure Stream Analytics işleri için Azure Data Lake depolama Gen1 çıkış (Önizleme) kimliğini doğrulamak için yönetilen kimlikleri kullanmak
description: ''
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/27/2018
ms.openlocfilehash: 72bf467cc0f2ba195aa4f25228bc9e08605cd4ee
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018610"
---
# <a name="use-managed-identities-to-authenticate-azure-stream-analytics-jobs-to-azure-data-lake-storage-gen1-output-preview"></a>Azure Stream Analytics işleri için Azure Data Lake depolama Gen1 çıkış (Önizleme) kimliğini doğrulamak için yönetilen kimlikleri kullanmak

Azure Stream Analytics, Azure Data Lake Storage (ADLS) Gen1 çıktıyla yönetilen kimlik doğrulama destekler. Kimlik Azure Active Directory içinde belirli bir Stream Analytics işi temsil eden kayıtlı bir yönetilen uygulama ve hedeflenen kaynak kimliğini doğrulamak için kullanılabilir. Yönetilen kimlik parola değişikliklerini veya 90 günde gerçekleşen kullanıcı belirteci süre sonu nedeniyle yeniden kimlik doğrulamaya zorlayabilir gerek gibi kullanıcı tabanlı kimlik doğrulama yöntemlerini sınırlamaları ortadan kaldırır. Ayrıca, yönetilen kimlikleri, Azure Data Lake depolama Gen1 output Stream Analytics işi dağıtımlarının Otomasyonu ile yardımcı olur.

Ziyaret [Azure Stream analytics'te sekiz yeni özellikler](https://azure.microsoft.com/en-us/blog/eight-new-features-in-azure-stream-analytics/) blog gönderisi için yeni özellikler hakkında daha fazla Bu önizleme sürümü ve okuma için kaydolun.

Bu makalede, bir Azure Data Lake depolama Gen1 veren Azure Stream Analytics işi için yönetilen kimlik etkinleştirmek için iki yolunu gösterir: Azure portalı ve Azure Resource Manager şablon dağıtımı aracılığıyla.

## <a name="enable-managed-identity-with-azure-portal"></a>Azure portalı ile yönetilen kimliği etkinleştirme

1. Yeni bir Stream Analytics işi oluşturma veya var olan bir işi, Azure portalında açarak başlatın. Ekranın sol tarafında bulunan menü çubuğundan seçin **yönetilen kimliği (Önizleme)** altında bulunan **yapılandırma**.

   ![Stream Analytics yönetilen kimlik Önizleme yapılandırın](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Seçin **kullanım sistem tarafından atanan yönetilen kimliği (Önizleme)** penceresinden sağ tarafta görüntülenir. Tıklayın **Kaydet** Azure Active Directory'de bir hizmet sorumlusu kimliğinin Stream Analytics işi oluşturmak için. Yeni oluşturulan kimlik yaşam döngüsünü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (diğer bir deyişle, hizmet sorumlusu), Azure tarafından otomatik olarak silinir.

   Yapılandırma kaydedilirken, hizmet sorumlusu nesne kimliği (OID) asıl aşağıda gösterildiği gibi kimlik olarak listelenir:

   ![Stream Analytics sorumlusu kimliği](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Hizmet sorumlusu, Stream Analytics işiyle aynı ada sahip. Örneğin, işinizin adı ise **MyASAJob**, oluşturulan hizmet sorumlusu adını da olduğu **MyASAJob**.

3. Kimlik doğrulama modu açılır ve select ADLS Gen1 çıkış havuzu, çıkış Özellikler penceresinde tıklayın **yönetilen kimliği (Önizleme)**.

4. Kalan özellikleri doldurun. ADLS çıktı oluşturma hakkında daha fazla bilgi edinmek için [stream analytics ile Data lake Store çıktı oluşturma](../data-lake-store/data-lake-store-stream-analytics.md). İşlemi tamamladığınızda, tıklayın **Kaydet**.

   ![Azure Data Lake depolama yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. ADLS Gen1 Genel Bakış sayfasına gidin ve tıklayarak **Veri Gezgini**.

   ![Data Lake depolamaya genel bakış yapılandırın](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Veri Gezgini bölmesinde seçin **erişim** tıklatıp **Ekle** erişim bölmesinde.

   ![Data Lake depolama erişimi yapılandırma](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Metin kutusuna **kullanıcı veya grup seçin** bölmesinde, hizmet sorumlusu adını yazın. Hizmet sorumlusu adını karşılık gelen Stream Analytics işinin adı olduğunu unutmayın. Asıl adını yazmaya başladığınızda, bu metin kutusunun altında görüntülenir. İstenen hizmet asıl adı'nı seçin ve tıklayın **seçin**.

   ![Bir hizmet asıl adı seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. İçinde **izinleri** bölmesinde, onay **yazma** ve **yürütme** izinleri atayabilirsiniz **bu klasör ve tüm alt öğeleri**. Ardından **Tamam**.

   ![Bir izin seçin](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Hizmet sorumlusu altında listelenen **atanan izinler** üzerinde **erişim** aşağıda gösterildiği gibi bölmesi. Şimdi, geri dönün ve Stream Analytics işinizi başlatın.

   ![Erişim listesi](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Data Lake depolama Gen1 dosya sistemi izinleri hakkında daha fazla bilgi için bkz: [Azure Data Lake depolama Gen1 erişim denetimi](../data-lake-store/data-lake-store-access-control.md).

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
                 "accountName": “myDataLakeAccountName",
                 "filePathPrefix": “cluster1/logs/{date}/{time}",
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
   Set-AzureRmDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   **Principalıd** hizmet sorumlusu nesne kimliği ve hizmet sorumlusu oluşturulduktan sonra portal ekranında listelenir. Resource Manager şablon dağıtımı kullanarak işi oluşturduysanız, nesne kimliği proje yanıtı kimlik özelliğinde listelenir.

   **Örnek**

   ```powershell
   PS > Set-AzureRmDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Yukarıdaki PowerShell komutu hakkında daha fazla bilgi için bkz [kümesi AzureRmDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/azurerm.datalakestore/set-azurermdatalakestoreitemaclentry?view=azurermps-6.8.1&viewFallbackFrom=azurermps-4.2.0#optional-parameters) belgeleri.

## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics ile Data lake Store çıkış oluşturma](../data-lake-store/data-lake-store-stream-analytics.md)
