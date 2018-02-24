### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Bir Linux sunucusu üzerinde göndererek yüklemeye hazırlanma

1. Linux bilgisayarı ve işlem sunucusu arasında ağ bağlantısı olduğundan emin olun.
2. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesap, kaynak Linux sunucusu üzerindeki bir **kök** kullanıcı olmalıdır. Göndermeli yüklemesi için yalnızca güncelleştirmeler için ve bu hesabı kullanın.
3. Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
4. Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
5. Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
6. SFTP alt sistemi ve parola kimlik doğrulaması sshd_config dosyasında etkinleştirin. Şu adımları uygulayın:

    a. **Kök** kullanıcı olarak oturum açın.

    b. İçinde **/etc/ssh/sshd_config** dosya, ile başlayan satırı bulun **PasswordAuthentication**.

    c. Satırı açıklamadan çıkarın ve değerini değiştirin **Evet**.

    d. İle başlayan satırı bulun **alt sistemi**, ve satırı açıklamadan çıkarın.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. **sshd** hizmetini yeniden başlatın.

7. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Şu adımları uygulayın:

    a. Yapılandırma sunucunuzda oturum açın.

    b. **cspsconfigtool.exe** dosyasını açın. Masaüstünde ve %ProgramData%\home\svsystems\bin klasöründe bir kısayol olarak kullanılabilir.

    c. Üzerinde **hesaplarını yönetme** sekmesine **hesabı Ekle**.

    d. Oluşturduğunuz hesabı ekleyin.

    d. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.
