<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="43d3d-101">このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="43d3d-102">/Src/app/graph.service.ts を開き、次の関数をクラスに追加します **。** `GraphService`</span><span class="sxs-lookup"><span data-stu-id="43d3d-102">Open **./src/app/graph.service.ts** and add the following function to the `GraphService` class.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a><span data-ttu-id="43d3d-103">新しいイベントフォームを作成する</span><span class="sxs-lookup"><span data-stu-id="43d3d-103">Create a new event form</span></span>

1. <span data-ttu-id="43d3d-104">フォームを表示する角度コンポーネントを作成し、この新しい関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-104">Create an Angular component to display a form and call this new function.</span></span> <span data-ttu-id="43d3d-105">CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component new-event
    ```

1. <span data-ttu-id="43d3d-106">コマンドが完了したら、/src/app/app-routing.module.ts の配列にコンポーネントを追加します `routes` **。**</span><span class="sxs-lookup"><span data-stu-id="43d3d-106">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. <span data-ttu-id="43d3d-107">**/Src/app/new-event**という名前のディレクトリに新しいファイルを**作成し、** 次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-107">Create a new file in the **./src/app/new-event** directory named **new-event.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    <span data-ttu-id="43d3d-108">このクラスは、新しいイベントフォームのモデルとして機能します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-108">This class will serve as the model for the new event form.</span></span>

1. <span data-ttu-id="43d3d-109">/Src/app/new-event/new-event.component.ts を開き、その内容を次のコードで置き換えます **。**</span><span class="sxs-lookup"><span data-stu-id="43d3d-109">Open **./src/app/new-event/new-event.component.ts** and replace its contents with the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. <span data-ttu-id="43d3d-110">**/Src/app/new-event/new-event.component.html**を開き、その内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="43d3d-110">Open **./src/app/new-event/new-event.component.html** and replace its contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. <span data-ttu-id="43d3d-111">変更を保存し、アプリを更新します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-111">Save the changes and refresh the app.</span></span> <span data-ttu-id="43d3d-112">[カレンダー] ページの [ **New event** ] ボタンを選択し、フォームを使用してユーザーの予定表にイベントを作成します。</span><span class="sxs-lookup"><span data-stu-id="43d3d-112">Select the **New event** button on the calendar page, then use the form to create an event on the user's calendar.</span></span>

    ![新しいイベントフォームのスクリーンショット](images/create-event.png)
