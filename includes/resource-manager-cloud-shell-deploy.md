## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

Şablonunuzu dağıtmak için [Cloud Shell](../articles/cloud-shell/overview.md) kullanabilirsiniz. Ancak, ilk şablonunuzu storage hesabınıza bulut Kabuğunuzu yüklemeniz gerekir. Daha önce Cloud Shell kullanmadıysanız, kurulumu hakkında bilgi için bkz. [Azure Cloud Shell’e Genel Bakış](../articles/cloud-shell/overview.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Cloud Shell kaynak grubunuzu seçin. Ad deseni `cloud-shell-storage-<region>` şeklindedir.

   ![Kaynak grubu seçin](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Cloud Shell için depolama hesabınızı seçin.

   ![Depolama hesabı seçme](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Seçin **BLOB'lar**.

   ![BLOB'ları seçin](./media/resource-manager-cloud-shell-deploy/select-blobs.png)

1. Seçin **+ kapsayıcı**.

   ![Kapsayıcı ekleme](./media/resource-manager-cloud-shell-deploy/add-container.png)

1. Kapsayıcı bir ad ve bir erişim düzeyi verilir. Bu makaledeki örnek şablonu hiçbir hassas bilgiler içerir, böylece anonim okuma erişimine izin verir. **Tamam**’ı seçin.

   ![Kapsayıcı değerleri sağlayın](./media/resource-manager-cloud-shell-deploy/provide-container-values.png)

1. Oluşturduğunuz kapsayıcısı seçin.

   ![Yeni kapsayıcı seçin](./media/resource-manager-cloud-shell-deploy/select-container.png)

1. **Karşıya Yükle**’yi seçin.

   ![BLOB karşıya yükleme](./media/resource-manager-cloud-shell-deploy/upload-blob.png)

1. Şablonunuzu bulup karşıya yükleyin.

   ![Dosya yükleme](./media/resource-manager-cloud-shell-deploy/find-and-upload-template.png)

1. Karşıya sonra şablonu seçin.

   ![Yeni şablonu seçin](./media/resource-manager-cloud-shell-deploy/select-new-template.png)

1. URL'yi kopyalayın.

   ![URL'yi kopyalayın](./media/resource-manager-cloud-shell-deploy/copy-url.png)

1. İstemi açın.

   ![Cloud Shell’i açma](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
