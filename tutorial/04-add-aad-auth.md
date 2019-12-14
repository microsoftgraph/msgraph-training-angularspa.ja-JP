<!-- markdownlint-disable MD002 MD041 -->

この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。 これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。 この手順では、アプリケーションに、[角度の Microsoft 認証ライブラリ](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)を統合します。

という名前`./src` `oauth.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

を`YOUR_APP_ID_HERE`アプリケーション登録ポータルからのアプリケーション ID に置き換えます。

> [!IMPORTANT]
> Git などのソース管理を使用している場合は、この時点で、ソース管理`oauth.ts`からファイルを除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。

を`./src/app/app.module.ts`開き、次`import`のステートメントをファイルの先頭に追加します。

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

次に、 `MsalModule`を`@NgModule`宣言`imports`内の配列に追加し、アプリ ID で初期化します。

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule,
  MsalModule.forRoot({
    clientID: OAuthSettings.appId
  })
],
```

## <a name="implement-sign-in"></a>サインインの実装

最初に、アプリに`User`表示されるユーザーに関する情報を保持するための単純なクラスを定義します。 という名前`./src/app` `user.ts`のフォルダーに新しいファイルを作成し、次のコードを追加します。

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

次に、認証サービスを作成します。 このためのサービスを作成することにより、認証方法にアクセスする必要がある任意のコンポーネントに容易に注入することができます。 CLI で次のコマンドを実行します。

```Shell
ng generate service auth
```

コマンドが完了したら、 `./src/app/auth.service.ts`ファイルを開き、その内容を次のコードで置き換えます。

```TypeScript
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
    let result = await this.msalService.loginPopup(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Login failed', JSON.stringify(reason, null, 2));
      });

    if (result) {
      this.authenticated = true;
      // Temporary placeholder
      this.user = new User();
      this.user.displayName = "Adele Vance";
      this.user.email = "AdeleV@contoso.com";
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
    let result = await this.msalService.acquireTokenSilent(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
      });

    // Temporary to display token in an error box
    if (result) this.alertsService.add('Token acquired', result);
    return result;
  }
}
```

これで、認証サービスを使用できるようになり、サインインを行うコンポーネントにそのサービスが挿入されるようになります。 から`NavBarComponent`始めます。 `./src/app/nav-bar/nav-bar.component.ts`ファイルを開き、次のように変更します。

- ファイル`import { AuthService } from '../auth.service';`の先頭にを追加します。
- クラスから`authenticated`プロパティ`user`とプロパティを削除し、それらを設定するコードを削除`ngOnInit`します。
- に`AuthService` `constructor`次のパラメーターを追加して、を`private authService: AuthService`挿入します。
- 既存の `signIn` メソッドを次のメソッドに置き換えます。

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- 既存の `signOut` メソッドを次のメソッドに置き換えます。

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

完了すると、コードは次のようになります。

```TypeScript
import { Component, OnInit } from '@angular/core';

import { AuthService } from '../auth.service';

@Component({
  selector: 'app-nav-bar',
  templateUrl: './nav-bar.component.html',
  styleUrls: ['./nav-bar.component.css']
})
export class NavBarComponent implements OnInit {

  // Should the collapsed nav show?
  showNav: boolean;

  constructor(private authService: AuthService) { }

  ngOnInit() {
    this.showNav = false;
  }

  // Used by the Bootstrap navbar-toggler button to hide/show
  // the nav in a collapsed state
  toggleNavBar(): void {
    this.showNav = !this.showNav;
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();
  }

  signOut(): void {
    this.authService.signOut();
  }
}
```

クラスの`authenticated`および`user`プロパティを削除したため、 `./src/app/nav-bar/nav-bar.component.html`ファイルも更新する必要があります。 そのファイルを開き、次のように変更します。

- のすべての`authenticated`インスタンスを`authService.authenticated`置換します。
- のすべての`user`インスタンスを`authService.user`置換します。

次に、 `HomeComponent`クラスを更新します。 クラスに加えたすべての変更`./src/app/home/home.component.ts`を、次の例外を使用して行います。 `NavBarComponent`

- クラスにはメソッドがありません`signOut` `HomeComponent`
- `signIn`メソッドを多少異なるバージョンに置き換えます。 このコードは`getAccessToken` 、アクセストークンを取得するために呼び出します。ここでは、トークンをエラーとして出力します。

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

完了すると、ファイルは次のようになります。

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor(private authService: AuthService) { }

  ngOnInit() {
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();

    // Temporary to display the token
    if (this.authService.authenticated) {
      let token = await this.authService.getAccessToken();
    }
  }
}
```

最後に、ナビゲーションバーに対し`./src/app/home/home.component.html`て行ったのと同じ置換を行います。

変更を保存し、ブラウザーを更新します。 [**サインインするにはここを**クリック] ボタンをクリックすると`https://login.microsoftonline.com`、にリダイレクトされます。 Microsoft アカウントを使用してログインし、要求されたアクセス許可に同意します。 アプリページが更新され、トークンが表示されます。

### <a name="get-user-details"></a>ユーザーの詳細を取得する

この時点で、認証サービスはユーザーの表示名と電子メールアドレスの定数値を設定します。 これでアクセストークンが作成されました。これらの値が現在のユーザーに対応するように、Microsoft Graph からユーザーの詳細を取得することができます。 を`./src/app/auth.service.ts`開き、次`import`のステートメントをファイルの先頭に追加します。

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

という名前`AuthService` `getUser`のクラスに新しい関数を追加します。

```TypeScript
private async getUser(): Promise<User> {
  if (!this.authenticated) return null;

  let graphClient = Client.init({
    // Initialize the Graph client with an auth
    // provider that requests the token from the
    // auth service
    authProvider: async(done) => {
      let token = await this.getAccessToken()
        .catch((reason) => {
          done(reason, null);
        });

      if (token)
      {
        done(null, token);
      } else {
        done("Could not get an access token", null);
      }
    }
  });

  // Get the user from Graph (GET /me)
  let graphUser = await graphClient.api('/me').get();

  let user = new User();
  user.displayName = graphUser.displayName;
  // Prefer the mail property, but fall back to userPrincipalName
  user.email = graphUser.mail || graphUser.userPrincipalName;

  return user;
}
```

アクセストークンを表示する警告を追加`getAccessToken`するメソッドで、次のコードを見つけて削除します。

```TypeScript
// Temporary to display token in an error box
if (result) this.alertsService.add('Token acquired', result);
```

`signIn`メソッドから次のコードを見つけ、削除します。

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

その場所に、次のコードを追加します。

```TypeScript
this.user = await this.getUser();
```

この新しいコードでは、Microsoft Graph SDK を使用してユーザーの詳細を取得`User`し、API 呼び出しから返された値を使用してオブジェクトを作成します。

`AuthService`クラス`constructor`のを変更して、ユーザーが既にログインしているかどうかを確認し、その場合は詳細情報を読み込みます。 既存`constructor`のを次のものに置き換えます。

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

最後に、 `HomeComponent`クラスから一時コードを削除します。 `./src/app/home/home.component.ts`ファイルを開き、既存`signIn`の関数を次のように置き換えます。

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

変更を保存してアプリを開始した場合は、サインイン後にホームページに戻る必要がありますが、UI は、サインインしていることを示すように変更する必要があります。

![サインイン後のホームページのスクリーンショット](./images/add-aad-auth-01.png)

右上隅にあるユーザーアバターをクリックして、[**サインアウト**] リンクにアクセスします。 [**サインアウト**] をクリックすると、セッションがリセットされ、ホームページに戻ります。

![[サインアウト] リンクがあるドロップダウンメニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>トークンの格納と更新

この時点で、アプリケーションには、API 呼び出しの`Authorization`ヘッダーで送信されるアクセストークンがあります。 これは、アプリがユーザーに代わって Microsoft Graph にアクセスできるようにするトークンです。

ただし、このトークンは存続期間が短くなります。 トークンが発行された後、有効期限が切れる時間になります。 アプリは MSAL ライブラリを使用しているため、トークンストレージを実装したり、ロジックを更新したりする必要はありません。 は`MsalService` 、ブラウザーのストレージにトークンをキャッシュします。 この`acquireTokenSilent`メソッドは、最初にキャッシュされたトークンをチェックし、期限が切れていない場合はそれを返します。 有効期限が切れている場合は、新しい要求を取得するためにサイレント要求を行います。
