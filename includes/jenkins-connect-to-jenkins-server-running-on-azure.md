1. Tarayıcınızda, Jenkins sanal makinenize gidin. Jenkins Pano güvenli olmayan HTTP erişilemez olduğundan, SSH tünel sanal makineye kullanmanız gerektiğini belirten bir ileti görüntüler.

    ![Jenkins SSH Jenkins sanal makineye bağlanmak için nasıl kullanılacağı hakkında bilgi sağlar.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ssh-instructions.png)

1. SSH erişimi olan bir bash veya terminal penceresi açın.

1. Aşağıdaki girin `ssh` komutu, değiştirme  **&lt;kullanıcı adı >** ve  **&lt;etki alanı >** Jenkins oluştururken, belirtilen değerler yer tutucularını sanal makine:

    ```bash
    ssh -L 127.0.0.1:8080:localhost:8080 <username>@<domain>.eastus.cloudapp.azure.com
    ```

1. İzleyin `ssh` Jenkins sanal makinede oturum ister.

1. İlk parola için kilidini açma Jenkins almak için aşağıdaki komutu girin.

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

1. Bash veya terminal penceresinde görüntülenen parolayı panoya kopyalayın.

1. Tarayıcınızda gidin `http://localhost:8080`.

1. İçine daha önce aldığınız parolayı yapıştırın **yönetici parolasını** alan ve select **devam**.

    ![Jenkins Jenkins sanal makine buna bağlanmak ilk kez kilidini açmak için kullanmanız gereken bir ilk parola depolar.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-unlock.png)

1. Seçin **önerilen Eklentileri yüklemek**.

    ![Jenkins, kolayca ilk girişte önerilen eklentileri koleksiyonu yüklemenize olanak tanır](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-customize.png)

1. Üzerinde **ilk yönetici kullanıcı oluştur** sayfasında, yönetici kullanıcı adı ve parola Jenkins pano oluşturun ve seçin **kaydedin ve son**.

1. Üzerinde **Jenkins hazır!** sayfasında, **Jenkins kullanmaya başlamak**.

    ![Jenkins yüklü ve erişimi için yapılandırılan sonra ve işleri oluşturmak için kullanmaya başlayabilirsiniz.](./media/jenkins-connect-to-jenkins-server-running-on-azure/jenkins-ready.png)