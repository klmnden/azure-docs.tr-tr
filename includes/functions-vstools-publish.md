1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. Seçin **Yeni Oluştur** ve ardından **yayımlama**. 

    ![Yeni işlev uygulaması oluşturma ve yayımlama](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

2. Visual Studio Azure hesabınızda zaten bağlanmadıysanız, seçin **Hesap Ekle...** .  

3. **App Service Oluştur** iletişim kutusunda, aşağıdaki tabloda belirtilen **Barındırma** ayarlarını kullanın: 

    ![Azure yerel çalışma zamanı](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Seçin **yeni** yeni bir kaynak grubu oluşturmak için.|
    | **[App Service Planı](../articles/azure-functions/functions-scale.md)** | Tüketim planı | Seçtiğinizden emin olun **tüketim** altında **boyutu** tıklattıktan sonra **yeni** yeni bir plan oluşturmak için. Ayrıca, bir **konumu** içinde bir [bölge](https://azure.microsoft.com/regions/) yakın veya diğer hizmetler yakın işlevlerinizi erişim.  |

    >[!NOTE]
    >Bir Azure depolama hesabı işlevleri çalışma zamanı tarafından gereklidir. Bir işlev uygulaması oluşturduğunuzda, bu nedenle, yeni bir Azure depolama hesabı sizin için oluşturulur.

4. Tıklatın **oluşturma** bir işlev uygulaması ve ilgili kaynakları bu ayarlarla Azure'da oluşturmak ve işlev proje kodunuza dağıtmak için. 

5. Dağıtım tamamlandıktan sonra Not **Site URL'si** işlevi uygulamanızda Azure adresidir değeri.

    ![Azure yerel çalışma zamanı](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
