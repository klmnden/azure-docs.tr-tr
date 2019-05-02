---
title: Azure üzerinde Terraform giriş.
description: Azure'da Terraform ile bir Azure Cosmos DB ile Azure Container Instances'a dağıtımını yaparak başlayın.
services: terraform
author: neilpeterson
ms.service: azure
ms.topic: quickstart
ms.date: 02/04/2019
ms.author: nepeters
ms.openlocfilehash: 3905296fca4285ce5b75cfb210d125f667428bfe
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724393"
---
# <a name="create-a-terraform-configuration-for-azure"></a>Terraform yapılandırma oluşturmak için Azure

Bu örnekte, Terraform yapılandırması oluşturma ve Azure'a bu yapılandırmayı dağıtma deneyimini elde edin. Tamamlandığında, bir Azure Cosmos DB örneğine, bir Azure Container Instance ve bu iki kaynak üzerinde çalışan bir uygulama dağıtacaksınız. Bu belge, Azure bulut Terraform araçları önceden yüklenmiş olan Kabuğu'nda, tüm iş tamamlandı olduğunu varsayar. Kendi sisteminizdeki örnek üzerinde çalışmak istiyorsanız, Terraform bulunan yönergeleri kullanarak yüklenebilir [burada](../virtual-machines/linux/terraform-install-configure.md).

## <a name="create-first-configuration"></a>İlk yapılandırması oluştur

Bu bölümde, bir Azure Cosmos DB örneğine yapılandırması oluşturur.

Seçin **şimdi deneyin** Azure cloud Shell'i açmak için. Açık sonra girin `code .` cloud shell Kod Düzenleyicisi'ni açmak için.

```azurecli-interactive
code .
```

Kopyalayıp Terraform aşağıdaki yapılandırmayı yapıştırın.

Bu yapılandırma, bir Azure kaynak grubu, rastgele bir tamsayı ve bir Azure Cosmos DB örneğine modeller. Rasgele tamsayı, Cosmos DB örnek adını kullanılır. Cosmos DB çeşitli ayarlar da yapılandırılır. Cosmos DB Terraform yapılandırmalar tam bir listesi için bkz. [Cosmos DB Terraform başvurusu](https://www.terraform.io/docs/providers/azurerm/r/cosmosdb_account.html).

Dosyayı Farklı Kaydet `main.tf` işiniz bittiğinde. Bu işlem yapılabilir Kod Düzenleyicisi'ni sağ üst kısmında üç nokta simgesini kullanarak.

```azurecli-interactive
resource "azurerm_resource_group" "vote-resource-group" {
  name     = "vote-resource-group"
  location = "westus"
}

resource "random_integer" "ri" {
  min = 10000
  max = 99999
}

resource "azurerm_cosmosdb_account" "vote-cosmos-db" {
  name                = "tfex-cosmos-db-${random_integer.ri.result}"
  location            = "${azurerm_resource_group.vote-resource-group.location}"
  resource_group_name = "${azurerm_resource_group.vote-resource-group.name}"
  offer_type          = "Standard"
  kind                = "GlobalDocumentDB"

  consistency_policy {
    consistency_level       = "BoundedStaleness"
    max_interval_in_seconds = 10
    max_staleness_prefix    = 200
  }

  geo_location {
    location          = "westus"
    failover_priority = 0
  }
}
```

[Terraform init](https://www.terraform.io/docs/commands/init.html) komut çalışma dizini başlatır. Çalıştırma `terraform init` Yeni yapılandırmanın dağıtımına hazırlanmak için terminal cloud shell'de.

```azurecli-interactive
terraform init
```

[Terraform planı](https://www.terraform.io/docs/commands/plan.html) komutu yapılandırmasını düzgün biçimlendirildiğini doğrulamak ve hangi kaynakların oluşturulacak, güncelleştirilmiş, yok veya görselleştirmek için kullanılabilir. Sonuçları bir dosyada depolanır ve daha sonra yapılandırmayı uygulamak için kullanılır.

Çalıştırma `terraform plan` yeni Terraform yapılandırmayı test etmek için.

```azurecli-interactive
terraform plan --out plan.out
```

Yapılandırma kullanarak uygulama [terraform uygulamak](https://www.terraform.io/docs/commands/apply.html) ve planı dosyasının adını belirtin. Bu komut, Azure aboneliğinizdeki kaynakları dağıtır.

```azurecli-interactive
terraform apply plan.out
```

Bunu yaptıktan sonra kaynak grubu oluşturuldu ve kaynak grubunda bir Azure Cosmos DB örneğine yerleştirilen görebilirsiniz.

## <a name="update-configuration"></a>Güncelleştirme yapılandırması

Bir Azure Container Instance içerecek şekilde yapılandırmasını güncelleştirin. Kapsayıcı okur ve verileri Cosmos DB'ye yazan bir uygulama çalıştırır.

Alt kısmına aşağıdaki yapılandırmayı kopyalama `main.tf` dosya. İşiniz bittiğinde dosyayı kaydedin.

İki ortam değişkenleri ayarlanır, `COSMOS_DB_ENDPOINT` ve `COSMOS_DB_MASTERKEY`. Bu değişkenleri konuma ve veritabanına erişmek için anahtar. Bu değişkenler için değerler son adımda oluşturduğunuz veritabanı örneğinden elde edilir. Bu işlem, ilişkilendirme bilinir. Terraform ilişkilendirme hakkında daha fazla bilgi için bkz: [ilişkilendirme söz dizimi](https://www.terraform.io/docs/configuration/interpolation.html).


Yapılandırma da tam etki alanı adını (FQDN) kapsayıcı örneğini döndüren bir çıktı bloğu içerir.

```azurecli-interactive
resource "azurerm_container_group" "vote-aci" {
  name                = "vote-aci"
  location            = "${azurerm_resource_group.vote-resource-group.location}"
  resource_group_name = "${azurerm_resource_group.vote-resource-group.name}"
  ip_address_type     = "public"
  dns_name_label      = "vote-aci"
  os_type             = "linux"

  container {
    name   = "vote-aci"
    image  = "microsoft/azure-vote-front:cosmosdb"
    cpu    = "0.5"
    memory = "1.5"
    ports {
      port     = 80
      protocol = "TCP"
    }

    secure_environment_variables {
      "COSMOS_DB_ENDPOINT"  = "${azurerm_cosmosdb_account.vote-cosmos-db.endpoint}"
      "COSMOS_DB_MASTERKEY" = "${azurerm_cosmosdb_account.vote-cosmos-db.primary_master_key}"
      "TITLE"               = "Azure Voting App"
      "VOTE1VALUE"          = "Cats"
      "VOTE2VALUE"          = "Dogs"
    }
  }
}

output "dns" {
  value = "${azurerm_container_group.vote-aci.fqdn}"
}
```

Çalıştırma `terraform plan` güncelleştirilmiş plan oluşturma ve değişiklik yapılmasına görselleştirin. Bir Azure Container Instance kaynak yapılandırmaya eklenmiş olduğunu görmeniz gerekir.

```azurecli-interactive
terraform plan --out plan.out
```

Son olarak, çalıştırma `terraform apply` yapılandırmayı uygulamak için.

```azurecli-interactive
terraform apply plan.out
```

Tamamlandığında, kapsayıcı örneğinin FQDN not alın.

## <a name="test-application"></a>Uygulamayı test etme

Kapsayıcı örneği FQDN'sini gidin. Her şeyin doğru şekilde yapılandırıldıysa, aşağıdaki uygulama görmeniz gerekir.

![Azure vote uygulaması](media/terraform-quickstart/azure-vote.jpg)

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, Azure kaynaklarını ve kaynak grubu kullanılarak kaldırılabilir [terraform yok](https://www.terraform.io/docs/commands/destroy.html) komutu.

```azurecli-interactive
terraform destroy -auto-approve
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, oluşturulabilir, dağıtılan ve bir Terraform yapılandırması yok. Azure'da Terraform kullanarak daha fazla bilgi için Azure Terraform sağlayıcısı belgelerine bakın.

> [!div class="nextstepaction"]
> [Azure Terraform sağlayıcısı](https://www.terraform.io/docs/providers/azurerm/)
