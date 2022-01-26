<script>
  import { Frame, TextView } from "@nativescript/core";

  import { Template } from "svelte-native/components";
  // import Asciidoctor from "../../node_modules/@asciidoctor/core/dist/browser/asciidoctor.js";
  // const asciidoctor = Asciidoctor();
  // let ascii = asciidoctor.convert("Hello, _Asciidoctor_");

  import { Application, Utils, Device } from "@nativescript/core";
  import { createEventDispatcher } from "svelte";

  function getActivity() {
    return Application.android.startActivity;
  }
  function statusBarShow() {
    getActivity()
      .getWindow()
      .getDecorView()
      .setSystemUiVisibility(android.view.View.SYSTEM_UI_FLAG_VISIBLE);
  }
  function statusBarHide() {
    getActivity()
      .getWindow()
      .getDecorView()
      .setSystemUiVisibility(android.view.View.SYSTEM_UI_FLAG_FULLSCREEN);
  }
  statusBarHide();

  let todos = [];
  let dones = [];

  //
  let lists = [
    { word: "apple", mean: "🍎 사과1" },
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

  const dispatch = createEventDispatcher();

  let btnShow = "hidden";
  function onWordTap(args) {
    btnShow = "visible";
    console.log(btnShow);
    btnShow = btnShow;
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
        todos = addToList(todos, item);
        dones = removeFromList(dones, item);
        break;
      case "Delete forever":
        dones = removeFromList(dones, item);
        break;
      case "Cancel" || undefined:
        break;
    }
  }
</script>

<page actionBarHidden="true">
  <actionBar title="word" class="actionBar" />

  <tabView
    androidTabsPosition="top"
    android:class="android-tab-view"
    tabBackgroundColor="#31bcff"
    selectedTabTextColor="#455b66"
    tabTextColor="#ffffff"
    androidSelectedTabHighlightColor="#455b66"
  >
    <tabViewItem title="&#xf0ca" class="fas tabViewItem">
      <listView items={lists} on:itemTap={onItemTap}>
        <Template let:item>
          <gridLayout columns="*,*">
            <label
              text={item.word}
              textWrap="true"
              class="card readWord"
              col="0"
            />
            <button class="cardBtn wordAB" on:tap={onWordTap}> 1wordAB </button>
            <button
              id="wordBtn"
              class="cardBtn wordA"
              translateX="-50%"
              width="25.5%"
              col="0"
              visibility={btnShow}
            >
              <formattedString>
                <span text="&#xf00c;" class="fas" />
              </formattedString>
            </button>
            <button
              class="cardBtn wordB"
              translateX="46%"
              width="25.5%"
              col="0"
              on:tap={() => {
                console.log(item.mean);
              }}>12222</button
            >
            <label
              text={item.mean}
              textWrap="true"
              class="card readMean"
              col="1"
            />
            <button class="cardBtn" col="1">check1</button>
          </gridLayout>
        </Template>
      </listView>
    </tabViewItem>

    <tabViewItem title="&#xf560" class="fas tabViewItem">
      <listView items={dones} on:itemTap={onDoneTap}>
        <Template let:item>
          <label text={item.name} textWrap="true" />
        </Template>
      </listView>
    </tabViewItem>
  </tabView>
</page>

<style>
  label {
    font-size: 16;
  }
  .tabViewItem {
    font-size: 25px;
  }
  .card {
    background-color: #c8ccdb;
    border-radius: 3;
  }
  .readWord {
    margin-right: 16px;
  }

  .cardBtn {
    font-size: 30px;
    color: #000000;
    margin: 0;
    padding: 0;
    box-shadow: none;
    transition: all 0.3s;
    background-color: #9e343493;
    /* background-color: #ffffff00; */
  }
  .cardBtn:hover,
  .cardBtn:active {
    box-shadow: 1;
  }
  /* .cardBtn.wordA,
  .cardBtn.wordB {
    visibility: hidden;
  } */
</style>
