<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込みます。 このアプリケーションでは、microsoft graph[クライアント](https://github.com/microsoftgraph/msgraph-sdk-javascript)ライブラリを使用して microsoft graph への呼び出しを行います。

## <a name="get-calendar-events-from-outlook"></a>Outlook から予定表のイベントを取得する

最初に、アプリ`Event`によって表示されるフィールドを定義するクラスを作成します。 という名前`./src/app` `event.ts`のディレクトリに新しいファイルを作成し、次のコードを追加します。

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

次に、すべてのグラフ呼び出しを保持する新しいサービスを追加します。 以前作成した認証サービスと同様に、このためのサービスを作成することで、Microsoft Graph へのアクセスが必要なコンポーネントにそのサービスを注入することができます。 CLI で次のコマンドを実行します。

```Shell
ng generate service graph
```

コマンドが完了したら、 `./src/app/graph.service.ts`ファイルを開き、その内容を次のように置き換えます。

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

このコードの内容を検討してください。

- このメソッドは、サービスのコンストラクタでグラフクライアントを初期化します。
- 次の方法`getEvents`で Graph クライアントを使用する関数を実装します。
  - 呼び出し先の URL は`/me/events`になります。
  - この`select`メソッドは、各イベントに対して返されるフィールドを、ビューが実際に使用するものだけに制限します。
  - メソッド`orderby`は、生成された日付と時刻で結果を並べ替えます。最新のアイテムが最初に表示されます。

この新しいメソッドを呼び出して呼び出しの結果を表示するための、角度のコンポーネントを作成します。 CLI で次のコマンドを実行します。

```Shell
ng generate component calendar
```

コマンドが完了したら、の`routes` `./src/app/app-routing.module.ts`配列にコンポーネントを追加します。

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

`./src/app/calendar/calendar.component.ts`ファイルを開き、その内容を次のように置き換えます。

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

ここでは、JSON のイベントの配列をページにレンダリングするだけです。 変更内容を保存し、アプリを再起動します。 サインインして、ナビゲーションバーの [**予定表**] リンクをクリックします。 すべてが動作する場合は、ユーザーの予定表にイベントの JSON ダンプが表示されます。

## <a name="display-the-results"></a>結果を表示する

これで、 `CalendarComponent`コンポーネントを更新して、よりわかりやすい方法でイベントを表示することができます。 最初に、 `ngOnInit`関数から通知を追加する一時コードを削除します。 更新された関数は、次のようになります。

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

次に、クラスに関数`CalendarComponent`を追加して`DateTimeTimeZone` 、オブジェクトを ISO 文字列に書式設定します。

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

最後に、 `./src/app/calendar/calendar.component.html`ファイルを開き、その内容を次のように置き換えます。

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

これにより、イベントのコレクションがループ処理され、テーブル行が1つずつ追加されます。 変更を保存し、アプリを再起動します。 [**予定表**] リンクをクリックすると、アプリがイベントの表を表示するようになります。

![イベントの表のスクリーンショット](./images/add-msgraph-01.png)
