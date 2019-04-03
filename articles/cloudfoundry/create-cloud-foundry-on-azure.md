---
title: Azure'da Pivotal Cloud Foundry küme oluşturma
description: Azure'da Pivotal Cloud Foundry (PCF) küme sağlama için gereken parametreleri ayarlama hakkında bilgi edinin
services: Cloud Foundry
documentationcenter: CloudFoundry
author: ruyakubu
manager: brunoborges
editor: ruyakubu
ms.assetid: ''
ms.author: ruyakubu
ms.date: 09/13/2018
ms.devlang: ''
ms.service: azure
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: 382e342f2144bcc6eeedafd74790bb442b8f9308
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884253"
---
# <a name="create-a-pivotal-cloud-foundry-cluster-on-azure"></a>Azure'da Pivotal Cloud Foundry küme oluşturma

Bu öğretici, oluşturmak ve azure'da Pivotal Cloud Foundry (PCF) kümesi sağlamak için gereken parametreler oluşturmak için hızlı adımlar sağlar. Pivotal Cloud Foundry çözüm bulmak için Azure'da bir arama yapmasına [Market](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

![Azure'da Pivotal Cloud Foundry arama](media/deploy/pcf-marketplace.png)


## <a name="generate-an-ssh-public-key"></a>SSH ortak anahtarı oluşturma

Windows, Mac veya Linux'ı kullanarak bir genel güvenli Kabuk (SSH) anahtar oluşturmak için birkaç yolu vardır.

```Bash
ssh-keygen -t rsa -b 2048
```

Daha fazla bilgi için [azure'da Windows ile SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

> [!NOTE]
>
> Hizmet sorumlusu oluşturmak için sahip hesabı izni gerekir. Ayrıca, hizmet sorumlusu oluşturma otomatikleştirmek için bir betik yazabilirsiniz. Örneğin, Azure CLI'yı kullanabilirsiniz [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest).

1. Azure hesabınızda oturum açın.

    `az login`

    ![Azure CLI oturumu açma](media/deploy/az-login-output.png )
 
    "ID" değeri olarak kopyalayın, **abonelik kimliği**ve daha sonra kullanmak üzere "Tenantıd" değerini kopyalayın.

2. Bu yapılandırma için varsayılan aboneliğinizi ayarlayın.

    `az account set -s {id}`

3. Bir Azure Active Directory uygulaması için PCF oluşturun. Benzersiz bir alfasayısal parola belirtin. Parola olarak Store, **clientSecret** daha sonra kullanmak üzere.

    `az ad app create --display-name "Svc Principal for OpsManager" --password {enter-your-password} --homepage "{enter-your-homepage}" --identifier-uris {enter-your-homepage}`

    Çıktı "AppID" değeri kopyalayın, **ClientID** daha sonra kullanmak üzere.

    > [!NOTE]
    >
    > Örneğin, kendi uygulama giriş sayfası ve tanımlayıcı bir URI seçin http://www.contoso.com.

4. Yeni uygulama kimliğiniz ile hizmet sorumlusu oluşturma

    `az ad sp create --id {appId}`

5. Hizmet sorumlunuzu izni rolünü katkıda bulunan olarak ayarlayın.

    `az role assignment create --assignee “{enter-your-homepage}” --role “Contributor”`

    Ya da kullanabilirsiniz

    `az role assignment create --assignee {service-principal-name} --role “Contributor”`

    ![Hizmet sorumlusu rol ataması](media/deploy/svc-princ.png )

6. Başarılı bir şekilde, hizmet sorumlusu uygulama kimliği, parola ve Kiracı kimliği kullanarak oturum açabildiğinizi doğrulayın

    `az login --service-principal -u {appId} -p {your-password}  --tenant {tenantId}`

7. Şu biçimde bir .json dosyası oluşturun. Kullanım **abonelik kimliği**, **Tenantıd**, **ClientID**, ve **clientSecret** daha önce kopyaladığınız değerleri. Dosyayı kaydedin.

    ```json
    {
        "subscriptionID": "{enter-your-subscription-Id-here}",
        "tenantID": "{enter-your-tenant-Id-here}",
        "clientID": "{enter-your-app-Id-here}",
        "clientSecret": "{enter-your-key-here}"
    }
    ```

## <a name="get-the-pivotal-network-token"></a>Pivotal ağ belirteci alma

1. Kaydolun veya oturum açın, [Pivotal ağ](https://network.pivotal.io) hesabı.
2. Sayfanın sağ üst köşesinde profil adınızı seçin. Seçin **profili Düzenle**.
3. Ekranı en alta sayfasının ve kopyalama için **eski API BELİRTECİ** değeri. Bu değer, **Pivotal ağ belirteci** daha sonra kullandığınız değer.

## <a name="provision-your-cloud-foundry-cluster-on-azure"></a>Azure'da Cloud Foundry kümenizi sağlama

Sağlama için ihtiyacınız olan tüm parametreleri artık, [azure'da Pivotal Cloud Foundry kümesi](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).
Parametreler girin ve PCF kümenizi oluşturun.

## <a name="verify-the-deployment-and-sign-in-to-the-pivotal-ops-manager"></a>Dağıtımı doğrulama ve Pivotal Ops Manager için oturum açın

1. PCF kümenizi dağıtım durumunu gösterir.

    ![Azure dağıtım durumu](media/deploy/deployment.png )

2. Seçin **dağıtımları** , PCF Ops Manager için kimlik bilgilerini almak için sol taraftaki gezinti bağlantısı. Seçin **dağıtım adı** sonraki sayfada.
3. Sol taraftaki gezinti bölmesinde **çıkışları** bağlantı URL'si, kullanıcı adı ve parola PCF Ops Manager görüntülenecek. URL "FQDN OPSMAN" değeridir.
 
    ![Cloud Foundry dağıtım çıkış](media/deploy/deploy-outputs.png )
 
4. URL, bir web tarayıcısında başlatın. Önceki adımdan oturum açmak için kimlik bilgilerini girin.

    ![Pivotal oturum açma sayfası](media/deploy/pivotal-login.png )
         
    > [!NOTE]
    >
    > Internet Explorer tarayıcı "Sitesi, güvenli olmayan" uyarı iletisi nedeniyle başarısız olursa seçin **daha fazla bilgi** ve Web sayfasına gidin. Firefox için seçin **öncelikli** ve devam etmek için bir sertifika ekleyin.

5. PCF Ops yöneticinize dağıtılan Azure örneklerinden görüntüler. Şimdi dağıtın ve buradan yönetin.
               
    ![Dağıtılan Azure örneğidir, Pivotal](media/deploy/ops-mgr.png )
 
