<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、新しい角度プロジェクトを作成します。

1. コマンドラインインターフェイス (CLI) を開き、ファイルを作成する権限があるディレクトリに移動し、次のコマンドを実行して、 [角度付き CLI](https://www.npmjs.com/package/@angular/cli) ツールをインストールし、新しい角度のアプリを作成します。

    ```Shell
    npm install -g @angular/cli@10.1.7
    ng new graph-tutorial
    ```

1. 角度 CLI は、詳細情報を求めるメッセージを表示します。 プロンプトに次のように応答します。

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. コマンドが完了したら、 `graph-tutorial` CLI のディレクトリに移動し、次のコマンドを実行してローカル web サーバーを開始します。

    ```Shell
    ng serve --open
    ```

1. 既定のブラウザーが開き、既定の角度ページが表示さ [https://localhost:4200/](https://localhost:4200) れます。 ブラウザーが開かない場合は、それを開き、を参照して [https://localhost:4200/](https://localhost:4200) 、新しいアプリが動作することを確認します。

## <a name="add-node-packages"></a>ノードパッケージを追加する

に進む前に、後で使用する追加のパッケージをインストールします。

- スタイル設定と共通コンポーネントの[ブートストラップ](https://github.com/twbs/bootstrap)。
- 角度からブートストラップコンポーネントを使用するための[ng](https://github.com/ng-bootstrap/ng-bootstrap) 。
- 日付と時刻を書式設定するための[モーメント](https://github.com/moment/moment)。
- [windows-iana](https://github.com/rubenillodo/windows-iana)
- Azure Active Directory に認証し、アクセストークンを取得するための[角度](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)。
- [microsoft graph-](https://github.com/microsoftgraph/msgraph-sdk-javascript) microsoft graph に電話をかけるためのクライアントです。

1. CLI で次のコマンドを実行します。

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. CLI で次のコマンドを実行して、角度ローカライズパッケージ (ng で必要) を追加します。

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>アプリを設計する

このセクションでは、アプリのユーザーインターフェイスを作成します。

1. **/Src/styles.css**を開き、次の行を追加します。

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. ブートストラップモジュールをアプリに追加します。 **/Src/app/app.module.ts**を開き、その内容を次のように置き換えます。

    ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        FormsModule,
        AppRoutingModule,
        NgbModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

1. **/Src/app**フォルダーに、 **ts**という新しいファイルを作成し、次のコードを追加します。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. ページ上のトップナビゲーションの角度コンポーネントを生成します。 CLI で、次のコマンドを実行します。

    ```Shell
    ng generate component nav-bar
    ```

1. コマンドが完了したら、 **/src/app/nav-bar/nav-bar.component.ts** を開き、その内容を次のように置き換えます。

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean;
      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: User;

      constructor() { }

      ngOnInit() {
        this.showNav = false;
        this.authenticated = false;
        this.user = null;
      }

      // Used by the Bootstrap navbar-toggler button to hide/show
      // the nav in a collapsed state
      toggleNavBar(): void {
        this.showNav = !this.showNav;
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: null
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = null;
      }
    }
    ```

1. **/Src/app/nav-bar/nav-bar.component.html**を開き、その内容を次のように置き換えます。

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. アプリのホームページを作成します。 CLI で次のコマンドを実行します。

    ```Shell
    ng generate component home
    ```

1. コマンドが完了したら、 **/src/app/home/home.component.ts** を開き、その内容を次のように置き換えます。

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: any;

      constructor() { }

      ngOnInit() {
        this.authenticated = false;
        this.user = {};
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com'
        };
      }
    }
    ```

1. **/Src/app/home/home.component.html**を開き、その内容を次のように置き換えます。

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. 単純なクラスを作成 `Alert` します。 **/Src/app**という名前のディレクトリに新しいファイルを作成し、次のコードを**追加します**。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. アプリでユーザーにメッセージを表示するために使用できる通知サービスを作成します。 CLI で、次のコマンドを実行します。

    ```Shell
    ng generate service alerts
    ```

1. **/Src/app/alerts.service.ts**を開き、その内容を次のように置き換えます。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. 通知を表示する通知コンポーネントを生成します。 CLI で、次のコマンドを実行します。

    ```Shell
    ng generate component alerts
    ```

1. コマンドが完了したら、 **/src/app/alerts/alerts.component.ts** を開き、その内容を次のように置き換えます。

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. **/Src/app/alerts/alerts.component.html**を開き、その内容を次のように置き換えます。

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. **/Src/app/app-routing.module.ts**を開き、行を `const routes: Routes = [];` 次のコードに置き換えます。

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. **/Src/app/app.component.html**を開き、内容全体を次のように置き換えます。

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. **/Src/assets**ディレクトリに、選択した名前の**no-profile-photo.png**のイメージファイルを追加します。 この画像は、ユーザーが Microsoft Graph に写真を持たない場合にユーザーの写真として使用されます。

すべての変更を保存し、ページを更新します。 この時点で、アプリの外観は大きく異なります。

![デザインが変更されたホーム ページのスクリーンショット](images/create-app-01.png)
