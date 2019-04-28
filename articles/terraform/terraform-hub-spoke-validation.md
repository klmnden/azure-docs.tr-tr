---
title: Azure'da Terraform ile merkez ve uç ağ doğrula
description: Tüm sanal ağ ile merkez ve uç ağ topolojisi doğrulamak için öğretici birbirine bağlı.
services: terraform
ms.service: azure
keywords: terraform, hub ve bağlı bileşen, ağlar, hibrit ağlar, devops, sanal makine, azure, vnet eşlemesi,
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 03/01/2019
ms.openlocfilehash: 157be65a19a1f790b911aa9d861c5f18fc8c0813
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128270"
---
# <a name="tutorial-validate-a-hub-and-spoke-network-with-terraform-in-azure"></a>Öğretici: Azure'da Terraform ile merkez ve uç ağ doğrula

Bu makalede, bu serinin önceki makalede oluşturulan terraform dosyaları yürütün. Tanıtım sanal ağlar arasındaki bağlantının doğrulanması sonucudur.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Merkez-uç topolojisinde merkez sanal ağa uygulamak için HCL (HashiCorp dili) kullanın
> * Dağıtılacak kaynaklar doğrulamak için Terraform planı kullanın
> * Azure'da kaynak oluşturmak için kullanımı Terraform Uygula
> * Farklı ağlarda arasındaki bağlantıyı doğrulayın
> * Tüm kaynakları yok etmek için Terraform'u kullanın

## <a name="prerequisites"></a>Önkoşullar

1. [Bir hub'ı oluşturup azure'da Terraform ile karma ağ topolojisi](./terraform-hub-spoke-introduction.md).
1. [Azure'da Terraform ile şirket içi sanal ağ oluşturma](./terraform-hub-spoke-on-prem.md).
1. [Azure'da Terraform ile merkez sanal ağ oluşturma](./terraform-hub-spoke-hub-network.md).
1. [Azure'da Terraform ile merkez sanal ağ Gereci oluşturma](./terraform-hub-spoke-hub-nva.md).
1. [Azure'da Terraform ile bir uç sanal ağları oluşturma](./terraform-hub-spoke-spoke-network.md).

## <a name="verify-your-configuration"></a>Yapılandırmanızı doğrulayın

Tamamladıktan sonra [önkoşulları](#prerequisites), uygun yapılandırma dosyalarının mevcut olduğunu doğrulayın.

1. [Azure portala](https://portal.azure.com) gidin.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd hub-spoke
    ```

1. Çalıştırma `ls` doğrulamak için komut `.tf` yapılandırma dosyaları önceki öğreticilerdeki oluşturulan listelenmiştir:

    ![Terraform Tanıtımı yapılandırma dosyaları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-config-files.png)

## <a name="deploy-the-resources"></a>Kaynakları dağıtma

1. Terraform sağlayıcı başlatın:
    
    ```bash
    terraform init
    ```
    
    !["Terraform init" komutunun örnek sonuçları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-init.png)
    
1. Çalıştırma `terraform plan` komutu yürütmeden önce dağıtım etkisini görmek için:

    ```bash
    terraform plan
    ```
    
    !["Terraform planı" komutunun örnek sonuçları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-plan.png)

1. Çözümü dağıtın:

    ```bash
    terraform apply
    ```
    
    Girin `yes` dağıtım onaylamanız istendiğinde.

    !["Terraform Uygula" komutunun örnek sonuçları](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-terraform-apply.png)
    
## <a name="test-the-hub-vnet-and-spoke-vnets"></a>Test hub'ı sanal ağ ve uç sanal ağları

Bu bölümde, merkez sanal ağa simülasyonu yapılmış şirket içi ortamdan bağlantısını test etme işlemi gösterilmektedir.

1. Azure portalında göz **onprem-vnet-rg** kaynak grubu.

1. İçinde **onprem-vnet-rg** sekmesinde, adlı sanal makine seçin **onprem vm**.

1. **Bağlan**’ı seçin.

1. Metnin yanında **VM yerel hesabı kullanarak oturum açma**, kopyalama **ssh** panoya komutu.

1. Bir Linux istemiyle çalıştırılabilen `ssh` simülasyonu yapılmış şirket içi ortama bağlanmak için. Belirtilen parola `on-prem.tf` parametre dosyası.

1. Çalıştırma `ping` merkez sanal ağda VM Sıçrama kutusu bağlanabilirliği test etmek için komut:

   ```bash
   ping 10.0.0.68
   ```

1. Çalıştırma `ping` komutu her bir uçtaki Vm'lere Sıçrama kutusu bağlanabilirliği test etmek için:

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

1. Çıkmak için ssh oturumunda **onprem vm** sanal makine girin `exit` basın &lt;Enter >.

## <a name="troubleshoot-vpn-issues"></a>VPN sorunlarını giderme

VPN hatalarını çözme hakkında daha fazla bilgi için bkz [karma bir VPN bağlantı sorunlarını giderme](/azure/architecture/reference-architectures/hybrid-networking/troubleshoot-vpn).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, öğretici serisinde oluşturduğunuz kaynakları silin.

1. Plana bildirilen kaynakları kaldırın:

    ```bash
    terraform destroy
    ```

    Girin `yes` kaynakları kaldırılmasını onaylamanız istendiğinde.

1. Dizinleri üst dizine değiştirin:

    ```bash
    cd ..
    ```

1. Silme `hub-scope` dizin (tüm dosyaları dahil):

    ```bash
    rm -r hub-spoke
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure'da Terraform kullanma hakkında daha fazla bilgi edinin](/azure/terraform)