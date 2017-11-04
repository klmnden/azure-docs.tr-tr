## <a name="download-install-and-register-the-azure-backup-agent"></a>İndirme, yükleme ve Azure Backup aracısını Kaydet
Azure yedekleme kasası oluşturduktan sonra bir aracı her veri ve Azure uygulamalarının yedekleme sağlayan Windows makinelerinizin (Windows Server, Windows istemci, System Center Data Protection Manager sunucusu veya Azure yedekleme sunucusu makine) yüklü olmalıdır .

1. Oturum [Yönetim Portalı](https://manage.windowsazure.com/)
2. Tıklatın **kurtarma Hizmetleri**, bir sunucuyla kaydetmek istediğiniz yedekleme kasasının seçin. Bu yedekleme kasası için hızlı başlangıç sayfası görüntülenir.
   
    ![Hızlı başlangıç](./media/backup-install-agent/quickstart.png)
3. Hızlı Başlangıç sayfasında, tıklatın **için Windows Server veya System Center Data Protection Manager veya Windows İstemcisi** altında seçeneği **aracıyı indir**. Tıklatın **kaydetmek** yerel makineye kopyalanması için.
   
    ![Aracı Kaydet](./media/backup-install-agent/agent.png)
4. Aracı yüklendikten sonra çift MARSAgentInstaller.exe Azure Backup agent kurulumunu başlatmak için tıklatın. Yükleme klasörü ve aracı için gereken geçici klasörü seçin. Belirtilen önbellek konumunu yedekleme verilerini % 5'en az boş alan olması gerekir.
5. İçinde internet'e bağlanmak için bir proxy sunucu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntılarını girin. Doğrulanmış bir proxy kullanıyorsanız, bu ekranda kullanıcı adı ve parola bilgilerinizi girin.
6. (Bu zaten mevcut değilse) Azure Backup aracısını yüklemeyi tamamlamak için Windows PowerShell ve .NET Framework 4.5 yükler.
7. Aracı yüklendikten sonra tıklayın **devam etmek için kayıt** iş akışı ile devam etmek için düğmesini.
   
   ![Kaydolma](./media/backup-install-agent/register.png)
8. Kasa kimlik bilgileri ekranında göz atın ve daha önce indirilen kasa kimlik bilgileri dosyası seçin.
   
    ![Kasa kimlik bilgileri](./media/backup-install-agent/vc.png)
   
    Kasa kimlik bilgileri dosyası yalnızca 48 için geçerlidir (portaldan yüklendikten sonra) saat. Varsa bu ekran oturum açma (örneğin "kasa kimlik bilgileri dosyasının süresi doldu sağlanan"), Azure portalına içinde herhangi bir hatayla karşılaşırsanız ve kasa kimlik bilgileri dosyasını yeniden indirin.
   
    Kasa kimlik bilgileri dosyası Kurulum uygulama tarafından erişilebilen bir konumda kullanılabilir olduğundan emin olun. Karşılaşırsanız ilgili hatalar erişim, kasa kimlik bilgileri dosyası bu makinede geçici bir konuma kopyalayın ve işlemi yeniden deneyin.
   
    Geçersiz kasa kimlik bilgileri hatayla (örneğin "geçersiz kasa kimlik bilgileri sağlandı") karşılaşırsanız dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile. Yeni bir kasa kimlik bilgilerini portaldan indirme işlemi yeniden deneyin. Bu hata genellikle kullanıcı üzerinde tıklarsa görülür **indirme kasa kimlik bilgileri** Azure portalında hızlı art arda seçeneği. Bu durumda, yalnızca ikinci kasa kimlik bilgilerini geçerli değil.
9. İçinde **şifreleme ayarı** ekran, bir parola oluşturabilir veya bir parola (en fazla 16 karakter) girin. Parolayı güvenli bir konumda kaydetmeyi unutmayın.
   
    ![Şifreleme](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Parola kaybolur veya unutulursa; Microsoft, yedekleme verilerini kurtarmak yardımcı olamaz. Son kullanıcı şifreleme parolası sahibi ve Microsoft son kullanıcı tarafından kullanılan parola görünürlüğe sahip değil. Lütfen bir kurtarma işlemi sırasında gerekli olduğu gibi dosyayı güvenli bir konuma kaydedin.
   > 
   > 
10. Tıkladığınızda **son** düğmesi makine başarıyla kasaya kayıtlı ve Microsoft Azure yedekleme başlatmak artık hazırsınız.
11. Microsoft Azure yedekleme tek başına kullanırken tıklayarak kayıt iş akışı sırasında belirtilen ayarları değiştirebileceğiniz **özelliklerini değiştirme** Azure Backup mmc ek bileşeninde seçeneği.
    
    ![Özelliklerini değiştir](./media/backup-install-agent/change.png)
    
    Alternatif olarak, veri koruma Yöneticisi'ni kullanırken tıklayarak kayıt iş akışı sırasında belirtilen ayarları değiştirebileceğiniz **yapılandırma** seçerek seçeneği **çevrimiçi** altında**Yönetim** sekmesi.
    
    ![Azure Yedekleme'yi yapılandırın](./media/backup-install-agent/configure.png)

