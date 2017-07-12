**Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. **Yeni Oluştur**'u seçin ve **Yayımla**'ya tıklayın. 

![Yeni işlev uygulaması oluşturma ve yayımlama](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

Henüz Visual Studio'yu Azure hesabınıza bağlamadıysanız **Hesap ekle...** öğesine tıklayın.  

**Uygulama Hizmetini Oluştur** iletişim penceresinde, tabloda belirtilen barındırma ayarlarını kullanın. 

![Azure yerel çalışma zamanı](./media/functions-vstools-publish/functions-vstools-publish.png)

| Ayar      | Önerilen değer  | Açıklama                                |
| ------------ |  ------- | -------------------------------------------------- |
| **Uygulama Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. |
| **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
| **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. |
| **[App Service Planı](../articles/azure-functions/functions-scale.md)** | Tüketim planı | Yeni bir plan oluşturduğunuzda **Boyut** bölümünde **Tüketim** seçeneğini belirlediğinizden emin olun.  |
| **[Depolama hesabı](../articles/storage/storage-create-storage-account.md#create-a-storage-account)** | Genel olarak benzersiz bir ad | Mevcut bir depolama hesabını kullanın veya yeni bir hesap oluşturun.   |

Azure'da bu ayarlara sahip bir işlev uygulaması oluşturmak için **Oluştur**'a tıklayın. 

Hazırlama işlemi tamamlandıktan sonra proje kodunuzu yeni işlev uygulamasına dağıtmak için **Yayımla**'ya tıklayın. 

![Azure yerel çalışma zamanı](./media/functions-vstools-publish/functions-vstools-publish-profile.png)

**Site URL'si** değerini (Azure'daki işlev uygulamanızın adresi) not edin. 