# <a name="persist-files-in-azure-cloud-shell"></a>Azure Cloud Shell, dosyaların kalıcı olması
Cloud Shell'i dosyaları oturumlarda kalıcı hale getirilmesi için Azure dosya depolama kullanır.

## <a name="set-up-a-clouddrive-file-share"></a>Clouddrive dosya paylaşımı ayarlama
İlk Başlat, Cloud Shell'i dosyaları oturumlarda kalıcı hale getirmek için yeni veya varolan bir dosya paylaşımını ilişkilendirmek isteyip istemediğinizi sorar.

> [!NOTE]
> Bash ve PowerShell aynı dosya paylaşımını paylaşın. Yalnızca bir dosya paylaşımı, Cloud shell'de otomatik olarak bağlama ile ilişkilendirilebilir.

### <a name="create-new-storage"></a>Yeni depolama oluşturma

Cloud Shell, desteklenen bölgede size en yakın olanında, sizin adınıza üç kaynak oluşturur, temel ayarları kullanın ve yalnızca bir abonelik seçin:
* Kaynak grubu: `cloud-shell-storage-<region>`
* Depolama hesabı: `cs<uniqueGuid>`
* Dosya Paylaşımı: `cs-<user>-<domain>-com-<uniqueGuid>`

![Abonelik ayarı](../articles/cloud-shell/media/persisting-shell-storage/basic-storage.png)

Dosya Paylaşımı bağlar olarak `clouddrive` içinde `$Home` dizin. Bu tek seferlik bir işlemdir ve dosya paylaşımı sonraki her oturumda otomatik olarak bağlar. 

> [!NOTE]
> Güvenlik için her bir kullanıcı kendi depolama hesaplarını sağlamanız gerekir.  Rol tabanlı erişim denetimi (RBAC), kullanıcılara katkıda bulunan erişimine sahip veya hesap düzeyinde depolama alanı yukarıda gerekir.

Bash hizmetinde dosya paylaşımını da sizin için otomatik olarak oluşturulduğu bir 5 GB'lık görüntüsünü içeren veri devam ederse, `$Home` dizin. 

### <a name="use-existing-resources"></a>Var olan kaynakları kullan

Gelişmiş seçeneğini kullanarak, mevcut kaynaklar ilişkilendirebilirsiniz. Depolama Kurulum istemi göründüğünde seçin **Gelişmiş ayarları göster** ek seçenekleri görmek için. Aşağı açılan menüler, atanan Cloud Shell bölgeniz ve yerel olarak yedekli depolama ve coğrafi olarak yedekli depolama hesapları için filtrelenir.

Dosya paylaşımları, kalıcı hale getirmek oluşturduğunuz bir 5 GB'lık görüntüsü almak, `$Home` dizin.

![Kaynak grubu ayarı](../articles/cloud-shell/media/persisting-shell-storage/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a>Bir Azure kaynak İlkesi ile kaynak oluşturmayı kısıtla
Cloud Shell'de oluşturduğunuz depolama hesapları ile etiketlenmiş `ms-resource-usage:azure-cloud-shell`. Kullanıcıların depolama hesapları, Cloud Shell'de oluşturmasını istiyorsanız oluşturma bir [etiketleri için bir Azure kaynak ilkesinden](../articles/azure-policy/json-samples.md) bu belirli bir etikete göre tetiklenir.

## <a name="supported-storage-regions"></a>Desteklenen depolama bölgeleri
Azure depolama hesapları için bağlama Cloud Shell makine ile aynı bölgede bulunmalıdır ilişkili.

Bölgenize atanan bulmak için şunları yapabilirsiniz:
* Notu "Gelişmiş Depolama ayarları" iletişim kutusunda görüntüleyin
* Sizin için oluşturulan depolama hesabı adına bakın (örn: `cloud-shell-storage-westus`)
* Çalıştırma `env` değişkeni bulun `ACC_LOCATION`

Cloud Shell makineler aşağıdaki bölgelerde mevcuttur:
|Alan|Bölge|
|---|---|
|Kuzey ve Güney Amerika|Doğu ABD, Güney Orta ABD, Batı ABD|
|Avrupa|Kuzey Avrupa, Batı Avrupa|
|Asya Pasifik|Hindistan Orta, Güneydoğu Asya|

