[az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI 2.0'ı kullanmaya başlama](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini listeleyin.

```azurecli
az account list --all
```

Kullanmak istediğiniz aboneliği belirtin.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```

