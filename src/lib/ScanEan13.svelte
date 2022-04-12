<script>
  import { createEventDispatcher } from 'svelte';
  import { Html5Qrcode, Html5QrcodeSupportedFormats } from "html5-qrcode";

  const dispatch = createEventDispatcher();

  let scanner;
  let scanning = false;

  function success(decodedText, decodedResult) {
    scanning = false;
    console.log(`Code matched = ${decodedText}`, decodedResult);
    scanner.stop();
		dispatch('success', {
			code: decodedText
		});
  }

  function scan() {
    scanning = true;
    const config = { 
        fps: 10, 
        qrbox: { width: 200, height: 60 },
        experimentalFeatures: {
            useBarCodeDetectorIfSupported: true
        },
        formatsToSupport: [Html5QrcodeSupportedFormats.UPC_EAN_EXTENSION]
      };
    scanner = new Html5Qrcode("reader");
    scanner.start({ facingMode: "environment" }, config, success);
  }

  function stop() {
    scanning = false;
    scanner.stop();
		dispatch('close');
  }
</script>

<main>
  <div id="reader" />
  {#if scanning}
    <button on:click="{stop}">stop</button>
  {:else}
    <button on:click={scan}>scan</button>
  {/if}
</main>

<style>
  #reader {
    width: 100%;
    max-width: 640px;
  }
</style>
