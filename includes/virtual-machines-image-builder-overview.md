---
author: cynthn
ms.author: cynthn
ms.date: 04/30/2019
ms.topic: include
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: 6eedc095f155a77cddf48211dbc4a677bf188112
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509929"
---
Standart sanal makine (VM) görüntülerini kuruluşların buluta taşımayı ve dağıtımlarında tutarlılığı sağlar. Görüntüler genellikle önceden tanımlı güvenlik ve yapılandırma ayarlarını ve gerekli yazılımı da içerir. Kendi görüntü işlem hattı ayarlama süresi, altyapı ve Kurulum gerektirir ancak Azure VM Görüntü Oluşturucu ile yalnızca görüntünüzü açıklayan basit bir yapılandırma sağlayın, hizmete gönderme ve görüntü oluşturulan dağıtılmış ve.
 
Azure VM Görüntü Oluşturucu (Azure Görüntü Oluşturucu), bir Windows veya Linux tabanlı Azure Market görüntüsü, var olan özel görüntüler veya Red Hat Enterprise Linux (RHEL) ISO ile ve kendi özelleştirmelerinizi eklemek başlatabilirsiniz olanak tanır. Görüntü Oluşturucusu oluşturulduğundan [HashiCorp Packer](https://packer.io/), ayrıca mevcut Packer Kabuk sağlayıcısı betiklerinizi içeri aktarabilirsiniz. Görüntülerinizin barındırılan, buna istediğiniz belirtebilirsiniz [Azure paylaşılan görüntü Galerisi](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries), yönetilen bir görüntü veya VHD olarak.

> [!IMPORTANT]
> Azure Görüntü Oluşturucu şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="preview-features"></a>Önizleme özellikleri

Önizleme için bu özellikler desteklenir:

- En düşük güvenlik ve kurumsal yapılandırmaları içerir ve kullanıcıların ihtiyaçlarına daha fazla özelleştirmek bölümler oluşturma altın temel görüntü.
- Var olan görüntülerden düzeltme eki uygulama, Görüntü Oluşturucu, sürekli olarak mevcut özel görüntüleri düzeltme eki olanak tanıyacaktır.
- Azure paylaşılan görüntü Galerisi ile tümleştirme, sürüm, dağıtılacak sağlar ve ölçek, küresel olarak görüntüler ve bir görüntü yönetimi sistemi sağlar.
- Varolan bir görüntü ile tümleştirme işlem hatları oluşturun, yalnızca Görüntü Oluşturucu, işlem hattından çağırma veya basit Önizleme Görüntü Oluşturucu Azure DevOps görevi kullanın.
- Varolan bir görüntü özelleştirme ardışık düzeni Azure'a geçirin. Var olan bir betik, komutları ve işlemleri görüntülerini özelleştirmek için kullanın.
- Red Hat Getir bilgisayarınızı kendi aboneliklerini desteğini kullanın. Red Hat Enterprise görüntüleri kullanmak için uygun, kullanılmayan Red Hat aboneliklerinizi ile oluşturun.
- VHD biçiminde görüntüleri oluşturma.
 

## <a name="regions"></a>Regions
Azure Görüntü Oluşturucu hizmeti bu bölgelerde Önizleme için kullanıma sunulacaktır. Bu bölgeler dışında görüntüleri dağıtılabilir.
- East US
- Doğu ABD 2
- Batı Orta ABD
- Batı ABD
- Batı ABD 2

## <a name="os-support"></a>İşletim sistemi desteği
AIB Azure Marketi'nde temel işletim sistemi görüntüleri destekler:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7.6
- CentOS 7.6
- Windows 2016
- Windows 2019

AIB RHEL ISO'YU'ın destekleyeceği için bir kaynak olarak:
- RHEL 7.3
- RHEL 7.4
- RHEL 7.5

RHEL 7.6, test edilen ancak desteklenmiyor.

## <a name="how-it-works"></a>Nasıl çalışır?


![Azure Görüntü Oluşturucu kavramsal çizimi](./media/virtual-machines-image-builder-overview/image-builder.png)

Azure Görüntü Oluşturucu, bir Azure kaynak sağlayıcısı tarafından erişilebilen tam olarak yönetilen bir Azure hizmetidir. Azure Görüntü Oluşturucu işlemi üç ana bölümden oluşur: kaynak, özelleştirme ve dağıtma, bu şablonda temsil edilir. Aşağıdaki diyagramda, bazı özelliklerini bileşenleri gösterilmektedir. 
 


**Görüntü Oluşturucu işlemi** 

![Azure Görüntü Oluşturucu işlemi kavramsal çizimi](./media/virtual-machines-image-builder-overview/image-builder-process.png)

1. Görüntüyü şablon bir .json dosyası oluşturun. Bu bir .json dosyası görüntü kaynağı, özelleştirmeler ve dağıtım hakkında bilgi içerir. Birden çok örnekler vardır [Azure Görüntü Oluşturucu GitHub deposu](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).
1. Hizmete gönderme, bu belirttiğiniz kaynak grubunda bir görüntü şablon yapıt oluşturur. Arka planda Görüntü Oluşturucu kaynak görüntüsü veya ISO ve gerektiğinde betikleri indirir. Bu biçimde aboneliğinizi otomatik olarak oluşturulan ayrı bir kaynak grubu depolanır: IT_\<DestinationResourceGroup>_\<TemplateName>. 
1. Görüntü şablonu oluşturulduktan sonra görüntü oluşturabilirsiniz. Arka planda görüntü Oluşturucusu bir VM, ağ ve depolama içinde IT_ oluşturmak için şablon ve kaynak dosyalarını kullanır\<DestinationResourceGroup > _\<TemplateName > kaynak grubu.
1. Görüntü oluşturmanın bir parçası, görüntünün Görüntü Oluşturucu dağıtır IT_ ek kaynakları ardından siler şablon göre\<DestinationResourceGroup > _\<TemplateName > için oluşturulan kaynak grubu işlemi.


## <a name="permissions"></a>İzinler

Yönetilen görüntülerine veya paylaşılan görüntü Galerisine görüntülerini dağıtmak Azure VM Görüntü Oluşturucu izin vermek için "Azure sanal makine Görüntü Oluşturucu" hizmeti için 'Katkıda bulunan' izinleri sağlamanız gerekir (uygulama kimliği: cf32a0cc-373c-47c9-9156-0db11f6a6dfc ) kaynak grupları. 

Mevcut özel yönetilen resim veya görüntü sürümü kullanıyorsanız, Azure görüntü Oluşturucusu en az bu kaynak gruplarını 'Reader' erişimi gerekir.

Azure CLI kullanarak erişimi atayabilirsiniz:

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName>
```

Hizmet hesabı bulunmazsa, rol ataması burada eklemekte olduğunuz abonelik henüz kaynak sağlayıcı kaydolmadı anlamına gelebilir.


## <a name="costs"></a>Maliyetler
Bazı işlem, ağ ve oluşturma, derleme ve görüntülerini Azure Görüntü Oluşturucu ile depolama, depolama maliyetlerine neden olur. Bu maliyetler, özel görüntüleri el ile oluşturma maliyetleri benzerdir. Kaynaklar için Azure ücretleri üzerinden ücretlendirilir. 

Görüntü oluşturma işlemi sırasında dosyaları indirilir ve depolanan `IT_<DestinationResourceGroup>_<TemplateName>` kaynak grubu, bir küçük depolama maliyetlerine neden olur. Bunlar tutmak istemediğiniz f görüntü şablon görüntü derlemeden sonra silin.
 
Görüntü Oluşturucusu bir VM D1v2 VM boyutu ve depolama kullanarak ve ağ sanal makine için gerekli oluşturur. Bu kaynaklar, yapı işleminin süresi boyunca en son ve Görüntü Oluşturucu görüntüsü oluşturma tamamlandıktan sonra silinir. 
 
Ağ çıkışı ücretlerine tabi olabilirsiniz, seçtiğiniz bölgelerde, görüntüyü Azure Görüntü Oluşturucu dağıtacak.
 
## <a name="next-steps"></a>Sonraki adımlar 
 
Azure Görüntü Oluşturucu ' ' ı denemek için makaleleri oluşturmak için bkz. [Linux](../articles/virtual-machines/linux/image-builder.md) veya [Windows](../articles/virtual-machines/windows/image-builder.md) görüntüler.
 
 
