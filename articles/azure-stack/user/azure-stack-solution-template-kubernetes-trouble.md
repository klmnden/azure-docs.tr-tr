---
title: Kubernetes için Azure Stack dağıtımınıza sorunlarını giderme | Microsoft Docs
description: Kubernetes için Azure Stack dağıtımınıza sorunlarını gidermeyi öğrenin.
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
ms.date: 10/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 472dfc04cea65cab39d177bb214c417d229b71d2
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956729"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Kubernetes için Azure Stack dağıtımınıza sorunlarını giderme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu.

Aşağıdaki makalede Kubernetes kümeniz sorun giderme sırasında arar. Dağıtım uyarıyı gözden geçirin ve dağıtımınızın durumunu dağıtım için gerekli öğeler tarafından gözden geçirin. Azure Stack veya Linux sanal makineleri barındıran Kubernetes dağıtım günlüklerini toplamak gerekebilir. Bir yönetim uç noktasından günlükleri almak için Azure Stack yöneticinizle birlikte çalışmak gerekebilir.

## <a name="overview-of-deployment"></a>Dağıtıma genel bakış

Kümenizi sorunlarını gidermeye başlamadan önce Azure Stack Kubernetes Küme dağıtımı işlemi gözden geçirmek isteyebilirsiniz. Dağıtım, sanal makineler oluşturmak ve kümeniz için ACS altyapısı yüklemek için bir Azure Resource Manager çözüm şablonu kullanır.

### <a name="deployment-workflow"></a>Dağıtım iş akışı

Küme dağıtımı için genel süreç Aşağıdaki diyagramda gösterilmektedir.

![Kubernetes işlem dağıtma](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Dağıtım adımları

1. Market öğesi giriş parametrelerini toplamak.

    Kubernetes kümesini ayarlamak için ihtiyacınız olan değerlere girin dahil olmak üzere:
    -  **Kullanıcı adı**: Kubernetes kümesini ve DVM parçası olan bir Linux sanal makineleri için kullanıcı adı.
    -  **SSH ortak anahtarı**: Kubernetes kümesini ve DVM bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılan anahtar.
    -  **Hizmet İlkesi**: Kubernetes Azure bulut sağlayıcısı tarafından kullanılan kimliği. İstemci kimliği, hizmet sorumlusu oluştururken sağladığınız uygulama kimliği olarak tanımlanır. 
    -  **İstemci gizli anahtarı**:, hizmet sorumlusu oluştururken oluşturduğunuz anahtar bunlar.

2. VM dağıtımı oluşturmak ve özel betik uzantısı.
    -  Market Linux görüntüsü kullanarak Linux VM dağıtımı oluşturma **Ubuntu Server 16.04 LTS**.
    -  İndirme ve çalıştırma müşteri betik uzantısını marketten. Komut dosyası **Linux 2.0 için özel betik**.
    -  DVM özel betiği çalıştırın. Betik aşağıdaki görevleri gerçekleştirir:
        1. Galeri uç noktası Azure Resource Manager meta veri uç noktasından alır.
        2. Active directory kaynak kimliği Azure Resource Manager meta veri uç noktasından alır.
        3. ACS altyapısı için API modelini yükler.
        4. ACS altyapısı için Kubernetes kümesi dağıtır ve Azure Stack bulut profiline kaydeder `/etc/kubernetes/azurestackcloud.json`.
3. Ana VM'ler oluşturun.

4. İndirin ve müşteri betik uzantıları çalıştırın.

5. Ana betiği çalıştırın.

    Betik aşağıdaki görevleri gerçekleştirir:
    - Etcd, Docker ve Kubernetes yükler kubelet gibi kaynakları. etcd makine kümesi arasında verileri depolamak için bir yol sağlayan bir dağıtılmış anahtar değer deposudur. Docker kapsayıcıları olarak bilinen tam kemikler işletim sistemi düzeyinde virtualizations destekler. Kubelet her Kubernetes düğümü üzerinde çalışan düğümü aracısıdır.
    - Etcd hizmet ayarlar.
    - Kubelet hizmet ayarlar.
    - Kubelet başlatır. Bu görev, aşağıdaki adımları içerir:
        1. API hizmetini başlatır.
        2. Denetleyici Hizmeti başlatır.
        3. Scheduler hizmetini başlatır.
6. Aracı VM'ler oluşturun.

7. İndirme ve çalıştırma müşteri betik uzantısını.

7. Aracı betiği çalıştırın. Aracısı özel betik aşağıdaki görevleri gerçekleştirir:
    - Etcd yükler
    - Kubelet hizmet ayarlar
    - Kubernetes kümesini birleştirir

## <a name="steps-for-troubleshooting"></a>Sorun giderme adımları

Kubernetes kümenizi destekleyen sanal makinelere günlüklerini toplayabilir. Ayrıca dağıtım günlüğünü gözden geçirebilirsiniz. Azure Stack, kullanılacağını ve Azure yığından dağıtımınızla ilgili günlüklerini almak için gereken sürümünü doğrulamak için Azure Stack yöneticinizle konuşun gerekebilir.

1. Gözden geçirme [dağıtım durumu](#review-deployment-status) ve [günlüklerini](#get-logs-from-a-vm) ana düğüm Kubernetes kümenize öğesinden.
2. Azure Stack en son sürümünü kullandığınızdan emin olun. Hangi sürümü kullandığınızdan emin değilseniz, Azure Stack yöneticinize başvurun. Kubernetes kümesi Marketi zaman 0.3.0 Azure Stack 1808 veya üzeri bir sürümünü gerektirir.
3.  VM oluşturma dosyalarınızı gözden geçirin. Aşağıdaki sorunları vardı:  
    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - VM oluşturma veya oluşturma hatası tetikleyen bir iç hata tetiklenen. Azure Stack aboneliğiniz için kapasite sınırlamaları dahil olmak üzere hata, bir dizi etkene neden olabilir.
    - VM için tam etki alanı adı (FDQN) bir yinelenen öneki ile başladığından emin olun.
4.  VM **Tamam**, DVM değerlendirebilirsiniz. DVM bir hata iletisi varsa:

    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - Ayrıcalıklı uç noktaları kullanarak, Azure Stack için günlükleri almak için Azure Stack yöneticinize başvurmanız gerekir. Daha fazla bilgi için [Azure Stack'te tanılama araçları](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Dağıtımınız hakkında bir sorunuz varsa, yayınlamak veya birisi zaten sorusuna cevap verdi varsa bkz [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>Dağıtım durumunu gözden geçirin

Kubernetes kümesini dağıtırken, herhangi bir sorun için kontrol etmek için dağıtım durumunu gözden geçirebilirsiniz.

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).
2. Seçin **kaynak grupları**ve ardından, Kubernetes kümesini dağıtırken kullandığınız kaynak grubu adını seçin.
3. Seçin **dağıtımları**ve ardından **dağıtım adı**.

    ![Sorun giderme](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Sorun giderme penceresi başvurun. Dağıtılan her bir kaynak, aşağıdaki bilgileri sağlar:
    
    | Özellik | Açıklama |
    | ----     | ----        |
    | Kaynak | Kaynak adı. |
    | Tür | Kaynak sağlayıcıya ve kaynak türü. |
    | Durum | Öğenin durumu. |
    | Zaman Damgası | Saat UTC zaman damgası. |
    | İşlem ayrıntıları | İşlem ayrıntıları işlemi, kaynak uç noktası ve kaynağın adını söz konusuydu kaynak sağlayıcısı gibi. |

    Her öğenin bir durum simgesi yeşil veya kırmızı vardır.

## <a name="get-logs-from-a-vm"></a>Bir sanal makineden günlükleri alın

Günlükler oluşturmak için kümeniz için ana VM bağlanmak bir bash istemi açın ve ardından bir komut çalıştırın gerekir. Sanal makine küme kaynak grubunuzda bulunabilir ve adlı asıl `k8s-master-<sequence-of-numbers>`. 

### <a name="prerequisites"></a>Önkoşullar

İhtiyacınız bir bash istemi makinedeki Azure Stack yönetmek için kullanabilirsiniz. Bash günlüklere erişmek komut dosyalarını çalıştırmak için kullanın. Bir Windows makinede Git ile yüklü bash isteminde kullanabilirsiniz. En son git sürümünü almak için bkz: [Git indirmeleri](https://git-scm.com/downloads).

### <a name="get-logs"></a>Günlükleri alın

Günlükleri almak için aşağıdaki adımları uygulayın:

1. Bir bash istemi açın. Bir Windows makinede Git kullanıyorsanız, bir bash istemi aşağıdaki yoldan açabilirsiniz: `c:\programfiles\git\bin\bash.exe`.
2. Şu bash komutlarını çalıştırın:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Windows üzerinde çalıştırmanız gerekmez `sudo`. Bunun yerine, hemen kullanabileceğiniz `chmod 744 getkuberneteslogs.sh`.

3. Aynı oturumda ortamınızla eşleşecek şekilde güncelleştirildi parametrelerle aşağıdaki komutu çalıştırın:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

4. Parametreleri gözden geçirin ve ortamınıza bağlı değerlerini ayarlayın.
    | Parametre           | Açıklama                                                                                                      | Örnek                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i,--dosya kimliği | Kubernetes ana VM bağlanmak için RSA özel anahtar dosyası. Anahtar ile başlamalıdır `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.pem                                                        |
    | y-,--konak          | Genel IP veya Kubernetes kümesi ana VM tam etki alanı adını (FQDN). VM adı ile başlayan `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | u-,--kullanıcı          | Kubernetes küme ana VM kullanıcı adı. Market öğesi yapılandırdığınızda bu adını ayarlayın.                                                                    | azureuser                                                                     |
    | -d--vmdhost       | Genel IP veya DVM FQDN'si. VM adı ile başlayan `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd dnsk8 frog.local.cloudapp.azurestack.external |

   Parametre değerleriniz eklediğinizde, aşağıdaki kodu şöyle görünebilir:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Başarılı çalıştırma günlükleri oluşturur.

    ![Oluşturulan günlükleri](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. Klasörlerdeki komutu tarafından oluşturulan günlükleri alın. Komut, yeni bir klasör oluşturur ve bunları zaman damgaları.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine kubernetes dvm.log

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes için Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)

[Bir Kubernetes kümesi Market'te (Azure Stack operatörü için) ekleyin.](../azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
