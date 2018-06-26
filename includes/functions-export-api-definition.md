## <a name="export-an-api-definition"></a>API tanımı dışarı aktarma
Gelen işlevinizi için OpenAPI tanımına sahip [işlevi için bir OpenAPI tanımı oluşturma](../articles/azure-functions/functions-openapi-definition.md). Bu işlem bir sonraki adımda PowerApps ve Microsoft Flow özel bir API kullanabilmeleri API tanımı verilmesidir.

> [!IMPORTANT]
> Azure'a, PowerApps için kullandığınız ve Microsoft Flow Kiracı aynı kimlik bilgileriyle oturum açmanız gerekir olduğunu unutmayın. Bu özel API oluşturmak ve PowerApps ve Microsoft Flow için kullanılabilmesi Azure sağlar.

1. İçinde [Azure portal](https://portal.azure.com), işlev uygulaması adınıza tıklayın (gibi **işlevi demo enerji**) > **Platform özellikleri** > **API tanımı** .

    ![API tanımı](media/functions-export-api-definition/api-definition.png)

1. Tıklatın **vermek için PowerApps + akış**.

    ![API tanımı kaynağı](media/functions-export-api-definition/export-api-1.png)

1. Sağ bölmede, tabloda belirtildiği gibi ayarları kullanın.

    |Ayar|Açıklama|
    |--------|------------|
    |**Verme modu**|Seçin **Express** özel API otomatik olarak oluşturulacak. Seçme **el ile** dışarı API tanımı, ancak sonra içeri gerekir, PowerApps ve Microsoft Flow el ile. Daha fazla bilgi için bkz: [verme PowerApps ve Microsoft Flow](../articles/azure-functions/app-service-export-api-to-powerapps-and-flow.md).|
    |**Ortam**|Özel API kaydedilmesi gereken ortamını seçin. Daha fazla bilgi için bkz: [ortamlarına genel bakış (PowerApps)](https://powerapps.microsoft.com/tutorials/environments-overview/) veya [ortamlarına genel bakış (Microsoft Flow)](https://us.flow.microsoft.com/documentation/environments-overview-admin/).|
    |**Özel API adı**|Gibi bir ad girin `Turbine Repair`.|
    |**API anahtar adı**|Uygulama ve akış oluşturucular görmelisiniz adı özel API Arabiriminde girin. Not Örnek yararlı bilgiler içerir.|
 
    ![PowerApps ve Microsoft Flow’a dışarı aktarma](media/functions-export-api-definition/export-api-2.png)

1. **Tamam**’a tıklayın. Özel API artık yerleşik ve belirttiğiniz ortama eklenir.