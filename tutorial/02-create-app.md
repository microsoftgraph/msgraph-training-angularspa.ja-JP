<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="755ae-101">このセクションでは、新しい角度プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="755ae-102">コマンドラインインターフェイス (CLI) を開き、ファイルを作成する権限があるディレクトリに移動し、次のコマンドを実行して、[角度付き CLI](https://www.npmjs.com/package/@angular/cli)ツールをインストールし、新しい角度のアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. <span data-ttu-id="755ae-103">角度 CLI は、詳細情報を求めるメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="755ae-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="755ae-104">プロンプトに次のように応答します。</span><span class="sxs-lookup"><span data-stu-id="755ae-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="755ae-105">コマンドが完了したら、CLI の`graph-tutorial`ディレクトリに移動し、次のコマンドを実行してローカル web サーバーを開始します。</span><span class="sxs-lookup"><span data-stu-id="755ae-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="755ae-106">既定のブラウザーが開き[https://localhost:4200/](https://localhost:4200) 、既定の角度ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="755ae-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="755ae-107">ブラウザーが開かない場合は、それを開き、 [https://localhost:4200/](https://localhost:4200)を参照して、新しいアプリが動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="755ae-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="755ae-108">ノードパッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="755ae-108">Add Node packages</span></span>

<span data-ttu-id="755ae-109">に進む前に、後で使用する追加のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="755ae-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="755ae-110">スタイル設定と共通コンポーネントの[ブートストラップ](https://github.com/twbs/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="755ae-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="755ae-111">角度からブートストラップコンポーネントを使用するための[ng](https://github.com/ng-bootstrap/ng-bootstrap) 。</span><span class="sxs-lookup"><span data-stu-id="755ae-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="755ae-112">角度で FontAwesome アイコンを使用する[fontawesome](https://github.com/FortAwesome/angular-fontawesome) 。</span><span class="sxs-lookup"><span data-stu-id="755ae-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="755ae-113">サンプルで使用されている FontAwesome アイコンの[fontawesome](https://github.com/FortAwesome/Font-Awesome)、[フリー (標準)](https://github.com/FortAwesome/Font-Awesome)の svg アイコン、および[フリーソリッドの svg アイコン](https://github.com/FortAwesome/Font-Awesome)。</span><span class="sxs-lookup"><span data-stu-id="755ae-113">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="755ae-114">日付と時刻を書式設定するための[モーメント](https://github.com/moment/moment)。</span><span class="sxs-lookup"><span data-stu-id="755ae-114">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="755ae-115">Azure Active Directory に認証し、アクセストークンを取得するための[角度](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)。</span><span class="sxs-lookup"><span data-stu-id="755ae-115">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="755ae-116">[microsoft graph-](https://github.com/microsoftgraph/msgraph-sdk-javascript) microsoft graph に電話をかけるためのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="755ae-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="755ae-117">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="755ae-117">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. <span data-ttu-id="755ae-118">CLI で次のコマンドを実行して、角度ローカライズパッケージ (ng で必要) を追加します。</span><span class="sxs-lookup"><span data-stu-id="755ae-118">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="755ae-119">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="755ae-119">Design the app</span></span>

<span data-ttu-id="755ae-120">このセクションでは、アプリのユーザーインターフェイスを作成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-120">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="755ae-121">`./src/styles.css`を開き、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="755ae-121">Open the `./src/styles.css` and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="755ae-122">ブートストラップおよび FontAwesome モジュールをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="755ae-122">Add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="755ae-123">を`./src/app/app.module.ts`開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-123">Open `./src/app/app.module.ts` and replace its contents with the following.</span></span>

    ```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
    import { FontAwesomeModule, FaIconLibrary } from '@fortawesome/angular-fontawesome';
    import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
    import { faUserCircle } from '@fortawesome/free-regular-svg-icons';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { NavBarComponent } from './nav-bar/nav-bar.component';
    import { HomeComponent } from './home/home.component';
    import { AlertsComponent } from './alerts/alerts.component';

    @NgModule({
      declarations: [
        AppComponent,
        NavBarComponent,
        HomeComponent,
        AlertsComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        NgbModule,
        FontAwesomeModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule {
      constructor(library: FaIconLibrary) {
        // Register the FontAwesome icons used by the app
        library.addIcons(faExternalLinkAlt, faUserCircle);
      }
     }
    ```

1. <span data-ttu-id="755ae-124">という名前`./src/app` `user.ts`のフォルダーに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="755ae-124">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="755ae-125">ページ上のトップナビゲーションの角度コンポーネントを生成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-125">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="755ae-126">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="755ae-126">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="755ae-127">コマンドが完了したら、 `./src/app/nav-bar/nav-bar.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-127">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
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

1. <span data-ttu-id="755ae-128">`./src/app/nav-bar/nav-bar.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-128">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="755ae-129">アプリのホームページを作成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-129">Create a home page for the app.</span></span> <span data-ttu-id="755ae-130">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="755ae-130">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="755ae-131">コマンドが完了したら、 `./src/app/home/home.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-131">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
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

1. <span data-ttu-id="755ae-132">`./src/app/home/home.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-132">Open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="755ae-133">単純な`Alert`クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-133">Create a simple `Alert` class.</span></span> <span data-ttu-id="755ae-134">という名前`./src/app` `alert.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="755ae-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="755ae-135">アプリでユーザーにメッセージを表示するために使用できる通知サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-135">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="755ae-136">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="755ae-136">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="755ae-137">`./src/app/alerts.service.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-137">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="755ae-138">通知を表示する通知コンポーネントを生成します。</span><span class="sxs-lookup"><span data-stu-id="755ae-138">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="755ae-139">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="755ae-139">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="755ae-140">コマンドが完了したら、 `./src/app/alerts/alerts.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-140">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="755ae-141">`./src/app/alerts/alerts.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-141">Open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="755ae-142">`./src/app/app-routing.module.ts`ファイルを開き、 `const routes: Routes = [];`行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-142">Open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="755ae-143">`./src/app/app.component.html` ファイルを開き、そのコンテンツ全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="755ae-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

<span data-ttu-id="755ae-144">すべての変更を保存し、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="755ae-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="755ae-145">この時点で、アプリの外観は大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="755ae-145">Now, the app should look very different.</span></span>

![デザインが変更されたホーム ページのスクリーンショット](images/create-app-01.png)
