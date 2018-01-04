1. Tarayıcınızda açın [Azure Market görüntüsü için Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Seçin **almak artık BT**.

    ![Jenkins Market görüntüsü için yükleme işlemini başlatmak için GIT BT şimdi seçin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Fiyatlandırma ayrıntıları ve koşulları bilgileri gözden geçirdikten sonra Seç **devam**.

    ![Jenkins Market görüntü fiyatlandırması ve koşulları bilgileri.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Seçin **oluşturma** Azure portalında Jenkins sunucusunu yapılandırmak için. 

    ![Jenkins Market görüntüsü yükleyin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. İçinde **Temelleri** sekmesinde, aşağıdaki değerleri belirtin:

    - **Ad** -girin `Jenkins`.
    - **Kullanıcı** -Jenkins çalışan sanal makinede oturum açarken kullanılacak kullanıcı adını girin.
    - **Kimlik doğrulama türü** - seçin **parola**.
    - **Parola** -Jenkins çalışan sanal makinede oturum açarken kullanılacak parolayı girin.
    - **Parolayı onaylayın** -Jenkins çalışan sanal makinede oturum açarken kullanılacak parolayı yeniden girin.
    - **Jenkins yayın türü** - seçin **LTS**.
    - **Abonelik** -içine Jenkins yüklemek istediğiniz Azure aboneliğini seçin.
    - **Kaynak grubu** - seçin **Yeni Oluştur**ve Jenkins yüklemenizi olun kaynakları gruplandırması için mantıksal bir kapsayıcı görevi görür kaynak grubu için bir ad girin.
    - **Konum** - seçin **Doğu ABD**.

    ![Kimlik doğrulama ve kaynak grubu bilgileri Jenkins için temel sekmede girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Seçin **Tamam** geçmek için **ayarları** sekmesi. 

1. İçinde **ayarları** sekmesinde, aşağıdaki değerleri belirtin:

    - **Boyutu** -Jenkins sanal makineniz için uygun boyutlandırma seçeneği belirleyin.
    - **VM disk türü** - iki HDD (sabit disk sürücüsü) belirtin veya Jenkins sanal makine için hangi depolama disk türünü belirtmek için (katı hal sürücüsü) SSD izin verilir.
    - **Genel IP adresi** -IP adresi adı varsayılan olarak belirttiğiniz - IP soneki önceki sayfasındaki Jenkins adı. Bu varsayılanı değiştirmek için seçeneğini kullanabilirsiniz.
    - **Etki alanı adı etiketi** -Jenkins sanal makine için tam bir URL için değeri belirtin.

    ![Ayarlar sekmesinde Jenkins için sanal makine ayarlarını girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Seçin **Tamam** geçmek için **Özet** sekmesi.

1. Zaman **Özet** sekmesini görüntüler, girdiğiniz bilgileri doğrulanır. Gördüğünüz sonra **geçirilen doğrulama** ileti, select **Tamam**. 

    ![Özet sekmesi görüntüler ve seçili seçeneklerinizi doğrular.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Zaman **oluşturma** seçin, sekmesini görüntüler **oluşturma** Jenkins sanal makine oluşturmak için. Sunucunuz hazır olduğunda, Azure portalında bir bildirim görüntüler.

    ![Jenkins hazır bildirimidir.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)