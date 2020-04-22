<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3ea89-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="3ea89-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="3ea89-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="3ea89-103">この手順では、アプリケーションに、[角度の Microsoft 認証ライブラリ](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)を統合します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="3ea89-104">という名前`./src` `oauth.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.ts.example":::

    <span data-ttu-id="3ea89-105">を`YOUR_APP_ID_HERE`アプリケーション登録ポータルからのアプリケーション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3ea89-106">Git などのソース管理を使用している場合は、この時点で、ソース管理`oauth.ts`からファイルを除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3ea89-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="3ea89-107">を`./src/app/app.module.ts`開き、次`import`のステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

    ```TypeScript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="3ea89-108">を宣言内の`imports`配列に追加し、アプリ ID で初期化します。 `@NgModule` `MsalModule`</span><span class="sxs-lookup"><span data-stu-id="3ea89-108">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports":::

## <a name="implement-sign-in"></a><span data-ttu-id="3ea89-109">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="3ea89-109">Implement sign-in</span></span>

<span data-ttu-id="3ea89-110">このセクションでは、認証サービスを作成し、サインインとサインアウトを実装します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-110">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="3ea89-111">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-111">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="3ea89-112">このためのサービスを作成することにより、認証方法にアクセスする必要がある任意のコンポーネントに容易に注入することができます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-112">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="3ea89-113">コマンドが完了したら、 `./src/app/auth.service.ts`ファイルを開き、その内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-113">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

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
        let result = await this.msalService.loginPopup(OAuthSettings)
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
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.add('Token acquired', result.accessToken);
          return result.accessToken;
        }
        return null;
      }
    }
    ```

1. <span data-ttu-id="3ea89-114">`./src/app/nav-bar/nav-bar.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-114">Open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. <span data-ttu-id="3ea89-115">を`./src/app/home/home.component.ts`開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-115">Open`./src/app/home/home.component.ts` and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-27":::

<span data-ttu-id="3ea89-116">変更を保存し、ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-116">Save your changes and refresh the browser.</span></span> <span data-ttu-id="3ea89-117">[**サインインするにはここを**クリック] ボタンをクリックすると`https://login.microsoftonline.com`、にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-117">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="3ea89-118">Microsoft アカウントを使用してログインし、要求されたアクセス許可に同意します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="3ea89-119">アプリページが更新され、トークンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-119">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="3ea89-120">ユーザーの詳細情報を取得する</span><span class="sxs-lookup"><span data-stu-id="3ea89-120">Get user details</span></span>

<span data-ttu-id="3ea89-121">この時点で、認証サービスはユーザーの表示名と電子メールアドレスの定数値を設定します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-121">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="3ea89-122">これでアクセストークンが作成されました。これらの値が現在のユーザーに対応するように、Microsoft Graph からユーザーの詳細を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-122">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="3ea89-123">を`./src/app/auth.service.ts`開き、次`import`のステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-123">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

    ```TypeScript
    import { Client } from '@microsoft/microsoft-graph-client';
    ```

1. <span data-ttu-id="3ea89-124">という名前`AuthService` `getUser`のクラスに新しい関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-124">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. <span data-ttu-id="3ea89-125">アクセストークンを表示する警告を追加`getAccessToken`するメソッドで、次のコードを見つけて削除します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-125">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```TypeScript
    // Temporary to display token in an error box
    this.alertsService.add('Token acquired', result);
    ```

1. <span data-ttu-id="3ea89-126">`signIn`メソッドから次のコードを見つけ、削除します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-126">Locate and remove the following code from the `signIn` method.</span></span>

    ```TypeScript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    ```

1. <span data-ttu-id="3ea89-127">その場所に、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-127">In its place, add the following code.</span></span>

    ```TypeScript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="3ea89-128">この新しいコードでは、Microsoft Graph SDK を使用してユーザーの詳細を取得`User`し、API 呼び出しから返された値を使用してオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-128">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="3ea89-129">ユーザーが`constructor`既に`AuthService`ログインしているかどうかを確認するには、クラスのを変更し、その場合は詳細情報を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-129">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="3ea89-130">既存`constructor`のを次のものに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-130">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. <span data-ttu-id="3ea89-131">一時コードを`HomeComponent`クラスから削除します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-131">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="3ea89-132">`./src/app/home/home.component.ts`ファイルを開き、既存`signIn`の関数を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3ea89-132">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet" highlight="5-6":::

<span data-ttu-id="3ea89-133">変更を保存してアプリを開始した場合は、サインイン後にホームページに戻る必要がありますが、UI は、サインインしていることを示すように変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3ea89-133">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![サインイン後のホーム ページのスクリーンショット](./images/add-aad-auth-01.png)

<span data-ttu-id="3ea89-135">右上隅にあるユーザーアバターをクリックして、[**サインアウト**] リンクにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="3ea89-135">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="3ea89-136">**[サインアウト]** をクリックすると、セッションがリセットされ、ホーム ページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="3ea89-136">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![[サインアウト] リンクのドロップダウン メニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="3ea89-138">トークンの格納と更新</span><span class="sxs-lookup"><span data-stu-id="3ea89-138">Storing and refreshing tokens</span></span>

<span data-ttu-id="3ea89-139">この時点で、アプリケーションには、API 呼び出しの`Authorization`ヘッダーで送信されるアクセストークンがあります。</span><span class="sxs-lookup"><span data-stu-id="3ea89-139">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="3ea89-140">これは、アプリがユーザーに代わって Microsoft Graph にアクセスできるようにするトークンです。</span><span class="sxs-lookup"><span data-stu-id="3ea89-140">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="3ea89-141">ただし、このトークンは一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="3ea89-141">However, this token is short-lived.</span></span> <span data-ttu-id="3ea89-142">トークンが発行された後、有効期限が切れる時間になります。</span><span class="sxs-lookup"><span data-stu-id="3ea89-142">The token expires an hour after it is issued.</span></span> <span data-ttu-id="3ea89-143">アプリは MSAL ライブラリを使用しているため、トークンストレージを実装したり、ロジックを更新したりする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3ea89-143">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="3ea89-144">は`MsalService` 、ブラウザーのストレージにトークンをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="3ea89-144">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="3ea89-145">この`acquireTokenSilent`メソッドは、最初にキャッシュされたトークンをチェックし、期限が切れていない場合はそれを返します。</span><span class="sxs-lookup"><span data-stu-id="3ea89-145">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="3ea89-146">有効期限が切れている場合は、新しい要求を取得するためにサイレント要求を行います。</span><span class="sxs-lookup"><span data-stu-id="3ea89-146">If it is expired, it makes a silent request to obtain a new one.</span></span>
