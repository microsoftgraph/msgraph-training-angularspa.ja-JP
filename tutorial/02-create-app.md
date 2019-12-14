<!-- markdownlint-disable MD002 MD041 -->

コマンドラインインターフェイス (CLI) を開き、ファイルを作成する権限があるディレクトリに移動し、次のコマンドを実行して、[角度付き CLI](https://www.npmjs.com/package/@angular/cli)ツールをインストールし、新しい角度のアプリを作成します。

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

角度 CLI は、詳細情報を求めるメッセージを表示します。 プロンプトに次のように応答します。

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

コマンドが完了したら、CLI の`graph-tutorial`ディレクトリに移動し、次のコマンドを実行してローカル web サーバーを開始します。

```Shell
ng serve --open
```

既定のブラウザーが開き[https://localhost:4200/](https://localhost:4200) 、既定の角度ページが表示されます。 ブラウザーが開かない場合は、それを開き、 [https://localhost:4200/](https://localhost:4200)を参照して、新しいアプリが動作することを確認します。

に進む前に、後で使用する追加のパッケージをインストールします。

- スタイル設定と共通コンポーネントの[ブートストラップ](https://github.com/twbs/bootstrap)。
- 角度からブートストラップコンポーネントを使用するための[ng](https://github.com/ng-bootstrap/ng-bootstrap) 。
- 角度で FontAwesome アイコンを使用する[fontawesome](https://github.com/FortAwesome/angular-fontawesome) 。
- サンプルで使用されている FontAwesome アイコンの[fontawesome](https://github.com/FortAwesome/Font-Awesome)、[フリー (標準)](https://github.com/FortAwesome/Font-Awesome)の svg アイコン、および[フリーソリッドの svg アイコン](https://github.com/FortAwesome/Font-Awesome)。
- 日付と時刻を書式設定するための[モーメント](https://github.com/moment/moment)。
- Azure Active Directory に認証し、アクセストークンを取得するための[角度](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)。
- [rxjs](https://github.com/ReactiveX/rxjs/tree/master/compat)。 `msal-angular`パッケージに必要です。
- [microsoft graph-](https://github.com/microsoftgraph/msgraph-sdk-javascript) microsoft graph に電話をかけるためのクライアントです。

CLI で次のコマンドを実行します。

```Shell
npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.5.0 @fortawesome/fontawesome-svg-core@1.2.25
npm install @fortawesome/free-regular-svg-icons@5.11.2 @fortawesome/free-solid-svg-icons@5.11.2
npm install moment@2.24.0 moment-timezone@0.5.27 @ng-bootstrap/ng-bootstrap@5.1.4
npm install @azure/msal-angular@0.1.4 rxjs-compat@6.5.3 @microsoft/microsoft-graph-client@2.0.0
```

## <a name="design-the-app"></a>アプリを設計する

最初に、ブートストラップ CSS ファイルをアプリに追加して、いくつかのグローバルスタイルも追加します。 `./src/styles.css`を開き、次の行を追加します。

```CSS
@import "~bootstrap/dist/css/bootstrap.css";

/* Add padding for the nav bar */
body {
  padding-top: 4.5rem;
}

/* Style debug info in alerts */
.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

次に、ブートストラップモジュールと FontAwesome モジュールをアプリに追加します。 を`./src/app/app.module.ts`開き、次`import`のステートメントをファイルの先頭に追加します。

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

その後、 `import`すべてのステートメントの後に次のコードを追加します。

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

`@NgModule`宣言で、既存`imports`の配列を次のように置き換えます。

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

これで、ページ上のトップナビゲーションの角度コンポーネントが生成されます。 CLI で、次のコマンドを実行します。

```Shell
ng generate component nav-bar
```

コマンドが完了したら、 `./src/app/nav-bar/nav-bar.component.ts`ファイルを開き、その内容を次のように置き換えます。

```TypeScript
import { Component, OnInit } from '@angular/core';

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
  user: any;

  constructor() { }

  ngOnInit() {
    this.showNav = false;
    this.authenticated = false;
    this.user = {};
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
      email: 'AdeleV@contoso.com'
    };
  }

  signOut(): void {
    // Temporary
    this.authenticated = false;
    this.user = {};
  }
}
```

`./src/app/nav-bar/nav-bar.component.html`ファイルを開き、その内容を次のように置き換えます。

```html
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <div class="container">
    <a routerLink="/" class="navbar-brand">Angular Graph Tutorial</a>
    <button class="navbar-toggler" type="button" (click)="toggleNavBar()" [attr.aria-expanded]="showNav"
    aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" [class.show]="showNav" id="navbarCollapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a routerLink="/" class="nav-link" routerLinkActive="active">Home</a>
        </li>
        <li *ngIf="authenticated" class="nav-item">
          <a routerLink="/calendar" class="nav-link" routerLinkActive="active">Calendar</a>
        </li>
      </ul>
      <ul class="navbar-nav justify-content-end">
        <li class="nav-item">
          <a class="nav-link" href="https://docs.microsoft.com/graph/overview" target="_blank">
            <fa-icon [icon]="[ 'fas', 'external-link-alt' ]" class="mr-1"></fa-icon>Docs
          </a>
        </li>
        <li *ngIf="authenticated" ngbDropdown placement="bottom-right" class="nav-item">
          <a ngbDropdownToggle id="userMenu" class="nav-link" href="javascript:undefined" role="button" aria-haspopup="true"
            aria-expanded="false">
            <div *ngIf="user.avatar; then userAvatar else defaultAvatar"></div>
            <ng-template #userAvatar>
              <img src="user.avatar" class="rounded-circle align-self-center mr-2" style="width: 32px;">
            </ng-template>
            <ng-template #defaultAvatar>
              <fa-icon [icon]="[ 'far', 'user-circle' ]" fixedWidth="true" size="lg"
                class="rounded-circle align-self-center mr-2"></fa-icon>
            </ng-template>
          </a>
          <div ngbDropdownMenu aria-labelledby="userMenu">
            <h5 class="dropdown-item-text mb-0">{{user.displayName}}</h5>
            <p class="dropdown-item-text text-muted mb-0">{{user.email}}</p>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="javascript:undefined" role="button" (click)="signOut()">Sign Out</a>
          </div>
        </li>
        <li *ngIf="!authenticated" class="nav-item">
          <a class="nav-link" href="javascript:undefined" role="button" (click)="signIn()">Sign In</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

次に、アプリのホームページを作成します。 CLI で次のコマンドを実行します。

```Shell
ng generate component home
```

コマンドが完了したら、 `./src/app/home/home.component.ts`ファイルを開き、その内容を次のように置き換えます。

```TypeScript
import { Component, OnInit } from '@angular/core';

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

その後、 `./src/app/home/home.component.html`ファイルを開き、その内容を次のように置き換えます。

```html
<div class="jumbotron">
  <h1>Angular Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API from Angular</p>
  <div *ngIf="authenticated; then welcomeUser else signInPrompt"></div>
  <ng-template #welcomeUser>
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  </ng-template>
  <ng-template #signInPrompt>
    <a href="javascript:undefined" class="btn btn-primary btn-large" role="button" (click)="signIn()">Click here to sign in</a>
  </ng-template>
</div>
```

これで、アプリがユーザーにメッセージを表示するために使用できる通知サービスを作成できるようになります。 最初に、単純な`Alert`クラスを作成します。 という名前`./src/app` `alert.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

CLI で、次のコマンドを実行します。

```Shell
ng generate service alerts
```

`./src/app/alerts.service.ts`ファイルを開き、その内容を次のように置き換えます。

```TypeScript
import { Injectable } from '@angular/core';
import { Alert } from './alert';

@Injectable({
  providedIn: 'root'
})
export class AlertsService {

  alerts: Alert[] = [];

  add(message: string, debug: string) {
    this.alerts.push({message: message, debug: debug});
  }

  remove(alert: Alert) {
    this.alerts.splice(this.alerts.indexOf(alert), 1);
  }
}
```

通知を表示するための警告コンポーネントを生成するようになりました。 CLI で、次のコマンドを実行します。

```Shell
ng generate component alerts
```

コマンドが完了したら、 `./src/app/alerts/alerts.component.ts`ファイルを開き、その内容を次のように置き換えます。

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AlertsService } from '../alerts.service';
import { Alert } from '../alert';

@Component({
  selector: 'app-alerts',
  templateUrl: './alerts.component.html',
  styleUrls: ['./alerts.component.css']
})
export class AlertsComponent implements OnInit {

  constructor(private alertsService: AlertsService) { }

  ngOnInit() {
  }

  close(alert: Alert) {
    this.alertsService.remove(alert);
  }
}
```

その後、 `./src/app/alerts/alerts.component.html`ファイルを開き、その内容を次のように置き換えます。

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

これらの基本コンポーネントが定義されたので、アプリを更新して使用します。 最初に、 `./src/app/app-routing.module.ts`ファイルを開き、 `const routes: Routes = [];`行を次のコードに置き換えます。

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

`./src/app/app.component.html` ファイルを開き、そのコンテンツ全体を次のように置き換えます。

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

すべての変更を保存し、ページを更新します。 この時点で、アプリの外観は大きく異なります。

![再設計されたホームページのスクリーンショット](images/create-app-01.png)
