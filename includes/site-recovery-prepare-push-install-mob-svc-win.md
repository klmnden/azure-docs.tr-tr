### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Bir Windows bilgisayarda göndermeli yüklemesi için hazırlama

1. Windows bilgisayar ve işlem sunucusu arasında ağ bağlantısı olduğundan emin olun.
2. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesap (yerel veya etki alanı) yönetici haklarına sahip. (Yalnızca göndermeli yüklemesi için ve aracı güncelleştirmeleri için bu hesabı kullanın.)

   > [!NOTE]
   > Bir etki alanı hesabı kullanmıyorsanız, yerel bilgisayarda Uzak kullanıcı erişim denetimi devre dışı bırakın. Yeni bir DWORD değeri HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System kayıt defteri anahtarı altında uzaktaki kullanıcı erişimi denetimini devre dışı bırakma eklemek için: **LocalAccountTokenFilterPolicy**. Değer kümesine **1**. Bu komut isteminde yapmak için aşağıdaki komutu çalıştırın:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. Windows Güvenlik Duvarı'nda korumak istediğiniz bilgisayarda, seçin **bir uygulama veya özelliğin güvenlik duvarı aracılığıyla izin**. Etkinleştirme **dosya ve Yazıcı Paylaşımı** ve **Windows Yönetim Araçları (WMI)**. Bir etki alanına ait bilgisayarlar için Grup İlkesi nesnesi (GPO) kullanarak güvenlik duvarı ayarlarını yapılandırabilirsiniz.

   ![Güvenlik duvarı ayarları](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin.
    1.  Yapılandırma sunucunuzda oturum açın.
    2.  **cspsconfigtool.exe** dosyasını açın. (Masaüstünde bir kısayolu bulunur ve kendisi %ProgramData%\home\svsystems\bin klasöründedir.)
    3.  Üzerinde **hesaplarını yönetme** sekmesine **hesabı Ekle**.
    4.  Oluşturduğunuz hesabı ekleyin.
    5.  Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.
