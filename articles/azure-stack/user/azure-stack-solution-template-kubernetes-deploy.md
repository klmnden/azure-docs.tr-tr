---
title: Azure Stack için bir Kubernetes kümesi dağıtma | Microsoft Docs
description: Azure Stack için bir Kubernetes kümesi dağıtmayı öğrenirsiniz.
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
ms.date: 05/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: edcea5f0a4b95725bf766632731f461334e829a9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420147"
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Azure Stack için bir Kubernetes kümesi dağıtma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Azure kapsayıcı Hizmetleri (ACS) Kubernetes özel Önizleme aşamasındadır. Bu makaledeki yönergeleri gerçekleştirmek için gereken Kubernetes Market öğesi erişim istemek, Azure Stack operatörü gerekir.

Aşağıdaki makalede dağıtıp tek ve eşgüdümlü bir işlemle Kubernetes için kaynakları sağlamak için bir Azure Resource Manager çözüm şablonu kullanarak arar. , Azure Stack yüklemesi hakkında gerekli bilgileri toplamak için şablonu oluşturun ve ardından, buluta dağıtın.

## <a name="kubernetes-and-containers"></a>Kubernetes ve kapsayıcılar

Kubernetes Azure Stack'te Azure kapsayıcı Hizmetleri (ACS) altyapısı tarafından oluşturulan Azure Resource Manager şablonlarını kullanarak yükleyebilirsiniz. [Kubernetes](https://kubernetes.io) dağıtımı otomatik hale getirmek için bir açık kaynak sistemi ölçeklendirme ve uygulamaların kapsayıcıları yönetme. A [kapsayıcı](https://www.docker.com/what-container) görüntü, bir VM ile benzerlik bulunur. Bir VM, kapsayıcı görüntüsü yalnızca bir uygulama, kod, kod, belirli kitaplıkları ve ayarları yürütmek için çalışma zamanı gibi çalışması için gereken kaynakları içerir.

Kubernetes için kullanabilirsiniz:

- Saniyeler içinde dağıtılabilir yüksek düzeyde ölçeklenebilir ve yükseltilebilir, uygulamaları geliştirin. 
- Uygulamanızın tasarımını basitleştirmek ve farklı Helm uygulamalar tarafından ve güvenilirliği geliştirmek. [Helm](https://github.com/kubernetes/helm) yükleyin ve Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır.
- Kolayca izleyin ve ölçek ile uygulamalarınızın durumunu tanılayın ve işlevsellik yükseltin.

## <a name="prerequisites"></a>Önkoşullar 

Başlamak için doğru izinlere sahip ve Azure Stack hazır olduğundan emin olun.

1. Azure Active Directory (Azure AD) kiracınız uygulamalar oluşturabilirsiniz doğrulayın. Kubernetes dağıtımı için bu izinlere ihtiyacınız olur.

    İzinlerinizi denetlerken ile ilgili yönergeler için bkz: [denetleyin Azure Active Directory izinlerini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

1. Azure Stack Linux VM'de oturum açmak için SSH ortak ve özel anahtar çifti oluşturun. Kümeyi oluştururken ortak anahtar gerekir.

    Bir anahtarı oluşturma ile ilgili yönergeler için bkz: [SSH anahtarı oluşturma](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Azure Stack Kiracı Portalı'nda geçerli bir aboneliğe sahip ve yeterli genel IP sahip yeni bir uygulama eklemek kullanılabilir adresleri denetleyin.

    Küme için bir Azure Stack dağıtılamıyor **yönetici** abonelik. Bir kullanıcı ** aboneliği kullanmanız gerekir. 

## <a name="create-a-service-principal-in-azure-ad"></a>Azure AD'de hizmet sorumlusu oluşturma

1. Oturum açmak için genel [Azure portalında](http://portal.azure.com).
1. Azure Stack örneği ile ilişkili Azure AD kiracısı'ı kullanarak oturum açmış denetleyin.
1. Azure AD uygulaması oluşturun.

    a. Seçin **Azure Active Directory** > **+ uygulama kayıtları** > **yeni uygulama kaydı**.

    b. Girin bir **adı** uygulama.

    c. Seçin **Web uygulaması / API**.

    d. Girin `http://localhost` için **oturum açma URL'si**.

    c. **Oluştur**’a tıklayın.

1. Not **uygulama kimliği**. Kümeyi oluştururken kimliği gerekir. Kimliği olarak başvurulan **hizmet sorumlusu istemci kimliği**.

1. Seçin **ayarları** > **anahtarları**.

    a. Girin **açıklama**.

    b. Seçin **her zaman geçerli olsun** için **Expires**.

    c. **Kaydet**’i seçin. Anahtar dize not edin. Anahtar dize, kümeyi oluştururken gerekir. Anahtar olarak başvurulan **hizmet sorumlusu istemci parolası**.



## <a name="give-the-service-principal-access"></a>Hizmet sorumlusu erişimi verin

Asıl kaynakları oluşturabilmesi aboneliğinizde hizmet sorumlusu erişimi verin.

1.  Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

1. Seçin **diğer hizmetler** > **kullanıcı aboneliklerini** > **+ Ekle**.

1. Oluşturduğunuz aboneliği seçin.

1. Seçin **erişim denetimi (IAM)** > seçin **+ Ekle**.

1. Seçin **sahibi** rol.

1. Hizmetiniz için asıl oluşturulan uygulama adı seçin. Arama kutusuna adını yazmanız gerekebilir.

1. **Kaydet**’e tıklayın.

## <a name="deploy-a-kubernetes-cluster"></a>Bir Kubernetes kümesi dağıtma

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).

1. Seçin **+ yeni** > **işlem** > **Kubernetes kümesi**. **Oluştur**’a tıklayın.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

1. Seçin **Temelleri** , Kubernetes kümesi oluşturun.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Girin **Linux VM yönetici kullanıcı adı**. Kubernetes kümesinin parçası olan bir Linux sanal makineleri ve DVM için kullanıcı adı.

1. Girin **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılır.

1. Girin **Yöneticisi profili, DNS ön eki** bölgeye benzersiz. Bu gibi bölgesi benzersiz bir adı olmalıdır `k8s-12345`. Deneme için seçtiğiniz aynı kaynak grubu adı en iyi yöntem.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

1. Girin **aracı havuzu profili sayısı**. Sayıyı kümedeki aracı sayısını içerir. 4'e 1'den olabilir.

1. Girin **hizmet sorumlusu ClientID** bu Kubernetes Azure bulut sağlayıcısı tarafından kullanılır.

1. Girin **hizmet sorumlusu istemci parolası** hizmet sorumlusu uygulama oluştururken oluşturuldu.

1. Girin **Kubernetes Azure bulut sağlayıcısı sürümü**. Kubernetes Azure sağlayıcısı sürümüdür. Azure Stack, her Azure Stack sürümü için özel bir Kubernetes yapı serbest bırakır.

1. Seçin, **abonelik** kimliği

1. Yeni bir kaynak grubu adını girin veya mevcut bir kaynak grubunu seçin. Kaynak adı alfasayısal ve küçük harf olması gerekir.

1. Seçin **konumu** kaynak grubu. Bu, Azure Stack yüklemeniz için seçtiğiniz bölgedir.

### <a name="specify-the-azure-stack-settings"></a>Azure Stack ayarlarını belirtin

1. Seçin **Azure Stack damga ayarları**.

    ![Çözüm Şablonu Dağıt](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings.png)

1. Girin **Kiracı Arm uç noktası**. Bu, bir Kubernetes kümesi için kaynak grubu oluşturmak için bağlanmak için Azure Resource Manager uç noktadır. Uç nokta için tümleşik bir sistem, Azure Stack operatörü almak gerekir. Azure Stack geliştirme Seti'ni (ASDK için), kullandığınız `https://management.local.azurestack.external`.

1. Girin **Kiracı kimliği** Kiracı. Bu değeri bulmaya yardıma ihtiyacınız varsa bkz [Kiracı kimliği alma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

## <a name="connect-to-your-cluster"></a>Kümenize bağlanın

Artık kümenize bağlanmaya hazırsınız. Ana küme kaynak grubunuzda bulunabilir ve adlı `k8s-master-<sequence-of-numbers>`. Ana düğümüne bağlanmak için bir SSH İstemcisi'ni kullanın. Asıl kullanabileceğiniz **kubectl**, kümenizi yönetmek için Kubernetes komut satırı istemcisi. Yönergeler için [Kubernetes.io](https://kubernetes.io/docs/reference/kubectl/overview).

Ayrıca bulabilirsiniz **Helm** paket Yöneticisini Yükleme ve kümenize uygulamalarını dağıtmak için yararlıdır. Yükleme ve Helm ile kümenizi kullanma yönergeleri için bkz: [helm.sh](https://helm.sh/).

## <a name="next-steps"></a>Sonraki adımlar

[Bir Kubernetes kümesi Market'te (Azure Stack operatörü için) ekleyin.](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
