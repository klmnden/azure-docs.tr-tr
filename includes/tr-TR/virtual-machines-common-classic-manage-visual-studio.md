Visual Studio'da Sunucu Gezgini kullanarak Azure'da sanal makineler oluşturabilirsiniz.

## <a name="create-an-azure-virtual-machine-in-server-explorer"></a>Sunucu Gezgini'nde bir Azure sanal makine oluşturma
Bir sanal makine oluştururken [Azure Yönetim Portalı](http://go.microsoft.com/fwlink/?LinkID=253103), sunucu Gezgini'nde komutları kullanarak Azure'da bir sanal makine oluşturabilirsiniz. Sanal makineler, örneğin, bir ön uç ortak yük dengeli genel bir uç nokta arkasında sağlamak için kullanılabilir.

### <a name="to-create-a-new-virtual-machine"></a>Yeni bir sanal makine oluşturmak için
1. Server Explorer'da açın **Azure** düğümü ve tıklatın **sanal makineleri**.
2. Bağlam menüsünde **sanal makine oluşturma**.
   
    **Yeni bir sanal makine oluşturma** Sihirbazı görünür.
   
    ![Sanal makine Oluştur komutu](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718342.png)
3. Üzerinde **bir abonelik seçin** sayfasında, sanal makine oluşturulurken kullanılacak aboneliği seçin ve ardından **sonraki**.
   
    Azure'da oturum değil,'ı tıklatın **oturum** oturum açmak için. Ardından, henüz seçili değilse, Azure aboneliğinizin açılır liste kutusunda seçin.
4. Üzerinde **bir sanal makine görüntüsü seçin** sayfasında, bir resim türünü seçin **görüntü türü** açılır liste kutusu ve bir sanal makine görüntülerini seçip **görüntü adı** liste kutusu . İşiniz bittiğinde tıklatın **sonraki**.
   
    ![Bir sanal makine görüntüsü seçin sayfası](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744137.png)
   
    Şu resim türleri seçebilirsiniz.
   
   * **Ortak görüntüleri** sanal makine görüntüleri işletim sistemleri ve Windows Server ve SQL Server gibi sunucu yazılımları listeler.
   * **MSDN görüntülerine** sanal makine görüntüleri Visual Studio ve Microsoft Dynamics gibi MSDN aboneleri için kullanılabilir yazılım listeler.
   * **Özel resimler** listeleri özelleştirilmiş ve oluşturduğunuz sanal makine görüntülerini genelleştirilmiş.
     
     Özelleştirilmiş ve genelleştirilmiş sanal makineler hakkında bilgi edinmek için [VM görüntüsü](https://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/). Bkz: [Windows sanal makinesi şablon olarak kullanmak için yakalama](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) bir sanal makineyi hızlı bir şekilde yeni oluşturmak için kullanabileceğiniz bir şablona devre dışı bırakma hakkında bilgi sanal makineleri önceden yapılandırılmış için.
     
     Sayfanın sağ tarafında görüntü hakkındaki bilgileri görmek için bir sanal makine görüntü adı tıklatabilirsiniz.
     
     > [!NOTE]
     > Sanal makine görüntülerini ekleyemezsiniz **ortak görüntüleri** veya **MSDN görüntülerine** salt okunur olduğundan listeler. Oluşturduğunuz tüm sanal makineler için eklenen **özel görüntüleri** listesi.
     > 
     > 
     
     Bir Visual Studio düzeyi abonelikle bir MSDN abone değilseniz, Visual Studio gibi çeşitli diğer görüntüleri içeren bir önceden oluşturulmuş Azure sanal makine oluşturabilirsiniz. Daha fazla bilgi için bkz: [bir sanal makine Visual Studio kullanarak görüntüleri Visual Studio 2013 galeri görüntüsü tarafından MSDN aboneleri için oluşturmak](http://visualstudio2013msdngalleryimage.azurewebsites.net) ve [MSDN Abonelikleri](https://www.visualstudio.com/products/msdn-subscriptions-vs). |
5. Üzerinde **sanal makine temel ayarları** sayfasında, bir makine adı girin ve ardından boyutu ve bir kullanıcı adı ve parola gibi sanal makinenin özelliklerini ekleyin. İşiniz bittiğinde tıklatın **sonraki**.
   
    Unutmanız durumunda bunları not almanız iyi bir fikirdir Uzak Masaüstü'nü kullanarak makinede oturum için yeni bir ad ve parolayı kullanacaksınız. Visual Studio'da bir Azure sanal makine oluşturduktan sonra boyutuna ve diğer ayarları değiştirebilirsiniz [Azure Yönetim Portalı](http://go.microsoft.com/fwlink/?LinkID=253103).
   
   > [!NOTE]
   > Sanal makine için daha büyük boyutları seçerseniz, ek ücretleri uygulanabilir. Bkz: [sanal makineler fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/virtual-machines/) daha fazla bilgi için.
   > 
   > 
6. Visual Studio'da oluşturulan sanal makineler bir bulut hizmeti gerektirir. Üzerinde **bulut hizmeti ayarlarını** sayfasında, sanal makine için bir bulut hizmeti seçin veya tıklatın **< Yeni Oluştur... >** aşağı açılan listesine bir bulut hizmeti veya yeni bir tane kullanmak istediğiniz zaten yoksa. Bir depolama hesabı de gereklidir, böylece bir depolama hesabı seçin (veya yeni bir depolama hesabı oluşturun) içinde **depolama hesabı** açılır liste kutusu. Bkz: [Microsoft Azure Storage'a giriş](../articles/storage/common/storage-introduction.md) daha fazla bilgi için.
7. (İsteğe bağlı olan) bir sanal ağ belirtmek istiyorsanız, sanal ağ ve alt açılır liste kutularını seçin.
   
    Bir kullanılabilirlik kümesi üyesi olan sanal makineleri farklı hata etki alanları için dağıtılır. Bkz: [Azure Virtual Network](https://azure.microsoft.com/services/virtual-network/) daha fazla bilgi için.
8. Sanal makinenize bir kullanılabilirlik kümesi (Ayrıca isteğe bağlı) ait olmasını istiyorsanız seçin **belirtin bir kullanılabilirlik kümesi** onay kutusunu işaretleyin ve ardından kullanılabilirlik açılır liste kutusunda kümesi seçin. İşiniz bittiğinde seçin **sonraki** düğmesi.
   
    Bir kullanılabilirlik kümesi, sanal makine eklemek, ağ hataları, yerel disk donanım hataları ve planlanan kapalı kalma süresi sırasında kullanılabilir kalmasını uygulamanızı yardımcı olur. Kullanmanız gereken [Azure Yönetim Portalı](http://go.microsoft.com/fwlink/?LinkID=253103) sanal ağlar, alt ağlar ve kullanılabilirlik oluşturmak için ayarlar. Bkz: [sanal makinelerin kullanılabilirliğini yönetme](https://azure.microsoft.com/documentation/articles/manage-availability-virtual-machines/) daha fazla bilgi için.
9. Üzerinde **uç noktaları** sayfasında, sanal makineniz kullanıcıları için kullanılabilir olmasını istediğiniz ortak uç noktalarını belirtin. Örneğin, Uzak Masaüstü'nü ve varsayılan olarak etkin olan PowerShell uç noktaları ek olarak HTTP (bağlantı noktası 80) etkinleştirmeyi seçebilirsiniz. Bir uç noktası eklemek için birinde seçin **bağlantı noktası adı** açılan liste kutusunu ve ardından **Ekle** düğmesi. Bir uç nokta kaldırmak için kırmızı seçin **X** uç noktalar listesinde adının yanındaki.
   
    ![Sanal makineler sihirbazındaki uç noktaları sayfası.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718351.png)
   
    Kullanılabilir uç noktaları, sanal makine için seçtiğiniz bulut hizmeti bağlıdır. Bkz: [Azure hizmet uç noktaları](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) daha fazla bilgi için.
   
   > [!NOTE]
   > Ortak uç noktaları Etkinleştirme Hizmetleri, sanal makinenizde Internet'e kullanılabilmesini sağlar. Uç noktaları için ayarı erişim denetim listeleri (ACL'ler) gibi yüklemek ve uç noktaları ve hizmetler, sanal makinede düzgün bir şekilde yapılandırmak emin olun. Bkz: [ayarlamak yukarı uç noktaları için bir sanal makine nasıl](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/) daha fazla bilgi için.
   > 
   > 
10. Bitirdikten sonra sanal makine ayarlarını yapılandırma, seçin **oluşturma** düğmesi sanal makine oluşturulamıyor.
    
     Azure sanal makine olarak **Azure etkinlik günlüğü** sanal makine oluşturma işlemi ilerlemesini gösterir.
    
     ![Sanal makine etkinlik günlüğü - sürüyor.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744138.png)
    
     Yalnızca sanal makine bilgilerini görüntülemek için seçin **sanal makineleri** sekmesinde **Azure etkinlik günlüğü**.
    
     ![Sanal makine etkinlik günlüğü - tamamlandı.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744139.png)
    
     İşlem başarıyla tamamlanırsa altında yeni bir sanal makine görünür **sanal makineleri** Sunucu Gezgininde. İçine tıklayarak oturum **Uzak Masaüstü kullanarak bağlanmak** kısayol.
    
     ![Server Explorer'da görünen sanal makine.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744140.png)

## <a name="manage-your-virtual-machines"></a>Sanal makinelerinizi yönetme
Sanal makine yapılandırma sayfasında, kapatma, bağlanma, yenileme ve seçilen sanal makine denetim noktaları eklemeyi yanı sıra görüntüleyebilir veya sanal makine ayarlarını değiştirin. Şunları yapabilirsiniz:

* Sanal makine boyutunu değiştirin.
* Sanal makine ile kullanmak üzere ayarlanmış kullanılabilirlik seçin.
* Ekle, Kaldır veya ortak uç noktaları için ayarları değiştirin.
* Ekleyin, kaldırın veya sanal makine uzantıları yapılandırın.
* Sanal makine ile ilişkili disklerle ilgili bilgileri görüntüleyin.

### <a name="view-or-change-virtual-machine-settings"></a>Sanal makine ayarlarını görüntülemek veya değiştirmek
1. Sunucu Gezgini'nde, sanal makinenizde seçin **Azure sanal makineleri** düğümü.
2. Kısayol menüsünden seçin **yapılandırma** sanal makine yapılandırma sayfasını görüntülemek için.
   
    ![Azure sanal makine yapılandırma sayfası](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744141.png)
3. Sanal makine bilgilerini görüntüleyin veya değiştirin.

### <a name="save-or-restore-the-status-of-your-virtual-machine"></a>Kaydedin veya sanal makine durumunu geri yükle
Sanal makinenizi yapılandırmanız ve yazılım üzerinde yükleme gibi sanal makine kontrol noktaları oluşturarak ilerleme durumunuzu düzenli olarak kaydetmek için iyi bir fikirdir. Bir denetim noktası bir anlık görüntüsü veya görüntüsü, sanal makinenin geçerli durumunu ' dir. Bir sanal makineyle yanlış giden veya sanal makine yeniden yapılandırmak isterseniz, üzerinde sıfırdan yerine önceki bir kontrol noktası durumuna geri zaman kazanabilirsiniz.

### <a name="to-create-a-virtual-machine-checkpoint"></a>Bir sanal makine kontrol noktası oluşturmak için
1. Sunucu Gezgini'nde, sanal makinenizde seçin **Azure sanal makineleri** düğümü.
2. Kısayol menüsünden seçin **yapılandırma** sanal makine yapılandırma sayfasını görüntülemek için.
3. Yapılandırma sayfasında, **görüntü yakalama** düğmesi.
   
    ![Azure yapılandırma sayfası yakalama düğmesi](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744142.png)
   
    **Sanal makine yakalama** iletişim kutusu görüntülenir.
   
    ![Azure yakalama sanal makine iletişim kutusu](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744143.png)
4. Bir görüntü etiketi ve açıklama girin. Bir varsayılan etiket ve açıklama sağlanır, ancak bunların kendi isterseniz üzerine yazabilirsiniz.
5. Bu sanal makinede Sysprep zaten çalıştırdıysanız seçin **sanal makinede Sysprep'i çalıştırdım** kutusu.
   
    Sysprep, başka şeylerin sistemleri özgü verileri sanal makinenin diğer kullanabileceğiniz şablonu kolaylaştırarak Windows sürümünden kaldıran bir araçtır. Bkz: [Windows sanal makinesi şablon olarak kullanmak için yakalama](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/) daha fazla bilgi için. VM yedeklemesi, Sysprep çalıştırılmadan önce yedekleyin.
6. Bitirdikten sonra yakalama ayarlarını yapılandırma, seçin **yakalama** kontrol noktası oluşturmak için düğmesi.
   
    Azure denetim noktası olarak **Azure etkinlik günlüğü** işlemin ilerlemesini gösterir.
   
    ![Bir sanal makine denetim noktası yakalama](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744144.png)
   
    Denetim noktası işlemi tamamlandığında içinde görürsünüz **Azure etkinlik günlüğü**.
   
    ![Denetim noktası işlemi tamamlandı](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744145.png)

## <a name="to-manage-virtual-machine-checkpoints"></a>Sanal makine kontrol noktaları yönetmek için
### <a name="to-restore-a-virtual-machine-to-a-previously-saved-state"></a>Bir sanal makine daha önce kaydedilen bir duruma geri yüklemek için
* Özetlenen adımları izleyin [adım adım: gerçekleştirmek bulut geri yükler, Microsoft Azure PowerShell - 2. parça kullanarak sanal makineleri](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx).

### <a name="to-delete-a-checkpoint"></a>Bir kontrol noktasını silmek için
1. Git [Azure Yönetim Portalı](http://go.microsoft.com/fwlink/?LinkID=253103).
2. Sanal makinenin yapılandırma sayfasında, **görüntüleri** sayfanın üst kısmındaki sekme.
3. Silin ve ardından istediğiniz kontrol noktasını seçin **silmek** altındaki sayfasının düğmesini.

## <a name="shut-down-your-virtual-machine"></a>Sanal makineyi Kapat
1. Sunucu Gezgini'nde, kapatmak istediğiniz sanal makineyi seçin **Azure sanal makineleri** düğümü.
2. Kısayol menüsünden seçin **kapatma** komutu ya da seçin **yapılandırma** sanal makine yapılandırma sayfasını görüntüleyin ve seçin **kapatma** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineler oluşturma hakkında daha fazla bilgi için bkz: [çalıştıran bir sanal makine Linux oluşturma](../articles/virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ve [Azure Önizleme Portalı'nda Windows çalıştıran bir sanal makine oluşturma](../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

