<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7d8ec-101">この演習では、Microsoft Graph をアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="7d8ec-102">このアプリケーションでは、microsoft graph[クライアント](https://github.com/microsoftgraph/msgraph-sdk-javascript)ライブラリを使用して microsoft graph への呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="7d8ec-103">Outlook から予定表のイベントを取得する</span><span class="sxs-lookup"><span data-stu-id="7d8ec-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="7d8ec-104">最初に、アプリ`Event`によって表示されるフィールドを定義するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-104">Start by creating an `Event` class that defines the fields that the app will display.</span></span> <span data-ttu-id="7d8ec-105">という名前`./src/app` `event.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-105">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

```TypeScript
// For a full list of fields, see
// https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0
export class Event {
  subject: string;
  organizer: Recipient;
  start: DateTimeTimeZone;
  end: DateTimeTimeZone;
}

// https://docs.microsoft.com/graph/api/resources/recipient?view=graph-rest-1.0
export class Recipient {
  emailAddress: EmailAddress;
}

// https://docs.microsoft.com/graph/api/resources/emailaddress?view=graph-rest-1.0
export class EmailAddress {
  name: string;
  address: string;
}

// https://docs.microsoft.com/graph/api/resources/datetimetimezone?view=graph-rest-1.0
export class DateTimeTimeZone {
  dateTime: string;
  timeZone: string;
}
```

<span data-ttu-id="7d8ec-106">次に、すべてのグラフ呼び出しを保持する新しいサービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-106">Next, add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="7d8ec-107">以前作成した認証サービスと同様に、このためのサービスを作成することで、Microsoft Graph へのアクセスが必要なコンポーネントにそのサービスを注入することができます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span> <span data-ttu-id="7d8ec-108">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-108">Run the following command in your CLI.</span></span>

```Shell
ng generate service graph
```

<span data-ttu-id="7d8ec-109">コマンドが完了したら、 `./src/app/graph.service.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-109">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Injectable } from '@angular/core';
import { Client } from '@microsoft/microsoft-graph-client';

import { AuthService } from './auth.service';
import { Event } from './event';
import { AlertsService } from './alerts.service';

@Injectable({
  providedIn: 'root'
})
export class GraphService {

  private graphClient: Client;
  constructor(
    private authService: AuthService,
    private alertsService: AlertsService) {

    // Initialize the Graph client
    this.graphClient = Client.init({
      authProvider: async (done) => {
        // Get the token from the auth service
        let token = await this.authService.getAccessToken()
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
  }

  async getEvents(): Promise<Event[]> {
    try {
      let result =  await this.graphClient
        .api('/me/events')
        .select('subject,organizer,start,end')
        .orderby('createdDateTime DESC')
        .get();

      return result.value;
    } catch (error) {
      this.alertsService.add('Could not get events', JSON.stringify(error, null, 2));
    }
  }
}
```

<span data-ttu-id="7d8ec-110">このコードの内容を検討してください。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="7d8ec-111">このメソッドは、サービスのコンストラクタでグラフクライアントを初期化します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-111">It initializes a Graph client in the constructor for the service.</span></span>
- <span data-ttu-id="7d8ec-112">次の方法`getEvents`で Graph クライアントを使用する関数を実装します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-112">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
  - <span data-ttu-id="7d8ec-113">呼び出し先の URL は`/me/events`になります。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-113">The URL that will be called is `/me/events`.</span></span>
  - <span data-ttu-id="7d8ec-114">この`select`メソッドは、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
  - <span data-ttu-id="7d8ec-115">メソッド`orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-115">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="7d8ec-116">この新しいメソッドを呼び出して呼び出しの結果を表示するための、角度のコンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-116">Now create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="7d8ec-117">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-117">Run the following command in your CLI.</span></span>

```Shell
ng generate component calendar
```

<span data-ttu-id="7d8ec-118">コマンドが完了したら、の`routes` `./src/app/app-routing.module.ts`配列にコンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-118">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

<span data-ttu-id="7d8ec-119">`./src/app/calendar/calendar.component.ts`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-119">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import * as moment from 'moment-timezone';

import { GraphService } from '../graph.service';
import { Event, DateTimeTimeZone } from '../event';
import { AlertsService } from '../alerts.service';

@Component({
  selector: 'app-calendar',
  templateUrl: './calendar.component.html',
  styleUrls: ['./calendar.component.css']
})
export class CalendarComponent implements OnInit {

  private events: Event[];

  constructor(
    private graphService: GraphService,
    private alertsService: AlertsService) { }

  ngOnInit() {
    this.graphService.getEvents()
      .then((events) => {
        this.events = events;
        // Temporary to display raw results
        this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
      });
  }
}
```

<span data-ttu-id="7d8ec-120">ここでは、JSON のイベントの配列をページにレンダリングするだけです。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="7d8ec-121">変更内容を保存し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-121">Save your changes and restart the app.</span></span> <span data-ttu-id="7d8ec-122">サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="7d8ec-123">すべてが動作する場合は、ユーザーの予定表にイベントの JSON ダンプが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="7d8ec-124">結果を表示する</span><span class="sxs-lookup"><span data-stu-id="7d8ec-124">Display the results</span></span>

<span data-ttu-id="7d8ec-125">これで、 `CalendarComponent`コンポーネントを更新して、よりわかりやすい方法でイベントを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="7d8ec-126">最初に、 `ngOnInit`関数から通知を追加する一時コードを削除します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-126">First, remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="7d8ec-127">更新された関数は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-127">Your updated function should look like this.</span></span>

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

<span data-ttu-id="7d8ec-128">次に、クラスに関数`CalendarComponent`を追加して`DateTimeTimeZone` 、オブジェクトを ISO 文字列に書式設定します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-128">Now add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

```TypeScript
formatDateTimeTimeZone(dateTime: DateTimeTimeZone): string {
  try {
    return moment.tz(dateTime.dateTime, dateTime.timeZone).format();
  }
  catch(error) {
    this.alertsService.add('DateTimeTimeZone conversion error', JSON.stringify(error));
  }
}
```

<span data-ttu-id="7d8ec-129">最後に、 `./src/app/calendar/calendar.component.html`ファイルを開き、その内容を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-129">Finally, open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <th scope="col">Organizer</th>
    <th scope="col">Subject</th>
    <th scope="col">Start</th>
    <th scope="col">End</th>
  </thead>
  <tbody>
    <tr *ngFor="let event of events">
      <td>{{event.organizer.emailAddress.name}}</td>
      <td>{{event.subject}}</td>
      <td>{{formatDateTimeTimeZone(event.start) | date:'short' }}</td>
      <td>{{formatDateTimeTimeZone(event.end) | date: 'short' }}</td>
    </tr>
  </tbody>
</table>
```

<span data-ttu-id="7d8ec-130">これにより、イベントのコレクションがループ処理され、テーブル行が1つずつ追加されます。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="7d8ec-131">変更を保存し、アプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-131">Save the changes and restart the app.</span></span> <span data-ttu-id="7d8ec-132">[**予定表**] リンクをクリックすると、アプリがイベントの表を表示するようになります。</span><span class="sxs-lookup"><span data-stu-id="7d8ec-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![イベントの表のスクリーンショット](./images/add-msgraph-01.png)
