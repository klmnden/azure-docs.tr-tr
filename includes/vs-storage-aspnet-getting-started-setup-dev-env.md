## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölümde, bir ASP.NET MVC uygulaması oluşturma, bağlantılı hizmetler bağlantısı ekleme, bir denetleyicisi ekleme ve gerekli ad alanı yönergeleri belirtme dahil olmak üzere, geliştirme ortamını ayarlama açıklanmaktadır.

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

1. Üzerinde **bağlı hizmet Ekle** iletişim kutusunda **Azure Storage**ve ardından **yapılandırma**.

    ![Bağlı hizmet iletişim kutusu](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Üzerinde **Azure Storage** iletişim kutusunda, istediğiniz çalışma ve seçmek istenen Azure depolama hesabı seçin **Ekle**.
