## <a name="download-install-and-register-the-azure-backup-agent"></a>İndirme, yükleme ve Azure Backup aracısını Kaydet
Kurtarma Hizmetleri kasası oluşturduktan sonra Azure için verileri yedeklemek için kullanılan yedekleme aracısı her Windows makinelerinizin (Windows Server, Windows istemci, System Center Data Protection Manager sunucusu veya Azure yedekleme sunucusu makine) yükleyin.

1. Aboneliğinizde açmak [Azure Portal](https://ms.portal.azure.com/).
2. Sol taraftaki menüden **Tüm hizmetler**’i seçin ve hizmet listesinde **Kurtarma Hizmetleri** yazın. **Kurtarma Hizmetleri kasaları**’na tıklayın.

   ![Kurtarma Hizmetleri kasasını açma](../articles/backup/media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)
3. Hızlı Başlangıç sayfasında, tıklatın **için Windows Server veya System Center Data Protection Manager veya Windows İstemcisi** altında seçeneği **aracıyı indir**. Tıklatın **kaydetmek** yerel makineye kopyalanması için.
   
    ![Aracı Kaydet](./media/backup-install-agent/agent.png)
4. Aracı yüklendikten sonra Azure Backup agent kurulumunu başlatmak için MARSAgentInstaller.exe çift tıklayın. Yükleme klasörü ve aracı için gereken geçici klasörü seçin. Belirtilen önbellek konumunu yedekleme verilerini % 5'en az boş alan gerekir.
5. İçinde internet'e bağlanmak için bir proxy sunucu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntılarını girin. Doğrulanmış bir proxy kullanıyorsanız, bu ekranda kullanıcı adı ve parola bilgilerinizi girin.
6. (Bu zaten mevcut değilse) Azure Backup aracısını yüklemeyi tamamlamak için Windows PowerShell ve .NET Framework 4.5 yükler.
7. Aracı yüklendikten sonra tıklayın **devam etmek için kayıt** iş akışı ile devam etmek için düğmesini.
   
   ![Kaydolma](./media/backup-install-agent/register.png)
8. Kasa kimlik bilgileri ekranında indirilen kasa kimlik bilgileri dosyası seçin.
   
    ![Kasa kimlik bilgileri](./media/backup-install-agent/vc.png)
   
    Kasa kimlik bilgileri dosyası yalnızca 48 için geçerlidir (portaldan yüklendikten sonra) saat. Bu ekran ("dosya süresi doldu örneğin kasa kimlik bilgileri sağlandı") içinde herhangi bir hatayla karşılaşırsanız, Azure portalında oturum açın ve kasa kimlik bilgileri dosyasını yeniden indirin.
   
    Kurulum uygulaması kasa kimlik bilgileri dosyası erişebilirsiniz emin olun. Erişim ile ilgili hatalarla karşılaşırsanız, kasa kimlik bilgileri dosyası yerel makine üzerinde geçici bir konuma kopyalayın ve işlemi yeniden deneyin.
   
    Karşılaşırsanız geçersiz bir kasa kimlik bilgisi hata "geçersiz kasa kimlik bilgileri sağlanan"gibi kasa kimlik bilgileri dosyası bozuk veya mu en yeni kimlik kurtarma hizmeti ile ilişkilendirilmiş değil. Yeni bir kasa kimlik bilgilerini portaldan indirme işlemi yeniden deneyin. Bu hata genellikle bir kullanıcı tıkladığında görülen **indirme kasa kimlik bilgileri** Azure portalında hızlı art arda seçeneği. Bu durumda, yalnızca ikinci kasa kimlik bilgilerini geçerli değil.
9. İçinde **şifreleme ayarı** ekran, bir parola oluşturabilir veya bir parola (en fazla 16 karakter) girin. Parolayı güvenli bir konumda kaydetmeyi unutmayın.
   
    ![Şifreleme](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Parola kaybolur veya unutulursa; Microsoft, yedekleme verilerini kurtarmak yardımcı olamaz. Son kullanıcı şifreleme parolası sahibi ve Microsoft son kullanıcı tarafından kullanılan parola görünürlüğe sahip değil. Bir kurtarma işlemi sırasında gerekli olduğu gibi dosyayı güvenli bir konuma kaydedin.
   > 
   > 
10. Tıkladığınızda **son** düğmesi makine başarıyla kasaya kayıtlı ve Microsoft Azure yedekleme başlatmak artık hazırsınız.
11. Microsoft Azure yedekleme tek başına kullanırken tıklayarak kayıt iş akışı sırasında belirtilen ayarları değiştirebileceğiniz **özelliklerini değiştirme** Azure Backup MMC ek bileşeninde seçeneği.
    
    ![Özelliklerini değiştir](./media/backup-install-agent/change.png)
    
    Alternatif olarak, veri koruma Yöneticisi'ni kullanırken tıklayarak kayıt iş akışı sırasında belirtilen ayarları değiştirebileceğiniz **yapılandırma** seçerek seçeneği **çevrimiçi** altında**Yönetim** sekmesi.
    
    ![Azure Yedekleme'yi yapılandırın](./media/backup-install-agent/configure.png)

