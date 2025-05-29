<script>
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
   });
</script>

<input
   type="text"
   oninput={(e) => {
      yText.delete(0, yText.length);
      yText.insert(0, e.currentTarget.value);
   }}
   {value}
/>
