---
title: Azure yığınında API sürümü profillerini yönetme | Microsoft Docs
description: Azure yığınında API sürümü profilleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: adbe88a44ac38868a68a6845c328ef4cf7fba60c
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604446"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Azure yığınında API sürümü profillerini yönet

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

API profilleri Azure kaynak sağlayıcısı ve Azure REST uç noktaları için API sürümü belirtin. API profilleri kullanılarak farklı dillerde özel istemcileri oluşturabilirsiniz. Her istemci API sürümü ve doğru kaynak sağlayıcısı için Azure yığınına başvurmak için bir API profili kullanır.

Tam olarak hangi her kaynak sağlayıcısı API sürümü Azure yığın ile uyumlu olduğunu sıralama gerek kalmadan Azure kaynak sağlayıcıları ile çalışmak için bir uygulama oluşturabilirsiniz. Yalnızca bir profil uygulamanıza Hizala; SDK, API sürümü döner.

Bu konuda size yardımcı olur:

 - API profilleri için Azure yığınına anlayın.
 - Çözümlerinizi geliştirmek için API profilleri nasıl kullanabileceğinizi öğrenin.
 - Kod özgü yönergeler nerede bulacağını bakın.

## <a name="summary-of-api-profiles"></a>API profilleri özeti

- API profilleri, Azure kaynak sağlayıcıları kümesi ve API sürümlerine göstermek için kullanılır.
- API profilleri arasında birden çok Azure bulut şablonları oluşturmak için oluşturulmuştur. Profilleri uyumlu ve kararlı bir arabirim için gereksinimini karşılamak üzere tasarlanmıştır.
- Profilleri yılda dört kez yayınlanır.
- Üç profil adlandırma kuralları kullanılır:
    - **en son**  
        Genel Azure'da yayımlanan en son API sürümü içerir.
    - **yyyy-mm-dd-hybrid**  
    Düzenlenen bir tempoyla yayımlanan, bu sürüm tutarlılık ve kararlılık arasında birden çok bulut odaklanır. Bu profili en iyi Azure yığını uyumluluk hedefler.
    - **yyyy-aa-gg-profili** en iyi kararlılık ve en son özellikleri arasında bulunur.

### <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Azure API profilleri ve Azure yığın uyumluluğu

Yeni Azure API profilleri Azure yığın ile uyumlu değildir. Azure yığın çözümlerinizi kullanmak için hangi profilleri tanımlamak için aşağıdaki adlandırma kurallarını kullanabilirsiniz.

**en son**  
Bu profilin Azure yığınında çalışmaz genel Azure bulunan en güncel API sürümü vardır. **En son** en çok sayıda önemli değişiklikler vardır. Profil kenara kararlılık ve diğer Bulutları ile uyumluluk koyar. En güncel API sürümü kullanmak çalışıyorsanız **son** kullanmanız gereken profilidir.

**Yyyy-aa-gg-karma**  
Bu profili, her yıl Mart ve Eylül'de serbest bırakılır. Bu profil, en iyi kararlılık ve çeşitli Bulutlar ile uyumluluk vardır. **Yyyy-aa-gg-karma** genel Azure ve Azure yığın hedeflemek için tasarlanmıştır. Bu profilde listelenen Azure API sürümleri Azure yığında listelenen olanlarla aynı olacaktır. Karma bulut çözümleri için kod geliştirmek için bu profili kullanın.

**yyyy-aa-gg-profili**  
Bu profil için genel Azure Haziran ve aralık yayımlanır. Bu profili Azure yığın karşı çalışmaz; Genellikle, birçok önemli değişiklikler olacaktır. En iyi kararlılık ve en son özellikleri, arasındaki farkı arasında bulunur ancak **son** ve bu profili **son** her zaman en yeni API sürümleri, ne zaman bağımsız olarak oluşur API Serbest bırakıldı. Yeni bir API sürümü için işlem API yarın oluşturulursa, örneğin, bu API sürümü listelenir **son**, de **yyyy-aa-gg-profili** bu profili zaten var olduğundan.  **yyyy-aa-gg-profili** Haziran önce veya aralık önce yayımlanan en güncel sürümleri kapsar.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager API profilleri

Azure yığın genel Azure'da bulunan API sürümleri en son sürümünü kullanmaz. Bir çözüm oluştururken, Azure yığın ile uyumlu olan her bir Azure kaynak sağlayıcısı için API sürümü bulmanız gerekir.

Bunun yerine her kaynak sağlayıcısı ve Azure yığını tarafından desteklenen belirli sürümü araştırma daha bir API profili kullanabilirsiniz. Profili bir dizi kaynak sağlayıcıları ve API sürümleri belirtir. SDK veya SDK ile yerleşik bir aracı profilinde belirtilen hedef api-version döner. API profilleriyle tüm şablonu için geçerli bir profil sürümü belirtebilirsiniz ve çalışma zamanında, Azure Resource Manager kaynak doğru sürümünün seçer.

API profilleri PowerShell, Azure CLI, SDK'sı ve Microsoft Visual Studio içinde sağlanan kod gibi Azure Kaynak Yöneticisi Araçları ile çalışır. Araç ve SDK'ları profilleri modülleri ve bir uygulama oluştururken içerecek şekilde kitaplıkları hangi sürümünün okumak için kullanabilirsiniz.

Örneğin, bir depolama alanı oluşturmak için PowerShell kullanın kullanarak hesap **Microsoft.Storage** API sürümü 2016-03-30 ve api-version ile 2015-12-01 Microsoft.Compute kaynak sağlayıcısı kullanarak bir VM'i destekler kaynak sağlayıcısı , PowerShell modülü destekleyen için depolama 2016-03-30 aramak gerekir ve işlem için 2015-02-01 modülü destekler ve yükleyin. Bunun yerine, bir profil kullanabilirsiniz. Cmdlet'ini kullanın ** yükleme profili * profilename *** ve PowerShell modülleri doğru sürümü yükler.

Benzer şekilde, Python tabanlı bir uygulama oluşturmak için Python SDK'yı kullanarak, profil belirtebilirsiniz. SDK'yı sağ modülleri betiğinizde belirtilen kaynak sağlayıcıları için yükler.

Bir geliştirici olarak çözümünüzü yazma odaklanabilirsiniz. Araştırma hangi API sürümleri, kaynak sağlayıcısı yerine ve hangi bulut çalışır birlikte, bir profili kullanın ve kodunuzu bu profili destekleyen tüm Bulutlar arasında çalıştığını bilmeniz.

## <a name="api-profile-code-samples"></a>API profili kod örnekleri

Profilleri kullanılarak çözümünüzü Azure yığını ile tercih ettiğiniz dili ile tümleştirmenize yardımcı olmak için kod örnekleri bulabilirsiniz. Şu anda aşağıdaki diller için yönergeler ve örnekler bulabilirsiniz:

- **PowerShell**  
Kullanabileceğiniz **AzureRM.Bootstrapper** modülü API sürümü profilleri ile çalışmak için gerekli PowerShell cmdlet'lerini alma PowerShell Galerisi ile de kullanılabilir. Bilgi için bkz: [kullanım API sürümü profilleri PowerShell](azure-stack-version-profiles-powershell.md).
- **Azure CLI 2.0**  
Azure yığın belirli API sürümü profili kullanmak için ortam yapılandırmanızı güncelleştirebilirsiniz. Bilgi için bkz: [kullanım API sürümü profilleri için Azure CLI 2.0](azure-stack-version-profiles-azurecli2.md).
- **GİT**  
Git SDK profil farklı Hizmetleri farklı sürümlerini farklı kaynak türleriyle birleşimidir. profilleri profiller altında kullanılabilir / yoluyla kendi sürümünde **YYYY-AA-GG** biçimi. Bilgi için bkz: [Git için kullanım API sürümü profilleri](azure-stack-version-profiles-go.md).
- **Ruby**  
Ruby SDK'sı için Azure yığın Resource Manager yapı ve altyapınızı yönetmenize yardımcı olan araçlar sağlar. Kaynak sağlayıcıları SDK işlem, sanal ağlar ve depolama ile Söyleniş dil içerir. Bilgi için bkz: [Ruby sürüm profilleriyle API kullanın](azure-stack-version-profiles-ruby.md)
- **Python**  
- Python SDK'sı Azure yığını ve genel Azure gibi farklı bulut platformları hedeflemesi için API sürümü profillerini destekler. Karma bulut çözümleri oluşturma API profillerini kullanabilirsiniz. Bilgi için bkz: [Python sürümü profilleriyle API kullanın](azure-stack-version-profiles-python.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
* [Kaynak sağlayıcısı API sürümleri profili tarafından desteklenen hakkındaki ayrıntıları inceleyin](azure-stack-profiles-azure-resource-manager-versions.md).