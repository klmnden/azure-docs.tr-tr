---
title: Azure yığınına Kubernetes kümesini dağıtma | Microsoft Docs
description: Azure yığınına Kubernetes küme dağıtmayı öğrenin.
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
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 7cf865f0ce75d8308d6d42306e8e05852f763cae
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-a-kubernetes-cluster-to-azure-stack"></a>Azure yığınına Kubernetes kümesi dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

> [!Note]  
> Azure kapsayıcı Hizmetleri (ACS) Kubernetes Azure yığında özel önizlemede değil. Azure yığın operatörünüze, bu makaledeki yönergeleri gerçekleştirmek için gereken Kubernetes Market öğesine erişimi istemeniz gerekir.

Aşağıdaki makalede dağıtmak ve kaynakları için Kubernetes içinde tek ve eşgüdümlü bir işlemle sağlamak için bir Azure Resource Manager çözüm şablonu kullanarak arar. , Azure yığın yüklemesi hakkında gerekli bilgileri toplamak için şablonu oluşturmak ve gerekir, buluta dağıtma.

## <a name="kubernetes-and-containers"></a>Kubernetes ve kapsayıcıları

Azure kapsayıcı Hizmetleri (ACS) Kubernetes Azure yığında yükleyebilirsiniz. [Kubernetes](https://kubernetes.io) dağıtımı, otomatikleştirmek için bir açık kaynak sistemi ölçekleme ve uygulamaların kapsayıcılarında yönetme. A [kapsayıcı](https://www.docker.com/what-container) bir VM için benzer bir görüntü bulunur. Kapsayıcı yalnızca görüntüdür VM kodu, belirli kitaplıkları ve ayarları yürütmek için çalışma zamanı kodu gibi bir uygulamayı çalıştırmak için gereken kaynakları içerir.

Kubernetes için kullanabilirsiniz:

- Saniye cinsinden dağıtılabilir yüksek düzeyde ölçeklenebilir, yükseltilebilir, uygulamaları geliştirin. 
- Tasarım, uygulamanızın basitleştirmek ve farklı Helm uygulamalar tarafından kendi güvenilirliğini artırmak. [Helm](https://github.com/kubernetes/helm) yükleyip Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır.
- Kolayca izlemek ve ölçek uygulamalarınızla durumunu tanılamak ve işlevsellik yükseltin.

## <a name="prerequisites"></a>Önkoşullar 

Başlamak için doğru izinlere sahip ve Azure yığın hazır olduğundan emin olun.

1. Azure Active Directory (Azure AD) kiracınızda uygulamalar oluşturabilirsiniz doğrulayın. Bu izinleri Kubernetes dağıtım için gerekir.

    İzinlerinizi denetlerken ile ilgili yönergeler için bkz: [denetleyin Azure Active Directory izinleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#check-azure-active-directory-permissions).

2. Azure yığın Linux VM'de oturum açmak için bir SSH ortak ve özel anahtar çifti oluşturur. Küme oluştururken, ortak anahtar gerekir.

    Bir anahtar oluşturma ile ilgili yönergeler için bkz: [SSH anahtarı oluşturma](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

3. Azure yığın Kiracı Portalı'nda geçerli bir aboneliğe sahip ve yeterli genel IP sahip yeni bir uygulama eklemek kullanılabilir adresleri denetleyin.

## <a name="create-a-service-principal-in-azure-ad"></a>Azure AD hizmet sorumlusu oluşturma

1. Oturum açmak için genel [Azure portal](http://www.poartal.azure.com).
2. Azure yığın ile ilişkili Azure AD kiracısı kullanarak imzalanmış denetleyin.
3. Azure AD uygulaması oluşturun.

    a. Seçin **Azure Active Directory** > **+ uygulama kayıtlar** > **yeni uygulama kaydı**.

    b. Girin bir **adı** uygulamanın.

    c. Seçin **Web uygulaması / API**

    d. Girin `http://localhost` için **oturum açma URL'si**.

    c. **Oluştur**'a tıklayın

4. Not **uygulama kimliği**. Küme oluştururken kimliği gerekir. Kimliği olarak başvurulur **hizmet sorumlusu istemci kimliği**.

5. Seçin **ayarları** > **anahtarları**.

    a. Girin **açıklama**.

    b. Seçin **her zaman geçerli olsun** için **Expires**.

    c. **Kaydet**’i seçin. Anahtar dizesini not edin. Küme oluştururken anahtar dizesi gerekir. Başvuru olarak anahtarıdır **hizmet asıl gizli**.



## <a name="give-the-service-principal-access"></a>Hizmet asıl erişmenizi

Asıl kaynakları oluşturabilmesi için aboneliğiniz için hizmet asıl erişim verin.

1.  Oturum [Yönetim Portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **kullanıcı aboneliklerini** > **+ Ekle**.

3. Oluşturduğunuz abonelik seçin.

4. Seçin **erişim denetimi (IAM)** > seçin **+ Ekle**.

5. Seçin **sahibi** rol.

6. Hizmetiniz için asıl oluşturulan uygulama adı seçin. Arama kutusuna adını gerekebilir.

7. **Kaydet**’e tıklayın.

## <a name="deploy-a-kubernetes-cluster"></a>Bir Kubernetes kümesini dağıtma

1. Açık [Azure yığın portal](https://portal.local.azurestack.external).

2. Seçin **+ yeni** > **işlem** > **Kubernetes küme**.

    ![Çözüm Şablonu Dağıt](../media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template.png)

3. Seçin **parametreleri** içinde çözüm şablonu dağıtın.

    ![Çözüm Şablonu Dağıt](../media/azure-stack-solution-template-kubernetes-cluster-add/azure-stack-kubernetes-cluster-solution-template-parameters.png)

2. Girin **Linux yönetici kullanıcı adı**. Kubernetes kümesinin parçası olan Linux sanal makineleri ve DVM için kullanıcı adı.

3. Girin **SSH ortak anahtarını** DVM ve Kubernetes kümenin bir parçası olarak oluşturulan tüm Linux makinelere yetkilendirme için kullanılır.

4. Girin **Kiracı uç noktası**. Bu Kubernetes küme kaynak grubu oluşturmak için bağlanmak için Azure Resource Manager uç noktadır. Uç nokta için tümleşik bir sistemi Azure yığın operatörünüze almak gerekir. Azure yığın Geliştirme Seti (ASDK için), kullandığınız `https://management.local.azurestack.external`.

5. Girin **kimliği Kiracı** Kiracı. Bu değer bulma yardıma gereksinim duyarsanız, bkz: [alma Kiracı kimliği](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). 

6. Girin **ana profili DNS öneki** bölgesine benzersiz. Bu bölge benzersiz bir ad gibi olmalıdır `k8s-12345`. Try için seçtiğiniz aynı kaynak grubu adı en iyi yöntem.

    > [!Note]  
    > Her küme için yeni ve benzersiz ana profili DNS ön ekini kullanın.

7. Aracıları kümedeki girin. Bu değer olarak adlandırılır **aracı havuzu profil sayısı**. 32 için 1'den olabilir

8. Girin **asıl uygulama kimliği hizmeti** bu Kubernetes Azure bulut sağlayıcısı tarafından kullanılır.

9. Girin **hizmet asıl gizli** hizmet asıl uygulaması oluştururken oluşturuldu.

10. Girin **Kubernetes Azure bulut sağlayıcı sürümü**. Bu Kubernetes Azure sağlayıcısı sürümüdür. Azure yığın her Azure yığın sürümü için özel bir Kubernetes yapı serbest bırakır.

12. **Tamam**’ı seçin.

### <a name="specify-the-solution-values"></a>Çözüm değerlerini belirtin

1. Seçin **abonelik**.

2. Yeni bir kaynak grubu adını girin veya varolan bir kaynak grubu seçin. Kaynak adı alfasayısal ve küçük olması gerekir.

3. Kaynak grubu konumu girin **yerel**.

4. Seçin **oluşturun.**

## <a name="connect-to-your-cluster"></a>Kümenize bağlanmak

Artık kümenize bağlanmak hazırsınız. İhtiyacınız olacak **kubectl**, Kubernetes komut satırı istemcisi. Bağlanma ve Azure kapsayıcı hizmetlerini belgelerinde küme yönetme yönergelerini bulabilirsiniz.   

Yönergeler için bkz: [kümeye Bağlan](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough#connect-to-the-cluster).

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes küme Market'te (Azure yığın işleci için) ekleyin.](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure üzerinde Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)