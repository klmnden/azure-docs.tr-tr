## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölümde, bir ASP.NET MVC uygulaması oluşturma, bağlantılı hizmetler bağlantısı ekleme, bir denetleyicisi ekleme ve gerekli ad alanı yönergeleri belirtme gibi geliştirme ortamını ayarlama aracılığıyla anlatılmaktadır.

### <a name="create-an-aspnet-mvc-app-project"></a>Bir ASP.NET MVC uygulaması projesi oluşturma

1. Visual Studio'yu açın.

1. Seçin **Dosya -> Yeni Proje ->** ana menüden

1. Üzerinde **yeni proje** iletişim kutusunda, aşağıdaki şekilde vurgulandığı gibi seçenekleri belirtin:

    ![ASP.NET projesi oluşturma](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. **Tamam**’ı seçin.

1. Üzerinde **yeni ASP.NET projesi** iletişim kutusunda, aşağıdaki şekilde vurgulandığı gibi seçenekleri belirtin:

    ![MVC belirtin](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. **Tamam**’ı seçin.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Bir Azure depolama hesabına bağlanmak üzere bağlantılı hizmetler kullanın

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **Ekle -> Hizmet bağlı**.

1. Üzerinde **bağlı hizmet Ekle** iletişim kutusunda **Azure Storage ile bulut depolama**ve ardından **yapılandırma**.

    ![Bağlı hizmet iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Üzerinde **Azure Storage** Azure depolama hesabı Bu öğretici için kullanılacak iletişim kutusunu seçin.  Yeni bir Azure depolama hesabı oluşturmak için tıklatın **yeni depolama hesabı oluşturma** ve formu doldurun.  Ya da mevcut bir depolama hesabını seçmek veya yeni bir tane oluşturduktan sonra tıklatın **Ekle**.  Visual Studio, Azure Storage ve bir depolama bağlantı dizesi için NuGet paketi yüklenir **Web.config**.

> [!TIP]
> İle depolama hesabı oluşturma konusunda bilgi almak için [Azure portal](https://portal.azure.com), bkz: [depolama hesabı oluşturma](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Bir Azure depolama hesabı kullanılarak da oluşturulabilir [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md), veya [Azure bulut Kabuk](../articles/cloud-shell/overview.md).

