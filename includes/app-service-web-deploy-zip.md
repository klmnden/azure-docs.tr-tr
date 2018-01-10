## <a name="deploy-uploaded-zip-file"></a>Yüklenen ZIP dosyasını dağıtma

Cloud Shell'de [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) komutunu kullanarak yüklenen ZIP dosyasını web uygulamanıza dağıtın. *\<app_name>* yerine web uygulamanızın adını yazdığınızdan emin olun.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myAppFiles.zip
```

Bu komut ZIP içindeki dosyaları ve dizinleri App Service uygulama klasörünüze (`\home\site\wwwroot`) dağıtır ve uygulamayı yeniden başlatır. Ek özel derleme işlemi yapılandırılmışsa bu işlemler de çalışır.
