<script lang="ts">
   import { onMount } from "svelte";
   import { WebsocketProvider } from "y-websocket";
   import * as Y from "yjs";

   const yDoc = new Y.Doc();
   new WebsocketProvider(
      "wss://demos.yjs.dev/ws",
      "sldkjflaijdqowiejaslkdjasldfzxcjlfjsdf",
      yDoc,
   );
   let value = $state("");
   const yText = yDoc.getText("name");
   yText.observe(() => {
      value = yText.toString();
   });
   onMount(() => {
      value = yText.toString();

      yText.observe((event) => {
         const newValue = yText.toString();
         if (newValue !== value) {
            value = newValue;
         }
      });
   });
   function handleInput(e: Event) {
      const newVal = (e.currentTarget as HTMLInputElement).value;

      // Find the first diff index
      let i = 0;
      while (i < value.length && value[i] === newVal[i]) i++;

      // Delete old chars
      if (value.length > i) {
         yText.delete(i, value.length - i);
      }

      // Insert new chars
      if (newVal.length > i) {
         yText.insert(i, newVal.slice(i));
      }

      // Update local value to reflect change immediately
      value = newVal;
   }
</script>

<input type="text" oninput={handleInput} {value} />
