---
title: Azure yığınında API sürümü profillerini yönetme | Microsoft Docs
description: Azure yığınında API sürümü profilleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 8A336052-8520-41D2-AF6F-0CCE23F727B4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 452ed1de0588b380747edaa44dd0cc3805c51392
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Azure yığınında API sürümü profillerini yönet

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

API profilleri Azure kaynak sağlayıcısı ve Azure REST uç noktaları için API sürümü belirtin. API profilleri kullanılarak farklı dillerde özel istemcileri oluşturabilirsiniz. Her istemci API sürümü ve doğru kaynak sağlayıcısı için Azure yığınına başvurmak için bir API profili kullanır. 

Tam olarak hangi her kaynak sağlayıcısı API sürümü Azure yığın ile uyumlu olduğunu sıralama gerek kalmadan Azure kaynak sağlayıcıları ile çalışmak için bir uygulama oluşturabilirsiniz. Yalnızca bir profil uygulamanıza Hizala; SDK'yı sağ API sürümleri için geri döner.


Bu konuda size yardımcı olur:
 - API profilleri için Azure yığınına anlayın.
 - Nasıl, çözümleri geliştirmek için API profillerini kullanabilirsiniz.
 - Kod özgü yönergeler için nereye.

## <a name="summary-of-api-profiles"></a>API profilleri özeti

- API profilleri, Azure kaynak sağlayıcıları kümesi ve API sürümlerine göstermek için kullanılır.
- API profilleri arasında birden çok Azure Bulutları şablonları oluşturmak geliştiriciler için oluşturulmuştur. Uyumlu ve kararlı arabirimleri gereksinimi karşılamak üzere tasarlanmıştır.
- Profilleri yılda dört kez yayınlanır.
- Üç profil adlandırma kuralları şunlardır:
    - **latest**  
        Azure'da yayımlanan en son API sürümü.
    - **yyyy-mm-dd-hybrid**  
    Düzenlenen bir tempoyla yayımlanan, bu sürüm tutarlılık ve kararlılık arasında birden çok bulut odaklanan.
    - **yyyy-mm-dd-profile**  
    En iyi kararlılık ve en son özellikleri arasında bulunur.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager API profilleri

Azure yığın en son sürümünü genel Azure'da bulunan API sürümlerinin kullanmaz. Kendi çözüm oluşturmaya, azure'da Azure yığın ile uyumlu olan her kaynak sağlayıcısı için API sürümü bulmanız gerekir.

Bunun yerine her kaynak sağlayıcısı ve Azure yığını tarafından desteklenen belirli sürümü araştırma daha bir API profili kullanabilirsiniz. Profili bir dizi kaynak sağlayıcıları ve API sürümleri belirtir. SDK veya SDK ile yerleşik bir aracı profilinde belirtilen hedef api-version döner. API profilleriyle tüm şablonu için geçerli bir profil sürümü belirtebilirsiniz ve çalışma zamanında, Azure Resource Manager kaynak doğru sürümünün seçer.

API profilleri PowerShell, Azure CLI, SDK'sı ve Microsoft Visual Studio içinde sağlanan kod gibi Azure Kaynak Yöneticisi Araçları ile çalışır. Araç ve SDK'ları profilleri modülleri ve bir uygulama oluştururken içerecek şekilde kitaplıkları hangi sürümünün okumak için kullanabilirsiniz.

Örneğin, bir depolama alanı oluşturmak için PowerShell kullanın kullanarak hesap **Microsoft.Storage** API sürümü 2016-03-30 ve api-version ile 2015-12-01 Microsoft.Compute kaynak sağlayıcısı kullanarak bir VM'i destekler kaynak sağlayıcısı , PowerShell modülü destekleyen için depolama 2016-03-30 aramak gerekir ve işlem için 2015-02-01 modülü destekler ve yükleyin. Bunun yerine, bir profil kullanabilirsiniz. Cmdlet'ini kullanın ** yükleme profili * profilename *** ve PowerShell modülleri doğru sürümü yükler.

Benzer şekilde, Python tabanlı bir uygulama oluşturmak için Python SDK'yı kullanarak, profil belirtebilirsiniz. SDK'yı sağ modülleri betiğinizde belirtilen kaynak sağlayıcıları için yükler.

Bir geliştirici olarak çözümünüzü yazma odaklanabilirsiniz. Araştırma hangi API sürümleri, kaynak sağlayıcısı yerine ve hangi bulut çalışır birlikte, bir profili kullanın ve kodunuzu bu profili destekleyen tüm Bulutlar arasında çalıştığını bilmeniz.

## <a name="api-profile-code-samples"></a>API profili kod örnekleri

Profilleri kullanılarak çözümünüzü Azure yığını ile tercih ettiğiniz dili ile tümleştirmenize yardımcı olmak için kod örnekleri bulabilirsiniz. Şu anda aşağıdaki diller için yönergeler ve örnekler bulabilirsiniz:

- **PowerShell**  
Kullanabileceğiniz **AzureRM.Bootstrapper** modülü API sürümü profilleri ile çalışmak için gerekli PowerShell cmdlet'lerini alma PowerShell Galerisi ile de kullanılabilir.  
Bilgi için bkz: [kullanım API sürümü profilleri PowerShell](azure-stack-version-profiles-powershell.md).
- **Azure CLI 2.0**  
Azure yığın belirli API sürümü profili kullanmak için ortam yapılandırmanızı güncelleştirebilirsiniz.  
Bilgi için bkz: [kullanım API sürümü profilleri için Azure CLI 2.0](azure-stack-version-profiles-azurecli2.md).
- **GO**  
Git SDK profil farklı Hizmetleri farklı sürümlerini farklı kaynak türleriyle birleşimidir. profilleri profiller altında kullanılabilir / yoluyla kendi sürümünde **YYYY-AA-GG** biçimi.  
Bilgi için bkz: [Git için kullanım API sürümü profilleri](azure-stack-version-profiles-go.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md)
* [Azure yığın kullanıcının PowerShell ortamını yapılandırma](azure-stack-powershell-configure-user.md)
* [Kaynak sağlayıcısı API sürümleri profili tarafından desteklenen hakkındaki ayrıntıları inceleyin](azure-stack-profiles-azure-resource-manager-versions.md).
