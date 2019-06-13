<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ea97c-101">コマンドラインインターフェイス (CLI) を開き、ファイルを作成する権限があるディレクトリに移動し、次のコマンドを実行して、[角度付き CLI](https://www.npmjs.com/package/@angular/cli)ツールをインストールし、新しい角度のアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

<span data-ttu-id="ea97c-102">角度 CLI は、詳細情報を求めるメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-102">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="ea97c-103">プロンプトに次のように応答します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-103">Answer the prompts as follows.</span></span>

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

<span data-ttu-id="ea97c-104">コマンドが完了したら、CLI の`graph-tutorial`ディレクトリに移動し、次のコマンドを実行してローカル web サーバーを開始します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-104">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

```Shell
ng serve --open
```

<span data-ttu-id="ea97c-105">既定のブラウザーが開き[https://localhost:4200/](https://localhost:4200) 、既定の角度ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-105">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="ea97c-106">ブラウザーが開かない場合は、それを開き、 [https://localhost:4200/](https://localhost:4200)を参照して、新しいアプリが動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-106">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

<span data-ttu-id="ea97c-107">に進む前に、後で使用する追加のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ea97c-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="ea97c-108">スタイル設定と共通コンポーネントの[ブートストラップ](https://github.com/twbs/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="ea97c-108">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="ea97c-109">角度からブートストラップコンポーネントを使用するための[ng](https://github.com/ng-bootstrap/ng-bootstrap) 。</span><span class="sxs-lookup"><span data-stu-id="ea97c-109">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="ea97c-110">角度で FontAwesome アイコンを使用する[fontawesome](https://github.com/FortAwesome/angular-fontawesome) 。</span><span class="sxs-lookup"><span data-stu-id="ea97c-110">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="ea97c-111">サンプルで使用されている FontAwesome アイコンの[fontawesome](https://github.com/FortAwesome/Font-Awesome)、[フリー (標準)](https://github.com/FortAwesome/Font-Awesome)の svg アイコン、および[フリーソリッドの svg アイコン](https://github.com/FortAwesome/Font-Awesome)。</span><span class="sxs-lookup"><span data-stu-id="ea97c-111">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="ea97c-112">日付と時刻を書式設定するための[モーメント](https://github.com/moment/moment)。</span><span class="sxs-lookup"><span data-stu-id="ea97c-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="ea97c-113">Azure Active Directory に認証し、アクセストークンを取得するための[角度](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)。</span><span class="sxs-lookup"><span data-stu-id="ea97c-113">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="ea97c-114">[rxjs](https://github.com/ReactiveX/rxjs/tree/master/compat)。 `msal-angular`パッケージに必要です。</span><span class="sxs-lookup"><span data-stu-id="ea97c-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), required for the `msal-angular` package.</span></span>
- <span data-ttu-id="ea97c-115">[microsoft graph-](https://github.com/microsoftgraph/msgraph-sdk-javascript) microsoft graph に電話をかけるためのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="ea97c-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="ea97c-116">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-116">Run the following command in your CLI.</span></span>

```Shell
npm install bootstrap@4.3.1 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.17
npm install @fortawesome/free-regular-svg-icons@5.8.1 @fortawesome/free-solid-svg-icons@5.8.1
npm install moment@2.24.0 moment-timezone@0.5.25 @ng-bootstrap/ng-bootstrap@4.1.2
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.5.1 @microsoft/microsoft-graph-client@1.6.0
```

## <a name="design-the-app"></a><span data-ttu-id="ea97c-117">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="ea97c-117">Design the app</span></span>

<span data-ttu-id="ea97c-118">最初に、ブートストラップ CSS ファイルをアプリに追加して、いくつかのグローバルスタイルも追加します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-118">Start by adding the Bootstrap CSS files to the app, as well as some global styles.</span></span> <span data-ttu-id="ea97c-119">`./src/styles.css`を開き、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-119">Open the `./src/styles.css` and add the following lines.</span></span>

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

<span data-ttu-id="ea97c-120">次に、ブートストラップモジュールと FontAwesome モジュールをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-120">Next, add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="ea97c-121">を`./src/app/app.module.ts`開き、次`import`のステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-121">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

<span data-ttu-id="ea97c-122">その後、 `import`すべてのステートメントの後に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-122">Then add the following code after all of the `import` statements.</span></span>

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

<span data-ttu-id="ea97c-123">`@NgModule`宣言で、既存`imports`の配列を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-123">In the `@NgModule` declaration, replace the existing `imports` array with the following.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

<span data-ttu-id="ea97c-124">これで、ページ上のトップナビゲーションの角度コンポーネントが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-124">Now generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="ea97c-125">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-125">In your CLI, run the following command.</span></span>

```Shell
ng generate component nav-bar
```

<span data-ttu-id="ea97c-126">コマンドが完了したら、 `./src/app/nav-bar/nav-bar.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-126">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="ea97c-127">`./src/app/nav-bar/nav-bar.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-127">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="ea97c-128">次に、アプリのホームページを作成します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-128">Next, create a home page for the app.</span></span> <span data-ttu-id="ea97c-129">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-129">Run the following command in your CLI.</span></span>

```Shell
ng generate component home
```

<span data-ttu-id="ea97c-130">コマンドが完了したら、 `./src/app/home/home.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-130">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="ea97c-131">その後、 `./src/app/home/home.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-131">Then open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="ea97c-132">これで、アプリがユーザーにメッセージを表示するために使用できる通知サービスを作成できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ea97c-132">Now create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="ea97c-133">最初に、単純な`Alert`クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-133">Start by creating a simple `Alert` class.</span></span> <span data-ttu-id="ea97c-134">という名前`./src/app` `alert.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

<span data-ttu-id="ea97c-135">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-135">In your CLI, run the following command.</span></span>

```Shell
ng generate service alerts
```

<span data-ttu-id="ea97c-136">`./src/app/alerts.service.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-136">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="ea97c-137">通知を表示するための警告コンポーネントを生成するようになりました。</span><span class="sxs-lookup"><span data-stu-id="ea97c-137">Now generate an alerts component to display alerts.</span></span> <span data-ttu-id="ea97c-138">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-138">In your CLI, run the following command.</span></span>

```Shell
ng generate component alerts
```

<span data-ttu-id="ea97c-139">コマンドが完了したら、 `./src/app/alerts/alerts.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-139">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="ea97c-140">その後、 `./src/app/alerts/alerts.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-140">Then open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

<span data-ttu-id="ea97c-141">これらの基本コンポーネントが定義されたので、アプリを更新して使用します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-141">Now with those basic components defined, update the app to use them.</span></span> <span data-ttu-id="ea97c-142">最初に、 `./src/app/app-routing.module.ts`ファイルを開き、 `const routes: Routes = [];`行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-142">First, open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

<span data-ttu-id="ea97c-143">`./src/app/app.component.html` ファイルを開き、そのコンテンツ全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ea97c-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

<span data-ttu-id="ea97c-144">すべての変更を保存し、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="ea97c-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="ea97c-145">この時点で、アプリの外観は大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="ea97c-145">Now, the app should look very different.</span></span>

![再設計されたホームページのスクリーンショット](images/create-app-01.png)
