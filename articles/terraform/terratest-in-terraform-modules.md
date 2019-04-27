---
title: Terratest kullanarak Azure'da Terraform modülleri test etme
description: Terraform modüllerinizi test etmek için Terratest’i kullanmayı öğrenin.
services: terraform
ms.service: azure
keywords: terraform, devops, depolama hesabı, azure, terratest, birim testi, tümleştirme testi
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 03/19/2019
ms.openlocfilehash: 9d621905122ab7bf64432323d7d11cf8f1b50750
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60888380"
---
# <a name="test-terraform-modules-in-azure-by-using-terratest"></a>Terratest kullanarak Azure'da Terraform modülleri test etme

> [!NOTE]
> Bu makaledeki örnek kod 0.12 (ve büyük) sürümü ile çalışmaz.

Yeniden kullanılabilir, birleştirilebilir oluşturmak için Azure Terraform modüllerini ve test edilebilir bileşenlerini kullanabilirsiniz. Terraform modülleri altyapı kodu işlemleri uygulamak yararlı olan kapsülleme dahil edilip derecelendirilir.

Terraform modülleri oluşturduğunuzda, kalite güvencesi uygulamak önemlidir. Ne yazık ki, sınırlı belgeleri nasıl birim testleri ve tümleştirme testleri Terraform modülleri'nde Yazar açıklamak kullanılabilir. Bir test altyapısı ve oluşturduk, biz benimsenen en iyi yöntemler Bu öğretici tanıtır bizim [Azure Terraform modüllerini](https://registry.terraform.io/browse?provider=azurerm).

Test altyapıları ve seçerseniz en popüler inceledik [Terratest](https://github.com/gruntwork-io/terratest) bizim Terraform modülleri test etmek için kullanılacak. Terratest, Go kitaplığı olarak uygulanır. Terratest yardımcı işlevleri ve desenleri ortak altyapı gibi HTTP isteğinde bulunan ve belirli bir sanal makineye erişmek için SSH kullanarak görevleri, test için koleksiyonu sağlar. Aşağıdaki liste, bazı Terratest kullanmanın önemli avantajları açıklanmaktadır:

- **Altyapı denetlemek için kullanışlı Yardımcıları sağladığı**. Bu özellik, gerçek altyapınızı gerçek ortamda doğrulamak istediğiniz zamanlar işinize yarar.
- **Klasör yapısı NET bir şekilde düzenlenmiştir**. Test çalışmalarınızı izleyin ve net bir şekilde düzenlenir [standart Terraform modülü klasör yapısını](https://www.terraform.io/docs/modules/create.html#standard-module-structure).
- **Tüm test çalışmalarını bir seferde yazılır**. Terraform kullanan Çoğu geliştirici, Git geliştiricilerin önerilir. Git geliştiricisiyseniz Terratest başka bir programlama dili öğrenmek zorunda değilsiniz. Ayrıca, test çalışmaları Terratest çalıştırmanız için gerekli olan yalnızca Git ve Terraform bağımlılıklardır.
- **Yüksek düzeyde genişletilebilir altyapısıdır**. Azure özgü özellikler dahil olmak üzere, Terratest üzerine ek işlevler genişletebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Platformdan bağımsız Bu uygulamalı makaledir. Bu makalede kullandığımız kod örnekleri Windows, Linux veya Macos'ta çalıştırabilirsiniz. 

Başlamadan önce aşağıdaki yazılımları yükleyin:

- **Go programlama dili**: Terraform test çalışmaları yazılır [Git](https://golang.org/dl/).
- **dep**: [dep](https://github.com/golang/dep#installation), Go’nun bağımlılık yönetimi aracıdır.
- **Azure CLI**: [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure kaynaklarını yönetmek için kullanabileceğiniz bir komut satırı aracıdır. (Terraform destekleyen bir hizmet sorumlusu Azure için kimlik doğrulama veya [Azure CLI aracılığıyla](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html).)
- **Görüntü**: Kullandığımız [yürütülebilir görüntü](https://github.com/magefile/mage/releases) çalışan Terratest çalışmaları basitleştirmek gösterilir. 

## <a name="create-a-static-webpage-module"></a>Statik bir web sayfası modülü oluşturma

Bu öğreticide, statik bir Web sayfası için bir Azure depolama blobu tek bir HTML dosyası yükleyerek sağlayan bir Terraform modülü oluşturun. Bu modül, dünya erişim geçici olarak kullanıcılara modülü döndüren bir URL ile bir Web sayfası sağlar.

> [!NOTE]
> Bu bölümünde açıklanan tüm dosyaları oluşturmak, [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH) konumu.

İlk olarak, adlı yeni bir klasör oluşturun `staticwebpage` , GoPath altında `src` klasör. Bu öğreticinin genel klasör yapısı aşağıdaki örnekte gösterilmiştir. Yıldız ile işaretlenmiş dosyalar `(*)` bu bölümdeki birincil odağı olan.

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

Statik Web modülü üç girişleri kabul eder. Girişleri bildirilen `./variables.tf`:

```hcl
variable "location" {
  description = "The Azure region in which to create all resources."
}

variable "website_name" {
  description = "The website name to use to create related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static home page HTML in your local file system."
  default     = "index.html"
}
```

Makalenin önceki bölümlerinde belirttiğimiz gibi bu modül de içinde bildirilen bir URL çıkışları `./outputs.tf`:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Modül ana mantığını dört kaynaklar sağlar:
- **Kaynak grubu**: Kaynak grubunun adı `website_name` tarafından eklenen giriş `-staging-rg`.
- **Depolama hesabı**: Depolama hesabının adıdır `website_name` tarafından eklenen giriş `data001`. Depolama hesabının adı kısıtlamaları için uyması için modül tüm özel karakterleri kaldırır ve küçük harfler tüm depolama hesabı adını kullanır.
- **ad kapsayıcısı sabit**: Kapsayıcı adındaki `wwwroot` ve depolama hesabı oluşturulur.
- **tek bir HTML dosyası**: HTML dosyasını okuyabilir `html_path` giriş ve yüklenen `wwwroot/index.html`.

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

Terratest tümleştirme testleri için tasarlanmıştır. Bu amaçla Terratest gerçek bir ortamda gerçek kaynak sağlar. Bazen, özellikle çok sayıda kaynak sağlama olduğunda tümleştirme test işleri olağanüstü büyük olabilir. Önceki bölümde diyoruz depolama hesabı adları dönüştürür mantıksal iyi bir örnektir. 

Ancak, gerçekte herhangi bir kaynak sağlamanız gerekmez. Yalnızca adlandırma dönüştürme mantığını doğru olduğundan emin olmak istiyoruz. Birim testleri Terratest'ın esnekliği sayesinde, kullanabiliriz. Birim testleri yerel (internet erişimi gerekli olsa da) test çalışmalarını çalıştırma. Birim test çalışmalarını `terraform init` ve `terraform plan` çıktısını ayrıştırmak için komutları `terraform plan` ve öznitelik değerleri karşılaştırmak bakın.

Bu bölümün geri kalanında, biz Terratest depolama hesabı adları dönüştürmek için kullanılan mantıksal doğru olduğundan emin olmak için birim testi uygulamak için kullanma açıklanmaktadır. Yalnızca yıldızla işaretlenmiş dosyaların ilgileniriz `(*)`.

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

İlk olarak, adlı boş bir HTML dosyası kullandığımız `./test/fixtures/storage-account-name/empty.html` yer tutucu olarak.

Dosya `./test/fixtures/storage-account-name/main.tf` test çalışması çerçevesidir. Bir giriş kabul ettiği `website_name`, olduğu da birim testleri girişi. Mantık burada gösterilmiştir:

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

Temel bir bileşeni birim testleri uygulamasıdır `./test/storage_account_name_unit_test.go`.

Geliştiriciler büyük olasılıkla görürsünüz türünde bir bağımsız değişken kabul ederek birim testini bir Klasik Git test işlev imzası eşleştiğini Git `*testing.T`.

Birim testi gövdesinde değişkeninde tanımlanan beş çalışmaları toplam sahibiz `testCases` (`key` , giriş olarak ve `value` beklenen çıkış olarak). Her birim test çalışması için önce çalıştırıyoruz `terraform init` ve test düzeni klasörü hedef (`./test/fixtures/storage-account-name/`). 

Ardından, bir `terraform plan` belirli test çalışması giriş kullanan komut (göz atın `website_name` tanımında `tfOptions`) sonucu kaydeder `./test/fixtures/storage-account-name/terraform.tfplan` (Genel klasör yapısı içinde listelenmeyen).

Bu sonuç dosyası kod tarafından okunabilen bir yapıya resmi Terraform planı ayrıştırıcının kullanılarak ayrıştırılır.

Şimdi biz ilginizi duyuyoruz öznitelikler arayın (Bu durumda, `name` , `azurerm_storage_account`) ve sonuçları beklenen çıktıyı ile karşılaştırın:

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
        // Specify the test case folder and "-var" options
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

Birim testlerini çalıştırmak için komut satırında aşağıdaki adımları tamamlayın:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables are present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

Yaklaşık bir dakika içinde geleneksel Git test sonucunu döndürür.

### <a name="integration-test"></a>Tümleştirme testi

Birim testleri aksine, tümleştirme testlerini kaynakları gerçek bir ortama bir uçtan uca perspektifi için hazırlamanız gerekir. Bu tür bir görev iyi bir iş Terratest yapar. 

Terraform modülleri için en iyi uygulamaları dahil yükleme `examples` klasör. `examples` Klasörü bazı uçtan uca örnekler içerir. Gerçek verileri içeren çalışma önlemek için bu örnekleri tümleştirme testleri test neden? Bu bölümde, yıldız ile işaretlenmiş üç dosyayı odaklanıyoruz `(*)` aşağıdaki klasörü yapısı içinde:

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

Örnekleri ile başlayalım. Adlı yeni bir örnek klasörüne `hello-world/` oluşturulur `./examples/` klasör. Burada, karşıya yüklenecek basit bir HTML sayfası sunuyoruz: `./examples/hello-world/index.html`.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demonstrate Terratest.</p>
</body>
</html>
```

Terraform örnek `./examples/hello-world/main.tf` birim testinde gösterilene benzer. Önemli bir fark yoktur: örnek adlı bir Web sayfası karşıya yüklenen HTML URL de yazdırır. `homepage`.

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

Terratest kullanırız ve klasik Git test işlevleri yeniden tümleştirme söz konusu test dosyası `./test/hello_world_example_test.go`.

Birim testleri, tümleştirme testlerini Azure'da gerçek kaynakları oluşturun. İşte bu adlandırma çakışmalarından kaçınmak için dikkatli olmanız gerekir. (Özel depolama hesabı adları gibi bazı genel olarak benzersiz adları için dikkat edin.) Bu nedenle, test mantığı ilk adımı bir rastgele oluşturmaktır `websiteName` kullanarak `UniqueId()` Terratest tarafından sağlanan işlev. Bu işlev, küçük harfler, büyük harfler ve sayılar içeren rastgele bir ad oluşturur. `tfOptions` Terraform komutların hedefleyen yapar `./examples/hello-world/` klasör. Ayrıca, emin olur `website_name` rastgele ayarlanmışsa `websiteName`.

Ardından birer birer `terraform init`, `terraform apply` ve `terraform output` yürütülür. Başka bir yardımcı işlevini kullanırız `HttpGetWithCustomValidation()`, Terratest tarafından sağlanır. HTML çıkışı karşıya yüklendiğini emin olmak için yardımcı işlevini kullanırız `homepage` tarafından döndürülen URL `terraform output`. Biz HTTP GET durum kodu ile karşılaştırma `200` ve bazı anahtar sözcükler HTML içerik arayın. Son olarak, Go'nun `defer` özelliğinden yararlanılarak `terraform destroy` komutunun yürütülmesi "taahhüt edildi".

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

    // Generate a random website name to prevent a naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify the test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output, and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demonstrate Terratest.")
    })
}
```

Tümleştirme testleri çalıştırmak için komut satırında aşağıdaki adımları tamamlayın:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables are present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

Yaklaşık iki dakika içinde geleneksel Git test sonucunu döndürür. Bu komutlar yürüterek birim testleri ve tümleştirme testleri hem de çalıştırabilirsiniz:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

Tümleştirme testleri, birim testleri (bir dakika boyunca beş birim durumları ile karşılaştırıldığında bir tümleştirme çalışması için iki dakika) değerinden daha uzun sürer. Ancak kararınız birim testleri kullanın veya bir senaryoda tümleştirme testleri. Genellikle, Terraform ve HCL işlevlerini kullanarak karmaşık mantık için birim testleri kullanmayı tercih ederiz. Biz genellikle bir kullanıcı için uçtan uca perspektif tümleştirme testleri kullanın.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Terratest çalışmalarının yürütülmesini basitleştirmek için mage kullanma 

Azure Cloud Shell'de test çalışmalarını bir kolayca görev değildir. Farklı dizine gidin ve farklı komutları yürütmek sahip. Cloud Shell kullanmaktan kaçınmak için biz derleme sistemi projemizdeki tanıtır. Bu bölümde, iş için bir Git yapı sistemi, görüntü, kullanırız.

Görüntü tarafından gereken tek şey `magefile.go` projenizin kök dizininde (ile işaretlenen `(+)` aşağıdaki örnekte):

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

İşte bir örnek `./magefile.go`. Git içinde yazılan bu derleme betiğindeki şu beş yapı adımları uygulayın:
- `Clean`: Bu adım, test yürütme sırasında oluşturulan tüm oluşturulan ve geçici dosyalar kaldırır.
- `Format`: Adımı `terraform fmt` ve `go fmt` kod tabanınızın biçimlendirmek için.
- `Unit`: Adım tüm birim testlerini çalıştırır (işlev adı kuralı kullanarak `TestUT_*`) altında `./test/` klasör.
- `Integration`: Adım benzer `Unit`, ancak isteğe bağlı olarak, birim testlerini yerine tümleştirme testleri yürütür (`TestIT_*`).
- `Full`: Adımı `Clean`, `Format`, `Unit`, ve `Integration` dizideki.

```go
// +build mage

// Build a script to format and run tests of a Terraform module project
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// The default target when the command executes `mage` in Cloud Shell
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

// A build step that removes temporary build and test files
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

Bir tam test paketi çalıştırmak için aşağıdaki komutları kullanabilirsiniz. Kod, önceki bir bölümde kullandık çalışan adımlar benzerdir. 

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only required when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables are present
GoPath/src/staticwebpage$ mage
```

Son komut satırında ek mage adımlarla değiştirebilirsiniz. Örneğin, kullanabileceğiniz `mage unit` veya `mage clean`. Katıştırmak için iyi bir fikirdir `dep` komutları ve `az login` magefile içinde. Size kodu buraya gösterme. 

Görüntü ile Git paket sistemini kullanarak da adımları paylaşabilir. Bu durumda, ortak bir uygulama başvuru ve bağımlılıkları bildirme tüm modüller arasında magefiles basitleştirebilirsiniz (`mg.Deps()`).

**İsteğe bağlı: Kabul testleri çalıştırmak için hizmet sorumlusu ortam değişkenlerini ayarlama**
 
Yürütme yerine `az login` testleri önce hizmet sorumlusu ortam değişkenlerini ayarlayarak Azure kimlik doğrulaması tamamlayabilirsiniz. Terraform yayımlar bir [ortam değişken adları listesi](https://www.terraform.io/docs/providers/azurerm/index.html#testing). (Bu ortam değişkenlerinden yalnızca ilk dördü gereklidir.) Terraform açıklayan ayrıntılı yönergeler de yayımlar nasıl [bu ortam değişkenleri değeri elde etmek](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html).

## <a name="next-steps"></a>Sonraki adımlar

* Terratest hakkında daha fazla bilgi için bkz: [Terratest GitHub sayfasına](https://github.com/gruntwork-io/terratest).
* Görüntü hakkında daha fazla bilgi için bkz. [mage GitHub sayfasına](https://github.com/magefile/mage) ve [mage Web sitesi](https://magefile.org/).
