<script>
  import { Template } from "svelte-native/components";
  import Asciidoctor from "../../node_modules/@asciidoctor/core/dist/browser/asciidoctor.js";
  const asciidoctor = Asciidoctor();

  let todos = [];
  let textFieldValue = "";
  let ascii = asciidoctor.convert("Hello, _Asciidoctor_");

  function onItemTap(args) {
    console.log(
      `Item ${todos[args.index].name} at index: ${args.index} was tapped`
    );
  }

  function onButtonTap() {
    if (textFieldValue === "") return; // Prevents users from entering an empty string.
    console.log("New task added: " + textFieldValue + "."); // Logs the newly added task in the console for debugging.
    todos = [{ name: textFieldValue }, ...todos]; // Adds tasks in the ToDo array. Newly added tasks are immediately shown on the screen.
    textFieldValue = ""; // Clears the text field so that users can start adding new tasks immediately.
  }
</script>

<page actionBarHidden="false" androidStatusBarBackground="red">
  <actionBar title="My Task1" />
  <tabView androidTabsPosition="bottom">
    <tabViewItem title="To Do">
      <gridLayout columns="*,120" rows="70,*">
        <!-- Configures the text field and ensures that pressing Return on the keyboard
              produces the same result as tapping the button. -->
        <textField
          col="0"
          row="0"
          bind:text={textFieldValue}
          hint={ascii}
          editable="true"
          on:returnPress={onButtonTap}
        />
        <button col="1" row="0" text="Add task" on:tap={onButtonTap} />

        <listView items={todos} on:itemTap={onItemTap} row="1" colSpan="2">
          <Template let:item>
            <label text={item.name} textWrap="true" />
          </Template>
        </listView>
      </gridLayout>
    </tabViewItem>

    <tabViewItem title="Completed">
      <label textWrap="true">
        This tab will list completed tasks for tracking.
      </label>
    </tabViewItem>
  </tabView>
</page>

<style>
</style>
