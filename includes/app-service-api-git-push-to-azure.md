API Uygulamanız için uzaktan dağıtım URL'si almak üzere Azure CLI'yi kullanın. Aşağıdaki komutta *\<uygulama_adı>* kısmını web uygulamanızın adıyla değiştirin.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Yerel Git dağıtımınızı uzaktan gerçekleştirilecek şekilde yapılandırın.

```bash
git remote add azure <URI from previous step>
```

Uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Daha önce dağıtım kullanıcısını oluştururken oluşturduğunuz parola istenir. Quickstart daha önce oluşturduğunuz parola ve değil Azure portalında oturum açmak için kullandığınız parolayı girdiğinizden emin olun.

```bash
git push azure master
```
