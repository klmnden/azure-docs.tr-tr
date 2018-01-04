İçinde [Azure portal](https://portal.azure.com), tıklatın **kaynak grupları** > **bulut-shell-depolama -\<your_region >**  >   **\<storage_account_name >**.

![Bulut Kabuk depolama hesabı bulunamadı](../articles/app-service/media/app-service-deploy-zip/upload-choose-storage-account.png)

İçinde **genel bakış** sayfa seçin depolama hesabının **dosyaları**.

Otomatik olarak oluşturulan dosya paylaşımı seçip **karşıya**. Bu dosya paylaşımı bulut kabuğunda takılı `clouddrive`.

![Karşıya Yükle düğmesini Bul](../articles/app-service/media/app-service-deploy-zip/upload-select-button.png)

Dosya seçiciyi ve ZIP dosyası seçin ve ardından **karşıya**. 

Bulut Kabuğu'nda kullanmak `ls` varsayılan karşıya yüklenen ZIP dosyasında görebildiğini doğrulamak için `clouddrive` paylaşın.

```azurecli-interactive
ls clouddrive
```
