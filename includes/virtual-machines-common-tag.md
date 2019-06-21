---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: ccc2b574ea054a1b0ecf32a1e59691050fb66fcf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188311"
---
## <a name="tagging-a-virtual-machine-through-templates"></a>Bir sanal makine şablonları aracılığıyla etiketleme
İlk olarak, şablonlar etiketleme sırasında bakalım. [Bu şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) Aşağıdaki kaynaklarda etiketleri yerleştirir: İşlem (sanal makine), depolama (depolama hesabı) ve ağ (genel IP adresi, sanal ağ ve ağ arabirimi). Bu şablon için bir Windows VM olmakla birlikte Linux VM'ler için uyarlanabilir.

Tıklayın **azure'a Dağıt** düğmesini [şablon bağlantısı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Bu gideceği [Azure portalında](https://portal.azure.com/) Bu şablon dağıtabileceğiniz.

![Basit Dağıtım etiketlerle](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Bu şablon, aşağıdaki etiketleri içerir: *Departman*, *uygulama*, ve *tarafından oluşturulan*. Ekleme/doğrudan şablonunda bu etiketleri farklı bir etiket adları isterseniz düzenleyebilir.

![Bir şablonda Azure etiketleri](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Gördüğünüz gibi etiketleri bir iki nokta (:) ayırarak, anahtar/değer çiftleri olarak tanımlanır. Etiketleri şu biçimde tanımlanmış olması gerekir:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Tercih ettiğiniz etiketlerle düzenleme işlemini tamamladıktan sonra şablon dosyasını kaydedin.

Ardından **parametreleri Düzenle** bölümünde, etiketlerinizi için değerleri doldurabilirsiniz.

![Azure portalında etiketleri Düzenle](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Tıklayın **Oluştur** etiketi değerlerinizi içeren bu şablonu dağıtabilirsiniz.

## <a name="tagging-through-the-portal"></a>Portal üzerinden etiketleme
Kaynaklarınızı etiketlerle oluşturduktan sonra görüntüleyin, ekleyin ve Portalı'nda etiketleri silin.

Etiketleri görüntülemek için etiketleri simgesini seçin:

![Azure portalında etiketleri simgesi](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Kendi anahtar/değer çifti tanımlayarak portalı üzerinden yeni bir etiket ekleyin ve kaydedin.

![Azure portalında Yeni Etiket Ekle](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Yeni etiket alanınızda artık kaynağınızın etiketler listesinde görünmelidir.

![Azure portalında kaydettiğiniz yeni etiket](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

