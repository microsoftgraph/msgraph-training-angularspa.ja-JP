<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c4113-101">このセクションでは、新しい角度プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="c4113-102">コマンドラインインターフェイス (CLI) を開き、ファイルを作成する権限があるディレクトリに移動し、次のコマンドを実行して、 [角度付き CLI](https://www.npmjs.com/package/@angular/cli) ツールをインストールし、新しい角度のアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@10.1.7
    ng new graph-tutorial
    ```

1. <span data-ttu-id="c4113-103">角度 CLI は、詳細情報を求めるメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="c4113-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="c4113-104">プロンプトに次のように応答します。</span><span class="sxs-lookup"><span data-stu-id="c4113-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="c4113-105">コマンドが完了したら、 `graph-tutorial` CLI のディレクトリに移動し、次のコマンドを実行してローカル web サーバーを開始します。</span><span class="sxs-lookup"><span data-stu-id="c4113-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="c4113-106">既定のブラウザーが開き、既定の角度ページが表示さ [https://localhost:4200/](https://localhost:4200) れます。</span><span class="sxs-lookup"><span data-stu-id="c4113-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="c4113-107">ブラウザーが開かない場合は、それを開き、を参照して [https://localhost:4200/](https://localhost:4200) 、新しいアプリが動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="c4113-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="c4113-108">ノードパッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="c4113-108">Add Node packages</span></span>

<span data-ttu-id="c4113-109">に進む前に、後で使用する追加のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="c4113-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="c4113-110">スタイル設定と共通コンポーネントの[ブートストラップ](https://github.com/twbs/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="c4113-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="c4113-111">角度からブートストラップコンポーネントを使用するための[ng](https://github.com/ng-bootstrap/ng-bootstrap) 。</span><span class="sxs-lookup"><span data-stu-id="c4113-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="c4113-112">日付と時刻を書式設定するための[モーメント](https://github.com/moment/moment)。</span><span class="sxs-lookup"><span data-stu-id="c4113-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="c4113-113">windows-iana</span><span class="sxs-lookup"><span data-stu-id="c4113-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="c4113-114">Azure Active Directory に認証し、アクセストークンを取得するための[角度](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)。</span><span class="sxs-lookup"><span data-stu-id="c4113-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="c4113-115">[microsoft graph-](https://github.com/microsoftgraph/msgraph-sdk-javascript) microsoft graph に電話をかけるためのクライアントです。</span><span class="sxs-lookup"><span data-stu-id="c4113-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="c4113-116">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4113-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. <span data-ttu-id="c4113-117">CLI で次のコマンドを実行して、角度ローカライズパッケージ (ng で必要) を追加します。</span><span class="sxs-lookup"><span data-stu-id="c4113-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="c4113-118">アプリを設計する</span><span class="sxs-lookup"><span data-stu-id="c4113-118">Design the app</span></span>

<span data-ttu-id="c4113-119">このセクションでは、アプリのユーザーインターフェイスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="c4113-120">**/Src/styles.css**を開き、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="c4113-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="c4113-121">ブートストラップモジュールをアプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="c4113-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="c4113-122">**/Src/app/app.module.ts**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="c4113-123">**/Src/app**フォルダーに、 **ts**という新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4113-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="c4113-124">ページ上のトップナビゲーションの角度コンポーネントを生成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="c4113-125">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4113-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="c4113-126">コマンドが完了したら、 **/src/app/nav-bar/nav-bar.component.ts** を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="c4113-127">**/Src/app/nav-bar/nav-bar.component.html**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="c4113-128">アプリのホームページを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-128">Create a home page for the app.</span></span> <span data-ttu-id="c4113-129">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4113-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="c4113-130">コマンドが完了したら、 **/src/app/home/home.component.ts** を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="c4113-131">**/Src/app/home/home.component.html**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="c4113-132">単純なクラスを作成 `Alert` します。</span><span class="sxs-lookup"><span data-stu-id="c4113-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="c4113-133">**/Src/app**という名前のディレクトリに新しいファイルを作成し、次のコードを**追加します**。</span><span class="sxs-lookup"><span data-stu-id="c4113-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="c4113-134">アプリでユーザーにメッセージを表示するために使用できる通知サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="c4113-135">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4113-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="c4113-136">**/Src/app/alerts.service.ts**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="c4113-137">通知を表示する通知コンポーネントを生成します。</span><span class="sxs-lookup"><span data-stu-id="c4113-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="c4113-138">CLI で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c4113-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="c4113-139">コマンドが完了したら、 **/src/app/alerts/alerts.component.ts** を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="c4113-140">**/Src/app/alerts/alerts.component.html**を開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="c4113-141">**/Src/app/app-routing.module.ts**を開き、行を `const routes: Routes = [];` 次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="c4113-142">**/Src/app/app.component.html**を開き、内容全体を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c4113-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. <span data-ttu-id="c4113-143">**/Src/assets**ディレクトリに、選択した名前の**no-profile-photo.png**のイメージファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="c4113-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="c4113-144">この画像は、ユーザーが Microsoft Graph に写真を持たない場合にユーザーの写真として使用されます。</span><span class="sxs-lookup"><span data-stu-id="c4113-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="c4113-145">すべての変更を保存し、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="c4113-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="c4113-146">この時点で、アプリの外観は大きく異なります。</span><span class="sxs-lookup"><span data-stu-id="c4113-146">Now, the app should look very different.</span></span>

![デザインが変更されたホーム ページのスクリーンショット](images/create-app-01.png)
