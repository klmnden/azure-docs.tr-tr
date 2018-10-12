---
title: Kubernetes (K8) Azure Stack dağıtımınıza sorunlarını giderme | Microsoft Docs
description: Kubernetes (K8) Azure Stack dağıtımınıza sorunlarını gidermeyi öğrenin.
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
ms.date: 10/11/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: fbb51d8dc3b1ea4c6b34120e8fe35474ae949cf2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116921"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Kubernetes için Azure Stack dağıtımınıza sorunlarını giderme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

> [!Note]  
> Azure Stack'te Kubernetes önizlemeye sunuldu.

Aşağıdaki makalede Kubernetes kümeniz sorun giderme sırasında arar. Dağıtım uyarıyı gözden geçirin ve dağıtımınızın durumunu dağıtım için gerekli öğeler tarafından gözden geçirin. Azure Stack veya Linux sanal makineleri barındıran Kubernetes dağıtım günlüklerini toplamak gerekebilir. Ayrıca, bir yönetim uç noktasından günlükleri almak için Azure Stack yöneticinizle birlikte çalışmanız gerekebilir.

## <a name="overview-of-deployment"></a>Dağıtıma genel bakış

Kümenizde sorun giderme için adımlara geçmeden önce Azure Stack Kubernetes Küme dağıtımı işlemi gözden geçirmek isteyebilirsiniz. Dağıtım, sanal makineler oluşturmak için bir Azure Resource Manager çözüm şablonu kullanır ve ACS altyapısı kümenizin yükler.

### <a name="deployment-workflow"></a>Dağıtım iş akışı

Küme dağıtımı için genel süreç Aşağıdaki diyagramda gösterilmektedir.

![Kubernetes işlem dağıtma](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Dağıtım adımları

1. Toplanan giriş parametreleri Market öğesi.

    Kubernetes gibi küme ayarlamak için gereken değerleri girin:
    -  **Kullanıcı adı** Kubernetes kümesinin parçası olan bir Linux sanal makineleri ve DVM için kullanıcı adı.
    -  **SSH ortak anahtarı** DVM ve Kubernetes kümesinin bir parçası olarak oluşturulan tüm Linux makinelerinin yetkilendirme için kullanılan anahtarı
    -  **Hizmet İlkesi** Kubernetes Azure bulut sağlayıcısı tarafından kullanılan kimliği. Uygulama kimliği olarak, hizmet sorumlusu oluştururken belirlenen istemci kimliği. 
    -  **İstemci gizli anahtarı** , hizmet sorumlusu oluştururken oluşturduğunuz anahtar oldukları.

2. VM dağıtımı oluşturur ve özel betik uzantısı.
    -  Market Linux görüntüsü kullanarak Linux VM dağıtımı oluşturur **Ubuntu Server 16.04 LTS**.
    -  İndir ve müşteri betik uzantısını marketten Yürüt. Komut dosyası **Linux 2.0 için özel betik**.
    -  DVM özel betiği çalıştırır. Komut dosyası:
        1. Galeri uç noktası, Azure Resource Manager meta veri uç noktasından alır.
        2. Active directory kaynak kimliği, Azure Resource Manager meta veri uç noktasından alır.
        3. ACS altyapısı için API modelini yükler.
        4. ACS altyapısı için Kubernetes kümesi dağıtır ve Azure Stack bulut profiline kaydeder `/etc/kubernetes/azurestackcloud.json`.
3. Ana Vm'lerden oluşturur.

    İndirir ve müşteri betik uzantısını yürütür.

4. Ana betiği çalıştırır.

    Komut dosyası:
    - Etcd, Docker ve Kubernetes yükler kubelet gibi kaynakları. etcd makine kümesi arasında verileri depolamak için bir yol sağlayan bir dağıtılmış anahtar değer deposudur. Docker kapsayıcıları olarak bilinen tam kemikler işletim sistemi düzeyinde virtualizations destekler. Kubelet her Kubernetes düğümü üzerinde çalışan düğümü aracısıdır.
    - Etcd hizmet ayarlar.
    - Kubelet hizmet ayarlar.
    - Kubelet başlatır. Bu, aşağıdakileri içerir:
        1. API hizmetini başlatır.
        2. Denetleyici hizmetini başlatır.
        3. Scheduler hizmetini başlatır.
5. Aracı VM'ler oluşturur.

    İndirir ve müşteri betik uzantısını yürütür.

6. Aracı betiği çalıştırır. Aracısı özel betik:
    - Etcd yükleyin.
    - Kubelet hizmetini ayarlayın.
    - Kubernetes kümesini birleştirir.

## <a name="steps-for-troubleshooting"></a>Sorun giderme adımları

Kubernetes kümenizi destekleyen Vm'lerde günlüklerini toplayabilir. Ayrıca dağıtım günlüğünü gözden geçirebilirsiniz. Azure Stack kullanmakta olduğunuz sürümünü doğrulamak için ve Azure dağıtımınızla ilgili yığından günlüklerini almak için Azure Stack yöneticinizle konuşun gerekebilir.

1. Gözden geçirme [dağıtım durumu](#review-deployment-status) ve [günlüklerini](#get-logs-from-a-vm) ana düğüm Kubernetes kümenize öğesinden.
2. Azure Stack en son sürümünü kullanmanız gerekir. Azure Stack sürümünüz hakkında şüpheleriniz varsa, Azure Stack yöneticinize başvurun. Kubernetes kümesi Marketi zaman 0.3.0 Azure Stack 1808 veya üzeri bir sürümünü gerektirir.
3.  VM oluşturma dosyalarınızı gözden geçirin. Aşağıdaki sorunlarla karşılaşmış olabilir:  
    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
    - VM oluşturma veya oluşturma hatası tetikleyen bir iç hata tetiklenen. Azure Stack aboneliğiniz için kapasite sınırlamaları dahil olmak üzere bir dizi etkene göre hatalarına neden.
    - VM için tam etki alanı adı (FDQN), yinelenen bir önek ile başlayan mu?
4.  VM **Tamam**, DVM değerlendirebilirsiniz. DVM bir hata iletisi varsa:

    - Ortak anahtar geçersiz olabilir. Oluşturduğunuz anahtarı gözden geçirin.  
     - Ayrıcalıklı uç noktaları kullanarak, Azure Stack için günlükleri almak için Azure Stack yöneticinize başvurmanız gerekir. Daha fazla bilgi için [Azure Stack'te tanılama araçları](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Dağıtımınız hakkında sorularınız varsa sorunuzu gönderin veya birisi zaten sorusuna cevap verdi varsa bkz [Azure Stack Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>Dağıtım durumunu gözden geçirin

Tüm sorunları gözden geçirmek için Kubernetes kümenizi dağıttığınızda, dağıtım durumunu gözden geçirebilirsiniz.

1. Açık [Azure Stack portalı](https://portal.local.azurestack.external).
2. Seçin **kaynak grupları**, ve ardından kullanılan kaynak grubu adını seçin, Kubernetes kümesini dağıtırken.
3. Seçin **dağıtımları** ardından **dağıtım adı**.

    ![Sorun giderme](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Sorun giderme penceresi başvurun. Dağıtılan her bir kaynak, aşağıdaki bilgileri sağlar.
    
    | Özellik | Açıklama |
    | ----     | ----        |
    | Kaynak | Kaynak adı. |
    | Tür | Kaynak sağlayıcıya ve kaynak türü. |
    | Durum | Öğenin durumu. |
    | Zaman Damgası | Saat UTC zaman damgası. |
    | İşlem ayrıntıları | İşlem ayrıntıları gibi kaynak sağlayıcısı işlemi, kaynak uç noktası ve kaynağın adı. |

    Her öğe bir durum simgesi yeşil veya kırmızı olacaktır.

## <a name="get-logs-from-a-vm"></a>Bir sanal makineden günlükleri alın

Kümeniz için ana VM bağlanmak için bir bash istemi açın ve günlükler oluşturmak için bir komut dosyasını çalıştırın. Ana küme kaynak grubunuzda bulunabilir ve adlı `k8s-master-<sequence-of-numbers>`. 

### <a name="prerequisites"></a>Önkoşullar

Bir bash ihtiyacınız olacak makinede kullanımınız, Azure Stack yönetmek için ister. Bash günlüklere erişmek komut dosyalarını çalıştırmak için kullanın. Bir Windows makinede yüklü Git bash isteminde kullanabilirsiniz. En son git sürümünü almak için bkz: [git indirmeleri](https://git-scm.com/downloads).

### <a name="get-logs"></a>Günlükleri alın

1. Bir bash istemi açın. Bir Windows makinede git kullanıyorsanız, bir bash istemi aşağıdaki yoldan açabilirsiniz: `c:\programfiles\git\bin\bash.exe`.
2. Şu bash komutlarını çalıştırın:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > Windows üzerinde çalıştırmanız gerekmez `sudo` ve yalnızca kullanabilirsiniz `chmod 744 getkuberneteslogs.sh`.

3. Aynı oturumunda aşağıdaki komutu ortamınızla eşleşecek şekilde güncelleştirildi parametrelerle çalıştırın.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

    Parametreleri gözden geçirin ve ortamınıza bağlı değerlerini ayarlayın.
    | Parametre           | Açıklama                                                                                                      | Örnek                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i,--dosya kimliği | Kubernetes ana VM bağlanmak için RSA özel anahtar dosyası. Anahtar ile başlamalıdır `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.pem                                                        |
    | y-,--konak          | Genel IP veya Kubernetes kümesi ana VM tam etki alanı adını (FQDN). VM adı ile başlayan `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | u-,--kullanıcı          | Kubernetes küme ana VM kullanıcı adı. Bu ad, Market öğesi yapılandırırken ayarlarsınız.                                                                    | azureuser                                                                     |
    | -d--vmdhost       | Genel IP veya DVM FQDN'si. VM adı ile başlayan `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd dnsk8 frog.local.cloudapp.azurestack.external |

   Parametre değerleriniz eklediğinizde, aşağıdaki gibi görünebilir:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Başarılı çalıştırma günlükleri oluşturur.

    ![Oluşturulan günlükleri](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. Klasörlerdeki komutu tarafından oluşturulan günlükleri alın. Komut yeni bir klasör oluşturur ve saat damgasının.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine kubernetes dvm.log

## <a name="next-steps"></a>Sonraki adımlar

[Kubernetes için Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md).

[Bir Kubernetes Market'te (Azure Stack operatörü için) ekleyin.](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Azure'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
