1. Birleşik Kurulum yükleme dosyasını çalıştırın.
2. **Başlamadan önce** bölümünde **Yapılandırma sunucusu ve işlem sunucusunu yükle**’yi seçin.
    ![Başlamadan önce](./media/site-recovery-add-configuration-server/combined-wiz1.png)
3. MySQL indirip yüklemek için **Üçüncü Taraf Yazılım Lisansı** bölümünde **Kabul Ediyorum**’a tıklayın.

    ![Üçüncü taraf yazılım](./media/site-recovery-add-configuration-server/combined-wiz105.PNG)
4. **Kayıt** menüsünde kasadan indirdiğiniz kayıt defteri anahtarını seçin.

    ![Kayıt](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. **İnternet Ayarları** alanında, yapılandırma sunucusunda çalışan Sağlayıcının Azure Site Recovery'ye İnternet üzerinden nasıl bağlanacağını belirtin.

   * O anda makine üzerinde ayarlanmış olan bir ara sunucu ile bağlanmak isterseniz **Var olan ara sunucu ayarlarıyla bağlan** seçeneğini belirleyin.
   * Sağlayıcı'nın doğrudan bağlanmasını istiyorsanız **Ara sunucu olmadan doğrudan bağlan** seçeneğini belirleyin.
   * Var olan ara sunucu kimlik doğrulaması gerektiriyorsa veya Sağlayıcı bağlantısı için özel bir ara sunucu kullanmak istiyorsanız **Özel ara sunucu ayarlarıyla bağlan** seçeneğini belirleyin.

     * Özel bir ara sunucu kullanırsanız adresi, bağlantı noktasını ve kimlik bilgilerini belirtmeniz gerekir.
     * Ara sunucu kullanıyorsanız [önkoşullar](#configuration-server-prerequisites) bölümünde açıklanan URL'lere izin vermiş olmanız gerekir.

     ![Güvenlik duvarı](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın (**Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

    ![Ön koşullar](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. **MySQL Yapılandırması** menüsünde, yüklü MySQL sunucu örneğinde oturum açmak için kimlik bilgileri oluşturun.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Çoğaltacaksanız, kurulum PowerCLI 6.0’ın yüklü olup olmadığını denetler.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.

    ![Yükleme konumu](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. **Ağ Seçimi** menüsünde, yapılandırma sunucusunun çoğaltma verilerini gönderip aldığı dinleyiciyi (ağ bağdaştırıcısı ve SSL bağlantı noktası) seçin. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Bağlantı noktası 9443’e ek olarak, çoğaltma işlemlerini düzenlemek için web sunucusu tarafından kullanılan bağlantı noktası 443 de açılır. Bağlantı noktası 443’ü çoğaltma trafiği göndermek veya almak için kullanmayın.

    ![Ağ seçimi](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle**’ye tıklayın. Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın.

    ![Özet](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Kayıt tamamlandıktan sonra, sunucu kasadaki **Ayarlar** > **Sunucular** dikey penceresinde görüntülenir.


<!--HONumber=Feb17_HO2-->


