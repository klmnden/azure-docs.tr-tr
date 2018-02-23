#### <a name="to-create-a-new-service"></a>Yeni hizmet oluşturmak için

1.  Microsoft hesabı kimlik bilgilerini kullanarak şu URL ile Azure portalında oturum açın: <https://portal.azure.com/>. Kamu portal cihazı dağıtma, oturum açın: <https://portal.azure.us/>

2.  Azure portalında tıklatın **+ kaynak oluşturma** &gt; **depolama** &gt; **StorSimple sanal serisi**.

    ![Yeni bir hizmet oluşturun](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  İçinde **StorSimple Aygıt Yöneticisi'ni** , açılan dikey penceresinde aşağıdakileri yapın:

    1.  Hizmetiniz için benzersiz bir **Kaynak adı** sağlayın. Kaynak adı Hizmeti'ni tanımlamak için kullanılan kolay bir addır. Ad harf, rakam ve tirelerden oluşan 2-50 karakter arası uzunlukta olabilir. Ad bir harf veya sayıyla başlamalı ve bitmelidir.

    2.  Açılan listeden bir **Abonelik** seçin. Abonelik fatura hesabınıza bağlıdır. Bu alan bir aboneliğiniz olmadığı sürece yoktur.

    3.  İçin **kaynak grubu**, var olan seçin veya yeni bir grup oluşturun. Daha fazla bilgi edinmek için bkz. [Azure kaynak grupları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/).

    4.  Hizmetiniz için bir **Konum** sağlayın. Bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services) hangi hizmetlerin kullanılabilir hangi bölgede daha fazla bilgi için. Genel olarak, seçin bir **konumu** , Cihazınızı dağıtmak istediğiniz coğrafi bölgeye yakın. Aşağıdakilerin de etkili olmasını isteyebilirsiniz:

        -   Var olan iş yükleri de dağıtmak için StorSimple cihazınızla düşündüğünüz Azure varsa, o veri merkezini kullanmanız önerilir.

        -   StorSimple Aygıt Yöneticisi'ni ve Azure depolama alanınızı iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir. Azure Depolama hesabı oluşturmak için Azure portalındaki Azure Depolama hizmetine gidin ve [Azure Depolama hesabı oluşturma](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) konusundaki adımları uygulayın. Bu hesabı oluşturduktan sonra, [Hizmet için yeni bir depolama hesabı yapılandırma](https://azure.microsoft.com/en-us/documentation/articles/storsimple-deployment-walkthrough/#configure-a-new-storage-account-for-the-service) konusundaki adımları uygulayarak bunu StorSimple Cihaz Yöneticisi hizmetine ekleyin.

        -   StorSimple cihaz Yöneticisi hizmeti kamu portalı sanal cihazı dağıtma, ABD Iowa ve ABD Virginia konumlarda kullanılabilir.

    5.  Seçin **yeni bir Azure depolama hesabı oluşturma** hizmetiyle otomatik olarak bir depolama hesabı oluşturmak için. Belirtin bir **depolama hesabı adı**. Verilerinizin farklı bir konumda olması gerekiyorsa bu kutunun işaretini kaldırın.

    6.  Panonuzda bu hizmetin hızlı bağlantısının olmasını istiyorsanız **Panoya sabitle** seçeneğini işaretleyin.

    7.  StorSimple Cihaz Yöneticisi’ni oluşturmak için **Oluştur**’a tıklayın.

        ![Yeni bir hizmet oluşturun](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Yönlendirilirsiniz **hizmet** giriş sayfası. Hizmetin oluşturulması birkaç dakika sürer. Hizmet sorunsuz oluşturulduktan sonra, uygun şekilde size bildirilir ve hizmetin durumu **Etkin** olarak değişir.


