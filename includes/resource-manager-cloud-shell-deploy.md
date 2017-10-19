## <a name="deploy-template-from-cloud-shell"></a>Cloud Shell'den şablon dağıtma

Şablonunuzu dağıtmak için [Cloud Shell](../articles/cloud-shell/overview.md) kullanabilirsiniz. Ancak, ilk olarak şablonunuzu Cloud Shell dosya paylaşımına yüklemeniz gerekir. Daha önce Cloud Shell kullanmadıysanız, kurulumu hakkında bilgi için bkz. [Azure Cloud Shell’e Genel Bakış](../articles/cloud-shell/overview.md).

1. [Azure Portal](https://portal.azure.com)’da oturum açın.

1. Cloud Shell kaynak grubunuzu seçin. Ad deseni `cloud-shell-storage-<region>` şeklindedir.

   ![Kaynak grubu seçin](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Cloud Shell için depolama hesabınızı seçin.

   ![Depolama hesabı seçme](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. **Dosyalar**’ı seçin.

   ![Dosya seçme](./media/resource-manager-cloud-shell-deploy/select-files.png)

1. Cloud Shell için dosya paylaşımı seçin. Ad deseni `cs-<user>-<domain>-com-<uniqueGuid>` şeklindedir.

   ![Dosya paylaşımı seçme](./media/resource-manager-cloud-shell-deploy/select-file-share.png)

1. **Dizin ekle**’yi seçin.

   ![Dizin ekleme](./media/resource-manager-cloud-shell-deploy/select-add-directory.png)

1. **Şablonlar** olarak adlandırıp **Tamam**’ı seçin.

   ![Ad dizini](./media/resource-manager-cloud-shell-deploy/name-templates.png)

1. Yeni dizininizi seçin.

   ![Dizin seçme](./media/resource-manager-cloud-shell-deploy/select-templates.png)

1. **Karşıya Yükle**’yi seçin.

   ![Karşıya yükleme seçme](./media/resource-manager-cloud-shell-deploy/select-upload.png)

1. Şablonunuzu bulup karşıya yükleyin.

   ![Dosya yükleme](./media/resource-manager-cloud-shell-deploy/upload-files.png)

1. İstemi açın.

   ![Cloud Shell’i açma](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
