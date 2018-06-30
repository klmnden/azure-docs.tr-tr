# <a name="persist-files-in-azure-cloud-shell"></a>Azure bulut Kabuk dosyalarında Sürdür
Bulut Kabuk oturumlarında dosyaları kalıcı hale getirmek için Azure File storage kullanır.

## <a name="set-up-a-clouddrive-file-share"></a>Clouddrive dosya paylaşımı ayarlama
İlk Başlat, bulut Kabuk oturumlarında dosyaları kalıcı hale getirmek için yeni veya varolan bir dosya paylaşımı ilişkilendirmek isteyip istemediğinizi sorar.

> [!NOTE]
> Bash ve PowerShell aynı dosya paylaşımını paylaşır. Yalnızca bir dosya paylaşımı bulut Kabuğu'nda otomatik olarak bağlama ile ilişkili olabilir.

### <a name="create-new-storage"></a>Yeni depolama alanı oluşturma

Temel ayarlar kullanın ve yalnızca bir abonelik seçin, bulut Kabuk üç kaynakları en yakın olan desteklenen bölgede sizin adınıza oluşturur:
* Kaynak grubu: `cloud-shell-storage-<region>`
* Depolama hesabı: `cs<uniqueGuid>`
* Dosya Paylaşımı: `cs-<user>-<domain>-com-<uniqueGuid>`

![Abonelik ayarı](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

Dosya Paylaşımı bağladığı olarak `clouddrive` içinde `$Home` dizin. Bu tek seferlik bir işlemdir ve dosya paylaşımı sonraki oturumlarda otomatik olarak bağlar. 

> [!NOTE]
> Güvenlik için her bir kullanıcı kendi depolama hesabı hazırlamanız.  Rol tabanlı erişim denetimi (RBAC) için kullanıcılara katkıda bulunan erişimi veya hesap düzey depolama üstünde gerekir.

Bash'te, dosya paylaşımı ayrıca sizin için otomatik olarak oluşturulduğu bir 5 GB görüntüsünü içeren veri devam ederse, `$Home` dizin. 

### <a name="use-existing-resources"></a>Var olan kaynakları kullanın

Gelişmiş seçeneğini kullanarak, var olan kaynakların ilişkilendirebilirsiniz. Depolama Kurulum istemi belirdiğinde seçin **Göster Gelişmiş ayarları** ek seçenekleri görüntülemek için. Aşağı açılır menüler atanan bulut Kabuk bölgeniz ve yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama hesapları için filtrelenir.

Bash'te, varolan dosya paylaşımları için kalıcı hale getirmek oluşturduğunuz bir 5 GB görüntüsü almak, `$Home` dizin.

![Kaynak grubu ayarı](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Bir Azure kaynak İlkesi ile kaynak oluşturma kısıtla
Bulut Kabuğu'nda oluşturduğunuz depolama hesapları ile etiketlenmiş `ms-resource-usage:azure-cloud-shell`. Bulut Kabuğu'nda depolama hesapları oluşturma kullanıcıların engellemek istiyorsanız oluşturma bir [etiketleri için Azure kaynak İlkesi](../articles/azure-policy/json-samples.md) bu belirli etiketi tarafından tetiklenir.

## <a name="supported-storage-regions"></a>Desteklenen depolama bölgeleri
İlişkili Azure depolama hesapları için takma bulut Kabuk makine ile aynı bölgede bulunmalıdır.

Atanan bölgenizi bulmak için olabilir:
* Not "Gelişmiş Depolama ayarları" iletişim kutusunda görüntüleyin
* Sizin için oluşturulan depolama hesabının adına bakın (örn: `cloud-shell-storage-westus`)
* Çalıştırma `env` ve değişken bulun `ACC_LOCATION`

Bulut Kabuk makine aşağıdaki bölgelerde var:
|Alan|Bölge|
|---|---|
|Kuzey ve Güney Amerika|Doğu ABD, Orta Güney ABD, Batı ABD|
|Avrupa|Kuzey Avrupa, Batı Avrupa|
|Asya Pasifik|Hindistan Orta, Güneydoğu Asya|

