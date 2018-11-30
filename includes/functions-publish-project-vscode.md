

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Visual Studio Code, işlevler projenizi doğrudan Azure’da dağıtmanıza olanak sağlar. Süreç kapsamında, Azure abonelik bir işlev uygulaması ve ilgili kaynakları oluşturursunuz. İşlev uygulaması, işlevlerinize ilişkin bir yürütme bağlamı sağlar. Proje, Azure aboneliğinizdeki yeni işlev uygulamasında paketlenir ve dağıtılır. 

Bu makalede, yeni bir işlev uygulaması oluşturduğunuz varsayılır. Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.

1. **Azure: İşlevler** alanında İşlev Uygulamasına Dağıt simgesini seçin.

    ![İşlev uygulaması ayarları](./media/functions-publish-project-vscode/function-app-publish-project.png)

1. Geçerli çalışma alanınız olan proje klasörünü seçin.

1. Birden fazla aboneliğiniz varsa işlev uygulamanızı barındırmak istediğiniz seçeneği belirleyin ve sonra **+ Yeni İşlev Uygulaması Oluştur**’u seçin.

1. İşlev uygulamanızı tanımlayan bir genelde benzersiz olan bir ad yazın ve Enter tuşuna basın. İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir.

1. **+ Yeni Kaynak Grubu Oluştur**’u seçin, `myResourceGroup` gibi bir kaynak grubu adı yazın ve Enter tuşuna basın. Ayrıca var olan bir kaynak grubunu kullanabilirsiniz.

1. Seçin **+ oluştur yeni depolama hesabı**, yeni depolama hesabı genel olarak benzersiz bir ad, Enter tuşuna basın ve işlev uygulaması tarafından kullanılan tür. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Var olan bir hesabı da kullanabilirsiniz.

1. Ayrıca, kendinize veya işlevlerinizin erişeceği diğer hizmetlere yakın bir [bölgede](https://azure.microsoft.com/regions/) yer alan bir konum seçin.

    İşlev uygulamasının oluşturulması, konumunuzu seçtikten sonra başlar. İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır.

1. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtma sonuçlarını görüntülemek üzere bildirimlerde **Çıktıyı Görüntüle**’yi seçin.

    ![İşlev uygulaması oluşturma çıktısı](./media/functions-publish-project-vscode/function-create-notifications.png)

1. Azure’daki yeni işlev uygulamasının URL’sini not edin. Bunu, proje Azure’da yayımlandıktan sonra işlevinizi test etmek için kullanırsınız.

    ![İşlev uygulaması oluşturma çıktısı](./media/functions-publish-project-vscode/function-create-output.png)

1. **Azure: İşlevler** alanında, yeni işlev uygulamasının aboneliğinizin altında görüntülendiğini görürsünüz. Bu düğümü genişlettiğinizde işlev uygulamasındaki işlevlerle birlikte uygulama ayarları ve işlev proxy’lerini görürsünüz.

    ![İşlev uygulaması ayarları](./media/functions-publish-project-vscode/function-app-project-settings.png)

    İşlev uygulaması düğümünde, Azure’daki işlev uygulamasına yönelik çeşitli yönetim ve yapılandırma görevlerini gerçekleştirmek için Ctrl tuşuna basarak tıklayın (sağ tıklama). Ayrıca işlev uygulamasını Azure portalında görüntülemeyi seçebilirsiniz.
