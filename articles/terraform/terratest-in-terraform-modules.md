---
title: Terratest kullanarak Azure’da Terraform modüllerini test etme
description: Terraform modüllerinizi test etmek için Terratest’i kullanmayı öğrenin.
services: terraform
ms.service: terraform
keywords: terraform, devops, depolama hesabı, azure, terratest, birim testi, tümleştirme testi
author: JunyiYi
manager: jeconnoc
ms.author: junyi
ms.topic: tutorial
ms.date: 10/19/2018
ms.openlocfilehash: 7feee063c7b311934f7d157a9dff62d803a041b0
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638728"
---
# <a name="test-terraform-modules-in-azure-using-terratest"></a>Terratest kullanarak Azure’da Terraform modüllerini test etme

Terraform modülleri yeniden kullanılabilen, bileştirilebilen ve test edilebilen bileşenler oluşturmak için kullanılır. “Kod olarak altyapı” dünyasına kapsüllemeyi getirirler.

Diğer yazılım bileşenlerinde olduğu gibi, Terraform modüllerinde de kalite denetimi önemli rol oynar. Ne yazık ki, Terraform modüllerinde birim testleri ve tümleştirme testleri yazma konusunu açıklayan çok az belge vardır. Bu öğreticide, [Azure Terraform modüllerimizi](https://registry.terraform.io/browse?provider=azurerm) derlerken benimsediğimiz test altyapısı ve en iyi yöntemler tanıtılır.

En popüler test altyapılarının hepsi gözden geçirildikten sonra [Terratest](https://github.com/gruntwork-io/terratest)’i kullanmayı seçtik. Terratest, Go kitaplığı olarak uygulanır. Belirli bir sanal makineye HTTP istekleri gönderme ve SSH oluşturma gibi yaygın altyapı testi görevleri için yardımcı işlev ve desenlerin bir koleksiyonunu sağlar. Terratest’in başlıca avantajlarından bazıları şunlardır:

- **Altyapıyı denetlemek için kullanışlı yardımcılar sağlar.** Bu özellik, gerçek altyapınızı gerçek ortamda doğrulamak istediğiniz zamanlar işinize yarar.
- **Klasör yapısı net bir şekilde düzenlenmiştir.** Test çalışmalarınız net bir şekilde düzenlenir ve [standart Terraform modülü klasör yapısına](https://www.terraform.io/docs/modules/create.html#standard-module-structure) uyar.
- **Tüm test çalışmaları Go dilinde yazılır.** Terraform geliştiricilerinin çoğu aynı zamanda Go geliştiricileri olduğu için, Terratest'in kullanılması başka bir bilgisayar dili öğrenme gereksinimini ortadan kaldırır. Ayrıca, Terratest’te test çalışmalarını yürütmek için yalnızca Go ve Terraform gerekir.
- **Bu altyapı üst düzeyde genişletilebilir.** Terratest'i, üzerine Azure'a özgü özellikler gibi ek işlevler koyarak genişletmek zor olmaz.

## <a name="prerequisites"></a>Ön koşullar

Bu uygulamalı kılavuz platformdan bağımsızdır; Windows, Linux veya MacOS’da çalıştırılabilir. Devam etmeden önce aşağıdaki yazılımları yükleyin:

- **Go Bilgisayar dili**: Terraform test çalışmaları [Go](https://golang.org/dl/) dilinde yazılır.
- **dep**: [dep](https://github.com/golang/dep#installation), Go’nun bağımlılık yönetimi aracıdır.
- **Azure CLI**: [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), Azure kaynaklarını yönetmeye yönelik bir komut satırı aracıdır. (Terraform, bir Hizmet Sorumlusu aracılığıyla veya [Azure CLI üzerinden](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html) Azure’da kimlik doğrulamasını destekler.)
- **mage**: Terratest çalışmalarınızı yürütmenin nasıl basitleştirileceğini öğrenmek için [mage yürütülebilir dosyasını](https://github.com/magefile/mage/releases) kullanacağız. 

## <a name="create-a-static-webpage-module"></a>Statik bir web sayfası modülü oluşturma

Bu öğreticide, Azure depolama blobuna tek bir HTML dosyası yükleyerek statik bir web sayfası sağlayacak bir Terraform modülü oluşturacaksınız. Bu modül, dünyanın her yerindeki kullanıcıların modül tarafından döndürülen bir URL aracılığıyla bu web sayfasına erişmesini sağlayacak.

> [!NOTE]
> Bu bölümde açıklanan tüm dosyalar [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH) yolunuzda oluşturulmalıdır.

İlk olarak GoPath’inizin `src` klasörünün altında `staticwebpage` adlı yeni bir klasör oluşturun. Bu öğreticinin genel klasör yapısı aşağıda belirtilmiştir. (Yıldız `(*)` ile işaretlenmiş dosyalar bu bölümün önemli odak noktalarıdır.)

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

Statik web sayfası modülü `./variables.tf` içinde bildirilen üç giriş kabul eder:

```hcl
variable "location" {
  description = "The Azure region in which all resources will be created."
}

variable "website_name" {
  description = "The website name which will be used to create a bunch of related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static homepage HTML in your local filesystem."
  default     = "index.html"
}
```

Daha önce de belirtildiği gibi, bu modül `./outputs.tf` içinde bildirilen bir URL çıkışı verir:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Bu bizi modülün ana mantığına getirir. Toplamda dört kaynak sağlar:
- `website_name` girişine `-staging-rg` eklenerek adlandırılan bir kaynak grubu.
- `website_name` girişine `data001` eklenerek adlandırılan bir depolama hesabı. Ancak depolama hesabının ad kısıtlamalarına uymak için, modül tüm özel karakterleri kaldırır ve adın tamamını küçük harfe dönüştürür.
- Yukarıdaki depolama hesabında `wwwroot` sabit adlı bir kapsayıcı oluşturulur.
- Tek bir HTML dosyası `html_path` girişinden okunur ve `wwwroot/index.html` konumuna yüklenir.

Statik web sayfası modülünün mantığı `./main.tf` içinde uygulanır:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = "${var.location}"
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = "${azurerm_resource_group.main.name}"
  location                 = "${azurerm_resource_group.main.location}"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = "${azurerm_resource_group.main.name}"
  storage_account_name  = "${azurerm_storage_account.main.name}"
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = "${azurerm_resource_group.main.name}"
  storage_account_name   = "${azurerm_storage_account.main.name}"
  storage_container_name = "${azurerm_storage_container.main.name}"
  source                 = "${var.html_path}"
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Birim testi

Terratest başlangıçta tümleştirme testleri için tasarlanmış bir araçtır. Bu da gerçek ortamlarda gerçek kaynaklar sağladığı anlamında gelir. Böyle işler bazen, özellikle sağlanması gereken çok miktarda kaynağınız varsa, olağanüstü büyük hale gelebilir. Önceki bölümde açıklanan depolama hesabı adlandırması dönüştürme mantığı iyi bir örnektir: aslında hiçbir bir kaynak sağlamamız gerekmez, yalnızca adlandırma dönüştürme mantığının doğruluğundan emin olmak istiyoruz.

Terratest’in esnekliği sayesinde, birim testlerini kullanarak bunu kolayca gerçekleştirebiliriz. Birim testleri yerel olarak yürütülen test çalışmalarıdır (yine de İnternet erişimi gerekir). Yalnızca `terraform init` ve `terraform plan` komutlarını yürütmek yeterli olur; birim testi çalışmaları `terraform plan` çıkışını ayrıştırır ve karşılaştırmak üzere öznitelik değerlerini arar.

Bu bölümün devamında, depolama hesabı adlandırması dönüştürme mantığının doğruluğundan emin olmak için Terratest'i kullanarak birim testi uygulamayı açıklayacağız. Yalnızca yıldız `(*)` ile işaretli dosyalarla ilgileneceğiz.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

İlk olarak, boş HTML dosyası (`./test/fixtures/storage-account-name/empty.html`) yalnızca bir yer tutucudur.

`./test/fixtures/storage-account-name/main.tf` dosyası test çalışmasının çatısını oluşturur. Yalnızca `website_name` girişini kabul eder ve bu aynı zamanda birim testlerinin de girişidir. Mantığı burada gösterilmiştir:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = "${var.website_name}"
  html_path    = "empty.html"
}
```

Son olarak, en önemli bileşen birim testlerinin uygulanmasıdır: `./test/storage_account_name_unit_test.go`

Go geliştiricisiyseniz, `*testing.T` türünde bir bağımsız değişken kabul ettiğinden klasik Go test işlevinin imzasıyla eşleştiğini fark edersiniz.

Birim testinin gövdesinde, `testCases` değişkeninde tanımlanan toplam beş çalışmamız vardır (anahtar giriştir ve değer de beklenen çıkıştır). Her birim testi çalışması için önce test düzeni klasörünü (`./test/fixtures/storage-account-name/`) hedefleyerek `terraform init` dosyasını çalıştıracağız. 

Bunun ardından, belirli bir test çalışması girişiyle (`tfOptions` içindeki `website_name` tanımına bakın) `terraform plan` komutu sonucu `./test/fixtures/storage-account-name/terraform.tfplan` konumuna kaydedecek (bu konum, genel dosya yapısında listelenmez).

Ardından, bu sonuç dosyası resmi Terraform plan ayrıştırıcısı kullanılarak kod tarafından okunabilir bir yapıya ayrıştırılacak.

Şimdi ilgilendiğimiz öznitelikleri arayacağız (bu örnekte `azurerm_storage_account` için `name`) ve bunları beklenen çıkışla karşılaştıracağız.

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions.Vars, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Birim testlerini çalıştırmak için, komut satırında aşağıdaki adımları tamamlamalısınız.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

Yaklaşık bir dakika sonra geleneksel Go test sonucunu göreceksiniz.

### <a name="integration-test"></a>Tümleştirme testi

Birim testlerinin aksine, tümleştirme testlerinin uçtan uca eksiksiz bir perspektifte gerçek ortamlara kaynak sağlaması gerekir. Terratest böyle işlerde iyidir. Terraform modülünün en iyi yöntemi bazı uçtan uca örnekler içeren `examples` klasörünün kullanımını da önerdiğinden, tümleştirme testleri olarak bu örnekleri test edebiliriz. Bu bölümde, yıldız `(*)` ile işaretlenmiş üç dosyaya odaklanacağız.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

İlk olarak örneklerle başlayalım. `./examples/` klasöründe `hello-world/` adlı yeni bir örnek klasör oluşturulur. Burada, `./examples/hello-world/index.html` konumuna yüklenecek basit bir HTML sayfası sağladık:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demostrate Terratest.</p>
</body>
</html>
```

Terraform `./examples/hello-world/main.tf` örneği birim testinde gösterilene benzer. Tek önemli farkı, karşıya yüklenen `homepage` adlı HTML'nin URL'sini yazdırmasıdır.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = "${var.website_name}"
}

output "homepage" {
  value = "${module.staticwebpage.homepage_url}"
}
```

Terratest ve klasik Go test işlevi, `./test/hello_world_example_test.go` tümleştirme test dosyasında da görülür.

Birim testlerinden farklı olarak, tümleştirme testleri Azure'da gerçek kaynaklar oluşturur ve işte bu nedenle ad çakışmalarını önleme konusunda dikkatli olmanız gerekir. (Depolama hesabı adları gibi genel olarak benzersiz adlara özellikle dikkat edin.) Dolayısıyla, mantığı test etmenin ilk adımı TerraTest tarafından sağlanan `UniqueId()` işlevini kullanarak rastgele bir `websiteName` oluşturmaktır. Bu işlev küçük harf, büyük harf veya sayı içeren rastgele bir ad oluşturur. `tfOptions`, tüm Terraform komutlarının `./examples/hello-world/` klasörünü hedeflemesini ve ayrıca `website_name` öğesinin rastgele `websiteName` olarak ayarlanmasını sağlar.

Ardından birer birer `terraform init`, `terraform apply` ve `terraform output` yürütülür. HTTP Get durum kodunu `200` ile karşılaştırarak ve HTML içeriğinde bazı anahtar sözcükler arayarak HTML'nin `terraform output` tarafından döndürülen çıkış `homepage` URL'sine yüklendiğinden emin olmak için, Terratest tarafından sağlanan başka bir yardımcı işlevi (`HttpGetWithCustomValidation()`) kullandık. Son olarak, Go'nun `defer` özelliğinden yararlanılarak `terraform destroy` komutunun yürütülmesi "taahhüt edildi".

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demostrate Terratest.")
    })
}
```

Tümleştirme testlerini çalıştırmak için komut satırında aşağıdaki adımları tamamlamanız gerekir.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

Yaklaşık iki dakika sonra geleneksel Go test sonucunu göreceksiniz. Kuşkusuz, aşağıdakini yürüterek hem birim testlerini hem de tümleştirme testlerini de çalıştırabilirsiniz:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

Sizin de görebileceğiniz gibi, tümleştirme testleri birim testlerinden çok daha uzun sürer (tek tümleştirme çalışması iki dakika sürerken, beş birim çalışması bir dakika sürer). Ama yine de birim testlerinin ne zaman kullanılacağı ve tümleştirme testlerinden ne zaman yararlanılacağı size kalmıştır. Normalde, Terraform HCL işlevlerinin kullanıldığı karmaşık mantık için birim testlerini, kullanıcının uçtan uca perspektifinden ise tümleştirme testlerini kullanmayı tercih ediyoruz.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Terratest çalışmalarının yürütülmesini basitleştirmek için mage kullanma 

Sizin de gördüğünüz gibi, test çalışmalarını kabukta yürütmek kolay bir iş değildir çünkü farklı dizinlere gitmeniz ve farklı komutlar yürütmeniz gerekir. İşte bu nedenle projemize derleme sistemini ekliyoruz. Bu bölümde, işi yapmak için Go derleme sistemi mage kullanacağız.

Mage için tek gereken şey projenizin kök dizininde (aşağıdaki şekilde `(+)` ile işaretlenmiş dizin) bir `magefile.go` bulunmasıdır.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Burada bir `./magefile.go` örneği görüyorsunuz. Go dilinde yazılmış bu derleme betiğinde beş derleme adımı uyguladık:
- `Clean`: Bu adım, test yürütülürken oluşturulan/geçici tüm dosyaları kaldırır.
- `Format`: Bu adım, kod tabanınızın biçimlendirmek için `terraform fmt` ve `go fmt` çalıştırır.
- `Unit`: Bu adım (`TestUT_*` işlev adlandırma kuralını kullanarak) `./test/` klasörü altındaki tüm birim testlerini çalıştırır.
- `Integration`: `Unit` ile benzerdir ama birim testleri yerine tümleştirme testlerini çalıştırır (`TestIT_*`).
- `Full`: Bu adım sırasıyla `Clean`, `Format`, `Unit', and `Integration` çalıştırır.

```go
// +build mage

// Build script to format and run tests of a Terraform module project.
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// Default target when execute `mage` in shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build/test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Önceki adımları çalıştırmaya benzer şeklide, aşağıdaki komutları kullanarak tam test paketini çalıştırabilirsiniz:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only requied when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage$ mage
```

Son komut satırını herhangi bir mage adımıyla (örneğin, `mage unit` veya `mage clean`) değiştirmekten çekinmeyin. Şimdi burada hala çok fazla komut satırı kaldığını düşünüyor olabilirsiniz ama hem `dep` hem de `az login` komutlarını magefile içine eklemek iyi bir yöntem olabilir. Ama biz burada kodu göstermeyeceğiz. Mage kullanmaya yönelik bir adım daha ekleyelim: Adımlar Go paket sistemi kullanılarak paylaşılabilir. Dolayısıyla tüm modülleriniz genelinde magefile dosyaları yalnızca ortak uygulamaya başvurma ve bağımlılıkları bildirme (`mg.Deps()`) yoluyla basitleştirilebilir.

> [!NOTE]
> **Seçenek: Onay testlerini çalıştırmak için hizmet sorumlusu ortam değişkenlerini ayarlama**
> 
> Testlerden önce `az login` komutunu yürütmek yerine, hizmet sorumlusu ortam değişkenlerini ayarlayarak Azure kimlik doğrulaması yapabilirsiniz. Terraform [ortam değişkeni adlarının listesini](https://www.terraform.io/docs/providers/azurerm/index.html#testing) yayımlar. (Bu ortam değişkenlerinden yalnızca ilk dördü gereklidir.) Terraform, [bu ortam değişkenlerinin değerini alma](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html) işlemini açıklayan ayrıntılı yönergeler de yayımlar.

## <a name="next-steps"></a>Sonraki adımlar

Terratest hakkında daha fazla bilgi için [GitHub sayfasına](https://github.com/gruntwork-io/terratest) bakın. [GitHub sayfasında](https://github.com/magefile/mage) ve [giriş sayfasında](https://magefile.org/), mage hakkında bazı yararlı bilgiler bulabilirsiniz.
