---
title: Kubernetes Azure Active Directory (Azure AD) kullanarak Azure Stack'e dağıtma | Microsoft Docs
description: Kubernetes Azure Active Directory (Azure AD) kullanarak Azure Stack dağıtmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 6e4402be7108f242e1d285ebe91dfece744f0805
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532159"
---
# <a name="deploy-kubernetes-to-azure-stack-using-azure-active-directory"></a>Kubernetes Azure Active Directory'yi kullanarak Azure Stack'e dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo preview tarafından şu anda desteklenmiyor.

Dağıtma ve Azure Active Directory (Azure AD), tek bir kimlik yönetim hizmeti işlemi Eşgüdümlü olarak kullanırken, Kubernetes için kaynakları ayarlamak için bu makaledeki adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için doğru izinlere sahip ve Azure Stack hazır olduğundan emin olun.

1. Azure Active Directory (Azure AD) kiracınız uygulamalar oluşturabilirsiniz doğrulayın. Kubernetes dağıtımı için bu izinlere ihtiyacınız olur.

    İzinlerinizi denetlerken ile ilgili yönergeler için bkz: [denetleyin Azure Active Directory izinlerini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).

1. Azure Stack Linux VM'de oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Kümeyi oluştururken ortak anahtar gerekir.

    Bir anahtarı oluşturma ile ilgili yönergeler için bkz: [SSH anahtarı oluşturma](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Azure Stack Kiracı Portalı'nda geçerli bir aboneliğe sahip ve yeterli genel IP sahip yeni bir uygulama eklemek kullanılabilir adresleri denetleyin.

    Küme için bir Azure Stack dağıtılamıyor **yönetici** abonelik. Kullanmanız gereken bir **kullanıcı** abonelik. 

1. Kubernetes kümesi, Market'te yoksa, Azure Stack yöneticinizle görüşün.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Azure hizmet sorumlusu oluşturun. Hizmet sorumlusu, uygulamanın Azure Stack kaynaklarına erişmenizi sağlar.

1. Oturum açmak için genel [Azure portalında](https://portal.azure.com).

1. Azure Stack örneği ile ilişkili Azure AD kiracısı'ı kullanarak oturum açmış denetleyin. Azure araç çubuğundaki filtre simgesine tıklayarak oturum açma işleminiz geçiş yapabilirsiniz.

    ![AD kiracısı seçin](media/azure-stack-solution-template-kubernetes-deploy/tenantselector.png)

1. Azure AD uygulaması oluşturun.

    a. Seçin **Azure Active Directory** > **+ uygulama kayıtları** > **yeni uygulama kaydı**.

    b. Girin bir **adı** uygulama.

    c. Seçin **Web uygulaması / API**.

    d. Girin `http://localhost` için **oturum açma URL'si**.

    c. **Oluştur**’a tıklayın.

1. **Uygulama Kimliği**’ni not alın. Kümeyi oluştururken kimliği gerekir. Kimliği olarak başvurulan **hizmet sorumlusu istemci kimliği**.

1. Seçin **ayarları** > **anahtarları**.

    a. Girin **açıklama**.

    b. Seçin **her zaman geçerli olsun** için **Expires**.

    c. **Kaydet**’i seçin. Anahtar dize not edin. Anahtar dize, kümeyi oluştururken gerekir. Anahtar olarak başvurulan **hizmet sorumlusu istemci parolası**.

## <a name="give-the-service-principal-access"></a>Hizmet sorumlusu erişimi verin

Asıl kaynakları oluşturabilmesi aboneliğinizde hizmet sorumlusu erişimi verin.

1.  Oturum [Azure Stack portalı](https://portal.local.azurestack.external/).

1. Seçin **tüm hizmetleri** > **abonelikleri**.

1. Kubernetes kümesini kullanmak için operatör tarafından oluşturulan aboneliği seçin.

1. Seçin **erişim denetimi (IAM)** > seçin **rol ataması Ekle**.

1. Seçin **katkıda bulunan** rol.

1. Hizmetiniz için asıl oluşturulan uygulama adı seçin. Arama kutusuna adını yazmanız gerekebilir.

1. **Kaydet**’e tıklayın.

## <a name="deploy-kubernetes"></a>Kubernetes dağıtma

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).

1. Seçin **+ kaynak Oluştur** > **işlem** > **Kubernetes kümesi**. **Oluştur**’a tıklayın.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Temel Bilgiler

1. Seçin **Temelleri** Kubernetes kümesi oluşturun.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Seçin, **abonelik** kimliği

1. Yeni bir kaynak grubu adını girin veya mevcut bir kaynak grubunu seçin. Kaynak adı alfasayısal ve küçük harf olması gerekir.

1. Seçin **konumu** kaynak grubu. Bu, Azure Stack yüklemeniz için seçtiğiniz bölgedir.

### <a name="2-kubernetes-cluster-settings"></a>2. Kubernetes küme ayarları

1. Seçin **Kubernetes küme ayarlarını** Kubernetes kümesi oluşturun.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-aad.png)

1. Girin **Linux VM yönetici kullanıcı adı**. Kubernetes kümesinin parçası olan bir Linux sanal makineleri ve DVM için kullanıcı adı.

1. Girin **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılır.

1. Girin **Yöneticisi profili, DNS ön eki** bölgeye benzersiz. Bu gibi bölgesi benzersiz bir adı olmalıdır `k8s-12345`. Deneme için seçtiğiniz aynı kaynak grubu adı en iyi yöntem.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

1. Seçin **Kubernetes ana havuzu profili sayısı**. Sayıyı ana havuzdaki düğüm sayısını içerir. 7 1'den olabilir. Bu değer, tek bir sayı olmalıdır.

1. Seçin **Kubernetes ana Vm'lerden oluşan bir VMSize**.

1. Seçin **Kubernetes düğüm havuzu profili sayısı**. Sayıyı kümedeki aracı sayısını içerir. 

1. Seçin **depolama profili**. Seçebileceğiniz **Blob Disk** veya **yönetilen Disk**. Bu VM'ler, VM boyutu Kubernetes düğüm belirtir. 

1. Seçin **Azure AD'ye** için **Azure Stack kimlik sistemi** Azure Stack yüklemenizin. 

1. Girin **hizmet sorumlusu ClientID** bu Kubernetes Azure bulut sağlayıcısı tarafından kullanılır. İstemci kimliği, hizmet sorumlusu oluştururken sağladığınız uygulama kimliği olarak tanımlanır.

1. Girin **hizmet sorumlusu istemci parolası** , hizmet sorumlusu oluştururken oluşturulur.

1. Girin **Kubernetes Azure bulut sağlayıcısı sürümü**. Kubernetes Azure sağlayıcısı sürümüdür. Azure Stack, her Azure Stack sürümü için özel bir Kubernetes yapı serbest bırakır.

### <a name="3-summary"></a>3. Özet

1. Özet'i seçin. Dikey bir Kubernetes kümesi yapılandırma ayarlarınızı doğrulama iletisi görüntüler.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Ayarlarınızı gözden geçirin.

3. Seçin **Tamam** kümenize dağıtmak için.

> [!TIP]  
>  Dağıtımınız hakkında sorularınız varsa sorunuzu gönderin veya birisi zaten sorusuna cevap verdi varsa bkz [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack).


## <a name="next-steps"></a>Sonraki adımlar

[Kümenize bağlanın](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)

[Kubernetes panosunu etkinleştir](azure-stack-solution-template-kubernetes-dashboard.md)
