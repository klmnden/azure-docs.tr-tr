---
title: Azure Stack'te Kubernetes panosuna erişme | Microsoft Docs
description: Azure stack'teki Kubernetes panosuna erişme hakkında bilgi edinin
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
ms.date: 03/07/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 02/27/2019
ms.openlocfilehash: 4e9df0d413b964b4a14cf9ca48db8b7956b441f9
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58482598"
---
# <a name="access-the-kubernetes-dashboard-in-azure-stack"></a>Azure Stack'te Kubernetes panosuna erişme 

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti* 
> [!Note]   
> Azure Stack'te Kubernetes önizlemeye sunuldu. Azure Stack bağlantısı kesilmiş senaryo preview tarafından şu anda desteklenmiyor. 

Kubernetes, temel yönetim işlemlerini için kullanabileceğiniz bir web Pano içerir. Bu pano, temel sistem durumu ve uygulamalarınız için ölçümleri görüntüleme, oluşturma ve Hizmetleri dağıtın ve mevcut uygulamaları düzenlemek olanak tanır. Bu makalede Azure Stack'te Kubernetes panosunu ayarlama gösterilmektedir.

## <a name="prerequisites-for-kubernetes-dashboard"></a>Kubernetes panosunu için Önkoşullar

* Azure Stack Kubernetes kümesi

    Bir Kubernetes kümesi için Azure Stack dağıtmış olmanız gerekir. Daha fazla bilgi için [dağıtma Kubernetes](azure-stack-solution-template-kubernetes-deploy.md).

* SSH istemcisi

    Güvenlik için bir SSH istemcisi gerekir, ana düğüm kümedeki bağlanın. Windows kullanıyorsanız, kullanabileceğiniz [Putty](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm). Kubernetes kümenizi dağıtılırken kullanılan özel anahtarı gerekir.

* FTP (PSCP)

    SSH ve SSH Dosya Aktarım Protokolü sertifikaları ana düğümü aracılığıyla Azure Stack yönetim makinenize aktarmak için destekleyen bir FTP istemcisi de gerekebilir. Kullanabileceğiniz [FileZilla](https://filezilla-project.org/download.php?type=client). Kubernetes kümenizi dağıtılırken kullanılan özel anahtarı gerekir.

## <a name="overview-of-steps-to-enable-dashboard"></a>Pano etkinleştirmek için adımlara genel bakış

1.  Kümedeki ana düğüm Kubernetes sertifikaları verilecek. 
2.  Sertifikaları, yönetim makinenizi Azure Stack'e içeri aktarın.
2.  Kubernetes web panoyu açın. 

## <a name="export-certificate-from-the-master"></a>Asıl sertifikasını dışarı aktarma 

Kümenizde ana düğüm Panosu URL'sini alabilirsiniz.

1. Genel IP adresi ve kullanıcı adı için Küme Yöneticisi, Azure Stack panodan alın. Bu bilgileri almak için:

    - Oturum [Azure Stack portalı](https://portal.local.azurestack.external/)
    - Seçin **tüm hizmetleri** > **tüm kaynakları**. Ana küme kaynak grubunuzda bulun. Ana adlı `k8s-master-<sequence-of-numbers>`. 

2. Ana düğüm portalda açın. Kopyalama **genel IP** adresi. Tıklayın **Connect** kullanıcı adınızı almak için **VM yerel hesabı kullanarak oturum açma** kutusu. Bu, kümeyi oluştururken ayarladığınız aynı kullanıcı adıdır. Connect dikey pencerede listelenen özel IP adresi yerine genel IP adresini kullanın.

3.  Ana düğümüne bağlanmak için bir SSH istemcisi açın. Windows üzerinde çalışıyorsanız, kullanabileceğiniz [Putty](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-connect-vm) bağlantı oluşturmak için. Ana düğüm için kullanıcı adı, genel IP adresini kullanacağınızı ve kümeyi oluştururken kullanılan özel anahtarı ekleyin.

4.  Terminal bağlandığında yazın `kubectl` Kubernetes'in komut satırı istemcisini açın.

5. Şu komutu çalıştırın:

    ```Bash   
    kubectl cluster-info 
    ``` 
    URL için panoyu bulun. Örneğin: `https://k8-1258.local.cloudapp.azurestack.external/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy`

6.  Otomatik olarak imzalanan sertifikayı ayıklayın ve PFX biçimine dönüştürün. Şu komutu çalıştırın:

    ```Bash  
    sudo su 
    openssl pkcs12 -export -out /etc/kubernetes/certs/client.pfx -inkey /etc/kubernetes/certs/client.key  -in /etc/kubernetes/certs/client.crt -certfile /etc/kubernetes/certs/ca.crt 
    ```

7.  Gizli diziler'ın listesini alın **kube-system** ad alanı. Şu komutu çalıştırın:

    ```Bash  
    kubectl -n kube-system get secrets
    ```

    Kubernetes Not-Pano-token -\<XXXXX > değeri. 

8.  Belirteç alın ve kaydedin. Güncelleştirme `kubernetes-dashboard-token-<####>` ile önceki adımdan gizli değer.

    ```Bash  
    kubectl -n kube-system describe secret kubernetes-dashboard-token-<####>| awk '$1=="token:"{print $2}' 
    ```

## <a name="import-the-certificate"></a>Sertifika içeri aktarma

1. Filezilla açın ve ana düğümüne bağlanın. İhtiyacınız olacak:

    - ana düğüm genel IP
    - Kullanıcı adı
    - özel bir gizli anahtarı
    - Kullanım **SFTP - SSH Dosya Aktarım Protokolü**

2. Kopyalama `/etc/kubernetes/certs/client.pfx` ve `/etc/kubernetes/certs/ca.crt` Azure Stack yönetim makinenize.

3. Dosya konumları not edin. Betik konumlar ile güncelleştirin ve PowerShell ile yükseltilmiş bir istemi açın. Güncelleştirilmiş betiği çalıştırın:  

    ```powershell   
    Import-Certificate -Filepath "ca.crt" -CertStoreLocation cert:\LocalMachine\Root 
    $pfxpwd = Get-Credential -UserName 'Enter password below' -Message 'Enter password below' 
    Import-PfxCertificate -Filepath "client.pfx" -CertStoreLocation cert:\CurrentUser\My -Password $pfxpwd.Password 
    ``` 

## <a name="open-the-kubernetes-dashboard"></a>Kubernetes panosunu açma 

1. Web tarayıcınız açılır pencere engelleyicisini devre dışı bırakın.

2. URL'yi tarayıcınıza belirtildiği komutu çalıştırdığınızda noktası `kubectl cluster-info`. Örneğin: https:\//azurestackdomainnamefork8sdashboard/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard: / proxy 
3. İstemci sertifikası seçin.
4. Belirteci girin. 
5. Ana düğüm üzerinde bash komut satırını yeniden ve izin vermek `kubernetes-dashboard`. Şu komutu çalıştırın:

    ```Bash  
    kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard 
    ``` 

    Komut dosyası sağlar `kubernetes-dashboard` bulut yönetici ayrıcalıkları. Daha fazla bilgi için [kümeleri için RBAC özellikli](https://docs.microsoft.com/azure/aks/kubernetes-dashboard).

Panoyu kullanabilirsiniz. Kubernetes panosunu hakkında daha fazla bilgi için bkz. [Kubernetes Web kullanıcı Arabirimi Panosu](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) 

![Azure Stack Kubernetes Panosu](media/azure-stack-solution-template-kubernetes-dashboard/azure-stack-kub-dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar 

[Kubernetes için Azure Stack dağıtma](azure-stack-solution-template-kubernetes-deploy.md)  

[Bir Kubernetes kümesi Market'te (Azure Stack operatörü için) ekleyin.](../azure-stack-solution-template-kubernetes-cluster-add.md)  

[Azure'da Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)  
