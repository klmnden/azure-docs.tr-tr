### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Bir Linux sunucusu üzerinde göndererek yüklemeye hazırlanma

1. Linux bilgisayar ile işlem sunucusu arasında ağ bağlantısı bulunduğundan emin olun.
2. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesap, kaynak Linux sunucusu üzerindeki bir **kök** kullanıcı olmalıdır. (Bu hesabı yalnızca göndererek yükleme ve güncelleştirmeler için kullanın.)
3. Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
4. Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
5. Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
6. SFTP alt sistemi ve parola ile kimlik doğrulamasını sshd_config dosyasında etkinleştirin:
  1.  **Kök** kullanıcı olarak oturum açın.
  2.  /etc/ssh/sshd_config dosyasında **PasswordAuthentication** ile başlayan satırı bulun.
  3.  Satırı açıklama durumundan çıkarın ve değerini **yes** (evet) olarak değiştirin.
  4.  **Subsystem** ile başlayan satırı bulun ve açıklama durumundan çıkarın.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. **sshd** hizmetini yeniden başlatın.

7. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin.
    1.  Yapılandırma sunucunuzda oturum açın.
    2.  **cspsconfigtool.exe** dosyasını açın. (Masaüstünde bir kısayolu bulunur ve kendisi %ProgramData%\home\svsystems\bin klasöründedir.)
    3.  **Hesapları Yönet** sekmesinde **Hesap Ekle**’ye tıklayın.
    4.  Oluşturduğunuz hesabı ekleyin. 
    5.  Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.
