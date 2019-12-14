<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="eb853-101">この演習では、Azure AD での認証をサポートするために、前の手順で作成したアプリケーションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="eb853-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="eb853-102">これは、Microsoft Graph を呼び出すために必要な OAuth アクセストークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="eb853-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="eb853-103">この手順では、アプリケーションに、[角度の Microsoft 認証ライブラリ](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md)を統合します。</span><span class="sxs-lookup"><span data-stu-id="eb853-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

<span data-ttu-id="eb853-104">という名前`./src` `oauth.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="eb853-105">を`YOUR_APP_ID_HERE`アプリケーション登録ポータルからのアプリケーション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eb853-106">Git などのソース管理を使用している場合は、この時点で、ソース管理`oauth.ts`からファイルを除外して、アプリ ID が誤ってリークしないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="eb853-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="eb853-107">を`./src/app/app.module.ts`開き、次`import`のステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

<span data-ttu-id="eb853-108">次に、 `MsalModule`を`@NgModule`宣言`imports`内の配列に追加し、アプリ ID で初期化します。</span><span class="sxs-lookup"><span data-stu-id="eb853-108">Then add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

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

## <a name="implement-sign-in"></a><span data-ttu-id="eb853-109">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="eb853-109">Implement sign-in</span></span>

<span data-ttu-id="eb853-110">最初に、アプリに`User`表示されるユーザーに関する情報を保持するための単純なクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="eb853-110">Start by defining a simple `User` class to hold the information about the user that the app displays.</span></span> <span data-ttu-id="eb853-111">という名前`./src/app` `user.ts`のフォルダーに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-111">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

<span data-ttu-id="eb853-112">次に、認証サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="eb853-112">Now create an authentication service.</span></span> <span data-ttu-id="eb853-113">このためのサービスを作成することにより、認証方法にアクセスする必要がある任意のコンポーネントに容易に注入することができます。</span><span class="sxs-lookup"><span data-stu-id="eb853-113">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span> <span data-ttu-id="eb853-114">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="eb853-114">Run the following command in your CLI.</span></span>

```Shell
ng generate service auth
```

<span data-ttu-id="eb853-115">コマンドが完了したら、 `./src/app/auth.service.ts`ファイルを開き、その内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-115">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

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

<span data-ttu-id="eb853-116">これで、認証サービスを使用できるようになり、サインインを行うコンポーネントにそのサービスが挿入されるようになります。</span><span class="sxs-lookup"><span data-stu-id="eb853-116">Now that you have the authentication service, it can be injected into the components that do sign-in.</span></span> <span data-ttu-id="eb853-117">から`NavBarComponent`始めます。</span><span class="sxs-lookup"><span data-stu-id="eb853-117">Start with the `NavBarComponent`.</span></span> <span data-ttu-id="eb853-118">`./src/app/nav-bar/nav-bar.component.ts`ファイルを開き、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="eb853-118">Open the `./src/app/nav-bar/nav-bar.component.ts` file and make the following changes.</span></span>

- <span data-ttu-id="eb853-119">ファイル`import { AuthService } from '../auth.service';`の先頭にを追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-119">Add `import { AuthService } from '../auth.service';` to the top of the file.</span></span>
- <span data-ttu-id="eb853-120">クラスから`authenticated`プロパティ`user`とプロパティを削除し、それらを設定するコードを削除`ngOnInit`します。</span><span class="sxs-lookup"><span data-stu-id="eb853-120">Remove the `authenticated` and `user` properties from the class, and remove the code that sets them in `ngOnInit`.</span></span>
- <span data-ttu-id="eb853-121">に`AuthService` `constructor`次のパラメーターを追加して、を`private authService: AuthService`挿入します。</span><span class="sxs-lookup"><span data-stu-id="eb853-121">Inject the `AuthService` by adding the following parameter to the `constructor`: `private authService: AuthService`.</span></span>
- <span data-ttu-id="eb853-122">既存の `signIn` メソッドを次のメソッドに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-122">Replace the existing `signIn` method with the following:</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- <span data-ttu-id="eb853-123">既存の `signOut` メソッドを次のメソッドに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-123">Replace the existing `signOut` method with the following:</span></span>

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

<span data-ttu-id="eb853-124">完了すると、コードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="eb853-124">When you're done, the code should look like the following.</span></span>

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

<span data-ttu-id="eb853-125">クラスの`authenticated`および`user`プロパティを削除したため、 `./src/app/nav-bar/nav-bar.component.html`ファイルも更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb853-125">Since you removed the `authenticated` and `user` properties on the class, you also need to update the `./src/app/nav-bar/nav-bar.component.html` file.</span></span> <span data-ttu-id="eb853-126">そのファイルを開き、次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="eb853-126">Open that file and make the following changes.</span></span>

- <span data-ttu-id="eb853-127">のすべての`authenticated`インスタンスを`authService.authenticated`置換します。</span><span class="sxs-lookup"><span data-stu-id="eb853-127">Replace all instances of `authenticated` with `authService.authenticated`.</span></span>
- <span data-ttu-id="eb853-128">のすべての`user`インスタンスを`authService.user`置換します。</span><span class="sxs-lookup"><span data-stu-id="eb853-128">Replace all instance of `user` with `authService.user`.</span></span>

<span data-ttu-id="eb853-129">次に、 `HomeComponent`クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="eb853-129">Next update the `HomeComponent` class.</span></span> <span data-ttu-id="eb853-130">クラスに加えたすべての変更`./src/app/home/home.component.ts`を、次の例外を使用して行います。 `NavBarComponent`</span><span class="sxs-lookup"><span data-stu-id="eb853-130">Make all of the same changes in `./src/app/home/home.component.ts` that you made to the `NavBarComponent` class with the following exceptions.</span></span>

- <span data-ttu-id="eb853-131">クラスにはメソッドがありません`signOut` `HomeComponent`</span><span class="sxs-lookup"><span data-stu-id="eb853-131">There is no `signOut` method in the `HomeComponent` class.</span></span>
- <span data-ttu-id="eb853-132">`signIn`メソッドを多少異なるバージョンに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-132">Replace the `signIn` method with a slightly different version.</span></span> <span data-ttu-id="eb853-133">このコードは`getAccessToken` 、アクセストークンを取得するために呼び出します。ここでは、トークンをエラーとして出力します。</span><span class="sxs-lookup"><span data-stu-id="eb853-133">This code calls `getAccessToken` to get an access token, which, for now, will output the token as an error.</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

<span data-ttu-id="eb853-134">完了すると、ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="eb853-134">When your done, the file should look like the following.</span></span>

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

<span data-ttu-id="eb853-135">最後に、ナビゲーションバーに対し`./src/app/home/home.component.html`て行ったのと同じ置換を行います。</span><span class="sxs-lookup"><span data-stu-id="eb853-135">Finally, make the same replacements in `./src/app/home/home.component.html` that you made for the nav bar.</span></span>

<span data-ttu-id="eb853-136">変更を保存し、ブラウザーを更新します。</span><span class="sxs-lookup"><span data-stu-id="eb853-136">Save your changes and refresh the browser.</span></span> <span data-ttu-id="eb853-137">[**サインインするにはここを**クリック] ボタンをクリックすると`https://login.microsoftonline.com`、にリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="eb853-137">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="eb853-138">Microsoft アカウントを使用してログインし、要求されたアクセス許可に同意します。</span><span class="sxs-lookup"><span data-stu-id="eb853-138">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="eb853-139">アプリページが更新され、トークンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb853-139">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="eb853-140">ユーザーの詳細を取得する</span><span class="sxs-lookup"><span data-stu-id="eb853-140">Get user details</span></span>

<span data-ttu-id="eb853-141">この時点で、認証サービスはユーザーの表示名と電子メールアドレスの定数値を設定します。</span><span class="sxs-lookup"><span data-stu-id="eb853-141">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="eb853-142">これでアクセストークンが作成されました。これらの値が現在のユーザーに対応するように、Microsoft Graph からユーザーの詳細を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="eb853-142">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span> <span data-ttu-id="eb853-143">を`./src/app/auth.service.ts`開き、次`import`のステートメントをファイルの先頭に追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-143">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

<span data-ttu-id="eb853-144">という名前`AuthService` `getUser`のクラスに新しい関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-144">Add a new function to the `AuthService` class called `getUser`.</span></span>

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

<span data-ttu-id="eb853-145">アクセストークンを表示する警告を追加`getAccessToken`するメソッドで、次のコードを見つけて削除します。</span><span class="sxs-lookup"><span data-stu-id="eb853-145">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

```TypeScript
// Temporary to display token in an error box
if (result) this.alertsService.add('Token acquired', result);
```

<span data-ttu-id="eb853-146">`signIn`メソッドから次のコードを見つけ、削除します。</span><span class="sxs-lookup"><span data-stu-id="eb853-146">Locate and remove the following code from the `signIn` method.</span></span>

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

<span data-ttu-id="eb853-147">その場所に、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="eb853-147">In its place, add the following code.</span></span>

```TypeScript
this.user = await this.getUser();
```

<span data-ttu-id="eb853-148">この新しいコードでは、Microsoft Graph SDK を使用してユーザーの詳細を取得`User`し、API 呼び出しから返された値を使用してオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="eb853-148">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

<span data-ttu-id="eb853-149">`AuthService`クラス`constructor`のを変更して、ユーザーが既にログインしているかどうかを確認し、その場合は詳細情報を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="eb853-149">Now change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="eb853-150">既存`constructor`のを次のものに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-150">Replace the existing `constructor` with the following.</span></span>

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

<span data-ttu-id="eb853-151">最後に、 `HomeComponent`クラスから一時コードを削除します。</span><span class="sxs-lookup"><span data-stu-id="eb853-151">Finally, remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="eb853-152">`./src/app/home/home.component.ts`ファイルを開き、既存`signIn`の関数を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eb853-152">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

<span data-ttu-id="eb853-153">変更を保存してアプリを開始した場合は、サインイン後にホームページに戻る必要がありますが、UI は、サインインしていることを示すように変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eb853-153">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![サインイン後のホームページのスクリーンショット](./images/add-aad-auth-01.png)

<span data-ttu-id="eb853-155">右上隅にあるユーザーアバターをクリックして、[**サインアウト**] リンクにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="eb853-155">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="eb853-156">[**サインアウト**] をクリックすると、セッションがリセットされ、ホームページに戻ります。</span><span class="sxs-lookup"><span data-stu-id="eb853-156">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![[サインアウト] リンクがあるドロップダウンメニューのスクリーンショット](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="eb853-158">トークンの格納と更新</span><span class="sxs-lookup"><span data-stu-id="eb853-158">Storing and refreshing tokens</span></span>

<span data-ttu-id="eb853-159">この時点で、アプリケーションには、API 呼び出しの`Authorization`ヘッダーで送信されるアクセストークンがあります。</span><span class="sxs-lookup"><span data-stu-id="eb853-159">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="eb853-160">これは、アプリがユーザーに代わって Microsoft Graph にアクセスできるようにするトークンです。</span><span class="sxs-lookup"><span data-stu-id="eb853-160">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="eb853-161">ただし、このトークンは存続期間が短くなります。</span><span class="sxs-lookup"><span data-stu-id="eb853-161">However, this token is short-lived.</span></span> <span data-ttu-id="eb853-162">トークンが発行された後、有効期限が切れる時間になります。</span><span class="sxs-lookup"><span data-stu-id="eb853-162">The token expires an hour after it is issued.</span></span> <span data-ttu-id="eb853-163">アプリは MSAL ライブラリを使用しているため、トークンストレージを実装したり、ロジックを更新したりする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="eb853-163">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="eb853-164">は`MsalService` 、ブラウザーのストレージにトークンをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="eb853-164">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="eb853-165">この`acquireTokenSilent`メソッドは、最初にキャッシュされたトークンをチェックし、期限が切れていない場合はそれを返します。</span><span class="sxs-lookup"><span data-stu-id="eb853-165">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="eb853-166">有効期限が切れている場合は、新しい要求を取得するためにサイレント要求を行います。</span><span class="sxs-lookup"><span data-stu-id="eb853-166">If it is expired, it makes a silent request to obtain a new one.</span></span>
