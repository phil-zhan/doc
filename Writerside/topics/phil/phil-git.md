# phil-git

1. 创建token  
   要在GitHub上生成一个新的个人访问令牌（Personal Access Token），可以按照以下步骤操作：
    - 登录您的GitHub账户：确保您已经登录到您的GitHub账户。
    - 选择“Settings”（设置）：点击页面右上角的头像，然后选择“Settings”。
    - 选择“Developer settings”（开发者设置）：在左侧导航栏中，向下滚动找到“Developer settings”，并点击它。
    - 选择“Personal access tokens”（个人访问令牌）：在“Developer settings”页面中，选择“Personal access tokens”。
    - 生成新令牌：在“Personal access tokens”页面中，点击“Generate token”按钮。
    - 设置令牌：在弹出的对话框中，为您的令牌提供一个描述性名称，并在“Expiration”（过期时间）部分选择令牌的有效期。您还可以选择是否让令牌永不过期，或者设定具体的过期日期。在“Select
      scopes”（选择范围）部分，选择您希望授予该令牌的具体权限。
    - 生成令牌：一旦您完成设置了所有必要的信息，点击“Generate token”按钮以生成您的令牌。请注意，生成的令牌会仅显示一次，因此务必将其复制并妥善保存。
    - 完成以上步骤后，您将获得一个唯一的个人访问令牌，可以用于授权应用程序或工具与您的GitHub账户进行交互。请确保将该令牌安全地保存在一个私密的地方，以防被未经授权的人获取和使用

2. 使用token
    ```Shell
    git remote set-url origin https://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@github.com/phil-zhan/doc.git
    
    ```
