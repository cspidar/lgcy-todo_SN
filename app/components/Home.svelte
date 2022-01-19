<script>
  import { TextView } from "@nativescript/core";

  import { Template } from "svelte-native/components";
  // import Asciidoctor from "../../node_modules/@asciidoctor/core/dist/browser/asciidoctor.js";
  // const asciidoctor = Asciidoctor();
  // let ascii = asciidoctor.convert("Hello, _Asciidoctor_");

  let todos = [];
  let dones = [];

  //
  let lists = [
    { word: "apple", mean: "🍎 사과" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
    { word: "banana", mean: "🍌 바나나" },
  ];
  let words = ["apple", "banana"];
  let means = ["🍎 사과", "🍌 바나나"];

  const removeFromList = (list, item) => list.filter((t) => t !== item);
  const addToList = (list, item) => [item, ...list];
  let textFieldValue = "";

  async function onItemTap(args) {
    // console.log(
    //   `Item ${todos[args.index].name} at index: ${args.index} was tapped`
    //   // 인덱스 접근: args.index / todos[args.index].name = todos[0].name
    // );

    let result = await action(
      "What do you want to do with this task?",
      "Cancel",
      ["Mark completed", "Delete forever"]
    );
    console.log(result); // Logs the selected option for debugging.

    let item = todos[args.index];
    switch (result) {
      case "Mark completed":
        dones = addToList(dones, item); // Places the tapped active task at the top of the completed tasks.
        todos = removeFromList(todos, item); // Removes the tapped active task.
        break;
      case "Delete forever":
        todos = removeFromList(todos, item); // Removes the tapped active task.
        break;
      case "Cancel" || undefined: // Dismisses the dialog
        break;
    }
  }

  function onButtonTap() {
    if (textFieldValue === "") return; // Prevents users from entering an empty string.
    console.log("New task added: " + textFieldValue + "."); // Logs the newly added task in the console for debugging.
    //
    todos = [{ name: textFieldValue }, ...todos]; // Adds tasks in the ToDo array. Newly added tasks are immediately shown on the screen.
    //
    textFieldValue = ""; // Clears the text field so that users can start adding new tasks immediately.
  }

  async function onDoneTap(args) {
    let result = await action(
      "What do you want to do with this task?",
      "Cancel",
      ["Mark To Do", "Delete forever"]
    );
    console.log(result); // Logs the selected option for debugging.

    let item = dones[args.index];
    switch (result) {
      case "Mark To Do":
        todos = addToList(todos, item); // Places the tapped active task at the top of the completed tasks.
        dones = removeFromList(dones, item); // Removes the tapped active task.
        break;
      case "Delete forever":
        dones = removeFromList(dones, item); // Removes the tapped active task.
        break;
      case "Cancel" || undefined: // Dismisses the dialog
        break;
    }
  }
</script>

<page actionBarHidden="false" androidStatusBarBackground="#000">
  <actionBar class="actionBar" paddingBottom="0px">
    <button text="Avengers Endgame 단어" col="1" class="action" />
  </actionBar>
  <tabView androidTabsPosition="bottom">
    <tabViewItem title="To Do">
      <!-- <textField
          col="0"
          row="0"
          bind:text={textFieldValue}
          hint="do something!"
          editable="true"
          on:returnPress={onButtonTap}
        /> -->
      <!-- <button
          col="1"
          row="0"
          text="Add task"
          on:tap={onButtonTap}
          class="-primary"
        /> -->

      <listView items={lists} on:itemTap={onItemTap} col="0">
        <Template let:item>
          <gridLayout columns="*,*">
            <label
              text={item.word}
              textWrap="true"
              class="card read-word"
              col="0"
            />
            <label
              text={item.mean}
              textWrap="true"
              class="card read-word"
              col="1"
            />
          </gridLayout>
        </Template>
      </listView>
    </tabViewItem>

    <tabViewItem title="Completed">
      <listView items={dones} on:itemTap={onDoneTap}>
        <Template let:item>
          <label text={item.name} textWrap="true" class="todo-item-completed" />
        </Template>
      </listView>
    </tabViewItem>
  </tabView>
</page>

<style>
  label {
    font-size: 16;
  }
  .todo-item-completed {
    color: #939393;
    text-decoration: line-through;
  }
  .card {
    border: 1px solid black;
  }
  .action {
    font-size: 15px;
    line-height: 1.6;
    margin: 0px;
    margin-bottom: 5px;
    padding: 0px;
    vertical-align: bottom;
    width: 700px;
    height: 120px;
    /* font-weight: bold; */
    /* 안드로이드 기본 폰트 확인해서 지정 필요 */
  }
  .actionBar {
    margin: 0;
    padding-top: 25px;
    height: 160px;
  }
</style>
