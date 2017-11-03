


## <a name="tagging-a-virtual-machine-through-templates"></a>Bir sanal makine şablonlarıyla etiketleme
İlk olarak, şablonları üzerinden etiketleme konumundaki bakalım. [Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) aşağıdaki kaynaklar etiketleri yerleştirir: işlem (sanal makine), depolama (depolama hesabı) ve ağ (genel IP adresi, sanal ağ ve ağ arabirimi). Bu şablon için bir Windows VM olmakla birlikte Linux VM'ler için uyarlanabilir.

Tıklatın **Azure'a Dağıt** gelen düğmesini [şablon bağlantısı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Bu gideceği [Azure portal](https://portal.azure.com/) bu şablonu dağıtabileceğiniz.

![Etiketler basit dağıtımı](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Bu şablon aşağıdaki etiketlerini içerir: *departmanı*, *uygulama*, ve *oluşturan*. Ekle/doğrudan şablonunda bu etiketler farklı etiket adları isterseniz Düzenle.

![Bir şablonu Azure etiketleri](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Gördüğünüz gibi etiketler iki nokta (:) ayırarak anahtar/değer çiftleri olarak tanımlanır. Etiketler bu biçimde tanımlanmış olması gerekir:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Tercih ettiğiniz etiketleriyle düzenlemeyi tamamladıktan sonra şablon dosyasını kaydedin.

İleri ' **parametreleri Düzenle** bölümünde etiketlerinizi için değerlerin doldurabilirsiniz.

![Azure portalında etiket düzenleme](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Tıklatın **oluşturma** etiketi değerleriniz ile bu şablonu dağıtmak için.

## <a name="tagging-through-the-portal"></a>Portalı aracılığıyla etiketleme
Kaynaklarınızın etiketleriyle oluşturduktan sonra görüntülemek, ekleyin ve Portalı'nda etiketleri silin.

Etiketler simge etiketleri görüntülemek için seçin:

![Azure portalında etiketleri simgesi](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Kendi anahtar/değer çifti tanımlayarak Portalı aracılığıyla yeni bir etiket eklemek ve kaydedin.

![Azure portalında Yeni Etiket Ekle](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Yeni etiket şimdi kaynağınız için etiketler listesinde gösterilmelidir.

![Azure portalında kaydedilen yeni etiket](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

