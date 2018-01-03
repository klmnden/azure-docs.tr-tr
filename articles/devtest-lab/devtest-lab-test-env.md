---
title: "VM ve PaaS ortamlarında test için Azure DevTest Labs kullanın | Microsoft Docs"
description: "Azure DevTest Labs VM ve PaaS test ortamı senaryoları için kullanmayı öğrenin."
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: v-craic
ms.openlocfilehash: dc54b1638fbea577f383ead47b83d29e677cd78f
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a>Sınama ortamlarında kullanım Azure DevTest Labs VM ve PaaS

Birçok temel senaryolar uygulamak için Azure DevTest Labs kullanabilirsiniz, ancak birincil senaryo Sınayıcılar için ana makineler için DevTest Labs kullanmakla ilgilidir. 

Bu senaryoda, DevTest Labs şu avantajları sağlar:

- Sınayıcılar hızlı bir şekilde yeniden kullanılabilir şablonlar ve yapıları kullanarak Windows ve Linux ortamlar sağlama tarafından kendi uygulamasının en son sürümünü test edebilirsiniz.
- Sınayıcılar kendi yükleme birden çok test aracılarını sağlama tarafından testi yukarı ölçeklendirebilirsiniz.
- Yöneticiler, sağlayarak maliyetleri denetleyebilirsiniz:
  - Sınayıcılar ihtiyaç duydukları olandan daha fazla VMs alınamıyor.
  - Sanal makineleri ne zaman kullanılmıyor kapatılır.

![DevTest Labs için eğitim kullanın](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Bu makalede, tester gereksinimleri ve bir laboratuvarı ayarlamanız için ayrıntılı adımlar izlemek için karşılamak üzere kullanılan çeşitli Azure DevTest Labs özellikler hakkında bilgi edinin.

## <a name="implementing-test-environments-with-azure-devtest-labs"></a>Azure DevTest Labs içeren test ortamlarında uygulama
1. **Laboratuvar oluşturma** 
   
    Labs Azure DevTest Labs başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra hızlı bir şekilde, oluşturabilirsiniz VM görüntülerini tanımlama maliyetleri ve daha fazlasını denetlemek için ilkeler ayarlama Laboratuvar için kullanıcı (sınayıcılar) ekleme gibi görevleri gerçekleştirebilir.  
   
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
   | [Azure Market görüntülerini yapılandırın](devtest-lab-configure-marketplace-images.md) |Beyaz liste Azure Market görüntülerini, yalnızca Sınayıcılar için istediğiniz görüntüleri seçim için kullanılabilir hale getirme öğrenin.|
   | [Özel bir görüntü oluşturun](devtest-lab-create-template.md) |Sınayıcılar hızla özel görüntü kullanarak bir VM oluşturmak için ihtiyacınız olan yazılım yüklenerek özel bir görüntü oluşturun.|
   | [Görüntü Fabrika hakkında bilgi edinin](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bir görüntü Fabrika ayarlamak ve nasıl kullanılacağını açıklayan bir videoyu izleyin.|

3. **Test makinelerini için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs formülde bir VM oluşturmak için kullanılan varsayılan özellik değerleri listesidir. Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağı seçerek laboratuvarda bir formül oluşturabilirsiniz. Her tester Laboratuvar formülde görebilir ve bir VM oluşturmak için kullanın. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makineleri oluşturmak için DevTest Labs formüller yönetme](devtest-lab-manage-formulas.md) |Görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek bir formül nasıl oluşturabileceğinizi öğrenin.|

3. **Çoklu VM test ortamları oluşturma** 
   
    Azure Resource Manager şablonları altyapısı ve Azure çözümünüzü yapılandırmasını tanımlayın ve art arda birden çok test tutarlı bir durumda sanal makineleri dağıtmak için kullanabilirsiniz.

    Azure PaaS kaynaklarına bir ortamda bir Resource Manager şablonunda sağlanabilir ve maliyet izlemesi görünür. Ancak, VM otomatik kapatma PaaS kaynaklarına geçerli değildir.

    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md) |Tutarlı bir durumda birden çok VM test ortamınızı nasıl dağıtabileceğiniz hakkında bilgi edinin.|

4. **Esnek VM özelleştirmeyi etkinleştirmek için yapıları oluşturma**

   Yapılar, dağıtmak ve bir VM sağlandıktan sonra Uygulamanızı yapılandırmak için kullanılır. Yapıtlar şunlar olabilir:

   - Aracılar, Fiddler ve Visual Studio gibi VM - yüklemek istediğiniz araçları.
   - Bir depoyu kopyalama gibi VM üzerinde-çalıştırmak istediğiniz eylemleri.
   - Test etmek istediğiniz uygulamalar.

   Birçok zaten kullanılabilir out-of--box ürünleridir. Ancak, özel gereksinimlerinizi karşılamak için daha fazla özelleştirme istiyorsanız, özel yapıtları oluşturabilirsiniz.

   Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [DevTest Labs VM için özel yapılar oluşturma](devtest-lab-artifact-author.md) |Sanal makineler için kendi özel yapılar laboratuvarınızda oluşturun.|
   | [Özel yapılar ve kullanmak için Azure Resource Manager şablonları Azure DevTest Labs'de depolamak için bir Git deposu ekleme](devtest-lab-add-artifact-repo.md) |Özel yapıtları kendi özel Git deposuna depolamayı öğrenin.|

5. **Denetim maliyetleri**
   
    Azure DevTest Labs laboratuvarda bir test laboratuvarında tarafından oluşturulan VM'ler en büyük sayısını belirtmek için bir ilke ayarlamanıza olanak sağlar. 
   
    Çalışma kümesi test ekibiniz varsa, zamanlama ve günün belirli bir zamanda tüm sanal makineleri durdurmak ve ardından bunları sonraki gün otomatik olarak yeniden istiyorsanız, laboratuar ortamında otomatik kapatma ve otomatik başlatma ilkeleri ayarlayarak, kolayca gerçekleştirebilirsiniz. 
   
    Uygulama geliştirme tamamlandığında, son olarak, tüm sanal makineleri aynı anda tek bir PowerShell Betiği çalıştırarak silebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuar ortamında ilkeleri ayarlayarak maliyetleri denetler. |
   | [Tüm Laboratuvar bir PowerShell komut dosyası kullanarak sanal makineleri silin](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Test tamamlandığında tek bir işlemde tüm labs silin.|

1. **Sanal ağ için bir laboratuvar ekleme** 
   
    Her bir laboratuvar oluşturulduğunda DevTest Labs yeni bir sanal ağ (VNET) oluşturur. Kendi VNET-Örneğin, ExpressRoute veya siteden siteye VPN kullanarak – yapılandırdıysanız, sanal makineleri oluşturulurken kullanılabilir olmasını sağlamak için Laboratuvarınızı ait sanal ağ ayarları bu VNET ekleyebilirsiniz.

    Ayrıca, VM oluşturulduğunda, bir VM için bir etki alanına katılan bir Azure Active Directory etki alanı katılma yapıya kullanılabilir yoktur. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de sanal ağ yapılandırma](devtest-lab-configure-vnet.md) |Azure portalını kullanarak Azure DevTest Labs içinde bir sanal ağ yapılandırma konusunda bilgi edinin.|

6. **Laboratuvar her tester ile paylaşma**
   
    Labs, denetleyicilerle paylaşmanızı bir bağlantıyı kullanarak doğrudan erişilebilir. Sahip oldukları sürece bile bir Azure hesabına sahip sahip olmayan bir [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account). Sınayıcılar diğer test eden tarafından oluşturulan VM'ler göremezsiniz.  
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs laboratuarda bir tester ekleyin](devtest-lab-add-devtest-user.md) |Sınayıcılar için Laboratuvarınızı eklemek için Azure Portalı'nı kullanın.|
   | [Sınayıcılar bir PowerShell Betiği kullanılarak laboratuvara ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Laboratuvarınızı ekleme edenlere otomatikleştirmek için PowerShell kullanın. |
   | [Laboratuvar bağlantısını Al](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Nasıl sınayıcılar Laboratuvar köprü üzerinden doğrudan erişebileceğiniz öğrenin.|

7. **Daha fazla takımlar için laboratuvar oluşturmayı otomatikleştirmek** 
   
    Laboratuvar oluşturma, özel ayarlar bir Resource Manager şablonu oluşturma ve aynı labs yeniden oluşturmak için kullanma dahil olmak üzere otomatik hale getirebilirsiniz. 
   
    Aşağıdaki tabloda bağlantıları tıklayarak edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Resource Manager şablonu kullanarak bir laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs Resource Manager şablonları kullanarak Azure DevTest Labs içinde oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

