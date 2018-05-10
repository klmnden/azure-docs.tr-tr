---
title: Geliştiriciler için Azure DevTest Labs kullanın | Microsoft Docs
description: Azure DevTest Labs Geliştirici senaryoları için kullanmayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 96432abe619ea23c1a06735567d00660e5430550
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-azure-devtest-labs-for-developers"></a>Geliştiriciler için Azure DevTest Labs kullanın
Azure DevTest Labs pek çok temel senaryolar uygulamak için kullanılabilir, ancak birincil senaryolardan biri, geliştiriciler için geliştirme makineleri barındıracak şekilde DevTest Labs kullanarak içerir. Bu senaryoda, DevTest Labs şu avantajları sağlar:

- Geliştiriciler, geliştirme makinelerinin isteğe bağlı olarak hızlı bir şekilde sağlayabilirsiniz.
- Geliştiriciler, geliştirme makinelerinin gerektiğinde kolayca özelleştirebilirsiniz.
- Yöneticiler, sağlayarak maliyetleri denetleyebilirsiniz:
  - Geliştiriciler, geliştirme için gerekenden daha fazla VMs alınamıyor.
  - Sanal makineleri ne zaman kullanılmıyor kapatılır. 

![DevTest Labs için eğitim kullanın](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Bu makalede, geliştirici gereksinimlerini karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikleri ve bir laboratuvarı ayarlamanız için izleyebileceğiniz ayrıntılı adımlar hakkında bilgi edinin.

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a>Azure DevTest Labs Geliştirici ortamlarıyla uygulama
1. **Laboratuvar oluşturma** 
   
    Labs Azure DevTest Labs başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra hızlı bir şekilde, oluşturabilirsiniz VM görüntülerini tanımlama maliyetleri ve daha fazlasını denetlemek için ilkeler ayarlama Laboratuvar için kullanıcı (geliştiriciler) ekleme gibi görevleri gerçekleştirebilir.  
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de Laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portalında Azure DevTest Labs'de Laboratuvar oluşturma öğrenin. |
2. **Sanal makineleri hazır Market görüntülerini ve özel görüntüleri kullanarak dakikalar içinde oluşturma** 
   
    Azure Marketi'nde görüntüler çok geniş bir yelpazedeki hazır görüntülerden seçer ve laboratuvara kullanılabilmesini. Hazır görüntüleri gereksinimlerinizi karşılamıyorsa, Laboratuvar hazır resim Laboratuvar özel görüntü olarak Azure marketi, ihtiyacınız olan tüm yazılım yükleme ve kaydetme VM kullanarak VM oluşturarak özel bir görüntü oluşturabilirsiniz.

    Özel resimler kullanacaksa, oluşturmak ve görüntüleri dağıtmak için bir resim Fabrika kullanmayı düşünün. Bir görüntü Fabrika düzenli olarak oluşturur ve yapılandırılmış görüntülerinizin otomatik olarak dağıtan bir yapılandırma olarak kodu çözümüdür. Bu, temel işletim sistemiyle bir VM oluşturulduktan sonra sistem el ile yapılandırmak için gereken süreyi kaydeder.
  
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırın](devtest-lab-configure-marketplace-images.md) |Beyaz liste Azure Market görüntülerini, yalnızca geliştiriciler için istediğiniz görüntüleri seçim için kullanılabilir hale getirme öğrenin.|
   | [Özel bir görüntü oluşturun](devtest-lab-create-template.md) |Böylece geliştiriciler, hızlı bir şekilde özel görüntü kullanarak bir VM oluşturabilir, gereken yazılımı yüklenerek özel bir görüntü oluşturun.|
   | [Görüntü Fabrika hakkında bilgi edinin](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bir görüntü Fabrika ayarlamak ve nasıl kullanılacağını açıklayan bir videoyu izleyin.|

3. **Geliştirici makinelerinde için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs formülde bir VM oluşturmak için kullanılan varsayılan özellik değerleri listesidir. Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağı seçerek laboratuvarda bir formül oluşturabilirsiniz. Her geliştirici Laboratuvar formülde görebilir ve bir VM oluşturmak için kullanın. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makineleri oluşturmak için DevTest Labs formüller yönetme](devtest-lab-manage-formulas.md) |Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek bir formül nasıl oluşturabileceğinizi öğrenin.|

4. **Esnek VM özelleştirmeyi etkinleştirmek için yapıları oluşturma**

   Yapılar, dağıtmak ve bir VM sağlandıktan sonra Uygulamanızı yapılandırmak için kullanılır. Yapıtlar şunlar olabilir:

   - Aracılar, Fiddler ve Visual Studio gibi VM - yüklemek istediğiniz araçları.
   - Bir depoyu kopyalama gibi VM üzerinde-çalıştırmak istediğiniz eylemleri.
   - Test etmek istediğiniz uygulamalar.

   Birçok zaten kullanılabilir out-of--box ürünleridir. Özel ihtiyaçlarınız için daha fazla özelleştirme isterseniz kendi özel yapılar oluşturabilirsiniz.

   Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [DevTest Labs VM için özel yapılar oluşturma](devtest-lab-artifact-author.md) |Sanal makineler için kendi özel yapılar laboratuvarınızda oluşturun.|
   | [Özel yapılar ve kullanmak için Azure Resource Manager şablonları Azure DevTest Labs'de depolamak için bir Git deposu ekleme](devtest-lab-add-artifact-repo.md) |Özel yapıtları kendi özel Git deposuna depolamayı öğrenin.|

5. **Denetim maliyetleri**
   
    Azure DevTest Labs laboratuvarda bir geliştirici tarafından oluşturulan VM'ler en büyük sayısını belirtmek için laboratuvarda bir ilke ayarlamanıza olanak sağlar. 
   
    Çalışma kümesi, geliştirici ekibiniz varsa, zamanlama ve günün belirli bir zamanda tüm sanal makineleri durdurmak ve ardından bunları sonraki gün otomatik olarak yeniden istiyorsanız, laboratuar ortamında otomatik kapatma ve otomatik başlatma ilkeleri ayarlayarak, kolayca gerçekleştirebilirsiniz. 
   
    Uygulama geliştirme tamamlandığında, son olarak, tüm sanal makineleri aynı anda tek bir PowerShell Betiği çalıştırarak silebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuar ortamında ilkeleri ayarlayarak maliyetleri denetler. |
   | [Tüm Laboratuvar bir PowerShell komut dosyası kullanarak sanal makineleri silin](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Geliştirme tamamlandığında, tek bir işlemde tüm labs silin.|

1. **Bir VM sanal ağ ekleme** 
   
    Her bir laboratuvar oluşturulduğunda DevTest Labs yeni bir sanal ağ (VNET) oluşturur. Kendi VNET-Örneğin, ExpressRoute veya siteden siteye VPN kullanarak – yapılandırdıysanız, sanal makineleri oluşturulurken kullanılabilir olmasını sağlamak için Laboratuvarınızı ait sanal ağ ayarları bu VNET ekleyebilirsiniz.

    Ayrıca, bir Azure Active Directory etki alanı katılma yapı VM oluşturulduğunda, bir VM bir etki alanına katılacak kullanılabilir yoktur. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md) |Azure portalını kullanarak Azure DevTest Labs içinde bir sanal ağ yapılandırma konusunda bilgi edinin.|

6. **Laboratuvar her developer ile paylaşma**
   
    Labs, geliştiricilere paylaşan bir bağlantıyı kullanarak doğrudan erişilebilir. Sahip oldukları sürece bile bir Azure hesabına sahip sahip olmayan bir [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account). Geliştiriciler, diğer geliştiriciler tarafından oluşturulan VM'ler göremezsiniz.  
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs laboratuarda bir geliştirici ekleyin](devtest-lab-add-devtest-user.md) |Geliştiriciler için Laboratuvarınızı eklemek için Azure Portalı'nı kullanın.|
   | [Geliştiriciler bir PowerShell Betiği kullanılarak laboratuvara ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Laboratuvarınızı ekleme geliştiricilerine otomatikleştirmek için PowerShell kullanın. |
   | [Laboratuvar bağlantısını Al](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Nasıl geliştiriciler Laboratuvar köprü üzerinden doğrudan erişebileceğiniz öğrenin.|

7. **Daha fazla takımlar için laboratuvar oluşturmayı otomatikleştirmek** 
   
    Laboratuvar oluşturma, özel ayarlar bir Resource Manager şablonu oluşturma ve aynı labs yeniden oluşturmak için kullanma dahil olmak üzere otomatik hale getirebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Resource Manager şablonu kullanarak bir laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs Resource Manager şablonları kullanarak Azure DevTest Labs içinde oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

