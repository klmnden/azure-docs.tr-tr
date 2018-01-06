## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Azure Backup hizmeti ile kimlik doğrulaması için kasa kimlik bilgilerini kullanma
Bir şirket içi sunucuyu (Windows İstemcisi veya Windows Server veya Data Protection Manager sunucu) Azure'a yedekleyebilirsiniz önce sunucunun kurtarma Hizmetleri kasası ile kimlik doğrulaması. Azure sunucusuyla kimlik doğrulaması için bir kasa kimlik bilgileri dosyası kullanın. Kasa kimlik bilgileri, Azure PowerShell ile kullanılan bir "yayımlama ayarları" dosya kavramı benzerdir.

### <a name="what-is-the-vault-credential-file"></a>Kasa kimlik bilgileri dosyası nedir?
Kasa kimlik bilgileri dosyası, her bir yedekleme kasası için portal tarafından oluşturulan bir sertifikadır. Portal daha sonra ortak anahtarı Access Control Service'e (ACS) yükler. Kullanıcıya, makine kayıt iş akışı içinde bir giriş olarak verilen iş akışının parçası olarak sertifikanın özel anahtarı kullanılabilir hale getirilir. Bu, yedekleme verilerini Azure Backup hizmetindeki tanımlanmış bir kasaya göndermek üzere makinenin kimliğini doğrular.

Kasa kimlik bilgileri yalnızca kayıt iş akışı sırasında kullanılır. Kasa kimlik bilgileri dosyası tehlikeye olun kullanıcının sorumluluğundadır. Yetkisiz bir kullanıcının eline geçmesi durumunda, söz konusu kasaya diğer makinelerin kaydedilebilmesi için kasa kimlik bilgileri dosyası kullanılabilir. Yedekleme verilerini müşteriye ait olan bir parola kullanılarak şifrelenir gibi ancak var olan yedekleme verilerinin gizliliği tehlikeye giremez. Bu endişenin ortadan kaldırılabilmesi için kasa kimlik bilgileri 48hrs içinde süresi dolacak şekilde ayarlanmıştır. Kez – herhangi bir sayıda yedekleme kasasının kasa kimlik bilgileri yükleyebilirsiniz, ancak kayıt iş akışı sırasında yalnızca en son kasa kimlik bilgilerini geçerlidir.

### <a name="download-the-vault-credential-file"></a>Kasa kimlik bilgilerini indirin
Kasa kimlik bilgilerini Azure portalından güvenli bir kanal üzerinden indirilir. Azure Backup hizmeti sertifikanın özel anahtarı farkında değildir ve özel anahtarı portalı veya hizmetinde kalıcı yapılmaz. Kasa kimlik bilgilerini yerel makineye indirmek için aşağıdaki adımları kullanın.

1. Açık [Azure portalı](https://ms.azure.portal.com/)
2. Sol taraftaki menüden seçin **tüm hizmetleri** ve Hizmetler listesinde yazın **kurtarma Hizmetleri**. Tıklatın **kurtarma Hizmetleri kasaları**.

   ![Açık kurtarma Hizmetleri kasası](../articles/backup/media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)
3. Hızlı Başlangıç sayfasında, tıklatın **indirme kasa kimlik bilgileri**. Portal indirme için kullanılabilir hale kasa kimlik bilgilerini oluşturur.
   
   ![İndirme](./media/backup-download-credentials/downloadvc.png)
4. Portal kasa adını ve geçerli tarih birleşimini kullanarak bir kasa kimlik bilgisi oluşturur. Tıklatın **kaydetmek** kasa kimlik bilgilerini Yerel hesabın indirmeler klasörüne indirin veya kasa kimlik bilgileri için bir konum belirtmek için Kaydet menüsünden Farklı Kaydet'i seçin.

### <a name="note"></a>Not
* Kasa kimlik bilgilerini makinenizden erişilebilen bir konuma kaydedildiğinden emin olun. Bir dosya paylaşımı/SMB depolanıyorsa erişim izinlerini denetleyin.
* Kasa kimlik bilgileri dosyası yalnızca kayıt iş akışı sırasında kullanılır.
* Kasa kimlik bilgileri dosyası 48hrs sonra süresi dolar ve portalından indirilebilir.
* Azure Backup başvuran [SSS](../articles/backup/backup-azure-backup-faq.md) akışında herhangi bir sorunuz için.

