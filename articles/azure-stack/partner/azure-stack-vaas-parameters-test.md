---
title: Test parametreleri için Azure Stack hizmet olarak doğrulama | Microsoft Docs
description: Yapılandırma dosyası ve test hakkında başvuru konusu, bir hizmet olarak Azure Stack doğrulama için parametreleri geçirin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 9f042779e3463f0d75dc6327b3553156edbf162a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162232"
---
# <a name="test-parameters-for-validation-as-a-service-azure-stack"></a>Test parametreleri için Azure Stack hizmet olarak doğrulama

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Herhangi bir Test paketini doğrulamaları (VaaS) hizmet olarak yürütme önce bir çözümü seçin ve bir test geçişi oluşturun.

- Kiracı kimlik, kayıtlı VaaS bilgilerinizle oturum açın.
- Yeni çözüm oluşturma (seçerek **Çözüm Ekle**) veya var olan tüm seçin.
- Seçin **Başlat** düğmesini **Test geçişleri** iş akışı kutucuk.

> [!Note]  
> Yeni bir çözüm giriş, doğrulama ve yeni bir test geçirmek her derleme dağıtım için o makine kümesinde her benzersiz makine kümesi/donanım yapılandırması oluşturun.

Testleri çalıştırmak için gerekli parametreleri girmeniz gerekecek **Test geçirmek bilgi** sayfası. Yeni test geçiş veya doğrulama çalışması başlatırken bu parametrelerin sağlar. Parametrelerin çoğu Azure Stack dağıttığınızda sağladığınız aynı değerlere sahip.

El ile listelenen değerler girebilirsiniz [Test parametreleri tablo](#test-parameters), veya tam Azure Stack damga bilgileri içeren dağıtım yapılandırma dosyasını karşıya yükleyin. Karşıya sonra portal değerleri dosyadan yükler.

> [!Note]  
> Arama ve bulma ve portalda oturum yapılandırma dosyası yüklenirken test parametre değerlerini yazarak kullanabilirsiniz.

## <a name="export-the-test-parameters-using-powershell"></a>PowerShell kullanarak test parametreleri dışarı aktarma

1. DVM veya Azure Stack ortamınıza erişimi olan herhangi bir makinede oturum açın.
2. Yükseltilmiş bir PowerShell penceresi açın ve çalıştırın:

    ````PowerShell  
        $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation'
        ConvertTo-Json $params > stampinfoproperties.json
    ````

3. Karşıya yükleme **stampinfoproperties.json** VaaS portalı.

## <a name="find-the-test-parameters-in-the-configuration-file"></a>Test parametreleri yapılandırma dosyasında bulunamıyor

ECE açarak parametre değerleri test bulabilirsiniz **yapılandırma dosyası**. DVM makinedeki dosyası aşağıdaki yolda bulabilirsiniz:  
`C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787`

## <a name="test-parameters"></a>Test parametreleri 

| Parametre    | Açıklama |
|-------------|-----------------|
| Azure Stack derleme                      | Olan azure Stack derleme veya sürüm numarası, örneğin, 1.0.170330.0 dağıtılan | 
| Kiracı Kimliği                              | Azure Active Directory Azure Stack dağıtım sırasında belirtilen Kiracı. Arama **AADTenant** yapılandırma dosyasını seçip **GUID** değerini `Id` etiketi. | 
| Bölge                                 | Azure Stack Dağıtım bölgesi. Arama **bölge** yapılandırma dosyası. | 
| Kiracı Kaynak Yöneticisi                | Azure Resource Manager uç noktası, örneğin Kiracı, `https://management.<ExternalFqdn>` | 
| Yönetici Kaynak Yöneticisi                 | Yönetici Azure Resource Manager uç noktası. Örneğin, `https://adminmanagement.<ExternalFqdn>` | 
| Dış FQDN                          | Dış ortam FQDN'si. Arama **ExternalFqdn** yapılandırma dosyası. | 
| Kiracı Kullanıcı                            | Azure ya da zaten sağlanmış Active Directory Kiracı Yöneticisi veya Hizmet Yöneticisi Azure AD dizinindeki tarafından sağlanması gerekiyor. Kiracı hesabı sağlama hakkında ayrıntılar için bkz. [Azure Active Directory'de yeni bir Azure Stack Kiracı hesabı ekleme](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad). Bu değer test çalıştırmasından kaynakları sağlamak için şablonları dağıtma gibi Kiracı düzeyinde işlemler gerçekleştirmek için kullanılan (sanal makinenin, depolama hesaplarını vb.) ve iş yüklerini çalıştırın. | 
| Hizmet Yöneticisi kullanıcı             | Azure Active Directory Yöneticisi, Azure Stack dağıtım sırasında belirtilen Azure AD Directory Kiracısı. Arama **AADTenant** yapılandırma dosyası ve bir değer seçin `UniqueName` yapılandırma dosyasındaki etiketi. | 

## <a name="checks-before-starting-the-tests"></a>Testlere başlamadan önce denetimleri

Testleri uzaktan işlemleri gerçekleştirin. Testleri çalıştıran makine, Azure Stack Uç noktalara erişimi olmalıdır. Aksi takdirde test çalışmaz. Şirket içi aracıda VaaS kullanıyorsanız, aracı çalıştıracağınız makinenin kullanın. Aşağıdaki denetimleri çalıştırarak makinenizi Azure Stack noktalarına erişimi olduğunu doğrulayabilirsiniz.

1. Taban URI erişilebildiğini kontrol edin. Bir komut istemi veya bash kabuğu ve aşağıdaki komutu çalıştırın:

    ```bash  
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Bir web tarayıcısı açın ve gidin `https://adminportal.<EXTERNALFQDN>` MAS portalı erişilebildiğini denetlemek için.

3. Azure AD kullanarak oturum açar, test geçiş oluştururken girdiğiniz yönetici adı ve parola değerlerini hizmeti.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
