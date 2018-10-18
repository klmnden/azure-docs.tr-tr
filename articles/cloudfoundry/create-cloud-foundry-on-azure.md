---
title: Azure’da Cloud Foundry oluşturma
description: Azure'da Cloud Foundry PCF kümesi sağlamak için gerekli parametreleri ayarlamayı öğrenin
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: Cloud Foundry
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: a0a3379a8a2579080d9b686917395feec9cf8f3d
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48250673"
---
# <a name="create-cloud-foundry-on-azure"></a>Azure’da Cloud Foundry oluşturma

Bu öğreticide Azure'da Pivotal Cloud Foundry PCF kümesi sağlamak için gerekli parametreleri oluşturma adımları gösterilmektedir.  Pivotal Cloud Foundry çözümünü Azure [Market](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry) araması yaparak bulabilirsiniz.

![Alternatif resim metni](media/deploy/pcf-marketplace.png "Azure'da Pivotal Cloud Foundry arama")


## <a name="generate-an-ssh-public-key"></a>SSH ortak anahtarı oluşturma

Windows, Mac veya Linux kullanarak ortak SSH anahtarı oluşturmanın birden fazla yöntemi vardır.

```Bash
ssh-keygen -t rsa -b 2048
```
- Ortamınıza özgü [yönergeler]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) için buraya tıklayın.

## <a name="create-a-service-principal"></a>Hizmet Sorumlusu oluşturma

> [!NOTE]
>
> Hizmet sorumlusu oluşturmak için Sahip izinlerine sahip olmanız gerekir.  Ayrıca bir betik yazarak da Hizmet Sorumlusu oluşturma işlemlerini otomatikleştirebilirsiniz. Örneğin bunun için Azure CLI [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest) cmdlet'ini kullanabilirsiniz.

1. Azure hesabınızda oturum açın.

    `az login`

    ![Alternatif resim metni](media/deploy/az-login-output.png "Azure CLI oturumu açma")
 
    “id” değerini daha sonra kullanmak üzere **subscription ID** ve **tenantId** değeri olarak kopyalayın.

2. Bu yapılandırma için varsayılan aboneliğinizi ayarlayın.

    `az account set -s {id}`

3. PCF örneğiniz için bir AAD uygulaması oluşturun ve benzersiz bir alfasayısal parola belirtin.  Parolayı daha sonra kullanmak üzere **clientSecret** olarak kaydedin.

    `az ad app create --display-name "Svc Prinicipal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    Çıktıdaki “appId” değerini daha sonra kullanmak üzere **ClientID** olarak kopyalayın.

    > [!NOTE]
    >
    > Uygulamanızın giriş sayfasını ve tanımlayıcı URI'sini seçin.  Örneğin: http://www.contoso.com.

4. Yeni “appId” değerinizle bir hizmet sorumlusu oluşturun.

    `az ad sp create --id {appId}`

5. Hizmet sorumlunuzun izin rolünü **Katkıda bulunan** olarak belirleyin.

    `az role assignment create --assignee “{enter-your-homepage}” --role “Contributor” `

    İsterseniz şunu da kullanabilirsiniz…

    `az role assignment create --assignee {service-princ-name} --role “Contributor” `

    ![Alternatif resim metni](media/deploy/svc-princ.png "Hizmet Sorumlusu rol ataması")

6. appId, password ve tenantId değerleriyle Hizmet Sorumlunuzda başarıyla oturum açabildiğinizi doğrulayın.

    `az login --service-principal -u {appId} -p {your-passward}  --tenant {tenantId}`

7. Yukarıda kopyaladığınız **subscription ID**, **tenantId**, **clientID** ve **clientSecret** değerlerini kullanarak aşağıdaki biçimde bir .json dosyası oluşturun.  Dosyayı kaydedin.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Pivotal Network Belirtecini alma

1. [Pivotal Network](https://network.pivotal.io) hesabı oluşturun veya oturum açın.
2. Sayfanın sağ üst köşesinde profil adınıza tıklayın ve "Edit Profile" (Profili Düzenle) öğesini seçin.
3. Sayfanın altına doğru inin ve **LEGACY API TOKEN** değerini kopyalayın.  Bu, daha sonra kullanacağınız **Pivotal Network Belirteci** değerinizdir.

## <a name="provision-your-cloud-foundry-on-azure"></a>Azure'da Cloud Foundry ortamınızı sağlama

1. Artık [Azure'da Pivotal Cloud Foundry](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry) kümenizi sağlamak için gerekli tüm parametrelere sahipsiniz.
2. Parametreleri girin ve PCF kümenizi oluşturun.

## <a name="verify-the-deployment-and-log-into-the-pivotal-ops-manager"></a>Dağıtımı doğrulama ve Pivotal Ops Manager oturumu açma

1. PCF kümenizin dağıtım durumu gösterilmelidir.

    ![Alternatif resim metni](media/deploy/deployment.png "Azure dağıtım durumu")

2. Sol taraftaki **Deployments** (Dağıtımlar) bağlantısına tıklayarak PCF Ops Manager kimlik bilgilerinizi alın ve bir sonraki sayfada **Deployment Name** (Dağıtım Adı) öğesine tıklayın.
3. Soldaki gezinti bölmesinde **Outputs** (Çıktılar) bağlantısına tıklayarak PCF Ops Manager URL, Kullanıcı adı ve Parola değerlerini görüntüleyin.  “OPSMAN-FQDN” değeri URL'yi gösterir.
 
    ![Alternatif resim metni](media/deploy/deploy-outputs.png "Cloud Foundry dağıtımı çıktısı")
 
4. URL'yi bir web tarayıcısında başlatın ve bir önceki adımda aldığınız kimlik bilgilerini girerek oturum açın.

    ![Alternatif resim metni](media/deploy/pivotal-login.png "Pivotal oturum açma sayfası")
         
    > [!NOTE]
    >
    > Internet Explorer sitenin güvenli olmadığı konusunda bir uyarı görüntülerse “Daha fazla bilgi”ye ve “Web sitesine ilerle"ye tıklayın.  Firefox'da Gelişmiş'e tıklayın ve devam etmek için sertifikayı ekleyin.

5. PCF Ops Manager sayfasında dağıtılan Azure örnekleri görüntülenmelidir. Artık uygulamalarınızı burada dağıtıp yönetmeye başlayabilirsiniz!
               
    ![Alternatif resim metni](media/deploy/ops-mgr.png "Pivotal'a dağıtılan Azure örneği")
 
