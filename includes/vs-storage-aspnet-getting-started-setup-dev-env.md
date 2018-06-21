## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölümde, geliştirme ortamını ayarlama aracılığıyla anlatılmaktadır. Bu, bir ASP.NET MVC uygulaması oluşturma, bağlı hizmetler bağlantısı ekleme, bir denetleyicisi ekleme ve gerekli ad alanı yönergeleri belirtmeyi içerir.

### <a name="create-an-aspnet-mvc-app-project"></a>Bir ASP.NET MVC uygulaması projesi oluşturma

1. Visual Studio'yu açın.

1. Ana menüden seçin **dosya** > **yeni** > **proje**.

1. İçinde **yeni proje** iletişim kutusunda **Web** > **ASP.NET Web uygulaması (.NET Framework)**. İçinde **adı** alanında, belirtin **StorageAspNet**. **Tamam**’ı seçin.

    ![Yeni Proje penceresinin Ekran iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. İçinde **yeni ASP.NET Web uygulaması** iletişim kutusunda **MVC**ve ardından **Tamam**.

    ![Ekran, yeni bir ASP.NET Web uygulaması iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Bir Azure depolama hesabına bağlanmak üzere bağlı hizmetler kullanın

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın.

2. Bağlam menüsünden seçin **Ekle** > **bağlı hizmet**.

1. İçinde **bağlantılı Hizmetler** iletişim kutusunda **Azure Storage ile bulut depolama**.

    ![Bağlantılı Hizmetler ekran iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. İçinde **Azure Storage** Azure depolama hesabı Bu öğretici için kullanılacak iletişim kutusunda, seçin. Yeni bir Azure depolama hesabı oluşturmak için seçin **yeni depolama hesabı oluşturma**ve formu doldurun. Ya da mevcut bir depolama hesabını seçmek veya yeni bir tane oluşturduktan sonra Seç **Ekle**. Visual Studio Azure Storage ve bir depolama bağlantı dizesi için NuGet paketi yükler **Web.config**.

> [!TIP]
> İle depolama hesabı oluşturma konusunda bilgi almak için [Azure portal](https://portal.azure.com), bkz: [depolama hesabı oluşturma](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Kullanarak bir depolama hesabı oluşturabilirsiniz [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md), veya [Azure bulut Kabuk](../articles/cloud-shell/overview.md).

