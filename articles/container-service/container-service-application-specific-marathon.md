<properties
   pageTitle="Uygulama veya kullanıcıya özel Marathon hizmeti | Microsoft Azure"
   description="Bir uygulama veya kullanıcıya özel Marathon hizmeti oluşturma"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Marathon, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# Bir uygulama veya kullanıcıya özel marathon hizmeti oluşturma

Azure Kapsayıcı Hizmeti, Apache Mesos ve Marathon’u önceden üzerinde yapılandırdığımız bir grup ana sunucu sağlar. Bunlar, uygulamalarınızı kümede düzenlemek için kullanılabilir, ancak en iyisi Ana sunucuları bu amaç için kullanmamaktır. Örneğin, Marathon yapılandırmasına ince ayar yapma ana sunucuların kendilerinde oturum açmayı ve değişiklikler yapmayı gerektirir; bu, standart olandan biraz daha farklı olan benzersiz ana sunucuları teşvik eder ve bağımsız olarak ilgilenilmeli ve yönetilmelidir. Ayrıca, bir takım tarafından istenen yapılandırma başka bir takım için en uygun yapılandırma olmayabilir. Bu makalede size bir kullanıcı veya uygulamaya özel Marathon hizmeti ekleme açıklanmaktadır.

Bu hizmet bir tek bir kullanıcı veya ekibe ait olduğundan, bunlar istenen herhangi bir şekilde yapılandırılabilir. Ayrıca, Azure Kapsayıcı Hizmeti hizmetin sürekli çalışmasını sağlar; hizmet başarısız olursa, Azure Kapsayıcı Hizmeti onu sizin için yeniden başlatır. Çoğu zaman kesinti yaşandığının farkına varmazsınız bile.

## Ön koşullar

[DCOS orchestrator türüyle Azure Kapsayıcı Hizmeti örneğini dağıtın](container-service-deployment.md), [istemcinizin kümenize](container-service-connect.md) bağlanabildiğinden ve [AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)] emin olun.

## Bir uygulama veya kullanıcıya özel Marathon hizmeti oluşturma.

Oluşturmak istediğiniz uygulama hizmeti adını tanımlayan bir JSON yapılandırma dosyası oluşturarak başlayın. Burada çerçeve adı olarak `marathon-alice` kullanıyoruz. Dosyayı `marathon-alice.json` benzeri şekilde kaydedin:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Ardından, Marathon örneğini yapılandırma dosyanızda ayarlanan seçenek grubuyla yüklemek için DC/OS CLI’yı kullanın.

```bash
dcos package install --options=marathon-alice.json marathon
```

Şimdi, DC/OS kullanıcı arabiriminizin hizmetler sekmesinde `marathon-alice` hizmetinizin çalıştığını görmelisiniz. Doğrudan erişmek isterseniz kullanıcı arabirimi `http://<hostname>/service/marathon-alice/` olur.

## Hizmete erişmek için DC/OS CLI’yı ayarlama

İsteğe bağlı olarak, `marathon.url` özelliğini aşağıdaki şekilde `marathon-alice` örneğini işaret edecek şekilde ayarlayarak DC/OS CLI’nizi bu yeni hizmete erişmek üzere yapılandırabilirsiniz.

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

`dcos config show` komutuyla CLI’nizin hangi Marathon örneğine göre çalıştığını doğrulayabilir ve `dcos config unset marathon.url` komutuyla ana Marathon hizmetinizi kullanmak üzere geri çevirebilirsiniz.


<!---HONumber=Jun16_HO2-->


