<!-- markdownlint-disable MD002 MD041 -->

この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。 これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。 この手順では、アプリケーションに、 [角度の Microsoft 認証ライブラリ](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) を統合します。

1. 「 **Oauth** 」という名前の新しいファイルを作成し、次のコードを追加し**ます。**

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    を `YOUR_APP_ID_HERE` アプリケーション登録ポータルからのアプリケーション ID に置き換えます。

    > [!IMPORTANT]
    > Git などのソース管理を使用している場合は、この時点で **oauth** ファイルをソース管理から除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。

1. **/Src/app/app.module.ts**を開き、次の `import` ステートメントをファイルの先頭に追加します。

    ```typescript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. `MsalModule`を `imports` 宣言内の配列に追加 `@NgModule` し、アプリ ID で初期化します。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports" highlight="6-11":::

## <a name="implement-sign-in"></a>サインインの実装

このセクションでは、認証サービスを作成し、サインインとサインアウトを実装します。

1. CLI で次のコマンドを実行します。

    ```Shell
    ng generate service auth
    ```

    このためのサービスを作成することにより、認証方法にアクセスする必要がある任意のコンポーネントに容易に注入することができます。

1. コマンドが完了したら、 **/src/app/auth.service.ts** を開き、その内容を次のコードに置き換えます。

    ```typescript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return null;
      }
    }
    ```

1. **/Src/app/nav-bar/nav-bar.component.ts**を開き、その内容を次のように置き換えます。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. **/Src/app/home/home.component.ts**を開き、その内容を次のように置き換えます。

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-32":::

変更を保存し、ブラウザーを更新します。 [ **サインインするにはここを** クリック] ボタンをクリックすると、にリダイレクトさ `https://login.microsoftonline.com` れます。 Microsoft アカウントを使用してログインし、要求されたアクセス許可に同意します。 アプリページが更新され、トークンが表示されます。

### <a name="get-user-details"></a>ユーザーの詳細情報を取得する

この時点で、認証サービスはユーザーの表示名と電子メールアドレスの定数値を設定します。 これでアクセストークンが作成されました。これらの値が現在のユーザーに対応するように、Microsoft Graph からユーザーの詳細を取得することができます。

1. **/Src/app/auth.service.ts**を開き、次の `import` ステートメントをファイルの先頭に追加します。

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. という名前のクラスに新しい関数を追加 `AuthService` `getUser` します。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. アクセストークンを表示する警告を追加するメソッドで、次のコードを見つけて削除し `getAccessToken` ます。

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. メソッドから次のコードを見つけ、削除し `signIn` ます。

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. その場所に、次のコードを追加します。

    ```typescript
    this.user = await this.getUser();
    ```

    この新しいコードでは、Microsoft Graph SDK を使用してユーザーの詳細を取得し、 `User` API 呼び出しから返された値を使用してオブジェクトを作成します。

1. ユーザーが既にログインしているかどうかを `constructor` 確認するには、クラスのを変更し、その `AuthService` 場合は詳細情報を読み込みます。 既存のを `constructor` 次のものに置き換えます。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. 一時コードをクラスから削除 `HomeComponent` します。 **/Src/app/home/home.component.ts**を開き、既存の `signIn` 関数を次のように置き換えます。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet":::

変更を保存してアプリを開始した場合は、サインイン後にホームページに戻る必要がありますが、UI は、サインインしていることを示すように変更する必要があります。

![サインイン後のホーム ページのスクリーンショット](./images/add-aad-auth-01.png)

右上隅にあるユーザーアバターをクリックして、[ **サインアウト** ] リンクにアクセスします。 **[サインアウト]** をクリックすると、セッションがリセットされ、ホーム ページに戻ります。

![[サインアウト] リンクのドロップダウン メニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>トークンの格納と更新

この時点で、アプリケーションには、API 呼び出しのヘッダーで送信されるアクセストークンがあり `Authorization` ます。 これは、アプリがユーザーに代わって Microsoft Graph にアクセスできるようにするトークンです。

ただし、このトークンは一時的なものです。 トークンが発行された後、有効期限が切れる時間になります。 アプリは MSAL ライブラリを使用しているため、トークンストレージを実装したり、ロジックを更新したりする必要はありません。 は、 `MsalService` ブラウザーのストレージにトークンをキャッシュします。 この `acquireTokenSilent` メソッドは、最初にキャッシュされたトークンをチェックし、期限が切れていない場合はそれを返します。 有効期限が切れている場合は、新しい要求を取得するためにサイレント要求を行います。
